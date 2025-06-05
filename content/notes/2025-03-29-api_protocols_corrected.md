+++
date = '2025-03-29'
draft = false
title = 'API協定'
categories = ['筆記']
tags = ['protocol']
+++

# REST (Representational State Transfer) 

- **REST 設計原則（Roy Fielding 6 個約束）**：
  * **資源識別**：每個資源有唯一 URI
  * **統一介面**：HTTP 方法對應 CRUD 操作
  * **無狀態**：每個請求包含所有必要資訊
  * **可快取性**：回應應明確指出是否可快取
  * **分層系統**：客戶端無法區分是否直接連接到終端伺服器
  * **按需程式碼**：（可選）伺服器可傳送可執行程式碼
- **REST 成熟度模型（Richardson Maturity Model）**：
  * **Level 0**：HTTP 作為傳輸機制
  * **Level 1**：資源導向設計
  * **Level 2**：正確使用 HTTP 動詞和狀態碼
  * **Level 3**：HATEOAS（超媒體控制）
- **RESTful 資源命名規範**：
  * 使用名詞而非動詞
  * 複數形式表示集合（/users）
  * 層次關係表達（/users/123/orders）
  * 小寫字母和連字符（kebab-case）
  * 避免深層嵌套（建議不超過 3 層）
- **HTTP 方法對應與狀態碼**：
  * **GET** → 讀取 (Read) → 200 OK, 404 Not Found
  * **POST** → 創建 (Create) → 201 Created, 400 Bad Request
  * **PUT** → 完全更新 (Update) → 200 OK, 204 No Content
  * **PATCH** → 部分更新 (Partial Update) → 200 OK, 204 No Content
  * **DELETE** → 刪除 (Delete) → 204 No Content, 404 Not Found
- **內容協商（Content Negotiation）**：
  * **Accept 標頭**：客戶端指定期望的回應格式
  * **Content-Type 標頭**：指定請求內容格式
  * **多格式支援**：JSON, XML, HTML 等
- **分頁機制**：
  * **Offset/Limit**：?offset=20&limit=10
  * **Page/Size**：?page=3&size=10
  * **Cursor-based**：?cursor=xyz&limit=10
  * **Link 標頭**：提供上一頁、下一頁連結
- **HATEOAS（Hypermedia as the Engine of Application State）**：
  * 超媒體作為應用狀態引擎
  * 回應中包含可用操作連結
  * 動態發現 API 功能
  * HAL (Hypertext Application Language) 格式
- **版本控制策略**：
  * **URI 版本**：/api/v1/users
  * **標頭版本**：Accept: application/vnd.company.v1+json
  * **查詢參數版本**：?version=1
  * **媒體類型版本**：application/vnd.api+json;version=1
- **錯誤處理最佳實踐**：
  * **統一錯誤格式**：包含錯誤碼、訊息、詳情
  * **Problem Details (RFC 7807)**：標準化錯誤回應
  * **適當的 HTTP 狀態碼**：4xx 客戶端錯誤，5xx 伺服器錯誤

# GraphQL 

- **核心概念**：
  * **查詢（Query）**：獲取資料，類似 SQL SELECT
  * **變更（Mutation）**：修改資料，類似 SQL INSERT/UPDATE/DELETE
  * **訂閱（Subscription）**：實時資料更新，WebSocket 基礎
- **Schema 定義語言（SDL）**：
  * **類型（Type）**：定義資料結構，包含 Scalar、Object、Interface、Union
  * **解析器（Resolver）**：處理資料獲取邏輯的函數
  * **指令（Directive）**：修改查詢執行行為，如 @deprecated、@include
- **查詢語言特性**：
  * **字段選擇**：只獲取需要的字段，避免 over-fetching
  * **別名（Alias）**：同時獲取同一資源的不同表示
  * **片段（Fragment）**：重用字段選擇集，提高可維護性
  * **變量（Variables）**：參數化查詢，提高安全性
  * **內聯片段**：條件字段選擇，處理聯合類型
