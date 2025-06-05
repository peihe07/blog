+++
date = '2025-03-31'
draft = false
title = 'Web服務協定'
categories = ['筆記']
tags = ['protocol']
+++


# SOAP (Simple Object Access Protocol) 

- **基本結構**：
  * **Envelope**：根元素，定義 XML 文檔為 SOAP 消息
  * **Header**：可選，包含應用特定信息（認證、路由等）
  * **Body**：必須，包含實際的消息內容
  * **Fault**：錯誤信息，類似 HTTP 狀態碼
- **SOAP 版本差異**：
  * **SOAP 1.1**：較舊版本，廣泛支援
  * **SOAP 1.2**：改進版本，更好的錯誤處理
- **傳輸綁定**：
  * **HTTP/HTTPS**：最常見，POST 方法
  * **SMTP**：電子郵件傳輸
  * **TCP**：直接 TCP 連接
  * **JMS**：Java 消息服務
- **消息模式**：
  * **請求-響應（Request-Response）**：同步通信
  * **單向（One-Way）**：異步通信，無回應
- **安全特性**：
  * **WS-Security**：消息級安全標準
  * **XML 數位簽名**：確保消息完整性和來源驗證
  * **XML 加密**：保護敏感數據
  * **WS-Trust**：安全令牌交換
- **相關標準（WS-* 堆疊）**：
  * **WSDL (Web Services Description Language)**：服務介面描述
  * **UDDI (Universal Description, Discovery and Integration)**：服務註冊和發現
  * **WS-Policy**：服務策略表達
  * **WS-ReliableMessaging**：可靠消息傳遞
- **優勢**：
  * 嚴格定義的結構和標準
  * 跨平台、跨語言支持
  * 豐富的企業級功能
  * 強大的安全機制
- **劣勢**：
  * 消息體積大（XML 開銷）
  * 處理複雜度高
  * 效能較低
  * 學習曲線陡峭

# XMPP (Extensible Messaging and Presence Protocol) 

- **核心功能**：
  * **即時消息（Instant Messaging）**：實時文本通信
  * **存在信息（Presence）**：用戶在線狀態管理
  * **聯繫人列表（Roster）**：好友管理和組織
- **協定特性**：
  * **基於 XML**：所有通信使用結構化 XML 格式
  * **即時性**：近乎實時的消息傳遞
  * **擴展性**：模組化設計，支援豐富擴展
- **分佈式架構**：
  * **去中心化設計**：類似電子郵件系統
  * **聯邦式網路**：使用域名標識服務器
  * **伺服器間通信**：S2S (Server-to-Server) 協定
- **身份識別系統**：
  * **JID (Jabber ID)**：username@domain/resource
  * **資源識別**：允許多點登錄（手機、電腦等）
  * **域名路由**：基於域名的消息路由
- **核心數據單元（XML Stanzas）**：
  * **&lt;message&gt;**：傳遞即時消息
  * **&lt;presence&gt;**：傳遞用戶狀態信息
  * **&lt;iq&gt; (Info/Query)**：請求-響應交互機制
- **連接管理**：
  * **XML 串流**：持久的 XML 連接
  * **心跳機制**：白空格 ping 保持連接
  * **重連機制**：自動重新建立連接
- **擴展性（XEP - XMPP Extension Protocols）**：
  * **XEP-0045**：多用戶聊天（群組聊天）
  * **XEP-0096**：檔案傳輸
  * **XEP-0166**：Jingle (語音/視頻通話)
  * **XEP-0060**：發布-訂閱
- **安全特性**：
  * **TLS 加密**：傳輸層安全
  * **SASL 認證**：可插拔認證機制
  * **端到端加密**：OTR、OMEMO 擴展
- **應用場景**：
  * 即時通訊系統（Jabber、Pidgin）
  * 物聯網設備通信
  * 實時協作平台
  * 遊戲內通信系統