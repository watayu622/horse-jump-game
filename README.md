# 🐴 初夢ジャンプ

新年をテーマにしたブラウザゲームです。馬を操作して「一富士二鷹三茄子」を飛び越えましょう！

## 🎮 遊び方

- **タップ / スペースキー** でジャンプ
- **連続タップで最大3段ジャンプ**（より高く飛べます）
- 🍆茄子、🗻富士山、🦅鷹を飛び越えてスコアを稼ごう

### 得点

| アイテム | 得点 |
|---------|------|
| 🍆 茄子 | +1点 |
| 🗻 富士山 | +3点 |
| 🦅 鷹 | +5点 |

### おすすめ

📱 スマホは**横向き**でプレイすると、先まで見えてプレイしやすい！

## ✨ 機能

- **チュートリアル** - 初めてでも安心の説明画面
- **BGM・効果音** - 音ありでより楽しくプレイ（オフも可能）
- **ハイスコア保存** - ブラウザに記録
- **オンラインランキング** - Supabase連携で世界中のプレイヤーと競争（オプション）
- **結果シェア** - LINEでスコアをシェア
- **結果画像保存** - スコア画像をダウンロード

## 🚀 セットアップ

### 基本（ローカルプレイ）

1. `index.html` をブラウザで開くだけ！

### GitHub Pages でホスティング

1. GitHubリポジトリを作成
2. `index.html` をアップロード
3. Settings → Pages → Source を `main` ブランチに設定
4. 数分後に `https://[ユーザー名].github.io/[リポジトリ名]/` でアクセス可能

### ランキング機能（オプション）

オンラインランキングを有効にするには、Supabaseの設定が必要です。

#### 1. Supabaseプロジェクト作成

1. [Supabase](https://supabase.com/) でアカウント作成
2. 「New Project」でプロジェクト作成
3. Project URL と anon key をメモ

#### 2. データベーステーブル作成

SQL Editorで以下を実行：

```sql
CREATE TABLE rankings (
    id BIGSERIAL PRIMARY KEY,
    nickname TEXT NOT NULL,
    score INTEGER NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- インデックス作成（スコア順の取得を高速化）
CREATE INDEX idx_rankings_score ON rankings(score DESC);

-- Row Level Security (RLS) を有効化
ALTER TABLE rankings ENABLE ROW LEVEL SECURITY;

-- 誰でも読み取り可能
CREATE POLICY "Anyone can read rankings"
ON rankings FOR SELECT
USING (true);

-- 誰でも挿入可能
CREATE POLICY "Anyone can insert rankings"
ON rankings FOR INSERT
WITH CHECK (true);
```

#### 3. index.htmlの設定

`index.html` 内の以下の部分を編集：

```javascript
const SUPABASE_URL = 'YOUR_SUPABASE_URL';  // 例: https://xxxxx.supabase.co
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';  // 例: eyJhbGciOiJIUzI1NiIs...
```

## 📁 ファイル構成

```
├── index.html    # ゲーム本体（HTML/CSS/JavaScript一体型）
├── README.md     # このファイル
└── ogp.png       # OGP画像（SNSシェア用、オプション）
```

## 🔧 カスタマイズ

### OGP画像の設定

SNSでシェアした時のプレビュー画像を設定するには：

1. 1200×630px の画像を用意
2. `ogp.png` としてindex.htmlと同じ場所に配置
3. index.html内のOGPメタタグを確認：

```html
<meta property="og:image" content="ogp.png">
```

### ゲームバランス調整

`index.html` 内の `setupDimensions()` 関数で調整可能：

```javascript
// ジャンプ高さ（画面の何%か）
const jumpHeight = groundY * 0.28;

// 滞空時間（フレーム数）
const hangTime = 40 * (refSize / 400);

// 移動速度
baseSpeed = 2.5;
```

## 🔒 プライバシー

### 収集するデータ

ランキング機能を使用する場合のみ：
- ニックネーム（ユーザーが入力）
- スコア
- 登録日時

**IPアドレスや個人情報は収集しません。**

### サーバーログ

Supabaseのサーバーログにはアクセス元IPが一時的に記録される場合があります（7日間で自動削除）。ログはプロジェクト管理者のみ閲覧可能です。

## 📜 ライセンス

MIT License

## 🙏 クレジット

- ゲームデザイン・開発: [あなたの名前]
- BGM: Web Audio APIによる自動生成
- ナスの画像: [いらすとや](https://www.irasutoya.com/)
- その他アイコン: 絵文字（システムフォント）

---

🎍 良いお年を！ 🎍
