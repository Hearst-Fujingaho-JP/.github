# Hearst-Fujingaho-JP GitHub Organization 運用ガイドライン

このドキュメントは、Hearst Fujingahoおよび、Hearst Digital Japan（以下 HDJ）が運用する GitHub Organization に参加する
すべてのメンバー（社員・業務委託・ベンダー）向けの共通ルールを定めたものです。

---

## 1. この Organization の目的

* HDJ が開発・運用するシステムおよび関連コードを管理する
* 社内外メンバーが安全かつ効率的に協業できる環境を提供する
* セキュリティ・監査・属人化防止を重視した運用を行う

---

## 2. 対象者

* HDJ 社員
* HDJ と契約関係にある業務委託・外部ベンダー
* 一時的に参加するプロジェクトメンバー

※ 本 Organization への参加は、Organization Owner（IT 部）の承認が必要です。

---

## 3. GitHub アカウントに関するルール

### 3.1 アカウント種別

* HDJ 社員は **原則として当社用のアカウントを作成し利用** してください
* 個人の既存アカウントを業務に利用することは禁止します
* 共有アカウントの作成および利用は禁止です
* Bot / CI / 自動化用途のアカウントは例外として許可しますが、Organization Owner（IT 部）の承認を必要とします

### 3.2 User name

* HDJ 社員はアカウント名の末尾に `-hdj` を付与してください
* ベンダーは自社を識別できるサフィックスの付与を推奨します

### 3.3 表示名（Display name）

以下の形式としてください：

* **社員：**

  ```
  First Name Last Name(HDJ)
  ```

  例：Taku Matsumoto(HDJ)

* **社員以外（推奨）：**

  ```
  First Name Last Name(会社名)
  ```

  例：Taro Yamada(ABC Corp)

※ Display name は GitHub 全体で共通のため、Organization ごとに変更はできません。

### 3.4 メールアドレス

* 社員： `hearst.co.jp` ドメインのメールアドレスを登録してください
* ベンダー： 業務用メールアドレスを登録してください
* フリーメールアドレス（Gmail、Yahoo Mail 等）のみでの利用は不可とします

### 3.5 二要素認証（2FA）

* **2FA は必須です**
* Organization 設定で 2FA を強制（Require two-factor authentication）します
* 2FA を有効化していないアカウントは Organization から自動的に除外されます
* 認証アプリ（TOTP）またはセキュリティキーの利用を推奨します（SMS は非推奨）

---

## 4. Organization レベルの設定方針

### 4.1 権限階層の基本方針

当社のセキュリティポリシーにより、Organization の特権管理者権限（Owner）は **IT 部のみ** が保有します。
一方、IT 部が直接関与しないプロジェクトでも各チームが自律的に運用できるよう、GitHub の権限委任機能を活用します。

| 権限層 | 対象者 | できること |
|---|---|---|
| **Organization Owner** | IT 部メンバー | Org 設定変更、リポジトリ作成/削除、Rulesets 管理、Team 作成 |
| **Repository Admin** | 業務上必要なプロジェクト管理者 | Webhooks・Secrets 管理、リポジトリ設定の変更（※ IT 部の承認が必要） |
| **Team Maintainer** | 各プロジェクトのリーダー | 担当 Team のメンバー追加/削除 |
| **Repository Maintain** | `{project}-managers` Team | PR 管理、ブランチ管理、リポジトリ設定の一部 |
| **Repository Write** | `{project}` Team | コード変更、PR 作成、Issue 管理 |

### 4.2 IT 部への依頼が必要な操作

