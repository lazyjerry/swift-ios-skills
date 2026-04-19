# Swift iOS Skills — iOS 26+ 與 SwiftUI 開發的 Agent Skills

[![License: PolyForm Perimeter](https://img.shields.io/badge/License-PolyForm%20Perimeter%201.0.0-blue.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/dpearson2699/swift-ios-skills)](https://github.com/dpearson2699/swift-ios-skills/stargazers)
[![Swift 6.3](https://img.shields.io/badge/Swift-6.3-F05138.svg)](https://swift.org)
[![Platform](https://img.shields.io/badge/Platform-iOS%20%7C%20iPadOS%20%7C%20macOS-000000.svg?logo=apple)](https://developer.apple.com)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-d97757.svg?logo=anthropic)](https://claude.ai/code)
[![OpenAI Codex](https://img.shields.io/badge/OpenAI%20Codex-compatible-10A37F.svg?logo=data:image/svg+xml;base64,PHN2ZyByb2xlPSJpbWciIHZpZXdCb3g9IjAgMCAyNCAyNCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cGF0aCBmaWxsPSJ3aGl0ZSIgZD0iTTIyLjI4MTkgOS44MjExYTUuOTg0NyA1Ljk4NDcgMCAwIDAtLjUxNTctNC45MTA4IDYuMDQ2MiA2LjA0NjIgMCAwIDAtNi41MDk4LTIuOUE2LjA2NTEgNi4wNjUxIDAgMCAwIDQuOTgwNyA0LjE4MThhNS45ODQ3IDUuOTg0NyAwIDAgMC0zLjk5NzcgMi45IDYuMDQ2MiA2LjA0NjIgMCAwIDAgLjc0MjcgNy4wOTY2IDUuOTggNS45OCAwIDAgMCAuNTExIDQuOTEwNyA2LjA1MSA2LjA1MSAwIDAgMCA2LjUxNDYgMi45MDAxQTUuOTg0NyA1Ljk4NDcgMCAwIDAgMTMuMjU5OSAyNGE2LjA1NTcgNi4wNTU3IDAgMCAwIDUuNzcxOC00LjIwNTggNS45ODk0IDUuOTg5NCAwIDAgMCAzLjk5NzctMi45MDAxIDYuMDU1NyA2LjA1NTcgMCAwIDAtLjc0NzUtNy4wNzI5em0tOS4wMjIgMTIuNjA4MWE0LjQ3NTUgNC40NzU1IDAgMCAxLTIuODc2NC0xLjA0MDhsLjE0MTktLjA4MDQgNC43NzgzLTIuNzU4MmEuNzk0OC43OTQ4IDAgMCAwIC4zOTI3LS42ODEzdi02LjczNjlsMi4wMiAxLjE2ODZhLjA3MS4wNzEgMCAwIDEgLjAzOC4wNTJ2NS41ODI2YTQuNTA0IDQuNTA0IDAgMCAxLTQuNDk0NSA0LjQ5NDR6bS05LjY2MDctNC4xMjU0YTQuNDcwOCA0LjQ3MDggMCAwIDEtLjUzNDYtMy4wMTM3bC4xNDIuMDg1MiA0Ljc4MyAyLjc1ODJhLjc3MTIuNzcxMiAwIDAgMCAuNzgwNiAwbDUuODQyOC0zLjM2ODV2Mi4zMzI0YS4wODA0LjA4MDQgMCAwIDEtLjAzMzIuMDYxNUw5Ljc0IDE5Ljk1MDJhNC40OTkyIDQuNDk5MiAwIDAgMS02LjE0MDgtMS42NDY0ek0yLjM0MDggNy44OTU2YTQuNDg1IDQuNDg1IDAgMCAxIDIuMzY1NS0xLjk3MjhWMTEuNmEuNzY2NC43NjY0IDAgMCAwIC4zODc5LjY3NjVsNS44MTQ0IDMuMzU0My0yLjAyMDEgMS4xNjg1YS4wNzU3LjA3NTcgMCAwIDEtLjA3MSAwbC00LjgzMDMtMi43ODY1QTQuNTA0IDQuNTA0IDAgMCAxIDIuMzQwOCA3Ljg3MnptMTYuNTk2MyAzLjg1NThMMTMuMTAzOCA4LjM2NCAxNS4xMTkyIDcuMmEuMDc1Ny4wNzU3IDAgMCAxIC4wNzEgMGw0LjgzMDMgMi43OTEzYTQuNDk0NCA0LjQ5NDQgMCAwIDEtLjY3NjUgOC4xMDQydi01LjY3NzJhLjc5Ljc5IDAgMCAwLS40MDctLjY2N3ptMi4wMTA3LTMuMDIzMWwtLjE0Mi0uMDg1Mi00Ljc3MzUtMi43ODE4YS43NzU5Ljc3NTkgMCAwIDAtLjc4NTQgMEw5LjQwOSA5LjIyOTdWNi44OTc0YS4wNjYyLjA2NjIgMCAwIDEgLjAyODQtLjA2MTVsNC44MzAzLTIuNzg2NmE0LjQ5OTIgNC40OTkyIDAgMCAxIDYuNjgwMiA0LjY2ek04LjMwNjUgMTIuODYzbC0yLjAyLTEuMTYzOGEuMDgwNC4wODA0IDAgMCAxLS4wMzgtLjA1NjdWNi4wNzQyYTQuNDk5MiA0LjQ5OTIgMCAwIDEgNy4zNzU3LTMuNDUzN2wtLjE0Mi4wODA1TDguNzA0IDUuNDU5YS43OTQ4Ljc5NDggMCAwIDAtLjM5MjcuNjgxM3ptMS4wOTc2LTIuMzY1NGwyLjYwMi0xLjQ5OTggMi42MDY5IDEuNDk5OHYyLjk5OTRsLTIuNTk3NCAxLjQ5OTctMi42MDY3LTEuNDk5N1oiLz48L3N2Zz4=)](https://developers.openai.com/codex)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-standard-green.svg?logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMzIiIGhlaWdodD0iMzIiIHZpZXdCb3g9IjAgMCAzMiAzMiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cGF0aCBmaWxsLXJ1bGU9ImV2ZW5vZGQiIGNsaXAtcnVsZT0iZXZlbm9kZCIgZD0iTTE2IDAuNUwyOS40MjM0IDguMjVWMjMuNzVMMTYgMzEuNUwyLjU3NjYxIDIzLjc1VjguMjVMMTYgMC41WiBNMTYgNUwyNS41MjYzIDEwLjVWMjEuNUwxNiAyN0w2LjQ3MzcyIDIxLjVWMTAuNUwxNiA1WiIgZmlsbD0id2hpdGUiLz48L3N2Zz4K)](https://agentskills.io)

針對 **iOS 26+** 開發最佳化的 76 個 agent skills，採用 Swift 6.3 及現代 Apple 框架。所有程式碼範例、Pattern（模式）與說明均以最新 API 為目標——Liquid Glass、簡化版並行處理（approachable concurrency）、Foundation Models、StoreKit 2、SwiftData、async/await URLSession 等。不含任何已棄用的 Pattern。

相容於 [Claude Code](https://claude.ai/code)、[OpenAI Codex](https://developers.openai.com/codex)、[Cursor](https://cursor.com)、[GitHub Copilot](https://github.com/features/copilot) 及 [40+ 其他 agent](https://skills.sh)。遵循開放的 [Agent Skills](https://agentskills.io) 標準。

每個 skill 皆完全獨立，互不依賴。按需安裝即可。

## 目錄

- [安裝](#安裝)
  - [推薦：透過 skills CLI 支援任何 agent](#推薦透過-skills-cli-支援任何-agent)
  - [Claude Code](#claude-code透過-plugin-marketplace)
  - [OpenAI Codex](#openai-codex)
  - [Claude Web App / Claude Desktop](#claude-web-app--claude-desktop)
  - [ChatGPT](#chatgpt)
- [Plugin 套件](#plugin-套件claude-code)
- [Skills 清單](#skills-清單)
  - [SwiftUI](#swiftui)
  - [Core Swift](#core-swift)
  - [App Experience 框架](#app-experience-框架)
  - [資料與服務框架](#資料與服務框架)
  - [AI 與機器學習](#ai-與機器學習)
  - [iOS 工程](#ios-工程)
  - [硬體與裝置整合](#硬體與裝置整合)
  - [平台整合](#平台整合)
  - [遊戲](#遊戲)
- [結構](#結構)
- [相容性](#相容性)
- [從 v2.x 升級](#從-v2x-升級)
- [支援](#支援)
- [授權條款](#授權條款)

## 安裝

### 推薦：透過 [skills CLI](https://github.com/vercel-labs/skills) 支援任何 agent

skills CLI 是推薦的安裝方式。

互動式安裝（推薦）：

```sh
npx skills add dpearson2699/swift-ios-skills
```

執行預設指令會開啟 skills CLI UI，讓你選擇要安裝哪些 skills，以及要安裝給哪些 agent。

為任何 coding agent 安裝全部內容：

```sh
npx skills add dpearson2699/swift-ios-skills --all
```

當你想自動安裝全部 76 個 skills 給任何 coding agent 時，使用 `--all`。

直接安裝指定的 skills：

```sh
npx skills add dpearson2699/swift-ios-skills --skill <skill-name> --skill <skill-name>
```

檢查已安裝 skills 的更新：

```sh
npx skills check
```

將已安裝的 skills 更新至最新版本：

```sh
npx skills update
```

透過 skills CLI 安裝後使用上述指令。

### Claude Code（透過 plugin marketplace）

新增 marketplace（一次性）：

```sh
/plugin marketplace add dpearson2699/swift-ios-skills
```

安裝全部：

```sh
/plugin install all-ios-skills@swift-ios-skills
```

或安裝主題式套件（套件可限制載入到 context window 的 skills 數量——若想要全部，請使用上方的 `all-ios-skills`，而非安裝多個套件）：

```sh
/plugin install swiftui-skills@swift-ios-skills
/plugin install swift-core-skills@swift-ios-skills
/plugin install ios-app-framework-skills@swift-ios-skills
/plugin install ios-data-framework-skills@swift-ios-skills
/plugin install ios-ai-ml-skills@swift-ios-skills
/plugin install ios-engineering-skills@swift-ios-skills
/plugin install ios-hardware-skills@swift-ios-skills
/plugin install ios-platform-skills@swift-ios-skills
/plugin install ios-gaming-skills@swift-ios-skills
/plugin install apple-kit-skills@swift-ios-skills
```

### OpenAI Codex

```sh
$skill-installer install https://github.com/dpearson2699/swift-ios-skills/tree/main/skills/<skill-name>
```

### Claude Web App / Claude Desktop

1. 從此 repo 下載你想要的 skill 資料夾
2. 將每個 skill 資料夾壓縮成 zip
3. 前往 **Settings > Capabilities**，啟用「Code execution and file creation」
4. 前往 **Customize > Skills**，點擊 **+**，再點 **Upload a skill**
5. 上傳 zip 檔案

### ChatGPT

1. 從此 repo 下載你想要的 skill 資料夾
2. 將每個 skill 資料夾壓縮成 zip
3. 在 ChatGPT 點擊個人頭像，選擇 **Skills**
4. 點擊 **New skill**，選擇 **Upload from your computer**
5. 上傳 zip 檔案

## Plugin 套件（Claude Code）

| Plugin | 包含的 skills |
|--------|----------------|
| **all-ios-skills** | 全部 76 個 skills |
| **apple-kit-skills** | 39 個 Apple Kit 框架 skills 及 CarPlay |
| **swiftui-skills** | swiftui-animation, swiftui-gestures, swiftui-layout-components, swiftui-liquid-glass, swiftui-navigation, swiftui-patterns, swiftui-performance, swiftui-uikit-interop, swiftui-webkit |
| **swift-core-skills** | swift-codable, swift-charts, swift-concurrency, swift-language, swift-testing, swiftdata |
| **ios-app-framework-skills** | activitykit, adattributionkit, alarmkit, app-clips, app-intents, avkit, carplay, mapkit, paperkit, pdfkit, photokit, push-notifications, storekit, tipkit, widgetkit |
| **ios-data-framework-skills** | cloudkit, contacts-framework, eventkit, financekit, healthkit, musickit, passkit, weatherkit |
| **ios-ai-ml-skills** | apple-on-device-ai, coreml, natural-language, speech-recognition, vision-framework |
| **ios-engineering-skills** | app-store-review, authentication, background-processing, cryptokit, debugging-instruments, device-integrity, ios-accessibility, ios-localization, ios-networking, ios-security, metrickit |
| **ios-hardware-skills** | accessorysetupkit, core-bluetooth, core-motion, core-nfc, dockkit, pencilkit, realitykit, sensorkit |
| **ios-platform-skills** | appmigrationkit, audioaccessorykit, browserenginekit, callkit, cryptotokenkit, energykit, homekit, permissionkit, relevancekit, shareplay-activities |
| **ios-gaming-skills** | gamekit, scenekit, spritekit, tabletopkit |

## Skills 清單

### SwiftUI

| Skill | 涵蓋內容 |
|-------|---------------|
| [swiftui-animation](skills/swiftui-animation/) | 彈簧動畫、PhaseAnimator、KeyframeAnimator、matchedGeometryEffect、SF Symbols |
| [swiftui-gestures](skills/swiftui-gestures/) | 點擊、拖曳、縮放、旋轉、長按、同步與序列手勢 |
| [swiftui-layout-components](skills/swiftui-layout-components/) | Grid、LazyVGrid、Layout 協定、ViewThatFits、自訂排版 |
| [swiftui-liquid-glass](skills/swiftui-liquid-glass/) | iOS 26 Liquid Glass、glassEffect、GlassEffectContainer、形變轉場 |
| [swiftui-navigation](skills/swiftui-navigation/) | NavigationStack、NavigationSplitView、程式化導航、深層連結（deep linking） |
| [swiftui-patterns](skills/swiftui-patterns/) | @Observable、NavigationStack、視圖組合、sheets、TabView、MV-pattern 架構 |
| [swiftui-performance](skills/swiftui-performance/) | 渲染效能、視圖更新最佳化、排版抖動、Instruments 效能分析 |
| [swiftui-uikit-interop](skills/swiftui-uikit-interop/) | UIViewRepresentable、UIHostingController、Coordinator、漸進式 UIKit 到 SwiftUI 遷移 |
| [swiftui-webkit](skills/swiftui-webkit/) | WebView、WebPage、導航策略、JavaScript 呼叫、本機內容、自訂 URL scheme |

### Core Swift

| Skill | 涵蓋內容 |
|-------|---------------|
| [swift-codable](skills/swift-codable/) | Swift Codable、JSONDecoder、JSONEncoder、CodingKeys、自訂解碼、巢狀 JSON |
| [swift-charts](skills/swift-charts/) | 長條圖、折線圖、面積圖、圓餅圖、甜甜圈圖、滾動、選取、標註 |
| [swift-concurrency](skills/swift-concurrency/) | Swift 6.2 並行處理、Sendable、actor、結構化並行、資料競爭安全 |
| [swift-language](skills/swift-language/) | Swift 6.3 特性、macro、result builder、property wrapper |
| [swift-testing](skills/swift-testing/) | Swift Testing 框架、@Test、@Suite、#expect、參數化測試、mock |
| [swiftdata](skills/swiftdata/) | @Model、@Query、#Predicate、ModelContainer、資料遷移、CloudKit 同步、@ModelActor |

### App Experience 框架

| Skill | 涵蓋內容 |
|-------|---------------|
| [activitykit](skills/activitykit/) | ActivityKit、Dynamic Island、鎖定畫面 Live Activities、push-to-update |
| [adattributionkit](skills/adattributionkit/) | 隱私保護廣告歸因、回傳資料（postback）、轉換值、再互動 |
| [alarmkit](skills/alarmkit/) | AlarmKit 系統鬧鐘與倒數計時器、鎖定畫面、Dynamic Island、Live Activities |
| [app-clips](skills/app-clips/) | App Clips、呼叫 URL、NFC、QR、App Clip Codes、App Group 交接 |
| [app-intents](skills/app-intents/) | App Intents 用於 Siri、Shortcuts、Spotlight、widget 及 Apple Intelligence |
| [avkit](skills/avkit/) | AVPlayerViewController、VideoPlayer、子母畫面（Picture-in-Picture）、AirPlay、字幕 |
| [carplay](skills/carplay/) | CarPlay 模板、導航、音訊、通訊、電動車充電 app |
| [mapkit](skills/mapkit/) | MapKit、CoreLocation、標註、地理編碼、路線規劃、地理圍欄 |
| [paperkit](skills/paperkit/) | PaperMarkupViewController、標記編輯、繪圖、圖形（iOS 26） |
| [pdfkit](skills/pdfkit/) | PDFView、PDFDocument、標註、文字搜尋、表單填寫、縮圖 |
| [photokit](skills/photokit/) | PhotosPicker、AVCaptureSession、相片圖庫、影片錄製、媒體權限 |
| [push-notifications](skills/push-notifications/) | UNUserNotificationCenter、APNs、豐富通知、靜默推播、service extension |
| [storekit](skills/storekit/) | StoreKit 2 購買、訂閱、SubscriptionStoreView、交易驗證 |
| [tipkit](skills/tipkit/) | 功能探索提示、情境式提示、提示規則、提示事件 |
| [widgetkit](skills/widgetkit/) | 主畫面、鎖定畫面與 StandBy widget、控制中心控制項、時間軸提供者 |

### 資料與服務框架

| Skill | 涵蓋內容 |
|-------|---------------|
| [cloudkit](skills/cloudkit/) | CKContainer、CKRecord、訂閱、共享、CKSyncEngine、SwiftData 同步 |
| [contacts-framework](skills/contacts-framework/) | CNContactStore、擷取請求、key descriptor、CNContactPickerViewController、儲存請求 |
| [eventkit](skills/eventkit/) | EKEventStore、EKEvent、EKReminder、週期規則、EventKitUI 編輯器與選擇器 |
| [financekit](skills/financekit/) | Apple Card、Apple Cash、Wallet 訂單、交易查詢、帳戶餘額 |
| [healthkit](skills/healthkit/) | HKHealthStore、查詢、統計資料、健身工作階段、背景傳遞 |
| [musickit](skills/musickit/) | MusicKit 授權、目錄搜尋、ApplicationMusicPlayer、MPRemoteCommandCenter |
| [passkit](skills/passkit/) | Apple Pay、PKPaymentRequest、PKPaymentAuthorizationController、Wallet pass |
| [weatherkit](skills/weatherkit/) | WeatherService、當前/每小時/每日天氣預報、警報、署名要求 |

### AI 與機器學習

| Skill | 涵蓋內容 |
|-------|---------------|
| [apple-on-device-ai](skills/apple-on-device-ai/) | Foundation Models 框架、Core ML、MLX Swift、裝置端 LLM 推論 |
| [coreml](skills/coreml/) | Core ML 模型載入、預測、MLTensor、運算單元設定、VNCoreMLRequest、MLComputePlan |
| [natural-language](skills/natural-language/) | NLTokenizer、NLTagger、情感分析、語言識別、詞嵌入（embedding）、翻譯 |
| [speech-recognition](skills/speech-recognition/) | SFSpeechRecognizer、裝置端識別、音訊緩衝處理 |
| [vision-framework](skills/vision-framework/) | Vision 文字識別、人臉/條碼偵測、影像分割、VisionKit DataScannerViewController |

### iOS 工程

| Skill | 涵蓋內容 |
|-------|---------------|
| [app-store-review](skills/app-store-review/) | App Review 準則、預防被拒絕、隱私清單（privacy manifest）、ATT、HIG 合規 |
| [authentication](skills/authentication/) | Sign in with Apple、ASAuthorizationController、passkey、生物辨識（LAContext）、憑證管理 |
| [background-processing](skills/background-processing/) | BGTaskScheduler、背景更新、URLSession 背景傳輸 |
| [cryptokit](skills/cryptokit/) | SHA256、HMAC、AES-GCM、ChaChaPoly、P256/Curve25519 簽章、ECDH、Secure Enclave |
| [debugging-instruments](skills/debugging-instruments/) | Xcode 除錯器、Instruments、os_signpost、MetricKit、崩潰符號化 |
| [device-integrity](skills/device-integrity/) | DeviceCheck（DCDevice 每裝置位元）、App Attest（DCAppAttestService 認證與斷言流程） |
| [ios-accessibility](skills/ios-accessibility/) | VoiceOver、Dynamic Type、自訂旋轉選單（rotor）、無障礙焦點、輔助技術支援 |
| [ios-localization](skills/ios-localization/) | String Catalog、複數化、FormatStyle、從右至左排版 |
| [ios-networking](skills/ios-networking/) | URLSession async/await、REST API、下載/上傳、WebSocket、分頁、重試、快取 |
| [ios-security](skills/ios-security/) | Keychain、Secure Enclave、ATS、憑證 pinning、資料保護、生物辨識 Keychain 存取 |
| [metrickit](skills/metrickit/) | MXMetricManager、卡頓診斷、崩潰報告、電力指標 |

### 硬體與裝置整合

| Skill | 涵蓋內容 |
|-------|---------------|
| [accessorysetupkit](skills/accessorysetupkit/) | 隱私保護的 BLE/Wi-Fi 配件探索、ASAccessorySession、選擇器 UI |
| [core-bluetooth](skills/core-bluetooth/) | CBCentralManager、CBPeripheral、BLE 掃描/連接、服務、特性（characteristic）、背景模式 |
| [core-motion](skills/core-motion/) | CMMotionManager、CMPedometer、加速度計、陀螺儀、活動識別、高度 |
| [core-nfc](skills/core-nfc/) | NFCNDEFReaderSession、NFCTagReaderSession、NDEF 讀取/寫入、背景標籤讀取 |
| [dockkit](skills/dockkit/) | DockAccessoryManager、相機主體追蹤、馬達控制、取景框定 |
| [pencilkit](skills/pencilkit/) | PKCanvasView、PKDrawing、PKToolPicker、Apple Pencil 繪圖與標記 |
| [realitykit](skills/realitykit/) | RealityView、entity、anchor、ARKit 世界追蹤、光線投射、場景理解 |
| [sensorkit](skills/sensorkit/) | 研究級感測器資料、環境光、鍵盤指標、裝置使用情況（需核准的研究） |

### 平台整合

| Skill | 涵蓋內容 |
|-------|---------------|
| [appmigrationkit](skills/appmigrationkit/) | 跨平台資料傳輸、MigrationController、匯出/匯入 extension（iOS 26） |
| [audioaccessorykit](skills/audioaccessorykit/) | 音訊配件功能、自動切換、裝置放置（iOS 26.4） |
| [browserenginekit](skills/browserenginekit/) | 替代瀏覽器引擎（歐盟）、行程管理、網頁內容 extension |
| [callkit](skills/callkit/) | CXProvider、CXCallController、PushKit VoIP 註冊、通話目錄 extension |
| [cryptotokenkit](skills/cryptotokenkit/) | TKTokenDriver、TKSmartCard、安全 token、憑證式驗證 |
| [energykit](skills/energykit/) | ElectricityGuidance、EnergyVenue、電網預測、負載事件提交、電力洞察 |
| [homekit](skills/homekit/) | HMHomeManager、配件、房間、動作、觸發器、MatterSupport 入網 |
| [permissionkit](skills/permissionkit/) | AskCenter、PermissionQuestion、子女通訊安全、CommunicationLimits |
| [relevancekit](skills/relevancekit/) | Widget 相關性信號、時間/位置相關性提供者（watchOS 26） |
| [shareplay-activities](skills/shareplay-activities/) | GroupActivity、GroupSession、GroupSessionMessenger、協同媒體播放 |

### 遊戲

| Skill | 涵蓋內容 |
|-------|---------------|
| [gamekit](skills/gamekit/) | Game Center、GKLocalPlayer、排行榜、成就、即時與回合制多人遊戲 |
| [scenekit](skills/scenekit/) | SCNView、SCNScene、3D 幾何、材質、光源、物理、SceneView |
| [spritekit](skills/spritekit/) | SKScene、SKSpriteNode、SKAction、物理模擬、粒子效果、SpriteView |
| [tabletopkit](skills/tabletopkit/) | 多人空間桌遊、棋子、卡牌、骰子、Group Activities（visionOS） |

## 結構

每個 skill 均遵循開放的 [Agent Skills](https://agentskills.io) 標準：

```
skills/
  skill-name/
    SKILL.md              # 必要 — 說明與元資料
    references/           # 選用 — 詳細參考資料
      some-topic.md
```

`SKILL.md` 包含 YAML frontmatter（`name`、`description`）及 Markdown 說明。`references/` 資料夾存放主檔案所指向的較長範例、進階 Pattern 及查閱表。

本 repo 包含 Apple 平台開發的原創教學內容與範例。凡涉及 Apple 框架、API、文件、WWDC session 或商標，相關素材仍歸 Apple Inc. 所有。本 repo 的授權僅適用於本專案的原創內容，不主張擁有或重新授權 Apple 的文件、商標、範例程式碼或其他第三方素材。

## 相容性

這些 skills 可與任何支援 [Agent Skills 標準](https://agentskills.io) 的 agent 搭配使用，包括：

- [Claude Code](https://claude.ai/code)（Anthropic）
- [OpenAI Codex](https://developers.openai.com/codex)
- [Cursor](https://cursor.com)
- [GitHub Copilot](https://github.com/features/copilot)
- [Windsurf](https://codeium.com/windsurf)
- [Roo Code](https://roocode.com)
- 及[更多](https://skills.sh)

## 從 v2.x 升級

v3.0 為重大版本更新。若先前已安裝 v2.x skills，請注意以下變更：

- **Skill 數量**：v2.2.0 有 57 個，v3.0.0 增至 76 個。
- **Skill 更名**：12 個既有 skill 已更名為 Apple Kit 框架名稱。舊路徑不再有效。請先解除安裝全部 skills 再重新安裝以完成升級。

  | v2.x 名稱 | v3.0 名稱 |
  |-----------|-----------|
  | `live-activities` | `activitykit` |
  | `mapkit-location` | `mapkit` |
  | `photos-camera-media` | `photokit` |
  | `homekit-matter` | `homekit` |
  | `callkit-voip` | `callkit` |
  | `metrickit-diagnostics` | `metrickit` |
  | `pencilkit-drawing` | `pencilkit` |
  | `passkit-wallet` | `passkit` |
  | `musickit-audio` | `musickit` |
  | `cloudkit-sync` | `cloudkit` |
  | `eventkit-calendar` | `eventkit` |
  | `realitykit-ar` | `realitykit` |
- **19 個全新 Kit 框架 skills**：avkit, gamekit, cryptokit, pdfkit, paperkit, spritekit, scenekit, financekit, accessorysetupkit, adattributionkit, carplay, appmigrationkit, browserenginekit, dockkit, sensorkit, tabletopkit, relevancekit, audioaccessorykit, cryptotokenkit。
- **新套件**：`apple-kit-skills`（全部 39 個 Apple Kit 框架 skills）及 `ios-gaming-skills`（GameKit、SpriteKit、SceneKit、TabletopKit）。
- **PaperKit 獨立成 skill**：PaperKit 內容已從 `pencilkit` 移除，單獨成為 `paperkit` skill。
- **Beta 框架**：`permissionkit`、`energykit`、`paperkit`、`relevancekit`、`appmigrationkit` 及 `audioaccessorykit` 需要 iOS/watchOS 26 beta，API 在正式版前可能異動。
- **所有 skills 仍完全獨立**：skill 之間不互相引用或依賴。

透過 skills CLI 升級：

```sh
npx skills add dpearson2699/swift-ios-skills
```

若要升級 Claude Code 套件，請重新安裝你所使用的套件（舊 skill 路徑不再有效）。

## 支援

若這些 skills 為你節省了時間或改善了工作流程，歡迎透過 [GitHub Sponsors](https://github.com/sponsors/dpearson2699) 支持持續維護。

您的支持有助於隨著 Apple 新版本發布、框架 API 演進、範例更新，以及在 Claude Code、Codex、Cursor、Copilot 等 agent 的相容性維護上持續投入。

## 授權條款

[PolyForm Perimeter 1.0.0](https://polyformproject.org/licenses/perimeter/1.0.0/) — 詳見 [LICENSE](LICENSE)

**實際意義：**

- 使用這些 skills 開發你的 iOS app — 允許
- 在閉源商業工作流程中使用這些 skills — 允許
- Fork 此 repo 並貢獻回饋 — 允許
- 與隊友分享這些 skills — 允許
- 取用這些 skills、重新品牌包裝為「Premium iOS Agent Skills」後販售 — **不允許**（此為競爭產品行為）

本專案與 Apple Inc. 無任何關聯，未獲 Apple Inc. 背書或贊助。
