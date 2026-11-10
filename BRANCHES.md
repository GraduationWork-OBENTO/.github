# ブランチ台帳

お勉当で使う作業用ブランチの一覧です。ブランチは**この台帳にある作業IDと名前だけ**を使います。
同じ作業を別の名前で始めたり、似た名前のブランチが並んだりするのを防ぎ、機能 → ブランチ → Pull Request をたどれるようにするためです。

- 置き場所：`docs/development/branches.md`
- 機能ID・画面IDは「基本設計書 第1章 表1-1」に合わせる
- グループ機能（F-GRP-01〜09）は廃止、コメント（F-SNS-09）は却下のため、この台帳には載せない
- 第6版で作らなくなった作業は、行を残して優先度を「対象外（第6版）」にする（理由は「3. 台帳に載せない機能・対象外にした作業」）

## 1. 使い方

### ブランチ名
`種類/作業ID-名前` の形にする。`feature/*` の名前は、下の表の「ブランチ名」をそのままコピーして使う。

| 種類 | 名前の付け方 | 例 |
|---|---|---|
| `feature/*` | 台帳の「ブランチ名」をそのまま使う | `feature/note-02-mynote-list` |
| `fix/*` | バグがある作業の作業ID ＋ 内容 | `fix/note-02-sort-order` |
| `hotfix/*` | バグがある作業の作業ID ＋ 内容 | `hotfix/img-03-size-check` |
| `clean/*` | 整理する作業の作業ID ＋ 内容（全体にかかるときは `base`） | `clean/rct-01-like-repository`、`clean/base-remove-groups` |
| `docs/*` | 下の「4. ドキュメントのブランチ」の名前を使う | `docs/db-design` |

### ルール
1. **1つの作業ID = 1人の担当 = 1つの feature ブランチ**。担当が決まったら「担当」欄を埋める
2. 作業IDは一度決めたら変えない・使い回さない。やめた作業は行を消さず「対象外」にする
3. 新しい作業が必要になったら、**PMが作業IDを採番**し、`docs/branches` ブランチで台帳に行を追加してからブランチを作る
4. 作業中のブランチと同じ「画面」を触る必要が出たら、先にその担当と相談する
5. 1つの作業IDごとに GitHub の Issue を1つ作り、進み具合は Issue（または GitHub Projects）で管理する。台帳には状態を書かない（台帳は「何をどこで作るか」を固定するためのもの）
6. Pull Request のタイトルは `[作業ID] 作業名` にする（例：`[note-02] マイノート一覧`）。Purpose に機能IDを書く
7. ブランチを作る前に `git fetch` して、同じ名前のブランチがないか確認する

### 作業IDの付け方
- 頭の文字は機能IDの分類に合わせる（`F-NOTE-03` → `note-xx`）。機能IDのない基盤の作業は `base`、DBは `db`
- 英小文字・数字・ハイフンだけを使う（大文字にしない。WindowsとMacでは大文字・小文字を区別しないため）
- 番号は分類ごとに01から振る

### たどり方（トレーサビリティ）
```text
機能ID（F-NOTE-03）
  → 作業ID（note-02）           … この台帳
  → ブランチ（feature/note-02-mynote-list）
  → Issue・Pull Request（[note-02] マイノート一覧）
  → コミット（add(notes): マイノート一覧を追加）
```

優先度は基本設計書 第6版の値。「前提」は先にマージしておく作業。

### 基盤・DB

| 作業ID | ブランチ名 | 作業名 | 機能ID | 画面 | 主なフォルダ | 前提 | 優先度 | 担当 |
|---|---|---|---|---|---|---|---|---|
| base-01 | `feature/base-01-repo-setup` | リポジトリの初期構成 | — | — | ルート（`compose.yaml`、`.env.example`、`README.md`） | — | Must | |
| base-02 | `feature/base-02-flutter-app` | Flutterの土台（起動・画面遷移・テーマ・通信） | — | — | `frontend/lib/app`、`frontend/lib/core` | base-01 | Must | |
| base-03 | `feature/base-03-fastapi-app` | FastAPIの土台（設定・DB接続・共通エラー） | — | — | `backend/app/core`、`backend/app/main.py` | base-01 | Must | |
| db-01 | `feature/db-01-initial-schema` | 最初のテーブル作成（Alembic） | — | — | `backend/alembic` | base-03、DB設計の確定 | Must | |
| base-04 | `feature/base-04-aws-deploy` | AWS環境・Web配信 | — | — | `infra/`、`compose.prod.yaml` | base-02、base-03 | Must | |

