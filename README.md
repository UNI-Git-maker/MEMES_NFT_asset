# MEMES_NFT_asset

NFTのメタデータと画像を **GitHub Pages** から配信するためのリポジトリ。

配信元: `https://uni-git-maker.github.io/MEMES_NFT_asset/`

```
metadata/<64桁の16進>.json   ← ウォレットが読むメタデータ
images/<ファイル名>.png       ← 実際の画像
```

## なぜこのリポジトリが必要か

ERC-1155 の `uri()` は `.../{id}` というテンプレートを返し、
クライアントは `{id}` を **64桁ゼロ埋めの小文字16進** に置き換える決まりになっている
（[EIP-1155](https://eips.ethereum.org/EIPS/eip-1155) の MUST 要件）。

ところが OpenSea で発行したコレクションは、**10進数のファイル名**でメタデータを置いていた。

```
ウォレットが探す : /0000000000000000000000000000000000000000000000000000000000000001
OpenSeaが置いた  : /1
```

このため MetaMask などの規格準拠クライアントが 404 になり、画像が表示されなかった。
OpenSea は自前のインデックスを使うので、OpenSea 上では正常に見えてしまい気付きにくい。

ここに正しい名前で置き直し、コントラクトの `setBaseURI` をこちらへ向けることで解決する。

## なぜ IPFS ではないのか

Pinata の無料枠は **500ファイル**（他に 1GB / 帯域10GB / ゲートウェイ10Kリクエスト）。
1イベントで100枚規模を使うため、回を重ねるとすぐ上限に達する。

GitHub Pages は帯域 100GB/月・リポジトリ 1GB まで。画像数十枚なら桁違いに余裕がある。

IPFS の利点（CIDが中身のハッシュなので参照が持ち主から独立する）は本物だが、
コントラクトの `setBaseURI` を持ち続ける限り、配信先はいつでも移せる。
所有権を放棄しない方針なので、この弱点は実害にならないと判断した。

## 注意

- **`raw.githubusercontent.com` は使わない。** JSON が `text/plain` で返るため、
  ウォレットによっては解釈に失敗する。GitHub Pages は MIME タイプが正しい
- ファイル名に **`.json` を付ける**。拡張子が無いと MIME タイプが不定になる
- `setBaseURI` には `.../metadata/{id}.json` まで含めた文字列を入れる

## 更新のしかた

メタデータは配布サイト側のリポジトリから生成する。

```bash
# MEMES_NFT_distribution 側で
npm run build:metadata -- --base https://uni-git-maker.github.io/MEMES_NFT_asset
```

画像は `images/` に置いて push する。