- **執行引擎**：
  * **解析（Parse）**：將查詢字串轉換為 AST
  * **驗證（Validate）**：檢查查詢語法和 Schema 合規性
  * **執行（Execute）**：調用 Resolver 獲取資料
  * **回應（Response）**：格式化結果為 JSON
- **快取策略**：
  * **查詢級快取**：整個查詢結果快取
  * **字段級快取**：個別字段結果快取
  * **DataLoader 模式**：批次處理和快取，解決 N+1 問題
- **安全考量**：
  * **查詢深度限制**：防止深層嵌套查詢攻擊
  * **查詢複雜度分析**：限制查詢的運算成本
  * **速率限制**：基於查詢複雜度的限流
  * **白名單查詢**：只允許預定義的查詢
- **與 REST 對比**：
  * **單一端點** vs REST 多端點
  * **按需獲取** vs REST 過度/不足獲取
  * **強類型系統** vs REST 通常無明確類型
  * **內省（Introspection）**：可查詢 Schema 信息
  * **學習曲線**：GraphQL 較陡峭
- **工具生態系統**：
  * **Apollo**：全端 GraphQL 平台
  * **Relay**：Facebook 的 GraphQL 客戶端
  * **GraphiQL**：互動式查詢 IDE
  * **Prisma**：資料庫工具和 ORM

# gRPC (Google Remote Procedure Call) 

- **Protocol Buffers (protobuf)**：
  * 高效二進制序列化格式，比 JSON 更小更快
  * 結構定義語言（.proto 文件），類型安全
  * 跨語言代碼生成，支援 10+ 程式語言
  * 向前/向後相容性，Schema 演進
- **通訊模式（四種 RPC 類型）**：
  * **一元 RPC（Unary）**：傳統請求-回應模式
  * **伺服器串流（Server Streaming）**：單一請求，多個回應
  * **客戶端串流（Client Streaming）**：多個請求，單一回應
  * **雙向串流（Bidirectional Streaming）**：多個請求，多個回應
- **基於 HTTP/2 的優勢**：
  * **多路復用**：單一連接處理多個請求
  * **頭部壓縮**：HPACK 壓縮減少開銷
  * **流量控制**：防止快速發送方壓垮接收方
  * **伺服器推送**：主動推送相關資源
- **內建功能與特性**：
  * **認證機制**：TLS、Token-based、OAuth 2.0
  * **負載均衡**：客戶端和伺服器端負載均衡
  * **健康檢查**：服務健康狀態監控
  * **截止時間（Deadline）**：請求超時控制
  * **重試策略**：自動重試失敗的請求
- **攔截器（Interceptors）**：
  * **一元攔截器**：處理一元 RPC 的中介軟體
  * **串流攔截器**：處理串流 RPC 的中介軟體
  * **應用場景**：日誌記錄、認證、監控、錯誤處理
- **錯誤處理**：
  * **gRPC 狀態碼**：16 個標準狀態碼（OK, CANCELLED, INVALID_ARGUMENT 等）
  * **錯誤詳情**：Google.Rpc.Status 提供結構化錯誤資訊
  * **重試機制**：基於狀態碼的自動重試
- **服務發現與反射**：
  * **反射服務**：動態查詢服務和方法定義
  * **健康檢查服務**：標準化健康檢查協定
  * **服務註冊**：與 Consul、etcd 等服務發現整合
- **與 REST 比較**：
  * **效能**：protobuf 序列化比 JSON 快 3-10 倍
  * **類型安全**：編譯時檢查 vs 執行時錯誤
  * **開發體驗**：需要 .proto 定義 vs 直接 HTTP 呼叫
  * **瀏覽器支援**：需要 gRPC-Web vs 原生支援
  * **除錯**：需要專用工具 vs 通用 HTTP 工具
- **實際應用場景**：
  * **微服務間通信**：高效能內部 API
  * **即時系統**：串流處理和即時資料
  * **移動應用**：減少電池消耗和網路使用
  * **IoT 設備**：低功耗、高效率通信