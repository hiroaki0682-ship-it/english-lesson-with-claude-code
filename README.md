# Claude Codeを英会話講師にする(サンプル)

note記事「Claude Codeを英会話講師にする」で紹介した仕組みのサンプルです。
Claude Codeのカスタムスラッシュコマンドと、進捗を記録するMarkdownファイルだけで、
「前回の弱点を覚えている英会話講師」を再現できます。

## 必要なもの

- [Claude Code](https://claude.com/claude-code)(月額課金プランが必要です)

## 使い方

1. このリポジトリをクローンし、Claude Codeで開く
2. Claude Codeで `/english-setup` と入力する。目的・現状レベル・練習したいシチュエーションをAIがヒアリングし、`data/skill-roadmap.md` と `data/english-lesson-state.md` の初期設定を代わりにやってくれる(CEFRなどの専門知識は不要。トラック(ビジネス/日常会話)は選んだシチュエーションから自動的に判断される)
3. Claude Codeで `/english-practice` と入力してレッスンを開始する
4. トラック(ビジネス/日常会話)を聞かれたら答える
5. 英語で会話練習が始まる。間違えたらその場で日本語で指摘される
6. 「終了」と言うと、`data/english-lesson-state.md` が自動的に更新される
7. 次回また `/english-practice` を実行すると、前回の続きからレッスンが始まる

## 含まれるファイル

| ファイル | 役割 |
|---|---|
| `.claude/commands/english-setup.md` | 初回のヒアリングを行い、ロードマップと状態ファイルを初期設定するカスタムコマンド |
| `.claude/commands/english-practice.md` | レッスンの進め方を定義したカスタムコマンド |
| `data/english-lesson-state.md` | トラックごとの進捗・次回の重点を記録する状態ファイル |
| `data/skill-roadmap.md` | 中長期のレベル目標を記録するロードマップ |

## 仕組み

```
(初回のみ) /english-setup を実行
        │
        ▼
目的・現状レベル・シチュエーションをAIがヒアリング(トラックは自動判断)
        │
        ▼
skill-roadmap.md / english-lesson-state.md を初期設定
        │
        ▼
/english-practice を実行
        │
        ▼
english-lesson-state.md を読む(前回の「次回の重点」を確認)
        │
        ▼
英語で会話練習(間違いはその場で日本語フィードバック)
        │
        ▼
「終了」と言う
        │
        ▼
english-lesson-state.md に書き戻す(進捗ログ追加・次回の重点を更新)
        │
        └──── 次回はここから再開 ──── ▶
```

## 前提・制限

- Claude Code(月額課金プラン)が必要です。無料ツールだけでの再現方法は別途検証中です。
- レッスン内容そのもの(会話の質)はClaude本体の実力に依存します。この仕組みが提供しているのは「前回の記録を次回に持ち越す」部分だけです。
