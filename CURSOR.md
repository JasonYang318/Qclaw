# CURSOR.md — Qclaw 專案開發規範

## 專案概述

Qclaw 是一個 Electron 桌面應用，為 OpenClaw CLI 提供圖形化操作介面。
技術棧：Electron + React + TypeScript + Vite + Mantine + Tailwind CSS。

## 專案結構

```
electron/
  main/             主程序（窗口管理、CLI 調用、IPC 處理）
  preload/          預加載腳本（安全橋接）
src/
  pages/            頁面元件（嚮導步驟、Dashboard、聊天等）
  components/       UI 元件
  lib/              業務邏輯（渠道注冊、提供商注冊等）
  shared/           共享模組（配置流程、網關診斷等）
  assets/           圖標與靜態資源
  types/            TypeScript 型別定義（含 IPC API 介面）
docs/               專案文檔
scripts/            建構與發佈腳本
build/              應用圖標與打包資源
```

## 開發指令

| 指令 | 用途 |
|------|------|
| `npm run dev` | 啟動開發伺服器 |
| `npm run typecheck` | TypeScript 型別檢查 |
| `npm test` | 執行測試 |
| `npm run build:app` | 前端 + 主程式編譯 |
| `npm run build` | 完整打包（需簽章憑證） |

## 提交前驗證清單

每次 commit 前必須通過：

```powershell
npm run typecheck
npm test
npm run build:app
```

`npm run build` 因 `forceCodeSigning: true` 在無憑證環境會失敗，屬預期行為。

## 跨層改動順序

新增 IPC 功能時，依照以下順序開發：

1. **型別定義** — `src/types/electron.d.ts`
2. **Preload 橋接** — `electron/preload/`
3. **IPC Handler** — `electron/main/ipc-handlers.ts`
4. **主程式邏輯** — `electron/main/`（新增服務檔案）
5. **UI 元件** — `src/pages/` 或 `src/components/`

## Commit 訊息格式

```
<type>: <簡述>
```

| 前綴 | 用途 |
|------|------|
| `feat` | 新功能 |
| `fix` | 修 bug |
| `docs` | 文件變更 |
| `refactor` | 重構 |
| `test` | 測試 |
| `chore` | 工具 / 設定 |

## 分支命名

- 新功能：`feat/<功能名>`
- 修 bug：`fix/<問題描述>`
- 文件：`docs/<主題>`

## CI 流程

GitHub Actions 會在每次 push / PR 執行：
- Node.js 安裝
- TypeScript 型別檢查
- Vitest 測試
- Model regression 測試

## 注意事項

- 授權協議：Apache-2.0
- UI 框架：Mantine 8 + Tailwind CSS 3
- 目標平台：macOS 11+、Windows 10+（x64）
- Node.js 版本：至少 22，推薦 24