### 共通（CMN）

| 作業ID | ブランチ名 | 作業名 | 機能ID | 画面 | 主なフォルダ | 前提 | 優先度 | 担当 |
|---|---|---|---|---|---|---|---|---|
| cmn-01 | `feature/cmn-01-image-storage` | 画像の保存先（ローカル・S3） | F-CMN-01 | — | `backend/app/integrations/storage` | base-03 | Must | |
| cmn-02 | `feature/cmn-02-ai-usage-limit` | AI利用回数の制限 | F-CMN-02 | — | `backend/app/modules/ai` | ai-01 | Must | |
| cmn-03 | `feature/cmn-03-notification` | お知らせ・通知 | F-CMN-03 | ヘッダー、SC-07、SC-12 | 未定 | — | 対象外（第6版） | |
| cmn-04 | `feature/cmn-04-view-log` | 閲覧記録 | F-CMN-04 | — | 未定 | — | 対象外（第6版） | |
| cmn-05 | `feature/cmn-05-idempotency` | 二重送信の防止（Idempotency-Key） | F-CMN-05 | SC-05〜08、SC-13 | `backend/app/core`、`frontend/lib/core` | base-02、base-03 | Must | |
| cmn-06 | `feature/cmn-06-cleanup-jobs` | 後片付けの定期処理（24時間たった作成中ノートの削除・画像削除の再試行・止まったAI処理の復旧） | F-CMN-06 | — | `backend/app/jobs` | cmn-01、ai-01 | Must | |

### 認証・アカウント（AUTH・ACC）

| 作業ID | ブランチ名 | 作業名 | 機能ID | 画面 | 主なフォルダ | 前提 | 優先度 | 担当 |
|---|---|---|---|---|---|---|---|---|
| auth-01 | `feature/auth-01-signup-login` | 会員登録・ログイン・ログアウト・認証の共通判定 | F-AUTH-01, 02, 03, 06 | SC-01、SC-12 | `features/auth`、`modules/auth` | db-01、認証方式の決定 | Must | |
| auth-02 | `feature/auth-02-password-reset` | パスワード再設定 | F-AUTH-04 | SC-01 | `features/auth`、`modules/auth` | auth-01 | Must | |
| auth-03 | `feature/auth-03-external-login` | 外部サービス認証 | F-AUTH-05 | SC-01 | `features/auth`、`modules/auth` | auth-01 | 対象外（第6版） | |
| acc-01 | `feature/acc-01-profile` | プロフィール表示・編集 | F-ACC-01, 02 | SC-10、SC-12 | `features/users`、`modules/users` | auth-01 | Must | |
| acc-02 | `feature/acc-02-account-settings` | アカウント情報変更・退会 | F-ACC-03, 04 | SC-12 | `features/users`、`modules/users` | auth-01 | Should | |
| acc-03 | `feature/acc-03-user-settings` | 利用者設定 | F-ACC-05 | SC-12 | `features/users`、`modules/users` | acc-01 | 対象外（第6版） | |

### 撮影・画像編集（IMG）

