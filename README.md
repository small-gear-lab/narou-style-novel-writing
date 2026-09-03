# narou-style-novel-writing

日本の投稿サイト（小説家になろう・カクヨム・アルファポリス等）でよく見られる
「なろう系」Web小説のノリを書くための、[Agent Skills](https://agentskills.io)
準拠の Claude Code / Codex CLI 向けスキル。

特定の作品・企画に紐づかない汎用スキルとして、文体・構成のノウハウと
「複数人でも/複数話をまたいでも破綻しない」ための実務的なチェック機構
（AI臭除去チェックリスト、継続性台帳、ボイス早見表、力量体系メモ、
5軸ルールチェック）を持つ。作品固有のキャラクター設定・企画ルールは
このスキルには持たせず、利用側（記事・原稿のリポジトリ）の資料として
別途管理する。

## 経緯

日本語・なろう系ハイファンタジー特化の Claude Skill / OSS は存在しなかった
（2026年9月時点で deep research 済み）。ただし文体・様式が近い**中国網文**
**韓国웹소설**向けの創作支援フレームワークは複数存在し、実装レベルで非常に
参考になる仕組み（AI臭除去のGate構造、継続性追跡、ボイス一貫性チェック）
を持っていた。それらを実際に clone して SKILL.md・エージェント定義・
プロンプト本文まで読み込み、**日本語のなろう系文体に合わせて構造ごと
翻訳・移植**して作ったのがこのスキル。単純な文言の逐語訳ではなく、
「AI臭・紋切り型を検出して削る」「継続性を台帳で管理する」「複数人で
書いてもキャラの声がブレないようにする」という**方法論**を日本語の
実例に置き換えている。

## 参照した先行OSS（クレジット）

調査・移植にあたり、以下のリポジトリの構造・チェック観点を参考にした
（コードやテキストの逐語コピーではなく、方法論の翻訳移植）。いずれも
寛容なライセンス（MIT / Apache-2.0）。

| リポジトリ | ライセンス | 参考にした主な要素 |
|---|---|---|
| [oh-story-claudecode](https://github.com/worldwonderer/oh-story-claudecode) | MIT | AI臭除去の Gate 構造（A〜G）、削除優先の原則、禁止語彙・句式表、力量体系ナレッジベースの形式 |
| [awesome-novel-studio](https://github.com/MJbae/awesome-novel-studio) | Apache-2.0 | 5軸ルールチェック（禁止表現・ボイス・呼称・沈黙演出・翻訳体）、ボイス早見カードの設計、敵対者設計原則 |
| [story-skills](https://github.com/danjdewhurst/story-skills) | MIT | 継続性台帳（state.md）のスキーマ、「死んだキャラが歩く」検出の考え方、mentions/characters の区別 |
| [Claude-Book](https://github.com/ThomasHoussin/Claude-Book) | MIT | 継続性レビューの6軸（位置・時系列・環境・知識境界・物品・因果）、レビュー出力フォーマット |
| [creative-writing-skills](https://github.com/haowjy/creative-writing-skills) | Apache-2.0 | voice保持・継続性追跡の設計思想（軽量版） |
| [vibe-noveling](https://github.com/TulanCN/vibe-noveling) | MIT | 参考程度（oh-story-claudecode と同系統） |

調査時のクローンは `~/work/oss/novel-writing-research/` に保存してある
（このリポジトリには含めない）。

## 構成

```
narou-style-novel-writing/
├── SKILL.md                              # 本体：トリガー・文体の型・手順
└── references/
    ├── ai-smell-jp.md                    # 日本語なろう系向けAI臭チェックリスト
    ├── continuity-ledger-template.md     # 継続性台帳のテンプレート+記入例
    ├── voice-sheet-template.md           # ボイス早見表 + 5軸ルールチェック
    └── power-system-template.md          # 力量体系（軽量版）テンプレート
```

## 使い方

Claude Code から使う場合は、対象プロジェクトの `.claude/skills/` 配下に
このリポジトリへの symlink を張る:

```sh
ln -s /path/to/narou-style-novel-writing /path/to/project/.claude/skills/narou-style-novel-writing
```

作品固有のキャラクター設定・企画ルール・継続性台帳の実データは、
利用側リポジトリ（例: note記事なら記事フォルダ配下の `assets/<slug>/`）に
`references/*-template.md` を元にしたファイルとして置く。このスキル自体は
テンプレートと方法論だけを持ち、特定作品のデータは持たない。

## ライセンス

Dual-licensed MIT / Apache-2.0（[LICENSE-MIT](LICENSE-MIT) /
[LICENSE-APACHE](LICENSE-APACHE)、`ffmpeg-gif-optimizer` と同じ形式）。
どちらか選べる形式で、上記クレジット元（MIT / Apache-2.0）とも
ライセンス的に整合する。
