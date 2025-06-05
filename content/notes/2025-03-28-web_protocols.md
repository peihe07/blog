+++
date = '2025-03-28'
draft = false
title = 'Web協定'
categories = ['筆記']
tags = ['protocol']
+++

# 訊息傳遞協定

# HTTP/HTTPS 

- **HTTP 方法**：
  * **GET**：獲取資源，不改變伺服器狀態
  * **POST**：創建資源，提交資料
  * **PUT**：完全替換資源
  * **PATCH**：部分更新資源
  * **DELETE**：刪除資源
  * **HEAD**：僅獲取標頭資訊
  * **OPTIONS**：查詢支援的方法
- **狀態碼分類**：
  * **1xx**：資訊性回應 (100 Continue, 101 Switching Protocols)
  * **2xx**：成功（200 OK, 201 Created, 204 No Content）
  * **3xx**：重定向（301 Moved Permanently, 302 Found, 304 Not Modified）
  * **4xx**：客戶端錯誤（400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found）
  * **5xx**：伺服器錯誤（500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable）
- **標頭分類**：
  * **通用標頭**：Date, Connection, Cache-Control
  * **請求標頭**：Host, User-Agent, Accept, Authorization
  * **回應標頭**：Server, Set-Cookie, Content-Type
  * **實體標頭**：Content-Length, Content-Encoding
- **常見 Content-Type**：
  * **text/html**：HTML 文檔
  * **application/json**：JSON 資料
  * **application/xml**：XML 資料
  * **multipart/form-data**：檔案上傳
  * **application/x-www-form-urlencoded**：表單資料
- **HTTP 版本演進**：
  * **HTTP/1.0**：每次請求建立新連接
  * **HTTP/1.1**：持久連接，管道化請求，Host 標頭
  * **HTTP/2**：多路復用，標頭壓縮，伺服器推送，二進制分幀
  * **HTTP/3**：基於 QUIC 協議，減少建立連接延遲，改善移動網路表現
- **快取控制機制**：
  * **ETag**：資源版本標識，強驗證器
  * **Cache-Control**：max-age, no-cache, no-store, public, private
  * **Last-Modified**：資源最後修改時間，弱驗證器
  * **If-Modified-Since**：條件請求，配合 Last-Modified
  * **If-None-Match**：條件請求，配合 ETag
- **HTTPS 安全機制**：
  * **TLS/SSL 加密**：保護資料傳輸
  * **證書驗證**：確認伺服器身份
  * **HSTS**：強制 HTTPS 連接
  * **混合內容政策**：防止 HTTP 資源洩露
- **安全與跨域**：
  * **CORS (Cross-Origin Resource Sharing)**：跨來源資源共享
  * **Cookie 安全屬性**：Secure, HttpOnly, SameSite
  * **CSP (Content Security Policy)**：內容安全政策
  * **CSRF 防護**：跨站請求偽造防護

# WebSocket 

- **協議特性**：
  * **URL 格式**：ws:// (非加密) 和 wss:// (TLS 加密)
  * **預設端口**：80 (ws://) 和 443 (wss://)
  * **基於 TCP**：可靠的雙向通信
- **建立連接 (WebSocket Handshake)**：
  1. 客戶端發送帶 `Upgrade: websocket` 和 `Sec-WebSocket-Key` 的 HTTP 請求
  2. 伺服器回應 101 狀態碼和 `Sec-WebSocket-Accept` 標頭
  3. 連接升級完成，切換到 WebSocket 協議
- **全雙工通信**：建立後可雙向即時傳輸資料，無需請求-回應模式
- **訊息框架類型**：
  * **文本框架**：UTF-8 編碼的文本訊息
  * **二進制框架**：原始二進制資料
  * **控制框架**：Ping/Pong 心跳、連接關閉
- **連接管理**：
  * **心跳機制**：Ping/Pong 檢測連接狀態
  * **正常關閉**：Close 框架協商關閉
  * **異常斷開**：網路中斷或應用錯誤
  * **重連機制**：自動重新建立連接
- **安全機制**：
  * **Origin 檢查**：防止跨站 WebSocket 劫持
  * **WSS 加密**：TLS 保護資料傳輸
  * **認證機制**：透過 Cookie 或 Token 驗證
- **應用場景**：
  * 即時聊天應用
  * 遊戲即時更新
  * 實時監控系統
  * 協作編輯工具
  * 股票價格推送
  * 直播彈幕系統
- **子協議與擴展**：
  * **Sec-WebSocket-Protocol**：指定應用層子協議
  * **壓縮擴展**：減少訊息大小
  * **自定義協議**：應用特定的通信格式
- **與其他技術比較**：
  * **vs HTTP 長輪詢**：更低延遲，減少伺服器負載
  * **vs Server-Sent Events**：雙向 vs 單向通信
  * **vs Socket.IO**：原生協議 vs 函式庫實作

# WebRTC (Web Real-Time Communication) 

- **核心 API 組件**：
  * **MediaStream**：獲取和管理音頻、視頻流
  * **RTCPeerConnection**：建立點對點連接和媒體傳輸
  * **RTCDataChannel**：傳輸任意資料（非媒體）
- **瀏覽器支援**：
  * **主流瀏覽器**：Chrome, Firefox, Safari, Edge
  * **移動端支援**：Android Chrome, iOS Safari
  * **API 標準化**：W3C 和 IETF 共同標準
- **NAT 穿透技術**：
  * **STUN (Session Traversal Utilities for NAT)**：發現公網 IP 和端口
  * **TURN (Traversal Using Relays around NAT)**：當 P2P 連接失敗時作為中繼服務器
  * **ICE (Interactive Connectivity Establishment)**：嘗試多種連接方式的框架
- **信令流程 (Signaling)**：
  1. **Offer/Answer 模型**：SDP (Session Description Protocol) 描述交換
  2. **ICE Candidate 交換**：網路路徑候選項協商
  3. **建立 P2P 連接**：直接點對點通信
- **媒體協商與編解碼器**：
  * **視頻編解碼器**：VP8, VP9, AV1, H.264
  * **音頻編解碼器**：Opus, G.711, G.722
  * **編解碼器協商**：SDP 中指定支援的格式
  * **分辨率和頻寬**：動態適應網路狀況
- **安全機制**：
  * **DTLS (Datagram Transport Layer Security)**：資料通道加密
  * **SRTP (Secure Real-time Transport Protocol)**：媒體流加密
  * **身份驗證**：TURN 伺服器認證
  * **Origin 安全**：同源政策保護
- **網路適應性**：
  * **頻寬偵測**：動態調整媒體品質
  * **錯誤恢復**：封包遺失重傳
  * **Jitter Buffer**：平滑播放延遲
  * **回音消除**：音頻處理
- **實際應用場景**：
  * **視訊會議**：Zoom, Google Meet, Microsoft Teams
  * **即時串流**：直播平台
  * **線上遊戲**：低延遲多人遊戲
  * **遠端桌面**：螢幕共享
  * **IoT 通信**：設備間直接通信
- **效能優化**：
  * **硬體加速**：GPU 編解碼支援
  * **Simulcast**：同時傳送多種解析度
  * **SVC (Scalable Video Coding)**：可擴展視頻編碼
  * **連接品質監控**：統計資料分析