| 作業ID | ブランチ名 | 作業名 | 機能ID | 画面 | 主なフォルダ | 前提 | 優先度 | 担当 |
|---|---|---|---|---|---|---|---|---|
| img-01 | `feature/img-01-capture-select` | 撮影・画像選択・プレビュー | F-IMG-01, 02, 03 | SC-05 | `features/camera` | base-02 | Must | |
| img-02 | `feature/img-02-crop-mask` | トリミング・回転・伏字 | F-IMG-05, 07 | SC-06 | `features/camera` | img-01 | Must | |
| img-03 | `feature/img-03-upload` | 画像のアップロード・保存 | F-IMG-08 | SC-06 | `features/camera`、`modules/uploads` | img-02、cmn-01、note-08 | Must | |
| img-04 | `feature/img-04-image-adjust` | 画像補正 | F-IMG-06 | SC-06 | `features/camera` | img-02 | Should | |
| img-05 | `feature/img-05-multi-page` | 複数ページの撮影（最大10ページ） | F-IMG-04 | SC-05 | `features/camera` | img-01 | Must | |
| img-06 | `feature/img-06-mask-detect` | 伏字候補の自動検出 | F-IMG-09 | SC-06 | `features/camera` | img-02 | Could（保留） | |

### AI要約（AI）

| 作業ID | ブランチ名 | 作業名 | 機能ID | 画面 | 主なフォルダ | 前提 | 優先度 | 担当 |
|---|---|---|---|---|---|---|---|---|
| ai-01 | `feature/ai-01-reconstruct` | AI要約の依頼・処理（Bedrock・仮実装） | F-AI-01, 02 | SC-06、SC-07 | `modules/ai`、`integrations/ai` | img-03 | Must | |
| ai-02 | `feature/ai-02-status-failure` | 処理状況の表示・失敗時の対応 | F-AI-03, 04 | SC-07（処理中・失敗） | `features/ai` | ai-01 | Must | |
| ai-03 | `feature/ai-03-review-edit` | 自然言語による修正 | F-AI-06 | SC-07（確認） | `features/ai`、`modules/ai` | ai-01 | Must | |
| ai-04 | `feature/ai-04-compare-label` | 元画像との比較・AI生成の表示 | F-AI-07, 08 | SC-02、SC-04、SC-07 | `features/ai` | ai-03 | Must | |
| ai-05 | `feature/ai-05-summary` | AI要約の生成 | F-AI-10 | 未定 | `modules/ai` | ai-01、要約の保存先の決定 | 対象外（第6版） | |
| ai-06 | `feature/ai-06-rerun` | AI要約の再生成（1ノート最大5回） | F-AI-09 | SC-07 | `features/ai`、`modules/ai` | ai-02 | Should | |
| ai-07 | `feature/ai-07-figure-redraw` | 図のAI描き直し | F-AI-11 | SC-07 | `modules/ai` | ai-03 | 対象外（第6版） | |
| ai-08 | `feature/ai-08-adopt-discard` | AI要約候補の採用・破棄 | F-AI-12 | SC-07（確認） | `features/ai`、`modules/ai` | ai-01 | Must | |

### ノート管理・公開範囲（NOTE・PRV）

| 作業ID | ブランチ名 | 作業名 | 機能ID | 画面 | 主なフォルダ | 前提 | 優先度 | 担当 |
|---|---|---|---|---|---|---|---|---|
| prv-01 | `feature/prv-01-access-check` | 閲覧権限の判定・画像の安全な配信 | F-PRV-04, 05 | 全閲覧画面 | `backend/app/core`、`modules/notes` | db-01、cmn-01 | Must | |
| note-01 | `feature/note-01-note-save` | ノート保存（科目・公開範囲・投稿情報） | F-NOTE-01, 08, F-PRV-01, F-SNS-01 | SC-08 | `features/notes`、`modules/notes` | img-03 | Must | |
| note-02 | `feature/note-02-mynote-list` | マイノート一覧 | F-NOTE-03 | SC-09 | `features/notes`、`modules/notes` | note-01 | Must | |
| note-03 | `feature/note-03-note-detail` | ノート詳細表示 | F-NOTE-05 | SC-04 | `features/notes`、`modules/notes` | prv-01、note-01 | Must | |
| note-04 | `feature/note-04-note-delete` | ノート削除 | F-NOTE-07 | SC-04、SC-09 | `features/notes`、`modules/notes` | note-03 | Must | |
| prv-02 | `feature/prv-02-visibility-change` | 公開範囲の変更 | F-PRV-03 | SC-04、SC-09 | `features/notes`、`modules/notes` | note-03 | Must | |
| note-05 | `feature/note-05-draft` | 下書き保存・再開 | F-NOTE-02 | SC-07、SC-09 | `features/notes`、`modules/notes` | note-02 | 対象外（第6版） | |
| note-06 | `feature/note-06-mynote-search` | マイノート検索 | F-NOTE-04 | SC-09 | `features/notes`、`modules/notes` | note-02 | Should | |
| note-07 | `feature/note-07-note-edit` | ノート編集（タイトル・タグ・ハッシュタグ・代表画像だけ） | F-NOTE-06 | SC-04 | `features/notes`、`modules/notes` | note-03 | Should | |
| prv-03 | `feature/prv-03-public-check` | 公開前チェック | F-PRV-02 | SC-08 | `features/notes` | note-01 | Should | |
| note-08 | `feature/note-08-creating-note` | 作成中ノートの開始 | F-NOTE-09 | SC-05 | `features/notes`、`modules/notes` | db-01 | Must | |

