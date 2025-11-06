# 💻 程式碼方法論 (Code Engineering Methodology)

---

## 一、導論：從指令到結構

傳統程式開發若缺乏架構思維，容易演變為「功能堆疊」的巨石式程式碼。這樣的系統難以維護、測試與擴充。

**模組化程式碼方法論**的核心思想是：

> 將程式視為邏輯模組的組合，使其具備結構清晰、可重用、可測試的工程特性。

---

## 二、方法論核心：八項原則

### 📘 三、原則對應摘要表（方法論總覽）

| 原則編號 | 名稱             | 關鍵理念             | 實踐焦點           | 關聯角色  |
| ---- | -------------- | ---------------- | -------------- | ----- |
| 1    | 程式即邏輯資產 (CaA)  | 程式具版本與測試性        | 使用 Git 管理      | 全員    |
| 2    | 模組化與單一職責 (SRP) | 一個模組只做一件事        | 拆分功能模組         | 開發者   |
| 3    | 關注點分離 (SoC)    | 資料、邏輯、展示分離       | 明確目錄架構         | 全員    |
| 4    | 組合優於繼承         | 函式與服務組合          | JSON / DI 管理依賴 | 架構師   |
| 5    | DRY 原則         | 功能不重複定義          | 建立共用模組         | 開發者   |
| 6    | 宣告式依賴管理        | 宣告「依賴關係」而非「手動整合」 | 使用配置或容器        | 架構師   |
| 7    | Context 介面     | 模組透過上下文溝通        | 維護共享狀態結構       | 系統工程師 |
| 8    | 可測試性           | 支援單元與整合測試        | 建立測試框架         | 全員    |

---

## 三、原則細節說明

### 原則一：程式即邏輯資產 (Code as Architecture - CaA)

**操作目標：**
讓所有程式模組具備版本管理、可測試、可追溯特性。

**操作規範：**

1. 程式碼需納入 Git 版本控制。
2. 重大變更需撰寫明確 Commit message。
3. 禁止以未版本化的臨時檔案作為正式版本。

---

### 原則二：模組化與單一職責 (Modularity & SRP)

**操作目標：**
每個函式或類別模組只負責一項明確功能。

**操作規範：**

1. 函式命名應清楚表達目的，如 `parse_json()`、`analyze_text()`。
2. 一個模組不得同時處理資料與展示邏輯。
3. 超過 200 行的模組需進行拆分。

---

### 原則三：關注點分離 (Separation of Concerns - SoC)

**操作目標：**
讓資料流、邏輯流與展示流彼此分離。

**操作規範：**

1. 目錄結構必須遵循以下格式：

```text
src/            → 程式核心邏輯
services/       → 商業邏輯或服務層
api/            → 輸入輸出介面層
config/         → 系統設定與宣告
scripts/        → 工具與自動化腳本
```

2. 禁止在同一檔案中同時處理資料與呈現邏輯。
3. 確保每層只負責其定義職責。

---

### 原則四：組合優於繼承 (Composition over Inheritance)

**操作目標：**
以組合的方式整合功能，而非建立複雜的繼承鏈。

**操作規範：**

1. 優先以「組合物件」實現多功能整合。
2. 避免多重繼承與深層父類關係。
3. 使用依賴注入 (DI) 或配置式宣告統一依賴管理。

---

### 原則五：DRY 原則 (Don't Repeat Yourself)

**操作目標：**
減少重複邏輯與代碼片段，確保功能集中管理。

**操作規範：**

1. 重複程式段應抽象為共用函式。
2. 共用邏輯統一放置於 `utils/` 或 `core/`。
3. 禁止複製貼上其他模組邏輯。

---

### 原則六：宣告式依賴管理 (Declarative Dependency Management)

**操作目標：**
讓依賴關係明確可追溯，而非隱性耦合。

**操作規範：**

