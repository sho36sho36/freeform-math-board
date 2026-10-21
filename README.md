# Math Board — Freeform風の手書き数式ホワイトボード

GitHub Pagesで公開できる、タッチ・Apple Pencil対応のホワイトボードです。

## 機能

- 手書き入力（マウス、タッチ、Apple Pencil）
- キャンバスの拡大・縮小、移動
- 消しゴム、全消去、元に戻す・やり直す
- ローカル保存、JSONの書き出し・読み込み
- ひとまとまりのストロークを、書き終わり後に自動認識
- 認識結果の近くに計算結果を表示
- 認識エンジンはブラウザ側で実行（APIキー不要）

## 重要：認識モデルの準備

`ink-on` のJavaScriptパッケージにモデルは含まれていません。次の3ファイルが必要です。

- `encoder_int8.onnx`
- `decoder_int8.onnx`
- `vocab.json`

1. [ink-on のGitHubリポジトリ](https://github.com/kimseungdae/ink-on) を開きます。
2. READMEの **Download Models** の案内から、配布されているモデルファイルを入手します。
3. このプロジェクトの `models/comer/` フォルダに3ファイルを置きます。最終的な配置は次のとおりです。

```
models/
  comer/
    encoder_int8.onnx
    decoder_int8.onnx
    vocab.json
```

モデルファイルは合計約7.2 MBです。リポジトリに大きなファイルを追加する前に、配布元のライセンス・利用条件を確認してください。

> 注意：このプロジェクトはモデルを同梱していません。モデルを配置するまでは、描画・移動・保存機能は使えますが、自動認識は動作しません。

## WindowsでGitHub Pagesに公開する手順

### 1. ZIPを展開

ダウンロードしたZIPを、例えば `C:\Users\あなたの名前\Desktop\freeform-math-board` に展開します。

### 2. モデルファイルを配置

上記3ファイルを `models\comer\` にコピーします。

### 3. GitHubにリポジトリを作成

1. GitHubにサインインします。
2. **New repository** を選びます。
3. リポジトリ名を `freeform-math-board` にします。
4. Publicを選び、Create repositoryを押します。

### 4. ファイルをアップロード

GitHubの新しいリポジトリ画面で **Add file → Upload files** を選び、`index.html`、`README.md`、`models` フォルダ内の3ファイルをアップロードして、**Commit changes** を押します。

ファイルが多い場合はGitを使う方法もあります。Git for Windowsをインストールした後、PowerShellでプロジェクトフォルダに移動して次を実行します。

```powershell
git init
git add index.html README.md models
git commit -m "Create Math Board"
git branch -M main
git remote add origin https://github.com/あなたのGitHubID/freeform-math-board.git
git push -u origin main
```

`あなたのGitHubID` は自分のGitHubユーザー名に置き換えてください。

### 5. GitHub Pagesを有効化

1. リポジトリの **Settings → Pages** を開きます。
2. **Build and deployment** の **Deploy from a branch** を選びます。
3. Branchを `main`、フォルダを `/(root)` にして **Save** を押します。
4. 数分待ち、表示されたURLを開きます。通常は `https://あなたのGitHubID.github.io/freeform-math-board/` 形式です。

### 6. iPhone・iPadで使う

公開URLをSafariで開きます。初回は認識ライブラリとモデルの読み込みに時間がかかる場合があります。初回ロード時はインターネット接続が必要です。

## ローカルで試す方法

`index.html` を直接ダブルクリックするより、VS CodeのLive ServerなどでローカルWebサーバー経由で開くことをおすすめします。モデルの読み込みにはHTTPSまたはlocalhostが必要になる場合があります。

## 現在の実装上の注意

- この版では、描画のまとまり（ペンを離してから一定時間）ごとに数式を認識します。隣り合う複数の数式を1つの式として同時認識する機能ではありません。
- 数式の認識は確率的で、特に `÷`、小数、分数、複雑な式などで誤認識することがあります。
- 計算パーサーは基本的な四則演算と括弧を対象にしています。分数や高度なLaTeXの計算は完全対応ではありません。
- `ink-on` のAPI・CDNの公開状況は変更される可能性があります。読み込みに失敗する場合は、ライブラリの最新READMEを確認してください。
- `index.html` の `initRecognition()` は、モデルを `./models/comer/` から読み込む設定です。
