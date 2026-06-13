# [地理探求　用語辞書](https://geography.hs.wing.osaka)

## ディレクトリ構造

```
/
├── public/
│   ├── favicon.ico
│   ├── robots.txt
│   ├── _headers
│   └── _redirects
├── app
│   ├── assets
│   │   └── global.scss
│   ├── components
│   │   └── Header.vue
│   ├── layouts
│   │   └── default.astro
│   ├── pages
│   │   └── index.vue
│   └── var
│       └── msg.ts
├── content
│   └── dict <- ファイル名は全て"index.md"
│       ├── jp/index.md <- 国家の場合ISO 3166-1に従う（ドメイン名とおおかた一致するが，GBなど例外あり）
│       ├── eu/index.md <- トップレベルドメインがあればそれに従う
│       └── asia_nies/index.md <- 単語を繋げる時は「_」（アンダースコア）を使う
└── package.json
```