1. 使用 `requirements.txt` 或 `package.json` 管理依賴。
2. 透過配置檔 (YAML / JSON) 宣告模組間依賴關係。
3. 禁止在程式中硬編碼外部依賴。

---

### 原則七：Context 介面 (Contextual Interfaces)

**操作目標：**
確保模組之間的溝通透過統一的 Context 物件。

**操作規範：**

1. 所有模組的資料交換須透過 Context 物件。
2. Context 結構需以 Schema 定義，例如：

```json
{
  "user": {"id": "", "role": ""},
  "session": {"input": "", "output": ""},
  "state": {"logs": []}
}
```

3. 模組僅能修改授權欄位。

---

### 原則八：可測試性 (Testability)

**操作目標：**
確保每個模組可被單元測試與整合測試覆蓋。

**操作規範：**

1. 每個功能需對應至少一個 `test_*.py` 測試檔案。
2. 單元測試聚焦於函式行為；整合測試模擬實際流程。
3. 若測試失敗，錯誤應指向具體模組。

---

## 四、三層架構導入：從函式到應用系統

在模組化方法論基礎上，建立對應於軟體工程的三層邏輯結構：

| 層級                  | 名稱           | 說明                | 類比概念 |
| ------------------- | ------------ | ----------------- | ---- |
| 🧩 Function（功能函式）   | 單一功能邏輯單元     | Function          |      |
| 💎 Module（功能模組）     | 整合多個函式的可重用單元 | Library / Package |      |
| 🚀 Application（應用層） | 組合多模組形成可執行系統 | Application       |      |

**邏輯：**

> Function 定義「功能」、Module 組合「邏輯」、Application 執行「系統」。

---

## 五、結語

透過八項原則與三層結構，程式開發不再是片段代碼的堆疊，而是一個有邏輯、有秩序、可維護的系統設計過程。

> **程式碼不只是指令，而是邏輯資產。**
> 每一段程式，都應是可重組、可驗證、可演化的邏輯單元。

---

# 🧭 雙軌治理架構設計指南（核心 GEM/程式碼 ⟷ 開發層級/專案）

> 目標：在同一個工作空間中，左軌建立**核心層**（GEM 與程式碼模組化治理），右軌建立**開發層**（層級在上、專案在下），兩邊以版本與介面契約解耦，並共用一套方法論（SRP/SoC/DRY/宣告式建構/SSOT/可測試性）。

---

## 1) 總體藍圖

```
/workspace
├─ core/                           # 左軌：核心層（穩定、可重用）
│  ├─ prompts/                    # 核心 GEM 與 components 的治理
│  │  ├─ components/              # fn_*.md（單一職責）
│  │  ├─ gems/                    # gem_*.json（組合模組）
│  │  ├─ tools/                   # build/validate/eval 工具
│  │  ├─ compiled_gems/           # 生成（唯讀）
│  │  └─ registry/                # 已發佈 GEM 索引/中繼資料
│  └─ code/                       # 核心程式碼（依方法論拆分模組）
│     ├─ src/
│     │  ├─ domain/               # 業務中立的核心邏輯（純函式/Pydantic）
│     │  ├─ services/             # 用組合方式封裝 domain（無框架耦合）
│     │  ├─ adapters/             # 對外適配（DB/向量庫/LLM/Cache）
│     │  ├─ api/                  # 對外 API（FastAPI/Express）
│     │  └─ utils/                # 共用工具（pure utilities）
│     ├─ tests/                   # 單元/整合測試
│     ├─ pyproject.toml|package.json
│     └─ Makefile
│
├─ dev/                            # 右軌：開發層（變動、貼近業務）
│  ├─ layers/                      # 層級在上：foundation/logic/interface
│  │  ├─ foundation/              # 共用規範、SSOT 模板、policy、guardrails
│  │  ├─ logic/                   # 對 core.prompts 的「封裝/路由/策略」
│  │  └─ interface/               # API Gateway/CLI/UI/調度器
│  └─ projects/                    # 專案在下：每專案自有上下文
│     ├─ <proj-A>/
│     │  ├─ ssot/                 # 專案級 SSOT schema + migrations
│     │  ├─ prompts/
│     │  │  ├─ recipes/           # 引用 core/registry 中發佈的 GEM
│     │  │  ├─ compiled_prompts/  # 生成（唯讀）
│     │  │  └─ evals/             # Prompt 回歸/紅隊測試集
│     │  ├─ app/                  # 專案應用（編排多 GEM 與 services）
│     │  ├─ adapters/             # 專案特定外掛（如客製 connector）
│     │  ├─ locks/                # gems.lock.json + deps.lock
│     │  ├─ tests/                # 專案整合/E2E 測試
│     │  └─ Makefile
│     └─ <proj-B>/ ...
│
└─ ci/                              # CI/CD pipelines（核心/開發分流）
```

