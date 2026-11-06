# Contributing
 
このファイルは、卒業制作「お勉当」のリポジトリで共通して利用する開発ルールをまとめたものです。
フロントエンド（Flutter）・バックエンド（FastAPI）・インフラを1つのリポジトリで管理するため、どこを変えたかが分かるように履歴を残します。
 
## 1. 基本方針
- 小さく変更する
- 1つの変更に1つの目的を持たせる
- 無関係な変更を混ぜない
- レビューしやすい形で履歴を残す
- 困ったら厳密さよりも分かりやすさを優先する
- 機能を追加するときは、対応する `frontend/lib/features/` と `backend/app/modules/` に作る
- 空の機能フォルダを先に作らず、実装するときに追加する
## 2. ブランチ運用
このプロジェクトでは、以下のブランチを使用します。
 
| ブランチ名 | 役割 | 派生元 | マージ先 |
|---|---|---|---|
| `main` | 安定版・完成版を置くブランチ（展示会・AWS環境で動かす版） | - | - |
| `develop` | 開発中の内容をまとめるブランチ | `main` | `main` |
| `feature/*` | 機能ごとの開発を行うブランチ | `develop` | `develop` |
 
### 運用ルール
- `main` には直接 push しない
- 作業は基本的に `feature/*` ブランチで行う
- 作業完了後は Pull Request で `develop` にマージする
- ある程度まとまったら `develop` を `main` にマージする
- 1ブランチ1目的を原則とする
- フロントとバックの両方を変える機能は、同じブランチで作ってよい（API の入出力を先に合意しておく）
### feature ブランチ名の例
- `feature/login-form`
- `feature/notes-list`
- `feature/camera-capture`
- `feature/ai-review-edit`
- `feature/note-like`
- `feature/report-account`
- `feature/admin-reports`
## 3. コミットルール
コミットメッセージは以下の形式を基本とします。
 
`type: 内容`
 
または
 
`type(scope): 内容`
 
### 使用する type
- `add`：新機能追加
- `fix`：バグ修正
- `update`：既存機能の改善
- `change`：仕様変更
- `clean`：整理・リファクタリング
- `remove`：削除
- `docs`：ドキュメント更新
### scope について
- scope は変更対象を分かりやすくするために付与する
- scope は必須ではない
- 付けられる場合は付ける
- 使用する scope は今後必要に応じて追加してよい
- 新しい scope を使う場合は、意味が分かる短い名前にする
- 表記ゆれを避けるため、既存の scope があればそれを優先する
- 機能の scope は、`features/` と `modules/` のフォルダ名に合わせる
### 使用する scope の例
 
#### 機能（`features/` ・ `modules/` と同じ名前）
- `auth`：会員登録・ログイン・ログアウト・パスワード再設定
- `users`：プロフィール・アカウント情報・退会
- `notes`：ノートの保存・一覧・詳細・編集・削除
- `camera`：撮影・画像選択・トリミング・伏字
- `uploads`：画像のアップロード・保存
- `ai`：AI再構成・要確認箇所・修正・要約
- `social`：フィード・検索・いいね・ブックマーク・フォロー
- `moderation`：通報・ブロック
- `admin`：運営管理（Web版のみ）
#### 共通（Flutter）
- `theme`：配色・文字・ボタンなど共通デザイン（`app/theme.dart`）
- `router`：画面遷移（`app/router.dart`）
- `core`：通信・共通エラー処理・共通部品（`core/`）
#### バックエンド
- `api`：API全体に関わる変更
- `db`：テーブル定義・マイグレーション（Alembic）
- `storage`：画像の保存先（ローカル・S3）
- `bedrock`：AI連携（Bedrock・仮実装）
#### インフラ
- `docker`：`compose.yaml` ・ `compose.prod.yaml` ・ Dockerfile
- `caddy`：Web配信・API転送の設定
- `aws`：AWSの構築・デプロイ手順
#### ドキュメント・設定
- `docs`：ドキュメント
- `config`：設定ファイル・環境変数の見本（`.env.example`）・依存関係
### コミット例（scopeなし）
- `add: ノート一覧の空状態を追加`
- `fix: 画像アップロード時の容量チェックを修正`
- `update: ホームのノートカードの見た目を調整`
- `change: 公開範囲を非公開・一般公開の2つに変更`
- `clean: NoteRepository の処理を整理`
- `remove: グループ機能のフォルダを削除`
- `docs: CONTRIBUTING.md を更新`
### コミット例（scopeあり）
- `add(notes): マイノート一覧を追加`
- `fix(camera): 回転後に伏字の位置がずれる問題を修正`
- `update(ai): 要確認箇所の表示を調整`
- `change(auth): ログイン画面の入力仕様を変更`
- `clean(social): いいねの処理を整理`
- `remove(db): 使わなくなったテーブルを削除`
- `add(db): follows テーブルのマイグレーションを追加`
- `docs(docs): DB設計を更新`
### コミット時のルール
- 1コミット1内容を意識する
- 無関係な変更を1つのコミットに含めない
- 後から見て内容が分かるメッセージにする
- 大きすぎる変更は分けてコミットする
- フロントとバックを両方変えた場合は、できるだけコミットを分ける
## 4. プッシュルール
- 区切りの良い単位で push する
- 明らかに壊れている状態では共有ブランチに push しない
- Pull Request 作成前に不要なファイルや差分が含まれていないか確認する
### push してはいけないもの
- `.env`（見本の `.env.example` だけ入れる）
- AWSの認証情報・本番のパスワード
- ローカル保存の画像フォルダ・ビルド成果物
- 実在する人の名前や個人情報が写ったノート画像（テスト用の画像はダミーを使う）
## 5. 共通ファイル・DB・APIの変更ルール
- 次の共通ファイルを変えるときは、Pull Request の Impact に書き、レビュー担当に知らせる
  - `frontend/lib/app/router.dart`、`frontend/pubspec.yaml`
  - `backend/app/main.py`、`backend/requirements.txt`
  - `compose.yaml`、`compose.prod.yaml`、`.env.example`
- API の入出力は、実装前にフロント・バックで合意する。正式な仕様は FastAPI が出す OpenAPI（`http://localhost:8000/docs`）とする
- テーブル構造の変更は、必ず Alembic のマイグレーション（`backend/alembic/versions/`）で行う
  - 1つの Pull Request に入れるマイグレーションは、なるべく1つにする
  - `develop` にマージ済みのマイグレーションは書き換えない（直すときは新しいマイグレーションを追加する）
  - マイグレーションを含む Pull Request をマージしたら、チームに知らせる（各自の MySQL で `alembic upgrade head` が必要になるため）
- 通常の API で認証を省略しない。認証の差し替えは `backend/tests/` の中だけで行う
- Bedrock の呼び出しには費用がかかるため、必要なときだけ呼び出す（呼び出し方の方針はチームで決めたものに従う）
## 6. Pull Request ルール
Pull Request では、以下の内容を記載する。
 
- Purpose：何のための変更か（関係する機能ID・画面IDを書く。例：F-NOTE-03、SC-09）
- Changes：何を変えたか
- Impact：影響する範囲（API の入出力、DB、共通ファイル、Web版・Android版のどちらか）
