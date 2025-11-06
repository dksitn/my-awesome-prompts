# 模組化提示詞管理方法論
(Modular Prompt Management Methodology)

## 1. 導論：問題所在 (Introduction: The Problem)

傳統的提示詞工程（Prompt Engineering）將每個提示詞 (Prompt) 視為一個獨立的、巨大的「巨石型 (Monolithic)」文字檔案。

這種方法在初期很簡單，但很快會導致管理上的災難：
* **難以維護 (Hard to Maintain):** 當您需要更新一個在 10 個不同提示詞中都使用到的「安全規則」時，您必須手動修改 10 個檔案。
* **無法重用 (Impossible to Reuse):** 當您想建立一個新提示詞 C，它需要提示詞 A 的「意圖分析」功能和提示詞 B 的「JSON 輸出」功能時，您唯一的辦法就是「手動複製貼上」。
* **容易出錯 (Error-Prone):** 複製貼上會導致細微的差異、遺漏或版本錯亂。

本方法論提出了一個**系統性的解決方案**，將軟體工程的核心原則應用於提示詞管理，實現您所追求的「**交互功能管理**」。

## 2. 核心原則 (The Core Principles)

本方法論建立在以下八個核心原則之上：

### 原則一：提示詞即程式碼 (Prompts as Code - PaC)
* **定義：** 這是最重要的思維轉變。提示詞**不是**靜態的純文字 (Text)，而是動態的、有功能的「程式碼 (Code)」。
* **實踐：** 它們必須被儲存在 Git 儲存庫中，而不是筆記本或 Google Docs。它們有版本、有 Bug、需要被測試和重構。

### 原則二：模組化與單一職責 (Modularity & Single Responsibility - SRP)
* **定義：** 每個「提示詞元件 (Prompt Component)」檔案，都應該只負責「一項」明確定義的功能，並且把它做好。
* **實踐：** 停止撰寫 500 行的「巨型提示詞」。取而代之的是，撰寫 10 個 50 行的「功能元件」，例如：
    * `fn_analyze_intent.md` (功能 a)
    * `fn_run_code_analysis.md` (功能 b)
    * `fn_format_json_report.md` (功能 j)

### 原則三：關注點分離 (Separation of Concerns - SoC)
* **定義：** 一個運作良好的系統，必須將「資料」、「邏輯」和「組裝」分開。
* **實踐：** 您的儲存庫結構應體現這一點：
    * `🧩 components/`：**元件庫 (The Library)**。存放所有可重用的提示詞「功能積木」。
    * `📜 recipes/`：**組裝配方 (The Build Specs)**。存放 JSON 檔案，*宣告*一個最終的提示詞需要*哪些*元件。
    * `📦 compiled_prompts/`：**編譯成品 (The Final Product)**。存放由「組裝器」自動生成的、可立即使用的「巨石型」提示詞。
    * `🛠️ tools/`：**組裝器 (The Assembler)**。存放 `build_prompt.py` 或 n8n 工作流，負責讀取「配方」並建構「成品」。

### 原則四：組合優於繼承 (Composition over Inheritance)
* **定義：** 這直接回應了您的「交互管理」需求。我們不應該有一個「基礎提示詞」然後去「修改」它（繼承）；我們應該像組裝樂高一樣，把獨立的元件「組合」起來（組合）。
* **實踐：** 當您需要 `{a, b, j}` 功能時，您不是去繼承 A 或 B，而是定義一個新的「配方」`spec_C.json`，其內容為 `["component_a", "component_b", "component_j"]`。

### 原則五：DRY (Don't Repeat Yourself)
* **定義：** 「不要重複你自己」。系統中的任何一項「功能邏輯」都應該只存在於一個、且唯一的一個地方。
* **實踐：** 如果功能 `b` (`fn_run_code_analysis.md`) 被 5 個不同的提示詞使用，它在 `components/` 目錄下仍然只有**一個檔案**。當您需要優化功能 `b` 時，您只需修改這一個檔案，然後重新「編譯」，所有 5 個提示詞就全部自動更新了。

### 原則六：宣告式建構 (Declarative Assembly)
* **定義：** 作為開發者，您應該只關心「*我想要什麼*」(What)，而不是「*我該如何做*」(How)。
* **實踐：** 您的日常工作應該是編輯 `recipes/` 中的 JSON 檔案（例如 `spec_C_abj.json`）——這是在「宣告」您的需求。您不應該去手動執行複製貼上——那是「組裝器」(`tools/build_prompt.py`) 該做的「命令式 (Imperative)」工作。

### 原則七：以 SSOT 為核心的介面 (SSOT-Centric Interfaces)
* **定義：** 元件 `{a}` 和元件 `{b}` 如何溝通？這是模組化最大的挑戰。答案是：它們不應該直接溝通，而是**透過一個共享的狀態資料庫 (SSOT) 進行溝通**。（這完美契合了您先前定義的 R0 角色）。
* **實踐：**
    * 元件 `a` (`fn_analyze_intent.md`) 的職責是分析使用者輸入，並將結果*寫入* R0 (SSOT) 的 `project_spec.intent` 欄位。
    * 元件 `b` (`fn_run_code_analysis.md`) 的職責是從 R0 的 `project_spec.intent` 欄位*讀取*意圖，然後執行相應的分析。
    * 這使得 `a` 和 `b` 成為「高內聚、低耦合」的獨立模組。

### 原則八：可測試性 (Testability)
* **定義：** 一個好的方法論必須能被驗證。
* **實踐：** 模組化讓測試變得更容易：
    * **單元測試 (Unit Test):** 您可以單獨測試 `fn_run_code_analysis.md` (功能 b)，給它一個模擬的 R0 狀態，看它是否能正確輸出分析。
    * **整合測試 (Integration Test):** 您可以測試 `compiled_prompts/prompt_C.md` (由 {a,b,j} 組成)，給它一個真實的使用者輸入，看它是否能跑完*完整*的 {a -> R0 -> b -> R0 -> j} 流程。

## 3. 總結：帶來的好處 (Conclusion: The Benefits)

採用此方法論，將使您的提示詞資產庫從一堆混亂的 `.txt` 檔案，轉變為一個專業的、可擴展的「**功能函式庫 (Function Library)**」。

* **敏捷性 (Agility):** 在幾秒鐘內建立並測試一個新的提示詞 (`{a, j, h}`)。
* **可維護性 (Maintainability):** 一次修改，全局生效。
* **可靠性 (Reliability):** 透過 SSOT 介面和單元測試，降低 Bug 風險。

---
