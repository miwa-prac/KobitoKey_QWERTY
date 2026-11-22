# KobitoKey — miwa-prac personal keymap

移植元: [miwa-prac/zmk-config-roBa](https://github.com/miwa-prac/zmk-config-roBa/blob/3269927c1ff2b3d0757c35758132ebcd11a95359/config/roBa.keymap)。43キーを小人キーの40キーへ移植。

## 基本配置

各行は左から右の順。最下段も左5キー・右5キー。

| 行 | 左5キー | 右5キー |
|---|---|---|
| 1 | Q W E R T | Y U I O P |
| 2 | A S D F G | H J K L , |
| 3 | Z X C V B | N M 未割当 未割当 未割当 |
| 4 | Win Esc Backspace Space/NUM Enter/Shift | Enter/Ctrl Space/ARROW Tab 未割当 Alt（長押しのみ） |

「タップ/ホールド」の順。左最下段2列目にEscを配置。右最下段はタップ無効・長押しAlt。SはタップでS、長押しでCtrl。iPad側もS長押しCtrlと右最下段Alt長押しを反映。

## 操作

- Lをホールド: 右トラックボールをスクロールへ。縦方向は維持し、横方向の反転を解除。
- 左トラックボールはカーソル操作。ボールを上へ動かすとカーソルが右へ動くよう90度回転。
- 小人キー標準のD＋F: 英数、J＋K: かな。
- F＋G: .、V＋B: _、Y＋U: -、H＋J: !、N＋M: @。
- レイヤー1の右手3列目・最下段（位置37）: Tab。Z＋Xのコンボは削除。
- 数字レイヤー中はC位置＋V位置: #。
- E＋RのFUNCTIONコンボ、左下3キーのSETTINGSコンボは削除。レイヤー1・6・12・13へのコンボ入口はありません。
- Q＋W: Bluetooth 0 / Windows、W＋E: Bluetooth 1 / Windows、Q＋W＋E: Bluetooth 2 / iPad。
- 右トラックボール操作: WIN（0）からAUTO_MOUSE（4）、IPAD（8）からIPAD_AUTO_MOUSE（14）へ自動切替。操作停止後2.5秒で復帰。キー入力後350msは誤作動防止のため切替を抑制。
- AUTO_MOUSE: U/I/O位置に左/中/右クリック。WINではSpace/NUM、iPadではSpace/NUMのレイヤータップを維持し、数字レイヤーへ入れる。位置34・38（WIN）、位置38（iPad）で通常レイヤーへ戻る。右最下段はタップ無効・長押しAlt。
- S長押しでCtrl。右トラックボールのスクロール中にSを押せばCtrl＋スクロールのズーム操作、右手クリックと組み合わせればCtrl＋クリックに使える。

## レイヤー

0 WIN、1 FUNCTION、2 NUM、3 ARROW、4 AUTO_MOUSE、5 SCROLL、6 SETTINGS、7 RESERVED、8 IPAD、9 IPAD_ARROW。
10〜13はiPadモードで下位レイヤーが遮られるのを防ぐNUM・SCROLL・FUNCTION・SETTINGSの複製。14はIPAD_AUTO_MOUSE。

roBaのエンコーダ操作は小人キーにエンコーダがないため移植していません。Layer-Tapのrequire-prior-idle-msは0にして、連続操作でホールドが無視される条件を外しています。

## ビルド・書込み

GitHub ActionsのBuild ZMK firmwareで左・右・settings_resetをビルド。左右に対応するUF2を書き込んでください。
ZMK Studioで保存済みのキーマップがある場合、Studioで設定をリセットして新しいファームウェアの配置を反映してください。
ケースの3DデータはReleasesから取得できます。
