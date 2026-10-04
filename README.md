# KobitoKey — miwa-prac personal keymap

移植元: [miwa-prac/zmk-config-roBa](https://github.com/miwa-prac/zmk-config-roBa/blob/3269927c1ff2b3d0757c35758132ebcd11a95359/config/roBa.keymap)。43キーを小人キーの40キーへ移植。

## 基本配置

各行は左から右の順。最下段も左5キー・右5キー。

| 行 | 左5キー | 右5キー |
|---|---|---|
| 1 | Q W E R T | Y U I O P |
| 2 | A S D F G | H J K L , |
| 3 | Z X C V B | N M 左クリック 右クリック 中クリック |
| 4 | Win かな 英数 Backspace Space/NUM | Enter/Shift Enter/Ctrl Space/ARROW . Esc/Alt |

「タップ/ホールド」の順。iPadモードではWin→Command、Enter/Ctrl→Command、Space/ARROW→iPad用矢印へ切替。

## 操作

- Lをホールド: 右トラックボールをスクロールへ。左トラックボールは常時スクロールの既存設定を維持。
- Z＋X: Tab、C＋V: _、H＋J: -、.＋H: !、N＋M: @。
- 数字レイヤー中はZ位置＋X位置: =、C位置＋V位置: #、H位置＋J位置: _。
- E＋Rをホールド: FUNCTION（F1〜F12）。その間H位置＋J位置: F13。
- 左下のWin・かな・英数を同時ホールド: SETTINGS。Y〜P位置でBluetooth 0〜4、最下段の.位置でbootloader、右端で全接続解除、3段目右端で現在の接続解除。
- Q＋W: Bluetooth 0 / Windows、W＋E: Bluetooth 1 / Windows、Q＋W＋E: Bluetooth 2 / iPad。
- 右トラックボール操作: 自動MOUSE（4）、5秒滞留。マウスボタンの位置27〜29はレイヤー維持対象。

## レイヤー

0 WIN、1 FUNCTION、2 NUM、3 ARROW、4 MOUSE、5 SCROLL、6 SETTINGS、7 RESERVED、8 IPAD、9 IPAD_ARROW。
10〜13はiPadモードで下位レイヤーが遮られるのを防ぐNUM・SCROLL・FUNCTION・SETTINGSの複製。

roBaのエンコーダ操作は小人キーにエンコーダがないため移植していません。Layer-Tapのrequire-prior-idle-msは0にして、連続操作でホールドが無視される条件を外しています。

## ビルド・書込み

GitHub ActionsのBuild ZMK firmwareで左・右・settings_resetをビルド。左右に対応するUF2を書き込んでください。
ZMK Studioで保存済みのキーマップがある場合、Studioで設定をリセットして新しいファームウェアの配置を反映してください。
ケースの3DデータはReleasesから取得できます。
