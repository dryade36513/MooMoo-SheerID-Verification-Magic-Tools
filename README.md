# 🔐 神秘哞ㄉ SheerID 驗證工具

[![GitHub Stars](https://img.shields.io/github/stars/ThanhNguyxn/SheerID-Verification-Tool?style=social)](https://github.com/ThanhNguyxn/SheerID-Verification-Tool/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Documentation](https://img.shields.io/badge/Docs-Website-2ea44f?style=flat&logo=github&logoColor=white)](https://thanhnguyxn.github.io/SheerID-Verification-Tool/)

一套全面的工具集合，用於自動化各種服務（Spotify、YouTube、Google One 等）的 SheerID 驗證工作流程。

---

## 🛠️ 可用工具

| 工具 | 類型 | 目標 | 描述 |
|------|------|--------|-------------|
| [spotify-verify-tool](./spotify-verify-tool/) | 🎵 學生 | Spotify 高級版 | 大學生身份驗證 |
| [youtube-verify-tool](./youtube-verify-tool/) | 🎬 學生 | YouTube 高級版 | 大學生身份驗證 |
| [one-verify-tool](./one-verify-tool/) | 🤖 學生 | Gemini Advanced | Google One AI 高級版驗證 |
| [boltnew-verify-tool](./boltnew-verify-tool/) | 👨‍🏫 教師 | Bolt.new | 教師身份驗證（大學）|
| [canva-teacher-tool](./canva-teacher-tool/) | 🇬🇧 教師 | Canva Education | 英國教師身份驗證（K-12）|
| [k12-verify-tool](./k12-verify-tool/) | 🏫 K12 | ChatGPT Plus | K12 教師身份驗證（高中）|
| [veterans-verify-tool](./veterans-verify-tool/) | 🎖️ 軍人 | 通用 | 軍人身份驗證 |
| [veterans-extension](./veterans-extension/) | 🧩 Chrome | 瀏覽器 | 軍人身份驗證的 Chrome 擴展 |

### 🔗 外部工具

| 工具 | 類型 | 描述 |
|------|------|-------------|
| [RoxyBrowser](https://roxybrowser.com?code=01045PFA) | 🦊 瀏覽器 | **防檢測瀏覽器** — 安全管理多個驗證帳戶而不被封禁 |
| [Check IP](https://ip123.in/en?code=01045PFA) | 🌐 Web | **檢查 IP** — 檢查您的 IP 地址和代理狀態 |
| [老司機瀏覽器檢查](https://www.browserscan.net/tc) | 🌐 Web | **檢查 IP** — 檢查您的 IP 地址和代理狀態 |
| [SheerID Verification Bot](https://t.me/SheerID_Verification_bot?start=ref_LdPKPES3Ej) | 🤖 Bot | 自動化 Telegram 驗證機器人 |
| [GPT Bot](https://t.me/vgptplusbot?start=ref_7762497789) | 🤖 Bot | 自動化驗證機器人 |
| [Student Card Generator](https://thanhnguyxn.github.io/student-card-generator/) | 🎓 工具 | 為手動驗證建立學生卡 |
| [Payslip Generator](https://thanhnguyxn.github.io/payslip-generator/) | 💰 工具 | 生成教師驗證用的薪資單 |

---

## 🧠 核心架構與邏輯

本存儲庫中的所有 Python 工具共享一個常見的優化架構，旨在實現高成功率。

### 1. 驗證流程
工具遵循標準化的「瀑布」流程：
1.  **數據生成**：創建與目標人群相符的現實身份（名字、出生日期、電子郵件）。
2.  **提交（`collectStudentPersonalInfo`）**：將數據提交至 SheerID API。
3.  **SSO 跳過（`DELETE /step/sso`）**：關鍵步驟。繞過需要登錄學校門戶網站的要求。
4.  **文件上傳（`docUpload`）**：上傳生成的證明文件（學生證、成績單或教師徽章）。
5.  **完成（`completeDocUpload`）**：向 SheerID 發出上傳完成的信號。

### 2. 智能策略

#### 🎓 大學策略（Spotify、YouTube、Gemini）
- **加權選擇**：使用精選的 **45+ 所大學**列表（美國、越南、日本、韓國等）。
- **成功跟蹤**：成功率較高的大學會被更頻繁地選中。
- **文件生成**：生成外觀逼真的學生證，具有動態名字和日期。

#### 👨‍🏫 教師策略（Bolt.new）
- **年齡定位**：生成較老的身份（25-55 歲）以符合教師人口統計。
- **文件生成**：創建「聘用證書」而不是學生證。
- **端點**：定位 `collectTeacherPersonalInfo` 而不是學生端點。

#### 🏫 K12 策略（ChatGPT Plus）
- **學校類型定位**：特別針對類型為 `type: "K12"`（而非 `HIGH_SCHOOL`）的學校。
- **自動通過邏輯**：K12 驗證通常在學校和教師信息相符時自動批准，無需文件上傳。
- **備用方案**：如果需要上傳，它將生成教師徽章。

#### 🎖️ 退伍軍人策略（ChatGPT Plus）
- **嚴格符合條件**：針對現役或在 **過去 12 個月內**退役的退伍軍人。
- **權威檢查**：SheerID 針對 DoD/DEERS 資料庫進行驗證。
- **邏輯**：預設最近的退役日期以最大化自動批准機會。

#### 🛡️ 反檢測模塊
所有工具現在都包含 `anti_detect.py`，提供：
- **隨機用戶代理**：10+ 個真實瀏覽器 UA 字符串（Chrome、Firefox、Edge、Safari）
- **類瀏覽器標頭**：適當的 `sec-ch-ua`、`Accept-Language` 等。
- **TLS 指紋欺騙**：使用 `curl_cffi` 模擬 Chrome 的 JA3/JA4 指紋
- **隨機延遲**：伽馬分佈計時以模擬人類行為
- **智能會話**：自動選擇最佳可用 HTTP 庫（curl_cffi > cloudscraper > httpx > requests）
- **NewRelic 標頭**：SheerID API 呼叫所需的跟蹤標頭
- **會話預熱**：驗證前的請求以建立合法的瀏覽器會話
- **電子郵件生成**：創建與大學域相符的現實學生電子郵件
- **代理地理匹配**：將代理位置與大學國家相匹配以保持一致性
- **多瀏覽器模擬**：在 Chrome、Edge 和 Safari 指紋之間輪換

#### 📄 文件生成模塊
新的 `doc_generator.py` 為生成的文件提供反檢測：
- **噪聲注入**：隨機像素噪聲以避免模板檢測
- **色彩變化**：6 種不同的色彩方案以確保獨特性
- **動態定位**：元素位置的 ±3px 方差
- **多種類型**：學生證、成績單、教師徽章
- **逼真細節**：隨機條形碼、二維碼、課程成績

> [!WARNING]
> **基於 API 的工具具有固有的限制**
>
> SheerID 使用高級檢測，包括：
> - **TLS 指紋識別**：Python `requests`/`httpx` 有可檢測的簽名
> - **信號情報**：IP 地址、設備屬性、電子郵件年齡分析
> - **AI 文件審查**：檢測偽造/模板文件
>
> 為獲得最佳結果：使用 **住宅代理** + 安裝 `curl_cffi` 進行 TLS 欺騙。
> 瀏覽器擴展通常比 API 工具具有更高的成功率。

> [!IMPORTANT]
> **Gemini/Google One 僅限美國（自 2026 年 1 月起）**
>
> `one-verify-tool` 僅適用於美國 IP。國際用戶將看到驗證失敗。

---

## 📋 快速入門

### 先決條件
- Python 3.8+
- `pip`

### 安裝

1.  **克隆存儲庫：**
    ```bash
    git clone https://github.com/dryade36513/MooMoo-SheerID-Verification-Magic-Tools.git
    cd SheerID-Verification-Tool
    ```

2.  **安裝依賴項：**
    ```bash
    pip install httpx Pillow
    ```

3.  **[可選] 增強反檢測：**
    ```bash
    pip install curl_cffi cloudscraper
    ```
    - `curl_cffi`：欺騙 TLS 指紋（JA3/JA4）看起來像真正的 Chrome
    - `cloudscraper`：繞過 Cloudflare 保護

4.  **運行工具（例如 Spotify）：**
    ```bash
    cd spotify-verify-tool
    python main.py "YOUR_SHEERID_URL"
    ```

---

## ⚠️ 免責聲明

本項目僅用於 **教育目的**。這些工具演示了驗證系統的工作原理以及如何對其進行測試。
- 不要將其用於欺詐目的。
- 作者不對任何不當使用負責。
- 尊重所有平台的服務條款。
