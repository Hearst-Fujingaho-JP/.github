# Organization 設定変更チェックリスト

ガイドラインに準拠するために必要な設定変更の一覧です。

**実施者：** Organization Owner（IT 部）のみが実施可能です。

---

## 1. セキュリティ（優先度：高）

> Settings > Authentication security

- [x] Require two-factor authentication を有効化
  - 対応済み（設定確認日: 2026-03-23）

> Settings > Security > Advanced Security > Configurations

- [x] Dependency graph を有効化（新規リポジトリのデフォルト）
  - 対応済み（設定日: 2026-03-23）
- [x] Dependabot alerts を有効化（新規リポジトリのデフォルト）
  - 対応済み（設定日: 2026-03-23）
- [x] Dependabot security updates を有効化（新規リポジトリのデフォルト）
  - 対応済み（設定日: 2026-03-23）
- [ ] 既存リポジトリに対しても上記を順次有効化
- ~~Secret scanning を有効化（新規リポジトリのデフォルト）~~
  - **対応不可：Team プランでは利用不可。GitHub Enterprise が必要。**
- ~~Secret scanning push protection を有効化（新規リポジトリのデフォルト）~~
  - **対応不可：Team プランでは利用不可。GitHub Enterprise が必要。**

---

## 2. メンバー権限の制限（優先度：高）

> Settings > Member privileges

- ~~Repository creation を「Private のみ許可」に変更~~
  - **対応不可：Team プランでは Public リポジトリ作成の制限が不可。全メンバー可のまま運用（ポリシー上は IT 部承認が必要）。**
- [x] Repository deletion and transfer を無効化（設定日: 2026-03-23）
- [x] Repository visibility change を無効化（設定日: 2026-03-23）
- ~~Outside collaborators の招待権限を Owner のみに制限~~
  - **対応不可：Team プランでは制限が不可。GitHub Enterprise が必要。**
- ~~Pages creation を Owner のみに制限~~
  - **方針変更：メンバーに許可することとした（2026-03-23）。**
- [x] Team の作成を Owner のみに変更（設定日: 2026-03-23）

---

## 3. 変更不要（現状維持）

- [x] デフォルトリポジトリ権限: None
- [x] Private リポジトリのフォーク: 禁止

---

## 4. Team Maintainer の設定（優先度：高）

Team の作成を Owner のみに制限する代わりに、各チームの日常運用を委任するための設定です。

> Organization > Teams > 各 Team > Settings > Members

既存および新規の Team に対して、以下を実施します：

- [ ] 組織（部署）単位の Team：各部署の責任者を **Team Maintainer** に指定
- [ ] プロジェクト単位の Team：各プロジェクトリーダーを **Team Maintainer** に指定
- [ ] Team Maintainer に対して、メンバー追加・削除の運用を委任する旨を周知

### Team Maintainer ができること

* 担当 Team へのメンバー追加・削除（Organization メンバーに限る）
* Team の Description 編集

### Team Maintainer にできないこと（IT 部への依頼が必要）

* Team の新規作成・削除
* Organization 外のユーザーの招待（Organization への招待が先に必要）
* Team のリポジトリアクセス権限の変更

---

## 5. リポジトリ Admin 権限の付与（優先度：中）

Maintain 権限では不足する業務上の理由がある場合に、リポジトリ Admin 権限を付与します。

### 付与フロー

1. プロジェクトリーダーが IT 部に申請（Teams help-github チャンネルまたはメール）
2. IT 部が必要性を確認し、承認
3. `{project}-admins` Team を作成（未作成の場合）し、対象リポジトリに Admin 権限を設定
4. 申請者を Team に追加

### Admin 権限が必要となる主なケース

* Webhooks の設定・管理（CI/CD 連携等）
* Repository secrets / Variables の管理（GitHub Actions 等）
* Deploy keys の管理
* リポジトリ単位の GitHub App のインストール

### 棚卸し

- [ ] 四半期ごとに Admin 権限の付与状況を確認し、不要な権限を Maintain に降格

---

## 6. Organization Rulesets の設定（優先度：高）

リポジトリ単位の Branch protection rules の代わりに、Organization Rulesets で一括管理します。
これにより、各リポジトリに Admin 権限を付与することなくブランチ保護を実現できます。

> Settings > Code and automation > Rules > Rulesets > New ruleset

### 6.1 デフォルトブランチ保護ルールセット

- [x] Ruleset 名: `default-branch-protection`（設定日: 2026-03-23）
- [x] Enforcement status: **Active**
- [x] Target: **All repositories**（既存リポジトリへの影響を考慮し、段階的に強化予定）

#### ルール設定

- [x] Restrict deletions: **有効**
- ~~Require a pull request before merging: 有効~~
  - **方針変更：既存リポジトリへの影響を考慮し、当面無効。今後チームへの周知後に有効化を検討。**
- ~~Require status checks to pass: 有効~~
  - **方針変更：CI 未整備のリポジトリが多いため無効。リポジトリ単位で整備後に有効化を検討。**
- [x] Block force pushes: **有効**
- [x] Enforce for administrators: **有効**

---

## 7. 変更不要（現状維持のブランチ保護）

旧方式（リポジトリ単位の Branch protection rules）で既に設定済みのリポジトリは、
Organization Rulesets 適用後に順次移行してください。

---

## 8. 実施時の注意事項

1. **2FA必須化の前に**: 全メンバーに2FA設定を依頼し、猶予期間（1〜2週間）を設ける
2. **リポジトリ作成権限の制限前に**: メンバーにリポジトリ作成が必要な場合のフロー（IT 部への依頼方法）を周知
3. **Team 作成権限の制限前に**: 既存の Team 構成を確認し、必要な Team Maintainer を事前に指定
4. **Organization Rulesets の適用前に**: 各リポジトリのCI/CDパイプラインへの影響を確認
5. **依頼チャンネルの周知**: [Teams help-github チャンネル](https://teams.microsoft.com/l/channel/19%3Ad70b44e383e84df09d7632d7c5b61673%40thread.tacv2/help-github?groupId=f71761c3-8b43-47e8-93d0-8d143f393647&tenantId=cbb25906-c027-4adc-bac7-c4fc6b93690b)での IT 部への依頼フローを周知
