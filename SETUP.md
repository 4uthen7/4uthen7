# 公開手順とカスタマイズ

## 1. リポジトリを作る

GitHub で **`4uthen7`** という名前の **public** リポジトリを新規作成します（プロフィール README は「ユーザー名と同じ名前のリポジトリ」の README が表示されます）。

```powershell
cd github-profile
git init
git add README.md SETUP.md assets
git commit -m "profile: animated hero"
git branch -M main
git remote add origin https://github.com/4uthen7/4uthen7.git
git push -u origin main
```

push すれば `https://github.com/4uthen7` の上部にこの README が出ます（`preview/` と `tools/` は push しなくても動きます）。

## 2. 仕組み（なぜ動くのか）

- GitHub の Markdown は **HTML / CSS / JS を無効化**します。だから動画や CSS は直接書けません。
- 代わりに **SVG の中に CSS アニメーションを入れて `<img>` として読ませる**と、ブラウザが普通の画像と同じようにアニメーションを再生します。GIF より軽く、拡大してもぼやけません。
- `<picture>` + `prefers-color-scheme` で、ダーク／ライトを自動で出し分けています。
- SVG は `<img>` 扱いなので **外部フォントは読めません**。フォントは OS 標準（Segoe UI / Noto Sans JP / Yu Gothic / Cascadia Mono）を指定しています。
- `prefers-reduced-motion` を入れてあるので、動きを減らす設定の人は静止画で見えます。

## 3. 名前を変える

`assets/hero-dark.svg` と `assets/hero-light.svg` の両方に、**同じ文字列が 2 か所ずつ**あります。必ず両方直してください（片方は表示用、もう片方は光の帯を文字の形に切り抜くマスク用です）。

```svg
<text x="64" y="236" class="jpName" fill="url(#ink)">高本</text>   <!-- 表示 -->
...
<mask id="mJP" ...>
  <text x="64" y="236" class="jpName" fill="#fff">高本</text>     <!-- マスク -->
</mask>
```

- 日本語名 → `class="jpName"`（Noto Sans JP Black / 150px）
- 欧文名 → `class="enName"`（Segoe UI Black / 122px）。`mEN` のマスクも同じ文字列にします。
- 長い名前にするときは `font-size` を下げてください（横 680px くらいまでが安全域）。

## 4. 色・速さを変える

| 変えたいもの | 場所 |
| --- | --- |
| アクセント色 | 各 SVG の `#7C5CFF`（紫）/ `#2BE0FF`（シアン）/ `#C9FF3D`（ライム） |
| 名前の切り替わり速度 | `.nameA` / `.nameB` / `.sheenA` / `.sheenB` の `animation: ... 9s` を同じ秒数に揃える |
| 下部の流れる文字 | `TYPESCRIPT · REACT · ...` の 2 か所（`textLength="1136"`） |
| キャッチコピー | `class="tw"` の `<tspan>`（1 文字ずつ）。文字を増減したら、カーソル位置 `x="632"` と `animation-delay` を調整 |

## 5. 任意：プレビュー

- `preview/index.html` … ブラウザで開くとダーク／ライトの動きをその場で確認できます
- `preview/hero-dark.gif` … 共有用の動画っぽいプレビュー（README には不要）

## 6. ハマりどころ

- リポジトリ名がユーザー名と一致していないとプロフィールに出ません。
- Private リポジトリだと README は表示されません。
- 画像パスは**大文字小文字まで**一致させてください（GitHub は Linux 上で動いています）。
- アニメーションが止まって見える場合は、OS の「アニメーションを減らす」設定がオンになっていないか確認してください。
