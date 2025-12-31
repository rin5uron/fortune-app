# 今年の運勢 - Fortune App

公式LINE内で遊べる「今年の運勢」ガチャアプリケーションです。
占い要素はほぼなく、軽くてどうでもいい内容を楽しむためのエンターテイメント機能です。

## 概要

- **目的**: 公式LINE登録者向けの新年特典コンテンツ
- **形式**: LIFF（LINE Front-end Framework）アプリ
- **デザイン**: ミニマル、シンプル、落ち着いた色合い
- **運勢パターン**: 全10種類

## デモ

デプロイURL: https://fortune-app-jet.vercel.app/

## 技術スタック

- **フロントエンド**: HTML5, CSS3, Vanilla JavaScript
- **ホスティング**: Vercel
- **連携**: LINE LIFF SDK v2

## LIFF設定

### 設定情報

```
LIFF ID: 2008804421-qXBqLT62
エンドポイントURL: https://fortune-app-jet.vercel.app/
サイズ: Full
Scope: profile, openid, chat_message.write
```

### LINE Developers Consoleでの設定手順

1. [LINE Developers Console](https://developers.line.biz/console/) にアクセス
2. プロバイダーとチャネルを選択
3. 「LIFF」タブ → 「追加」をクリック
4. 以下を入力：
   - LIFFアプリ名: 今年の運勢
   - サイズ: Full
   - エンドポイントURL: `https://fortune-app-jet.vercel.app/`
   - Scope: `profile`, `openid`, `chat_message.write`
   - ボットリンク機能: オン
5. 保存して LIFF ID をコピー
6. `index.html` の `LIFF_ID` 変数に設定

## ディレクトリ構成

```
fortune-app/
├── images/
│   ├── main.png       # トップページ用メイン画像
│   ├── 01.png         # 運勢パターン①用画像
│   ├── 02.png         # 運勢パターン②用画像
│   ├── ...
│   └── 10.png         # 運勢パターン⑩用画像
├── index.html         # メインアプリケーション
├── spec.md            # 機能仕様書
├── .gitignore
└── README.md
```

## 機能説明

### 1. トップページ
- 「HAPPY NEW YEAR」メッセージ
- メイン画像（富士山と琴音ちゃん）
- 「今年の運勢を占う」ボタン

### 2. ガチャ演出
シンプルなテキスト表示アニメーション：
- 「ガチャ中…」
- 「運勢選択中…」
- 「そろそろ出ます…」

各テキストは約0.8秒間隔でフェードイン表示されます。

### 3. 運勢結果表示
- ランダムに選ばれた運勢カード
- 運勢専用画像（01.png〜10.png）
- 運勢テキスト
- LINE共有ボタン

### 4. LINE共有機能
- LIFF Share Target Picker を使用
- 運勢結果をテキストでLINE友だちに共有可能

## 運勢パターン一覧

1. アイス運（春夏秋冬アイスを楽しむ）
2. リンゴ運（ラッキー果物）
3. 睡眠運（早く寝られる日が増える）
4. コンビニ運（お気に入り商品に出会える）
5. 笑い運（どうでもいいことで笑う）
6. 寄り道運（思いがけない楽しい時間）
7. 食べる運（おいしいものに出会える）
8. 天気運（天気予報を見るとスムーズ）
9. 機嫌運（なんとなく機嫌よく過ごせる）
10. 選択運（選んだものが正解になる）

## ローカル開発

```bash
# リポジトリをクローン
git clone https://github.com/rin5uron/fortune-app.git
cd fortune-app

# ローカルサーバーで起動（例: Python）
python3 -m http.server 8000

# ブラウザで開く
open http://localhost:8000
```

## デプロイ

### Vercel へのデプロイ

1. Vercel アカウントでログイン
2. 「New Project」をクリック
3. GitHub リポジトリを連携
4. `fortune-app` を選択
5. デプロイ設定はデフォルトのまま「Deploy」

## トーン＆注意事項

- 占いとしての正確性は一切ありません
- 深い意味はありません
- 何回引いてもOK
- 信じなくてもOK
- クスッとできたら成功

## ライセンス

このプロジェクトは個人用途のため、ライセンスは設定していません。

## 作者

- GitHub: [@rin5uron](https://github.com/rin5uron)
