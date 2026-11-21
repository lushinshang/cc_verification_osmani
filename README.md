# 讓 agent 拿證據交差：Claude Code 團隊的驗證做法（深度導讀）

Addy Osmani 在 claude.dev 部落格（2026-11-16）寫 Claude Code 團隊怎麼讓 coding agent 用證據驗證自己的工作：前半是 E2E、性質、差異三種值得投資的測試，後半是團隊讓檢查本身保持誠實的做法（verify skill、三輪失敗交人、依 diff 觸發的可靠性清單、改動碰不到評它的檢查、commit 閘門先觀察再強制、審查審查者、漏網變規則並定期修剪）。本導讀對照文中連結的官方文件，分清哪些有文件佐證、哪些只是團隊自述。

## 製作聲明

本導讀的分析、撰稿、翻譯、查證、審稿與插圖都由 AI 模型完成，不是人類分析，也沒有經過人工逐句校稿。

- 主控與撰稿：Claude Opus 5.5。
- 查證：Opus 級 subagent 開啟原文與文中全部連結逐項核對；Codex 與 agy（Gemini 4 Argon）各自獨立查證。重要說法須兩方以上一致且有官方或第一手來源才寫成確定，只有原文自述者在文中標明。
- 分析角度：參考 `ai-mentor-agents` 的分析框架，由 Opus 級 subagent 整理成備忘（`research/lens_ai-mentor-agents.md`），只當撰稿參考。
- 審稿：Opus 級 subagent 分三段獨立審查（事實、編輯與可讀性、終審），最後由一個 Opus 模型依潤稿準則（`editor-in-chief` skill）修飾文字並標示重點。發布後再經總編輯整理：由 Claude Opus 5.5 依同一準則逐句對照原文，修正措辭、統一導讀與譯本的術語，未新增事實。
- 全文譯本：Opus 級模型做術語表、審校與合併，Sonnet 級模型逐段翻譯，再經總編輯整理統一術語並修順數句，未經人工校稿，見 [translation-zh.html](translation-zh.html)。
- 圖片：封面由 Codex 生圖（不含文字）；流程圖為程式繪製的 SVG；單元測試與 E2E 失敗訊息對照依原文配圖說明以 HTML 元件重畫，不是原圖。

## 檔案

| 檔案 | 說明 |
|---|---|
| `cc_verification_osmani.md` | 導讀正文（Markdown） |
| `index.html` | 導讀網頁（瀏覽器直接開啟） |
| `translation-zh.html` | 全文繁中譯本（非官方） |
| `images/hero-16x9.png`、`images/hero-9x16.png`、`images/og-hero.jpg` | 封面（桌面、手機、社群預覽） |
| `images/fig-pipeline-zh.svg`、`images/fig-pipeline-zh-mobile.svg` | 檢查流程示意圖（桌面、手機） |
| `qa/` | 版面驗收截圖與結果 |
| `research/` | 需求、證據帳本、三方查證、透鏡備忘、術語表與譯文工作檔、審稿紀錄、建置腳本 |

重建：

```bash
python3 research/gen_svg.py
python3 research/build.py --root . --kind guide
python3 research/build.py --root . --kind translation
```

## 原始資料

- 類型：網頁文章（英文）。
- 名稱：Addy Osmani，〈How the Claude Code team thinks about verification〉，claude.dev，2026-11-16。
- 網址：https://claude.dev/blog/how-the-claude-code-team-thinks-about-verification/
- 本機全文：`research/source_a1.md`（主控 session 抓取的 Markdown 複本，僅供核對，不公開）。
- 逐字稿：不適用（非影音來源）。

## 重要來源

- 官方文件：[Customize Code Review](https://code.claude.com/docs/en/code-review-customize)、[Read and request reviews on a pull request](https://code.claude.com/docs/en/code-review-pull-requests)、[Hooks reference](https://code.claude.com/docs/en/hooks)、[/goal](https://code.claude.com/docs/en/goal)、[Skills](https://code.claude.com/docs/en/skills)。
- 第一手（同團隊）：[How the Claude Code team does code review](https://claude.dev/blog/solving-code-review-at-anthropic/)（2026-10-15）、[Building verification loops in Claude Code with skills](https://claude.com/resources/articles/building-verification-loops-in-claude-code-with-skills)（2026-07-22）、[Claude Managed Agents](https://claude.com/resources/articles/claude-managed-agents)（2026-04-08）。

## 狀態

已發布（2026-11-21）：https://lushinshang.github.io/cc_verification_osmani/