以下の操作は Organization Owner 権限が必要なため、IT 部への依頼が必要です。
依頼は [Microsoft Teams の help-github チャンネル](https://teams.microsoft.com/l/channel/19%3Ad70b44e383e84df09d7632d7c5b61673%40thread.tacv2/help-github?groupId=f71761c3-8b43-47e8-93d0-8d143f393647&tenantId=cbb25906-c027-4adc-bac7-c4fc6b93690b)またはメール（it-is@hearst.co.jp）で受け付けます。

* リポジトリの新規作成・削除・可視性変更
* Team の新規作成・削除
* Outside Collaborator の招待
* Organization Rulesets の変更
* Organization 設定の変更
* リポジトリ Admin 権限の付与（下記 4.3 参照）

### 4.3 リポジトリ Admin 権限の付与

リポジトリの Admin 権限は、Organization Owner（特権管理者）には該当しませんが、最小権限の原則に基づき、**Maintain 権限では不足する業務上の正当な理由がある場合に限り** 付与できます。

**Admin 権限が必要となる主なケース：**

* Webhooks の設定・管理（CI/CD 連携等）
* Repository secrets / Variables の管理（GitHub Actions 等）
* Deploy keys の管理
* リポジトリ単位の GitHub App のインストール

**付与の手続き：**

1. プロジェクトリーダーが IT 部に申請（必要な理由を明記）
2. IT 部が承認し、対象の Team にリポジトリ Admin 権限を設定
3. **四半期ごと** の棚卸し時に、Admin 権限の必要性を再確認

※ Maintain 権限で業務が遂行できる場合は、Admin 権限を付与しません。

### 4.4 プロジェクトチームで完結できる操作

以下の操作は Organization Owner 権限なしで実行でき、各プロジェクトチームの裁量で行えます。

* Team メンバーの追加・削除（Team Maintainer が実施）
* Pull Request のレビュー・マージ
* ブランチの作成・削除（保護対象外のブランチ）
* Issue・Project の管理
* リポジトリの設定変更（Maintain または Admin 権限の範囲内）

### 4.5 メンバーの基本権限

| 設定項目 | 推奨値 | 説明 |
|---|---|---|
| デフォルトリポジトリ権限 | `None` | メンバーは Team 経由でのみリポジトリにアクセス |
| リポジトリ作成 | **Owner のみ** | リポジトリ作成は IT 部が対応 |
| リポジトリ削除 | **Owner のみ** | 誤削除防止のため、IT 部のみが実施 |
| リポジトリ可視性の変更 | **Owner のみ** | Private → Public への意図しない変更を防止 |
| 外部コラボレーターの招待 | **Owner のみ** | 権限管理を IT 部に集約 |
| リポジトリのフォーク | **禁止** | コードの意図しない外部流出を防止 |
| Pages の作成 | **Owner のみ** | 意図しない公開コンテンツの作成を防止 |
| Team の作成 | **Owner のみ** | Team の作成は IT 部が対応（日常運用は Team Maintainer に委任） |

### 4.6 セキュリティ機能（新規リポジトリのデフォルト）

| 設定項目 | 推奨値 | 説明 |
|---|---|---|
| Dependency graph | **有効** | 依存関係の可視化 |
| Dependabot alerts | **有効** | 脆弱性のある依存パッケージの自動検出 |
| Dependabot security updates | **有効** | 脆弱性修正の自動 PR 作成 |
| Secret scanning | **有効** | コミットされた秘密情報の検出 |
| Secret scanning push protection | **有効** | 秘密情報を含む push のブロック |

※ 既存リポジトリについても順次有効化してください。

---

## 5. 権限管理の方針

### 5.1 基本原則

* 個人に直接 Repository 権限を付与することは禁止します
* 権限は **Team 単位** で付与します
* Role（Read / Write / Maintain / Admin）は **最小権限の原則** を適用します

### 5.2 Team の種類

Team は **組織（部署）単位** と **プロジェクト単位** の 2 種類を使い分けます。
いずれも作成は IT 部に依頼してください。

#### 組織（部署）単位の Team

部署の階層構造に対応した Team を作成し、GitHub の親子 Team 機能で階層化します。

* 部署 Team には **Write 以下** の権限を付与します
* Maintain 以上の権限が必要な階層には、対応する **`-managers` Team** を作成します
* `-managers` Team には **Maintain 権限**（必要に応じて Admin 権限）を付与します

  ```
  hdj                              … 全社（Read / Write）
  ├── hdj-digital                  … デジタル部門
  │   ├── hdj-digital-engineering  … エンジニアリング（Write 権限）
  │   └── hdj-digital-managers     … デジタル部門管理者（Maintain 権限）
  └── hdj-editorial                … 編集部門
  ```

#### プロジェクト単位の Team

部署横断のプロジェクトや、ベンダーとの協業には、プロジェクト単位の Team を作成します。

  ```
  {プロジェクト名}                 … 一般メンバー（Write 権限）
  {プロジェクト名}-managers        … プロジェクト管理者（Maintain 権限）
  ```

#### 権限の原則

| Team の種類 | サフィックスなし | `-managers` |
|---|---|---|
| 付与可能な最大権限 | **Write** | **Maintain**（申請により Admin も可、セクション 4.3 参照） |

* ベンダーは専用の Team（プロジェクト単位または組織単位）に所属させ、必要なリポジトリのみにアクセスを許可します
* 不要になった Team は速やかに削除またはアーカイブします（IT 部に依頼）

### 5.3 Team Maintainer の活用

* IT 部が Team を作成する際に、プロジェクトリーダーを **Team Maintainer** に指定します
* Team Maintainer は以下の操作が可能です：
  * 担当 Team へのメンバー追加・削除
  * Team の説明（Description）の編集
* Team Maintainer は Organization Owner 権限を必要としないため、セキュリティポリシーに準拠しつつ日常運用を各チームに委任できます
* Team Maintainer の指定・変更は IT 部に依頼してください

### 5.4 Outside Collaborator

* Outside Collaborator は原則として使用せず、Team に所属させることを推奨します
* やむを得ず Outside Collaborator として招待する場合は、Organization Owner（IT 部）の承認と期限の設定を必要とします
* **四半期ごと** に Outside Collaborator の棚卸しを実施します

---

## 6. リポジトリ管理

### 6.1 命名規則

* リポジトリ名は **小文字の英数字とハイフン** を使用します（アンダースコアは非推奨）
* 基本構成は **`{プロジェクト概要}-{リソースタイプ}`** とします
* プロジェクト概要は、必要に応じて **`{ブランド}-{機能}`** のように分割できます
* リソースタイプが不要な場合（単一構成のプロジェクト等）は省略可能です

  ```
  {プロジェクト概要}-{リソースタイプ}
  {ブランド}-{機能}-{リソースタイプ}
  ```

#### プロジェクト概要の例

| 要素 | 例 |
|---|---|
| ブランド名 | `hearst`, `elle`, `elleshop`, `25ans`, `fujingaho`, `harpersbazaar`, `mensclub` |
| 機能・サービス名 | `ec`, `user`, `editorial`, `enquete`, `shop` |

#### リソースタイプの例

| リソースタイプ | 用途 |
|---|---|
| `web` | フロントエンド / Web アプリケーション |
| `api` | バックエンド API |
| `batch` | バッチ処理 |
| `db` | データベーススキーマ・マイグレーション |
| `tools` | ツール・ユーティリティ |
| `docs` | ドキュメント |
| `infra` | インフラ定義（IaC） |

#### 命名例

  ```
  hearst-ec                … ECプロジェクト（単一構成）
  elleshop-web             … ELLE SHOP フロントエンド
  hearst-user-api          … ユーザー管理 API
  erm-docs                 … ERMプロジェクトのドキュメント
  ```

### 6.2 デフォルトブランチ

* 新規リポジトリのデフォルトブランチ名は **`main`** とします
* 既存リポジトリの `master` → `main` への変更は、影響範囲（CI/CD、デプロイ設定等）を考慮して計画的に実施してください

### 6.3 リポジトリの可視性

* 原則として **Private** とします
* Public にする必要がある場合は、Organization Owner（IT 部）の承認を必要とします
* Public リポジトリには機密情報が含まれていないことを確認してください

### 6.4 リポジトリのライフサイクル

* 開発が終了し保守も不要になったリポジトリは **Archive** してください
* Archive されたリポジトリは読み取り専用となり、意図しない変更を防止できます
* **半年ごと** にリポジトリの棚卸しを実施し、不要なリポジトリの Archive を検討します

### 6.5 リポジトリの必須ファイル

各リポジトリには以下のファイルを含めることを推奨します：

| ファイル | 必須/推奨 | 説明 |
|---|---|---|
| `README.md` | 必須 | プロジェクト概要、セットアップ手順、デプロイ方法 |
| `.gitignore` | 必須 | 不要ファイルのコミット防止 |

---

## 7. ブランチ保護ルール

### 7.1 Organization Rulesets の活用

ブランチ保護は **Organization Rulesets**（Organization レベルのルールセット）で一括管理します。
Rulesets は Organization Owner（IT 部）が設定・管理し、対象リポジトリに自動適用されるため、
各プロジェクトチームがリポジトリごとに設定する必要はありません。

※ Organization Rulesets を利用することで、ブランチ保護を Organization 全体で一元管理できます。
リポジトリ Admin 権限を持つ Team がある場合でも、Organization Rulesets による保護が優先されます。

### 7.2 デフォルトブランチ（main / master）の保護

Organization Rulesets で、すべてのアクティブなリポジトリのデフォルトブランチに以下のルールを適用します：

| 設定項目 | 推奨値 | 説明 |
|---|---|---|
| Require a pull request before merging | **有効** | 直接 push の禁止 |
| Dismiss stale pull request approvals | **有効** | 新しい push があった場合、過去の承認を取り消し |
| Require status checks to pass | **有効（CI がある場合）** | CI/CD のパスを必須化 |
| Require branches to be up to date | **有効** | マージ前にブランチの最新化を必須化 |
| Enforce for administrators | **有効** | 管理者にも同じルールを適用 |
| Block force pushes | **有効** | 履歴の改変を禁止 |
| Block deletions | **有効** | デフォルトブランチの削除を禁止 |

### 7.3 適用対象

* 本番環境にデプロイされるリポジトリは **必須** とします
* 個人の検証用・一時的なリポジトリは Rulesets の対象から除外できます（IT 部に依頼）

---

## 8. 開発・レビューの基本ルール

### 8.1 ブランチ戦略

* 直接 `main` / `master` ブランチへ push しないでください
* 作業用ブランチを作成し、Pull Request を経てマージします
* ブランチ名は以下の形式を推奨します：

  ```
  feature/{概要}     … 新機能
  fix/{概要}         … バグ修正
  hotfix/{概要}      … 緊急修正
  chore/{概要}       … メンテナンス・設定変更
  ```

### 8.2 Pull Request

* Pull Request には変更の概要と目的を記載してください
* レビュワーを指定し、承認を得てからマージしてください
* セルフマージ（自分で作成した PR を自分で承認・マージ）は原則禁止です
* マージ後のブランチは削除してください

### 8.3 コードレビュー

* レビュワーは変更内容を理解し、以下の観点でレビューしてください：
  * 機能要件を満たしているか
  * セキュリティ上の問題がないか
  * 機密情報がコミットされていないか

---

## 9. Commit の注意点

* Commit author は個人が特定できる名前・メールアドレスを使用してください
* 業務コードは HDJ Organization のリポジトリにのみ commit してください
* **以下の情報を絶対に commit しないでください：**
  * API キー、トークン、パスワード
  * データベース接続文字列
  * 秘密鍵・証明書
  * 個人情報
* 万が一 commit してしまった場合は、直ちに該当シークレットを無効化・ローテーションし、Organization Owner（IT 部）に報告してください（履歴から完全に削除するには別途対応が必要です）

---

## 10. ベンダー参加・退出ルール

### 10.1 参加時

* Organization への招待は IT 部（Organization Owner）が実施します
* 招待後、Team への所属は **Team Maintainer**（プロジェクトリーダー）が対応できます
* 付与する権限は業務に必要な最小限とします（原則 Write まで）
* 参加時に本ガイドラインを周知し、遵守の同意を得てください

### 10.2 契約期間中

* Team 内の権限変更（メンバーの追加・削除）は Team Maintainer が対応できます
* Organization レベルの権限変更が必要な場合は、IT 部に申請してください
* プロジェクト異動時は、旧プロジェクトの Team から削除し、新プロジェクトの Team に追加します（Team Maintainer が対応）

### 10.3 契約終了・退出時

* 契約終了日をもって Organization から削除します（IT 部が実施）
* Team Maintainer は契約終了前に、担当 Team からのメンバー削除を実施してください
* 関連する Team メンバーシップもすべて削除されます
* 個人アカウント自体は削除しません（GitHub の仕様上、削除権限がありません）
* Issue / PR の履歴は保持されます
* **退出後もアクセス可能な状態になっていないか確認してください**

### 10.4 定期棚卸し

* **四半期ごと** に以下を実施します：
  * メンバー一覧と契約状況の突合
  * Outside Collaborator の確認と不要アカウントの削除
  * 長期間アクティビティのないアカウントの確認

---

## 11. セキュリティ・インシデント対応

### 11.1 報告対象

以下の事象を発見した場合は、速やかに Organization Owner（IT 部）へ報告してください：

* リポジトリへの不正アクセスの疑い
* 機密情報（トークン、パスワード等）の漏洩
* 身に覚えのないコミットや変更
* Dependabot / Secret scanning からの Critical アラート
* その他セキュリティ上の懸念事項

### 11.2 報告先

* **一次報告先：** Organization Owner メンバー（[Teams help-github](https://teams.microsoft.com/l/channel/19%3Ad70b44e383e84df09d7632d7c5b61673%40thread.tacv2/help-github?groupId=f71761c3-8b43-47e8-93d0-8d143f393647&tenantId=cbb25906-c027-4adc-bac7-c4fc6b93690b) / メール）
* **IT 部門：** it-is@hearst.co.jp

### 11.3 対応フロー

1. 発見者が Organization Owner（IT 部）に報告
2. Organization Owner が影響範囲を調査
3. 必要に応じてアクセス権の一時停止、シークレットのローテーション等の緊急対応を実施
4. 原因の特定と再発防止策の策定
5. 関係者への周知

---

## 12. 監査ログの活用

* Organization の Audit Log を定期的に確認します
* 以下のイベントを重点的に監視します：
  * メンバーの追加・削除
  * リポジトリの作成・削除・可視性変更
  * ブランチ保護ルールの変更
  * Outside Collaborator の招待
  * Organization Owner 権限の変更

---

## 13. 本ルールの変更について

* 本ドキュメントの内容は、必要に応じて更新されます
* 変更は Pull Request を通じて行い、Organization Owner メンバーのレビューを経て反映します
* 重要な変更がある場合は、全メンバーへ別途周知を行います

---

## 14. 問い合わせ先

* **GitHub Organization Owner（IT 部）：** リポジトリ作成、Team 作成、Org 設定変更等の依頼先
  * Teams: [help-github チャンネル](https://teams.microsoft.com/l/channel/19%3Ad70b44e383e84df09d7632d7c5b61673%40thread.tacv2/help-github?groupId=f71761c3-8b43-47e8-93d0-8d143f393647&tenantId=cbb25906-c027-4adc-bac7-c4fc6b93690b)
  * メール: it-is@hearst.co.jp
* **GitHub Organization Owner メンバー：** masashirohiroi, michi55, mikikomiura, taku-hdj, yasuhirokojima-ops
* **各プロジェクトの Team Maintainer：** Team メンバーの追加・削除等の日常運用

---

最終更新日：2026-02-16