### SNS・いいね・ブックマーク（SNS・RCT）

| 作業ID | ブランチ名 | 作業名 | 機能ID | 画面 | 主なフォルダ | 前提 | 優先度 | 担当 |
|---|---|---|---|---|---|---|---|---|
| sns-01 | `feature/sns-01-home-feed` | ホームフィード・科目での絞り込み | F-SNS-02, 03 | SC-02 | `features/social`、`modules/social` | prv-01 | Must | |
| sns-02 | `feature/sns-02-post-search` | 投稿検索 | F-SNS-04 | SC-03 | `features/social`、`modules/social` | sns-01 | Must | |
| sns-03 | `feature/sns-03-follow` | フォロー・フォロー解除 | F-SNS-08 | SC-02、SC-10 | `features/social`、`modules/social` | sns-01、acc-01 | Must | |
| rct-01 | `feature/rct-01-like` | いいね登録・取消・いいね数 | F-RCT-01, 02 | SC-02、SC-04 | `features/social`、`modules/social` | note-03 | Must | |
| rct-02 | `feature/rct-02-bookmark` | ブックマーク登録・一覧・閲覧不可の扱い | F-RCT-03, 04, 05 | SC-02、SC-04、SC-09 | `features/social`、`modules/social` | note-03 | Must | |
| sns-04 | `feature/sns-04-post-edit` | 投稿の編集・取り下げ | F-SNS-06 | SC-04、SC-10 | `features/social`、`modules/social` | note-01 | Should | |
| sns-05 | `feature/sns-05-user-posts` | 利用者ごとの投稿一覧 | F-SNS-07 | SC-10 | `features/social`、`modules/social` | acc-01 | Should | |
| sns-06 | `feature/sns-06-others-note` | 他者ノートの閲覧 | F-SNS-05 | SC-04 | `features/notes` | note-03 | 対象外（第6版） | |

### 通報・ブロック・運営管理（MOD・ADM）

| 作業ID | ブランチ名 | 作業名 | 機能ID | 画面 | 主なフォルダ | 前提 | 優先度 | 担当 |
|---|---|---|---|---|---|---|---|---|
| mod-01 | `feature/mod-01-report-note` | 投稿の通報 | F-MOD-01 | SC-13 | `features/moderation`、`modules/moderation` | note-03 | Must | |
| mod-02 | `feature/mod-02-report-account` | アカウントの通報 | F-MOD-02 | SC-13 | `features/moderation`、`modules/moderation` | mod-01、acc-01 | Must | |
| mod-03 | `feature/mod-03-block` | ブロック・ブロック一覧・表示制御 | F-MOD-03, 04, 05 | SC-04、SC-12、SC-13 | `features/moderation`、`modules/moderation` | prv-01 | Should | |
| adm-01 | `feature/adm-01-admin-access` | 管理者の権限制御 | F-ADM-01 | SC-14 | `features/admin`、`modules/moderation` | auth-01 | Must | |
| adm-02 | `feature/adm-02-report-handling` | 通報の確認・対応・対応記録 | F-ADM-02, 03, 04 | SC-14 | `features/admin`、`modules/moderation` | adm-01、mod-01 | Must | |
| adm-03 | `feature/adm-03-post-user-manage` | 利用者管理 | F-ADM-06 | SC-14 | `features/admin`、`modules/moderation` | adm-02 | Should | |
| adm-04 | `feature/adm-04-contact-poster` | 投稿者への連絡 | F-ADM-07 | SC-14 | `features/admin` | adm-02 | 対象外（第6版） | |
| adm-05 | `feature/adm-05-post-manage` | 投稿管理 | F-ADM-05 | SC-14 | `features/admin`、`modules/moderation` | adm-02 | Could（保留。API設計書 v3.0 にAPIなし） | |

