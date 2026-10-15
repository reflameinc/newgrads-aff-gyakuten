# newgrads-aff-gyakuten — 新卒｜アフィ（逆転就活）

- 公開URL: https://reflame.co.jp/lp/newgrads/aff/gyakuten/
- 予約完了ページ: https://reflame.co.jp/lp/newgrads/aff/gyakuten/thanks/
- 担当: 鈴村さん
- 仕組みの元: [reflameinc/newgrads-aff](https://github.com/reflameinc/newgrads-aff)（プルリクエストをマージすると本番に反映される仕組み）
- 困ったとき: 有川へ。画面に出た文をそのまま貼ってください

## いまの状態（2026-10-15 時点）

| 項目 | 状態 | やる人 |
|---|---|---|
| 自動反映の設定ファイル（`.github/`・`scripts/`） | 済み。newgrads-aff と同じ内容に、下の「このLPだけの違い」を足したもの | — |
| main を守るルール・production 環境・サーバーの指紋（`DEPLOY_KNOWN_HOSTS`） | 済み | — |
| サーバーに入る鍵（`DEPLOY_SSH_KEY`） | **未** | 中山さん（作業A） |
| LPのファイル（`index.html`・`thanks/index.html`・`assets/`） | **未**。このリポジトリにはまだLPが入っていない | 鈴村さん（作業B） |
| 本番へ反映するスイッチ（`DEPLOY_ENABLED`） | オフ | 有川（作業C） |

スイッチがオフの間は、マージしても本番は変わりません。AとBの両方が済んだら、Cに進みます。AとBはどちらが先でもかまいません。

### このLPだけの違い（newgrads-aff との差）

- 予約完了ページ `thanks/index.html` も本番へ送る。検品では、thanks にも noindex・GTM があること、画像の参照が `../assets/` を向いていることを確かめる。thanks が無いと止まる。
- 転送先は `/lp/newgrads/aff/gyakuten/` に固定。初回の転送で `gyakuten/` フォルダが作られる（サーバーで事前に作る必要はない）。

## 作業A: サーバーに入る鍵を登録する（中山さんのPC）

10/6 に12本へ登録したのと同じ鍵を、このリポジトリにも登録します。鍵は GitHub から取り出せないため、鍵のあるPCでしかできません。

1. 中山さんのPCで Claude Code を開く（フォルダはどこでもよい）。
2. 下の文をそのまま貼る。

```
reflameinc/newgrads-aff-gyakuten に、本番サーバーの SSH 鍵を登録してください。

- 鍵は、10/6 に reflameinc の LP リポジトリ12本の production 環境へ DEPLOY_SSH_KEY として登録したのと同じ秘密鍵です。このPCの中から探してください（~/.ssh/ など）。
- 見つけた鍵が正しいことを、次のコマンドが ok を返すことで確かめてください。
  ssh -i <鍵のパス> -p 8022 -o BatchMode=yes r1300591@www1151.onamae.ne.jp "echo ok"
- 鍵の中身は、画面にもファイルにも出さないでください。
- 確かめられたら、次のコマンドで登録してください。
  gh secret set DEPLOY_SSH_KEY --env production -R reflameinc/newgrads-aff-gyakuten < <鍵のパス>
- 最後に gh secret list --env production -R reflameinc/newgrads-aff-gyakuten を実行し、DEPLOY_SSH_KEY と DEPLOY_KNOWN_HOSTS の2つが並んでいることを見せてください。
```

3. うまくいったとき: `DEPLOY_SSH_KEY` と `DEPLOY_KNOWN_HOSTS` の2行が出る。
4. うまくいかないとき: 「鍵が見つからない」「Permission denied」「HTTP 403」などの文を、そのまま有川へ送る。

## 作業B: LPのファイルを入れる（鈴村さんのPC）

制作したLPを、このリポジトリに入れます。

1. 自分のPCで Claude Code を開く。
2. 下の文の `<　>` を、LPの元ファイル（`index.html` と `thanks/` と `img/` が入っているフォルダ）の場所に書き換えて貼る。

```
reflameinc/newgrads-aff-gyakuten をこのPCにクローンして、新卒｜アフィ（逆転就活）のLP（https://reflame.co.jp/lp/newgrads/aff/gyakuten/）のファイルを入れ、プルリクエストを作ってください。
進め方と決まりは、そのリポジトリの README.md の「作業B」に書いてあります。先に読んでから始めてください。
LPの元ファイルは <このPCのフォルダの場所> にあります。
```

3. Claude Code がプルリクエストを作ったら、表示されたURLをブラウザで開く。
4. 画面の下のほうに、緑のチェックと「check」の文字が出るまで待つ（1〜2分）。
5. 緑になったら「Merge pull request」→「Confirm merge」を押す（この時点ではまだ本番は変わらない）。
6. 有川へ「newgrads-aff-gyakuten にLPを入れてマージしました」と連絡する（作業Cへ）。

赤い × が出たときは、×の横の「Details」を開き、🛑 が付いた行を Claude Code に貼って「これを直して」と頼みます。直らなければ、その行を有川へ送ってください。

### 決まり（Claude Code はここを読んで作業する）

- **入れるもの**: 公開する本体の `index.html`、予約完了ページの `thanks/index.html`、画像などの `assets/` だけ。元のフォルダにある配色違いの HTML、古い版、作業用のスクリプトなどは入れない。どれが本体か分からなければ、担当に聞く。
- **置き方**: `index.html` はいちばん上の階層、予約完了ページは `thanks/index.html`。画像・CSS・JS・フォント・動画は、すべて `assets/` の中に置く（`thanks/` の中に画像を置かない）。
- **`img/` を `assets/` に変える**: 元ファイルの画像が `img/` に入っているときは、フォルダ名を `assets/` に変え、参照（`src`・`href`・`srcset`・CSS の `url()`・OGP画像のURL）も全部書き換える。`index.html` からは `assets/…`、`thanks/index.html` からは `../assets/…` で参照する。
- **`assets/` に置ける種類**: png・jpg・jpeg・webp・gif・avif・ico・css・js・woff・woff2・mp4・webm。ファイル名は英数字と `_` `-` `.` だけ、拡張子は小文字。
- **svg はファイルとして置けない**: png か webp に変換して参照を書き換えるか、HTML の中に直接書く（インラインSVG）。見た目が変わらないことを確かめる。
- **計測と予約は本番用にする**: 確認用に計測タグや予約カレンダーを外した版があれば、入れるのは外す前の本番用。`index.html` と `thanks/index.html` の両方に noindex と GTM（`GTM-5KH7WRCP`）を入れる。`dataLayer` に送る `lp` の値は、ほかのLPと見分けられる名前にする。中身に迷ったら、勝手に決めず担当に見せて決めてもらう。
- **main に直接は入れられない**: ブランチを作る → プルリクエストを作る → 検品（check）が緑 → マージ、の順。
- **`.github/` と `scripts/` は触らない**: 変えると中山さんのレビューが必要になる。
- **検品は GitHub の画面で見る**: Mac に最初から入っている bash では `scripts/check.sh` がエラーで止まる。手元で動かなくても、プルリクエストの check が緑なら問題ない。

## 作業C: 本番へ反映するスイッチを入れる（有川）

1. 作業Bのマージ後、Actions の `deploy` が「▶ 予行演習モード」で動き、「転送予定 N 件」の一覧に `index.html`・`thanks/index.html`・`assets/…` が出ていることを有川が確かめる（作業Aが済んでいないと、ここで「Secret DEPLOY_SSH_KEY が未登録」と出て止まる）。
2. 有川がリポジトリ変数 `DEPLOY_ENABLED` を `true` にして、`deploy` をやり直す。
3. 本番へ転送され、「✓ 本番の index.html がリポジトリと一致」「✓ 本番の thanks/index.html がリポジトリと一致」「✓ assets 全て HTTP 200」が出れば完了。
4. 本番で1件予約し、thanks に移ること・計測が動くことを担当と確かめる。

## 作業A〜Cが終わったあとの、ふだんの直し方

reflame-marketing のフォルダで Claude Code に「逆転就活のLPのここを直して」と頼みます（`lp` スキル）。ブランチを作る → 直す → プルリクエスト → check が緑 → マージ、の順で、マージした時点で本番に反映されます。戻すときは、マージ済みプルリクエストの「Revert」を押し、できたプルリクエストをマージします。

**GitHub に入れたLPは、サーバーへ直接上げない（FileZilla 等を使わない）。** 直接上げると、次の公開で GitHub の中身に上書きされて消えます。