---

## 2) 契約與解耦（兩軌相連的五個接點）

1. **GEM Registry**（`core/prompts/registry/`）：核心層唯一發佈源；開發層只透過 registry 取用。
2. **Lockfile**（`dev/projects/*/locks/gems.lock.json`）：固定專案實際採用的 GEM 版本與 SHA；確保可重現與安全回滾。
3. **SSOT Schema**：專案級定義（`dev/projects/*/ssot/`）；核心 GEM 僅能讀寫授權欄位（在 GEM spec 的 `io_rules` 明確標註）。
4. **Service API 契約**（`core/code/src/api/` + OpenAPI/JSON Schema）：開發層 app 僅以 API/SDK 調用核心 services，不直接穿透 domain 實作。
5. **Eval/Tests 門檻**：核心層負責功能正確性（unit），開發層負責場景有效性（eval/integration）；CI 在兩邊都設 Gate。

---

## 3) 核心程式碼的模組化拆分（依前述方法論）

* **SRP/SoC**：`domain/`（純邏輯）與 `adapters/`（I/O）分離；`services/` 用組合聚合 domain 能力。
* **Ports & Adapters（六邊形/洋蔥架構）**：

  * *Ports*：service 介面（抽象）
  * *Adapters*：具體連接（DB/LLM/消息匯流排/向量庫）
* **宣告式依賴**：以 `pyproject.toml`/`package.json` 與 `container/di` 建構依賴，不在核心中硬編碼外部端點。
* **可測試性**：domain 100% 可單測；service 以 mock adapters 測；adapter 以合約測試確保對外一致。

**範例骨架（Python）**

```
core/code/src/
├─ domain/
│  ├─ intent.py           # analyze_intent(data) -> Intent
│  ├─ summary.py          # generate_summary(doc, intent) -> Summary
│  └─ types.py            # Pydantic/BaseModel 型別
├─ services/
│  └─ knowledge_extractor.py  # 組合 domain：先意圖→再摘要→產出結構
├─ adapters/
│  ├─ llm_openai.py       # 呼叫模型，封裝重試/費用/觀測
│  └─ vec_pgvector.py     # 任何外部系統皆在 adapters
└─ api/
   └─ http.py             # FastAPI，定義路由/DTO，不含業務邏輯
```

---

## 4) 核心 GEM 的治理（與核心程式碼一致的原則）

* **Components（fn_*.md）**：前置 YAML meta（name/version/reads/writes/contracts）。
* **GEM（gem_*.json）**：以宣告式清單組合 components，並在 `io_rules.authorize_writes` 授權欄位；`stage_order` 控制流程。
* **Registry 發佈**：每次發佈產生 `index.json`（含 name/version/sha/compat），供專案 lockfile 解析。
* **測試**：components 單測（以 mock SSOT）；GEM 整合測試（以樣本 SSOT、檢驗輸出結構）。

---

## 5) 右軌：層級在上、專案在下的運作