### 追加機能候補（EXT）

| 作業ID | ブランチ名 | 作業名 | 機能ID | 画面 | 主なフォルダ | 前提 | 優先度 | 担当 |
|---|---|---|---|---|---|---|---|---|
| ext-01 | `feature/ext-01-material-ai` | 教材の撮影・AI解説 | F-EXT-01, 02 | SC-15 | 未定 | ai-01 | 対象外（第6版） | |
| ext-02 | `feature/ext-02-learning-record` | 学習の記録 | F-EXT-03 | SC-16 | 未定 | cmn-04 | 対象外（第6版） | |

## 3. 台帳に載せない機能・対象外にした作業

### 台帳に載せない機能

| 機能ID | 理由 |
|---|---|
| F-GRP-01〜09 | グループ機能の廃止 |
| F-SNS-09 | コメントは却下 |
| F-AI-05 | 要確認箇所の提示は第6版で不採用（K-02）。ai-03 から外した |

### 対象外にした作業（第6版）

| 作業ID | 理由 |
|---|---|
| cmn-03 | お知らせ・通知は第6版で対象外 |
| cmn-04 | 閲覧記録は第6版で対象外 |
| auth-03 | 外部サービス認証は第6版の機能一覧にない |
| acc-03 | 利用者設定（初期公開範囲・通知）は第6版でなくした |
| ai-05 | AI要約の生成（F-AI-10）は ai-01 に統合 |
| ai-07 | 図のAI描き直しは第6版の機能一覧にない |
| note-05 | 下書きは第6版で対象外（作成中ノートは24時間で自動削除） |
| sns-06 | 他者ノートの閲覧はノート詳細（F-NOTE-05、note-03）に統合 |
| adm-04 | 投稿者への連絡は第6版の機能一覧にない |
| ext-01、ext-02 | SC-15・16 が第6版で対象外 |

ブランチ名は変えない（ルール2）。ai-01 の `reconstruct` のように古い言葉が残っているものも、名前はそのまま使う。


## 4. ドキュメントのブランチ

ドキュメントだけを変えるときは、次の名前を使う（同じ資料を同時に2人で直さない）。

| ブランチ名 | 対象 |
|---|---|
| `docs/readme` | `README.md` |
| `docs/contributing` | `CONTRIBUTING.md` |
| `docs/branches` | この台帳 |
| `docs/api` | `docs/api/` |
| `docs/db-design` | `docs/database/` |
| `docs/development` | `docs/development/`（この台帳を除く） |

## 5. 変更履歴

| 日付 | 内容 |
|---|---|
| 2026-11-06 | 初版（基本設計書 第4版の機能一覧から作成。グループ廃止を反映） |
| 2026-11-10 | 基本設計書 第6版に合わせて更新。第6版で作らない作業（cmn-03・04、auth-03、acc-03、ai-05・07、note-05、sns-06、adm-04、ext-01・02）を対象外にした。cmn-05・06、ai-08、note-08、adm-05 を追加。img-05 を Must、adm-03 を利用者管理（Should）にし、ai-01・03・04・06、note-01・07、img-03 の内容を直した |
