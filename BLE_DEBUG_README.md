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
