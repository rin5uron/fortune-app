# Learning Notes

This file contains what I learned through the fortune-app project, organized by date.

---

## 📚 目次

- [2026/01/01 - フォント最適化と画像圧縮でモバイルUX改善](#20260101---フォント最適化と画像圧縮)
- [2025/12/31 - LIFF URLとVercel URL直接アクセスの違い、権限エラー解決](#20251231---liff-urlとvercel-urlの違い)

---

## 2026/01/01 - フォント最適化と画像圧縮

### 概要

ユーザーフィードバックを基に、スマホでの読みやすさとページ読み込み速度を改善しました。

---

### ユーザーからの声

**問題点**:
1. 「文字が小さくて見えない」
2. 「友達登録めんどくさい」
3. 「何これ」「へえ」← あまり響かなかった様子

**解釈**:
- スマホでの視認性が悪い
- アプリの起動が遅い（画像が重い）
- コンテンツの魅力が伝わりきっていない

---

### 実施した改善

#### 1. フォントサイズの拡大

**変更内容:**

| デバイス | 変更前 | 変更後 | 拡大率 |
|---------|--------|--------|--------|
| **デスクトップ** | 1.1em | 1.3em | +18% |
| **スマホ (600px以下)** | 0.95em | 1.2em | +26% |
| **小画面 (400px以下)** | 0.9em | 1.1em | +22% |

**コード変更:**
```css
/* デスクトップ */
.fortune-text {
    font-size: 1.3em;  /* 1.1em → 1.3em */
    line-height: 2;
}

/* スマホ (600px以下) */
@media (max-width: 600px) {
    .fortune-text {
        font-size: 1.2em;  /* 0.95em → 1.2em */
        line-height: 1.9;
    }
}

/* 小画面 (400px以下) */
@media (max-width: 400px) {
    .fortune-text {
        font-size: 1.1em;  /* 0.9em → 1.1em */
    }
}
```

**結果:**
- ✅ スマホで運勢テキストがはっきり読める
- ✅ 高齢者や視力の弱い人にも配慮

---

#### 2. 画像の圧縮・最適化

**問題:**
- 各画像が 2.4〜2.7MB と超重量級
- 合計 27MB でページ読み込みが遅い
- モバイル回線では起動に20秒以上かかる

**解決方法:**
macOSの標準ツール `sips` を使用してPNG → JPEG変換

```bash
# 圧縮コマンド
sips -s format jpeg -s formatOptions 70 "01.png" --out "01.jpg"
```

**圧縮結果:**

| 項目 | 変更前 | 変更後 | 削減率 |
|------|--------|--------|--------|
| **形式** | PNG | JPEG (品質70) | - |
| **1枚のサイズ** | 2.4〜2.7MB | 364〜453KB | **約83%削減** |
| **合計サイズ** | 約27MB | 約4.5MB | **約83%削減** |

**パフォーマンス改善（推定）:**

| 回線速度 | 変更前 | 変更後 | 改善率 |
|---------|--------|--------|--------|
| **4G (10Mbps)** | 約22秒 | 約3.6秒 | **約6倍速** |
| **WiFi (50Mbps)** | 約4.3秒 | 約0.7秒 | **約6倍速** |

**コード変更:**
```javascript
// 画像パスを .png → .jpg に変更
const fortunes = [
  {
    title: 'たくさんのアイスに出会えるでしょう',
    image: 'images/01.jpg',  // .png → .jpg
    text: '...'
  },
  // ...
];
```

---

### 技術的な学び

#### sipsコマンドの使い方

**基本構文:**
```bash
sips -s format jpeg -s formatOptions [品質] [入力] --out [出力]
```

**品質の目安:**
- `100`: 最高品質（ファイルサイズ大）
- `70`: 良好な品質とサイズのバランス ← 今回採用
- `50`: やや画質劣化、サイズ最小

**一括変換例:**
```bash
for f in 01.png 02.png 03.png; do
  sips -s format jpeg -s formatOptions 70 "$f" --out "${f%.png}.jpg"
done
```

---

#### 画像最適化のベストプラクティス

1. **Web用画像は JPEG が基本**
   - 写真 → JPEG（今回のケース）
   - イラスト、ロゴ → PNG または SVG
   - アニメーション → WebP または GIF

2. **品質設定の目安**
   - 品質70〜80: Webで十分な品質
   - ユーザーは圧縮をほぼ気づかない
   - ファイルサイズは元の10〜20%に削減可能

3. **モバイルファーストの重要性**
   - モバイルユーザーが多い場合、ページ読み込み速度は最優先
   - 1秒遅延 = コンバージョン7%減少（Google調査）

---

### その他の改善

#### alt属性の修正

**変更理由:**
- 画像内の特定キャラクター名を削除
- 汎用的な説明に変更

**変更内容:**
```html
<!-- 変更前 -->
<img src="images/main.jpg" alt="琴音ちゃんと富士山" class="main-image">
<img src="${randomFortune.image}" alt="琴音ちゃん" class="fortune-image">

<!-- 変更後 -->
<img src="images/main.jpg" alt="富士山" class="main-image">
<img src="${randomFortune.image}" alt="今年の運勢" class="fortune-image">
```

---

### 今後の改善案

#### コンテンツの魅力向上

**ユーザーの反応が薄かった原因（推測）:**
1. 運勢の内容が軽すぎる？
2. ビジュアルのインパクト不足？
3. シェアしたくなる要素がない？

**検討中の改善策:**
- 運勢結果に「ラッキーカラー」「ラッキーアイテム」を追加
- 結果画面をスクショしやすいデザインに
- SNS共有機能の追加（Twitter, Instagram）
- アニメーション演出の強化

#### 友達登録のハードルを下げる

**問題:**
- LIFF URLを経由する必要がある
- 登録手順がわかりにくい

**検討中の改善策:**
- QRコードを用意
- リッチメニューに配置
- 初回起動時のチュートリアル

---

### まとめ（2026/01/01）

#### 改善内容
1. **フォントサイズ拡大** → スマホで読みやすく
2. **画像圧縮（83%削減）** → 起動速度6倍向上
3. **alt属性修正** → 汎用的な説明に

#### 成果
- ✅ モバイルでの視認性向上
- ✅ ページ読み込み速度が約6倍に
- ✅ データ通信量を83%削減（27MB → 4.5MB）

#### 次のステップ
- コンテンツの魅力向上（運勢内容の見直し）
- SNS共有機能の追加
- 友達登録フローの改善

---

## 2025/12/31 - LIFF URLとVercel URLの違い

### 概要

LIFF URLを使わずにVercel URLを直接開くと、LIFFが正しく認識されず「メッセージ送信の権限が許可されていません」エラーが発生する問題を解決しました。

---

### 発生した問題

**症状:**
- LINEアプリ内でVercel URL（`https://fortune-app-jet.vercel.app/`）を直接開く
- 「この機能はLINEアプリ内でのみ利用できます」エラーが表示される
- `isInClient: false` と表示される（LINEアプリ内なのに）

---

### 原因

**Vercel URLとLIFF URLの違い:**

| 開き方 | URL形式 | 結果 |
|--------|---------|------|
| ❌ Vercel URL直接 | `https://fortune-app-jet.vercel.app/` | LIFFとして認識されない |
| ✅ LIFF URL | `https://liff.line.me/2008804421-qXBqLT62` | LIFFとして正しく認識される |

**なぜVercel URL直接だとダメなのか:**
1. LINEアプリ内ブラウザで開いても、LIFF URLを経由しないとLIFFの認証フローが正しく動作しない
2. `liff.sendMessages()` は LIFF として認証された状態でないと権限エラーになる

---

### 解決方法

**LIFF URLで開く:**

```
❌ 間違い: https://fortune-app-jet.vercel.app/
✅ 正解:  https://liff.line.me/2008804421-qXBqLT62
```

**LIFF URLの構造:**
```
https://liff.line.me/[あなたのLIFF_ID]
```

**このプロジェクトのLIFF URL:**
```
https://liff.line.me/2008804421-qXBqLT62
```

---

### 学んだこと

1. **LIFF URL経由で開く**: 直接Vercel URLを開かず、必ず `https://liff.line.me/LIFF_ID` 形式で開く
2. **エンドポイントURL設定は変更不要**: LINE Developers ConsoleのエンドポイントURLは `https://fortune-app-jet.vercel.app/` のままでOK
3. **デバッグ情報を出力する**: `isInClient`、`isLoggedIn` などの値をログに出力しておくと問題特定が早い

**参考:**
- 同様の問題が love-counter プロジェクトでも発生（2025/12/25）
- 詳細は `/Users/rin5uron/Desktop/personal/counterapp-collection/love-counter/docs/learning-notes.md` を参照

---
