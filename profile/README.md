# お勉当 | OBENTO

### 学びを詰めて、みんなで分け合う。

**お勉当（OBENTO）** は、手書きノートのAI要約と学習内容の共有を組み合わせた、学習支援SNSアプリケーションです。

手書きで学んだ内容をデジタル化し、整理・保存するだけでなく、他の学習者と知識を共有できる環境を目指しています。

本プロジェクトは、6名の学生による卒業制作として開発しています。

---

## About

紙のノートには、自分の言葉や図を使って自由に学習内容を整理できる良さがあります。

一方で、過去のノートを探しにくい、外出先で確認しにくい、他の人と共有する際に手間がかかるといった課題もあります。

**お勉当は、手書き学習の良さを活かしながら、AIとSNSによって学習内容をより活用しやすくすることを目指しています。**

### Our Concept

- **詰める** — 手書きした学習内容をAIで整理して保存する
- **持ち運ぶ** — いつでもどこでも自分のノートを復習する
- **分け合う** — SNSを通じて他の人の学びを共有する
- **栄養にする** — お互いの知識を吸収し、切磋琢磨する

## Features

### 01. ノート撮影・画像編集

手書きノートを撮影、または端末から画像を選択してデジタル化します。

- カメラ撮影・画像選択
- 画像のトリミング・回転
- 画像補正・伏字処理
- 複数ページの画像管理

### 02. AIによるノート要約

撮影したノート画像をAIで解析し、学習内容をMarkdown形式の文章に整理します。

- AIによる学習内容の解析・要約
- 要約結果のプレビュー
- 自然言語によるAIへの修正指示
- 元画像と要約結果の比較
- 要約結果の採用・破棄

### 03. マイノート管理

作成したノートを保存し、必要なときに検索・閲覧できます。

- ノートの保存・一覧表示
- 科目・キーワードによる検索
- タイトルやタグなどの管理
- 元画像とAI要約の閲覧
- ノートの編集・削除

### 04. 学習SNS

自分の学習成果を公開し、他の学習者が作成したノートから知識を得ることができます。

- ホームフィード
- 科目別フィルタリング
- 投稿検索
- フォロー・フォロー中の投稿表示
- いいね・ブックマーク
- 公開範囲の設定（非公開・一般公開）

### 05. 安全な学習環境

安心して学習内容を共有できるよう、プライバシー保護や不適切な投稿への対応を考慮しています。

- アカウント認証
- ノートの公開範囲管理
- 投稿・アカウントの通報
- 利用者のブロック
- 運営管理機能

※ 上記は設計上の機能を含み、すべての実装完了を示すものではありません。

## Tech Stack

基本設計書第6版に基づく設計上の技術構成です。

| Category | Technologies |
|---|---|
| Frontend | Flutter / Dart |
| State Management | Riverpod |
| Routing | go_router |
| HTTP Client | Dio |
| Backend | Python / FastAPI |
| ORM | SQLAlchemy |
| Database | MySQL 8.4 LTS |
| Authentication | Amazon Cognito |
| AI | Amazon Bedrock |
| Image Processing | Pillow |
| Storage | Amazon S3 |
| Infrastructure | AWS EC2 / Caddy |
| Container | Docker / Docker Compose |
| Version Control | Git / GitHub |
| Project Management | GitHub Projects / GitHub Issues |

※ 技術構成の一部には最終確認中の事項があり、今後変更する可能性があります。

## System Architecture

アプリケーションは、FlutterによるフロントエンドとFastAPIによるバックエンドを中心に構成します。

**Frontend**

- Flutter Web
- Flutter Android

**Backend**

- FastAPIによるREST API
- ノート・ユーザー・SNS機能の管理
- AI要約処理と画像処理
- 認証トークンの検証

**Infrastructure & Services**

- Amazon Cognito：ユーザー認証
- Amazon Bedrock：AI要約
- Amazon S3：画像保存
- MySQL：アプリケーションデータ
- AWS EC2：API・Webサーバーの実行
- Caddy：HTTPS・Webコンテンツ配信

開発環境ではDocker Composeを活用し、チーム内での環境統一を図ります。

## Project Goals

本プロジェクトでは、以下の3つを主要な目標としています。

**1. 学習内容のデジタル化を支援する**

手書きノートをAIで整理することで、学習記録を保存・活用しやすくします。

**2. 復習の利便性を向上させる**

ノートの分類や検索により、必要な学習内容へ簡単にアクセスできるようにします。

**3. 学習者同士の知識共有を促進する**

SNSを通じて他の学習者の知識や視点に触れ、相互に学び合える環境を目指します。

## Development Team

本プロジェクトは、6名の学生によるチーム開発です。

| Item | Description |
|---|---|
| Project | お勉当（OBENTO） |
| Team | びぎゃらーず |
| Members | 6名 |
| Development Period | 2026–2027 |
| Project Type | 卒業制作 |
| Development Method | GitHubを用いたチーム開発 |

要件定義・基本設計・実装・テストまで、チームで協力して開発を進めています。

## Development Status

**Status: Under Development**

現在、設計資料に基づき開発を進めています。

各機能の進捗や技術的な決定事項は、GitHub IssuesおよびGitHub Projectsで管理します。

---

### 学びを詰めて、みんなで分け合う。

*OBENTO Development Team — 2026–2027*
