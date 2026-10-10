# KobitoKey — miwa-prac personal keymap

移植元: [miwa-prac/zmk-config-roBa](https://github.com/miwa-prac/zmk-config-roBa/blob/3269927c1ff2b3d0757c35758132ebcd11a95359/config/roBa.keymap)。43キーを小人キーの40キーへ移植。

## 基本配置

各行は左から右の順。最下段も左5キー・右5キー。

| 行 | 左5キー | 右5キー |
|---|---|---|
| 1 | Q W E R T | Y U I O P |
| 2 | A S D F G | H J K L , |
| 3 | Z X C V B | N M 左クリック 右クリック 中クリック |
| 4 | Win 未割当 Backspace Space/NUM Enter/Shift | Tab Enter/Ctrl Space/ARROW 未割当 Esc/Alt |

「タップ/ホールド」の順。レイヤー0の左最下段2列目・右最下段4列目を未割当にし、右最下段をTab・Enter・Space・未割当・Escの順に配置。各キーのLT・Mod-Tapをそのまま維持。iPadの単独キー配置は今回変更していません。

## 操作

- Lをホールド: 右トラックボールをスクロールへ。左トラックボールは常時スクロールの既存設定を維持。
- 小人キー標準のD＋F: 英数、J＋K: かな。
- F＋G: .、V＋B: _、Y＋U: -、H＋J: !、N＋M: @。
- レイヤー1の右手3列目・最下段（位置37）: Tab。Z＋Xのコンボは削除。
- 数字レイヤー中はC位置＋V位置: #。
- E＋RのFUNCTIONコンボ、左下3キーのSETTINGSコンボは削除。レイヤー1・6・12・13へのコンボ入口はありません。
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
