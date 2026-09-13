# MEMES_NFT_asset

NFTのメタデータと画像を **GitHub Pages** から配信するリポジトリ。

配信元: https://uni-git-maker.github.io/MEMES_NFT_asset/

## 構成

```
metadata/1.json 〜 17.json   ← ウォレットが読むメタデータ。ファイル名は tokenId（10進）
contract.json                ← コレクション情報（名前・説明・アイコン）
images/*.png                 ← 実際の画像
.nojekyll                    ← ファイル名が _ で始まっても配信されるようにする
```

コントラクトの `uri(id)` が `<配信元>/metadata/<id>.json` を返すので、
**ファイル名は tokenId と一致していなければならない。**

| tokenId | レア度 | 画像 |
|---|---|---|
| 1〜5 | N | `n-01.png` 〜 `n-05.png` |
| 6〜8 | R | `r-01.png` 〜 `r-03.png` |
| 9〜11 | SR | `sr-01.png` 〜 `sr-03.png` |
| 12〜16 | SSR | `ssr-01.png` 〜 `ssr-05.png` |
| 17 | LAST ONE | `lastone.png` |

## 更新のしかた

手で編集しない。配布サイト側のスタジオが両方を書き出す。

```bash
# MEMES_NFT_distribution 側で
npm run studio      # http://127.0.0.1:4000
```

画像を差し替えるだけなら、`images/` の同名ファイルを上書きして push すればよい。

## 関連リポジトリ

| | 役割 |
|---|---|
| [MEMES_NFT_contract](https://github.com/UNI-Git-maker/MEMES_NFT_contract) | コントラクト。`uri(id)` がここを指す |
| [MEMES_NFT_distribution](https://github.com/UNI-Git-maker/MEMES_NFT_distribution) | 配布サイトとスタジオ |