* **layers/foundation**：集中存放規範（SSOT 模板、guardrails、policy、模型路由規則）。
* **layers/logic**：負責「如何選 GEM、如何路由、如何做前後處理」；例如：根據文本長度/語言/風險等條件，選擇不同 GEM。
* **layers/interface**：對外連接（API Gateway、CLI、UI）；避免專案各自封裝。
* **projects/**：僅做「情境定義與編排」：

  * `ssot/`：專案欄位與 migration
  * `prompts/recipes/`：引用 registry 中的 GEM
  * `app/`：把 layers 與核心 service 串起來（編排）
  * `locks/`：固定採用版本

---

## 6) 版本與發布：雙軌 CI/CD（最小可行清單）

**核心層 CI（core/）**

* `validate-prompts`：schema、授權欄位、版本相容
* `build-gems`：編譯至 `compiled_gems/`
* `unit-tests`：domain/services/adapters 單元測試
* `publish-registry`：產生/更新 `registry/index.json`

**開發層 CI（dev/）**

* `sync-registry`：拉取最新 registry
* `update-lock`（手動觸發）→ `eval` → `approve` → `lockfile commit`
* `build-recipes`：輸出 `compiled_prompts/`
* `integration-tests`：專案整合/E2E
* `snapshot`：打包（compiled_prompts + lockfiles + config）供部署與回滾

---

## 7) 最小樣板（可直接複製）

**GEM 宣告**（`core/prompts/gems/gem_knowledge_extractor.json`）

```json
{
  "name": "gem_knowledge_extractor",
  "version": "1.0.0",
  "modules": [
    {"name": "fn_analyze_intent", "version_range": ">=1.0 <2.0"},
    {"name": "fn_generate_summary", "version_range": ">=1.2 <2.0"},
    {"name": "fn_format_json_output", "version_range": ">=1.0 <2.0"}
  ],
  "io_rules": {
    "authorize_writes": {
      "fn_analyze_intent": ["project_spec.intent", "project_spec.analysis"],
      "fn_generate_summary": ["project_spec.summary"],
      "fn_format_json_output": ["project_spec.report"]
    },
    "stage_order": ["intent", "analysis", "summary", "report"]
  }
}
```

**專案 Recipe**（`dev/projects/proj-A/prompts/recipes/demo_research.json`）

```json
{
  "name": "demo_research",
  "version": "0.1.0",
  "use_gems": [
    {"name": "gem_knowledge_extractor", "version": "1.0.0"}
  ],
  "context": "研究報告的抽取/摘要/風險提示",
  "guardrails": ["不得產出個資", "引用需標註來源"]
}
```

**Lockfile**（`dev/projects/proj-A/locks/gems.lock.json`）

```json
{
  "locked_at": "2025-11-06T10:00:00Z",
  "registry": "../core/prompts/registry/index.json",
  "entries": [
    {"name": "gem_knowledge_extractor", "version": "1.0.0", "sha": "9f31..."}
  ]
}
```

---

## 8) 決策準則（何時放左軌？何時放右軌？）

* **放左軌（核心）**：與業務無關的共用邏輯、跨專案可重用、需要長期維護的能力（components/GEM、純 domain/service）。
* **放右軌（開發）**：場景特定的上下文、策略路由、使用者體驗、部署與營運配置（recipes、interface、app 編排）。

> 簡單記：**能被三個以上專案共用的東西 → 左軌；只在一個專案有意義 → 右軌。**

---

## 9) 小結

* 左軌（核心）負責「**可重用與可驗證的能力**」，以 SRP/SoC/DRY/宣告式組裝為準則，並透過 registry 對外發佈。
* 右軌（開發）負責「**場景化的交付**」，以層級在上、專案在下的方式進行編排，並用 lockfile 鎖定採用版本。
* 兩軌以 **SSOT/Schema/Lockfile/API** 連接，CI 雙軌把關，確保**穩定可演進、專案可自治**。
