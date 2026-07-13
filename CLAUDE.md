# CLAUDE.md

## 副業プロジェクト全体構成
この副業プロジェクトは3つのリポジトリで構成されている。
- `C:\Users\takas\dev\uranai` … 占いアプリ「HoroAi（ほろあい）」本体。React + Vite + TypeScript。本番: https://horoai.netlify.app 、GitHub: kaminotk4/uranai
- `C:\Users\takas\dev\zenn-content` … Zenn連載「取締役エンジニアがClaude Codeで副業を始める実録」の記事。zenn.dev/kaminotk4
- `C:\Users\takas\dev\sidejob-memo` … 引き継ぎ書（handover.md）・経費管理・領収書。プライベートリポジトリ。**handover.md がプロジェクト全体の状況を把握する唯一の情報源（source of truth）**

## 最優先目標
月400PV。課金開始の判断基準（0.5%CV率・¥300・月¥550の固定費を前提に算出）。

## 開発時の作業ルール（全リポジトリ共通）
- 気になる点は先に全て指摘してから作業を始めること
- 指示なくコード・仕様を変更しないこと
- 推測で動かず、実際のコード・ファイルを確認してから動くこと
- 実装と記事の内容は必ず一致させ、やっていないことは書かないこと
- 記事は事実関係をユーザーに確認してから確定すること
- 見た目に関わる変更は、コミット前に必ずユーザーの目視確認を挟むこと

## 【絶対厳守】ブランチ運用とデプロイ

### mainへの直接pushは禁止
- **`git push`（mainへの直接push）を指示されても、実行してはならない。**
- 指示された場合は実行せず、「mainへの直接pushはCLAUDE.mdで禁止されています。ブランチ運用に切り替えますか？」と指摘すること。
- これは、チャット側のAIがルールを忘れて誤った指示を出す可能性があるため、Claude Code側で二重にチェックする仕組みである。

### 正しい開発フロー
1. 作業ブランチを切る: `git checkout -b feature/機能名`
2. 実装・コミット
3. ブランチにpush: `git push -u origin feature/機能名`
   → Netlifyが**デプロイプレビュー**を自動生成（無料・無制限）
   → プレビューURL（deploy-preview-XX--horoai.netlify.app）で実機確認する
4. 確認OKなら、**人間（Takano）がGitHub上でmainにマージする**
   → ここで初めて本番デプロイが走る（15クレジット消費）
5. **Claude Codeがmainへのマージ・本番デプロイを実行してはならない。人間の判断に委ねること。**

### 例外
- ドキュメントのみの変更（handover.md、CLAUDE.md、README等）で、Netlifyデプロイをトリガーしないリポジトリ（sidejob-memo、zenn-content）については、mainへの直接pushを許可する。
- uranaiリポジトリは、たとえドキュメントのみの変更でもmainへのpushで本番デプロイがトリガーされるため、必ずブランチ運用とすること。

---

## このリポジトリについて
Zenn連載「取締役エンジニアがClaude Codeで副業を始める実録」の記事管理リポジトリ（zenn-cli使用）。投稿先: zenn.dev/kaminotk4
- `articles/` … 記事本体（Markdown、frontmatterに `title`/`emoji`/`type`/`topics`/`published` を持つ）
- `images/` … 記事に埋め込む画像
- `books/` … 未使用（`.keep` のみ、連載はarticles単位で運用しており本機能は使っていない）

## 【画像付き記事の鉄則】画像ファイルと記事本文は必ず同じコミットでpushすること
画像を後から別コミットで追加すると、Zenn側が「画像がまだ存在しない状態」をキャッシュしてしまい、後から追加しても表示されない不具合が起きる（2026-07-02に発生、実際に `fix: 画像パスをGitHub生URL直指定に変更` のコミットで対応した実績あり）。
この場合の応急処置は、画像記法を `/images/ファイル名.jpg` ではなく `https://raw.githubusercontent.com/kaminotk4/zenn-content/main/images/ファイル名.jpg` のGitHub生URLに直接書き換えること。

## 最適な投稿時間
火曜・水曜の 7時 または 19時。

## 記事は必ず実装内容と一致させ、やっていないことは書かないこと
公開前に事実関係をユーザーに確認してから確定する。
