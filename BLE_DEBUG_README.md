# KobitoKey BLE 接続失敗の診断（検証用ブランチ）

**main ブランチには影響しません。** このブランチの Central（左）用ビルドだけ、USB CDC ACM シリアルに ZMK / Zephyr のログを出力します。キーマップや左右通信の設定は維持しています。

## ビルド・書き込み

1. このブランチの GitHub Actions「Build ZMK firmware」が成功したことを確認します。
2. Actions の run ページの `firmware` artifact（ZIP）をダウンロードして展開します。
3. **`KobitoKey_left_ble_debug.uf2`** だけを左側（Central）に書き込みます。
4. 右側（Peripheral）は現状のファームウェアのままで構いません。診断中は電源を切っても構いません。
5. **Settings Reset は実施しない**でください。既存のペアリング保存状況をなるべく維持して原因を観察します。

## Windows 11 でログ採取

1. 左側を USB ケーブルで Windows に接続します（BLE接続の成否を見るため、USB を挿したままでOKです）。
2. Windows「デバイス マネージャー」→「ポート (COM と LPT)」で `USB Serial Device (COMx)` を見つけます。
3. PuTTY / Tera Term 等でその COM ポートを **115200 baud / 8 data bits / parity none / 1 stop bit** で開き、ログ表示・保存を開始します。USB CDC では baud rate は実際の USB 速度を決めません。
4. ポートを開いたまま、Windows の「Bluetooth とデバイス」→「デバイスの追加」で **KobitoKey** を選び、接続失敗を再現します。
5. 失敗時刻の前後 **20〜30秒** を保存します。必要であれば Bluetooth を一度オフ・オンし、もう一度実施します。
6. `connected`, `disconnected`, `security`, `pair`, `smp`, `auth`, `reason`, `err`, `fail` を探します。

ログ用設定:
- `build.yaml`: 左側のみ `zmk-usb-logging` snippet を使用（通常の Studio USB UART snippet との二重使用を回避）。
- `KobitoKey_left.conf`: `CONFIG_BT_CONN_LOG_LEVEL_DBG=y`, `CONFIG_BT_SMP_LOG_LEVEL_DBG=y`, `CONFIG_LOG_BUFFER_SIZE=8192`。
- **ZMK Studio の USB 通信は診断ビルドでは使用しません。**

**注意:** SMP デバッグログにはペアリング関連の機微なデータが含まれる場合があります。ログを公開 Issue 等に投稿する前に鍵/アドレス/識別子などを確認して伏せてください。診断完了後は main の通常ファームウェアへ戻してください。

**補足:** USB 接続中は ZMK が USB HID 出力を優先する場合がありますが、BLE のリンク確立とセキュリティイベントはシリアルログで確認できます。USB による挙動変化が疑われる場合は、USB 無しの BLE 検証と結果を比較してください。


## パスキー認証の A/B 検証（2026-10-12 追加）

Windows の接続失敗ログで `smp_pairing_complete: got status 0x3` / `Security failed ... err 4` を確認しました。これは認証要件を満たせなかったことを示します。クラッシュを意味するコードではありません。認証の不一致と保存済み Bond 情報のどちらが原因かはまだ確定していません。

このブランチでは、同一コミットから左側 Central 用に **2 つの診断ファームウェア**をビルドします。

| UF2 | 説明 |
| --- | --- |
| `KobitoKey_left_ble_debug.uf2` | 従来の診断用（変更なし） |
| `KobitoKey_left_ble_passkey_debug.uf2` | 従来の診断用 ＋ `CONFIG_ZMK_BLE_PASSKEY_ENTRY=y` |

`build.yaml` の passkey ビルドに `cmake-args: -DCONFIG_ZMK_BLE_PASSKEY_ENTRY=y` を追加し、`KobitoKey_left.conf` は書き換えていません。GitHub Actions の Kconfig 出力に **`CONFIG_ZMK_BLE_PASSKEY_ENTRY=y`** が現れることを確認してから書き込んでください。main には影響しません。

### テスト手順

1. GitHub Actions の最新の成功 run → `firmware` ZIP を展開し、`KobitoKey_left_ble_passkey_debug.uf2` を左側 Central に書き込みます。右側は書き換えません。
2. Windows の Bluetooth 設定に以前の KobitoKey 登録が残っていれば削除します。できれば未使用の Bluetooth プロファイルを選んでください。**まずは Settings Reset を実施せずに検証**します。
3. 左側 Central を USB で接続し、PuTTY で COM ポートを開きます（115200 / 8N1、ログ保存を有効化）。
4. Windows から KobitoKey を追加します。Windows が **6 桁の PIN / パスキー**を表示したら、**PC のキーではなく KobitoKey 左側のキーで入力**し、Enter で確定します。
   - Windows 配列のレイヤー 2（NUM）は左下 4 列目の **Space キーを押し続ける**と有効になります。
   - その間の数字配置は **Q=0 / W=1 / E=2 / R=3 / S=4 / D=5 / F=6 / X=7 / C=8 / V=9** です。
   - 6 桁入力後、Space を離し、左手最下段の 5 列目（通常は Shift/Enter のタップで Enter）を押します。
   - この入力手順はキーマップの Windows デフォルトレイヤーでの操作です。右側が動かなくても左側だけで入力できます。
5. 接続可否と `putty.log` の `pairing` / `security` / `smp` / `err` を確認します。

**重要:** `CONFIG_ZMK_BLE_PASSKEY_ENTRY` はパスキー入力を提供するだけであり、現時点で接続成功を保証するものではありません。新旧の UF2 で認証エラーがどう変化したかを比較してください。初回テスト後は必要に応じて両側の Bond 情報を整理します。

診断ログには鍵情報や Bluetooth アドレスなどが含まれる場合があります。公開する前に確認し、診断完了後は通常版へ戻してください。
