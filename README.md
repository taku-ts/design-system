# @taku-ts/design-system

個人サイト [taku-ts.com](https://taku-ts.com) のための、**CSS だけで作った最小限のデザインシステム**です。

- CSS ファイルを 1 つ読み込むだけで使えます（ビルドツール不要）
- Astro・Vue・素の HTML など、フレームワークを問わず使えます
- npm レジストリには公開していません。GitHub から直接インストールします

カタログ（見た目の一覧）は [`index.html`](./index.html) です。

---

## インストール

### npm（GitHub のタグを指定）

```bash
npm install github:taku-ts/design-system#v0.1.0
```

`package.json` には次のように記録されます。

```json
{
  "dependencies": {
    "@taku-ts/design-system": "github:taku-ts/design-system#v0.1.0"
  }
}
```

### CDN（`<link>` だけで使う）

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/taku-ts/design-system@v0.1.0/styles/index.css" />
```

## 読み込み方

### Astro

レイアウトなど、全ページで使われるファイルの frontmatter で読み込みます。

```astro
---
import "@taku-ts/design-system";
---
```

### 素の HTML

```html
<link rel="stylesheet" href="./node_modules/@taku-ts/design-system/styles/index.css" />
```

## 使い方の例

```html
<main class="container stack stack--lg">
  <div class="card stack stack--sm">
    <h2 class="card__title">Card title</h2>
    <p class="card__body">説明文</p>
    <a class="btn btn--primary" href="/blog/">記事を読む</a>
  </div>
</main>
```

---

## 含まれるもの

### ファイル構成

| ファイル | 内容 |
| --- | --- |
| `styles/index.css` | エントリーポイント。下の 3 つを `@import` でまとめる |
| `styles/tokens.css` | デザイントークン（CSS 変数） |
| `styles/components.css` | 全体の土台と部品のクラス |
| `styles/icons.css` | アイコンの見た目のルール |

### トークン（`tokens.css`）

| 種類 | 変数 |
| --- | --- |
| Color | `--color-bg` `--color-surface` `--color-border` `--color-text` `--color-muted` `--color-accent` `--color-accent-hover` `--color-on-accent` `--color-link` `--color-link-hover` |
| Space | `--space-1`（4px）〜 `--space-8`（64px） |
| Font | `--font-sans` `--font-mono` |
| Font size | `--font-size-sm` `--font-size-base` `--font-size-lg` `--font-size-xl` `--font-size-2xl` |
| Line height | `--line-height-tight` `--line-height-base` |
| Font weight | `--font-weight-normal` `--font-weight-bold` |
| Radius | `--radius-sm` `--radius-md` `--radius-full` |
| Layout | `--container-max` |
| Motion | `--transition-fast` |

Space の詳細：

| 変数 | 値 |
| --- | --- |
| `--space-1` | 0.25rem（4px） |
| `--space-2` | 0.5rem（8px） |
| `--space-3` | 0.75rem（12px） |
| `--space-4` | 1rem（16px） |
| `--space-5` | 1.5rem（24px） |
| `--space-6` | 2rem（32px） |
| `--space-7` | 3rem（48px） |
| `--space-8` | 4rem（64px） |

### クラス

| クラス | 用途 |
| --- | --- |
| `.container` | 最大幅を決めて中央寄せし、左右に余白を取る |
| `.stack` / `.stack--sm` / `.stack--lg` | 子要素を縦に並べ、間隔をそろえる（16px / 8px / 32px） |
| `.btn` | ボタン（`<button>` と `<a>` のどちらにも使える） |
| `.btn--primary` | メインの操作 |
| `.btn--ghost` | 控えめな操作（背景なし） |
| `.card` | 背景より一段浮いた面 |
| `.card__title` / `.card__body` | カードの見出し・本文 |
| `.icon` | アイコンの基本（周りの文字の 1.25 倍） |
| `.icon--sm` / `.icon--lg` | アイコンのサイズ（1 倍 / 1.5 倍） |
| `.icon--muted` / `.icon--accent` | アイコンの色 |
| `.icon-label` | アイコンと文字を横に並べる |

ボタンを押せない状態にするには、`<button disabled>` または `<a aria-disabled="true">` を使います。

### 全体へのスタイル

読み込むと、次のスタイルがページ全体に適用されます。

- すべての要素に `box-sizing: border-box`
- `body` の余白・背景色・文字色・フォント・行間
- リンク（`a`）の色と下線の位置（`.btn` などのクラスを付けたリンクは、クラス側の色が優先されます）

---

## アイコン

このパッケージには、アイコンの画像や JavaScript は**含まれていません**。`icons.css` で定義しているのは、サイズ・色・縦位置・間隔といった**見た目のルールだけ**です。

アイコンの表示には [Iconify](https://iconify.design/) の Web Component を使うことを想定しています。利用するサイトで、スクリプトを一度だけ読み込みます。

```html
<script src="https://code.iconify.design/iconify-icon/3.0.5/iconify-icon.min.js"></script>

<!-- UI アイコン（Material Symbols） -->
<iconify-icon class="icon" icon="material-symbols:home-outline"></iconify-icon>

<!-- サービスのロゴ（Simple Icons） -->
<a class="btn" href="https://github.com/taku-ts">
  <iconify-icon class="icon" icon="simple-icons:github" aria-hidden="true"></iconify-icon>
  GitHub
</a>
```

- Iconify Web Component は JavaScript で動作し、アイコンのデータを外部の API から取得します。
- 装飾目的のアイコンには `aria-hidden="true"` を付けます。アイコンだけのボタンには `aria-label` でボタンの名前を付けます。
- Simple Icons のロゴを使うときは、各社の商標・ブランドガイドラインに従ってください。

---

## 設計方針

- **CSS と HTML だけで作る。** ビルドツール、UI フレームワーク、Storybook は使いません。
- **値はトークンだけを使う。** 部品の色や余白には、`tokens.css` の変数だけを使います。値が限られるので、見た目が自然とそろいます。
- **余白は親がまとめて決める。** 子要素の一つひとつに margin を付けず、`.stack` の `gap` で間隔を決めます。
- **変数は「役割」で名前を付ける。** `--color-blue` ではなく `--color-accent` にします。値を変えても名前と中身が食い違いません。
- **命名は BEM 風にする。** `__` は部品の一部分（`.card__title`）、`--` は種類（`.btn--primary`）を表します。
- **全体へのスタイルは最小限にする。** リセット CSS は持たず、`box-sizing`・`body`・リンクの色だけを設定します。
- **配色はダークのみ。** ライトテーマは必要になったら検討します。
- **フォントは端末にあるものを使う。** Mac・iOS はヒラギノ、Windows はメイリオ、それ以外は標準のゴシック体です。Web フォントは読み込みません。

## カタログの確認

```bash
python3 -m http.server 8000
```

ブラウザで `http://localhost:8000` を開きます。カタログでは、アイコンの確認のために Iconify を読み込んでいます。

---

## バージョン管理

- バージョンは [セマンティック バージョニング](https://semver.org/lang/ja/)（`MAJOR.MINOR.PATCH`）に従い、Git のタグ（`v0.1.0` など）で管理します。
  - `PATCH`：見た目の微調整や不具合の修正
  - `MINOR`：クラスやトークンの追加（今までの使い方はそのまま使える）
  - `MAJOR`：クラスやトークンの削除・名前の変更（使う側の修正が必要）
- `0.x` の間は、`MINOR` の更新でも互換性のない変更が入ることがあります。
- 利用側を更新するときは、`package.json` のタグを書き換えて `npm install` し、サイトを再ビルドします。**このリポジトリに push するだけでは、利用側の CSS は更新されません。**

```bash
npm install github:taku-ts/design-system#v0.2.0
```

## ライセンス

[MIT](./LICENSE)
