+++
date = '2025-03-29'
draft = false
title = '訊息傳遞協定'
categories = ['筆記']
tags = ['protocol']
+++

# MQTT (Message Queuing Telemetry Transport)

- **協定基本特性**：
  * **輕量級協定**：專為低頻寬、高延遲、不穩定網路設計
  * **預設端口**：1883 (非加密)、8883 (TLS/SSL 加密)
  * **基於 TCP**：確保可靠的訊息傳遞
  * **二進制協定**：高效的網路使用
- **發布/訂閱模式**：
  * 透過主題（Topic）進行消息交換
  * 發布者和訂閱者解耦合
  * 支援一對多和多對多通信模式
- **主題結構**：
  * 層次化結構（如 home/livingroom/temperature）
  * 使用 `/` 作為層級分隔符
  * 支援萬用字元：`+`（單層）、`#`（多層）
- **QoS (Quality of Service) 等級**：
  * **QoS 0**：最多一次（消息可能丟失），Fire and Forget
  * **QoS 1**：至少一次（確保送達，可能重複），需要 PUBACK 確認
  * **QoS 2**：恰好一次（確保一次且僅一次送達），四次握手機制
- **進階功能**：
  * **保留消息（Retained Message）**：伺服器保存最後一條消息，新訂閱者立即收到
  * **遺囑消息（Last Will and Testament）**：客戶端異常斷開時發送預設消息
  * **會話持久化（Persistent Session）**：客戶端離線時保存訂閱和未送達消息
  * **Keep Alive**：心跳機制保持連接活躍
- **MQTT 5.0 新特性**：
  * **消息過期**：Message Expiry Interval 設定消息生命週期
  * **主題別名**：Topic Alias 減少頻寬使用
  * **用戶屬性**：User Properties 自定義鍵值對
  * **共享訂閱**：Shared Subscriptions 負載均衡
  * **原因碼**：詳細的錯誤和狀態資訊
- **安全機制**：
  * **TLS/SSL 加密**：保護資料傳輸
  * **用戶名/密碼認證**：基本身份驗證
  * **用戶端憑證**：雙向 TLS 認證
  * **存取控制清單**：主題層級權限控制
- **實際應用場景**：
  * **IoT 設備通信**：感測器資料收集
  * **智慧家居**：設備控制和狀態監控
  * **工業物聯網**：製造設備監控
  * **移動應用**：推播通知、即時更新

# AMQP (Advanced Message Queuing Protocol) 

- **協定版本差異**：
  * **AMQP 0.9.1**：廣泛使用，RabbitMQ 預設版本
  * **AMQP 1.0**：ISO 標準，與 0.9.1 不相容
- **核心組件**：
  * **Exchange**：接收生產者消息並路由到佇列
  * **Queue**：存儲消息直到被消費
  * **Binding**：連接 Exchange 和 Queue 的規則
  * **Virtual Host**：邏輯分組，提供隔離
- **Exchange 類型**：
  * **Direct**：精確匹配路由鍵，一對一路由
  * **Topic**：模式匹配路由鍵，支援 `*` 和 `#` 萬用字元
  * **Fanout**：廣播到所有綁定佇列，忽略路由鍵
  * **Headers**：基於消息頭部屬性匹配，靈活路由
- **消息確認機制**：
  * **自動確認（Auto ACK）**：消息送達即確認，可能丟失
  * **手動確認（Manual ACK）**：消費者處理後顯式確認
  * **拒絕機制**：NACK 和 Reject 處理失敗消息
- **可靠性保證**：
  * **交易支持**：可將多個操作作為一個原子單元
  * **發布者確認**：Publisher Confirms 確保消息到達
  * **消費者確認**：Consumer Acknowledgments 確保處理完成
- **持久化機制**：
  * **消息持久化**：消息存儲到磁碟
  * **佇列持久化**：佇列定義持久化
  * **Exchange 持久化**：Exchange 定義持久化
- **進階功能**：
  * **TTL (Time To Live)**：消息和佇列過期時間
  * **死信佇列（DLX）**：處理失敗或過期消息
  * **延遲消息**：定時發送消息
  * **優先佇列**：基於優先級的消息處理
- **主要實作產品**：
  * **RabbitMQ**：最受歡迎的 AMQP 實作
  * **Apache Qpid**：Apache 軟體基金會項目
  * **Azure Service Bus**：微軟雲端訊息服務

# QUIC (Quick UDP Internet Connections) 

- **基本特性**：
  * 基於 UDP 的傳輸層協議，由 Google 開發
  * HTTP/3 的基礎協議
  * 結合 TCP、TLS 和 HTTP/2 的功能
  * IETF RFC 9000 標準化
- **主要優勢**：
  * **減少連接建立延遲**：0-RTT 或 1-RTT 連接建立
  * **避免隊頭阻塞**：多路複用無阻塞
  * **連接遷移**：網絡切換時保持連接
  * **內建加密**：集成 TLS 1.3 安全特性
- **關鍵技術**：
  * **連接 ID**：允許客戶端 IP 變化時保持連接
  * **多路複用**：單一連接上的獨立資料流
  * **流量控制**：每個流和整個連接的控制
  * **擁塞控制**：改進的控制算法
- **安全特性**：
  * **預設加密**：所有 QUIC 連接都經過加密
  * **TLS 1.3 整合**：最新的安全標準
  * **前向安全性**：定期更新加密金鑰
  * **防重放攻擊**：內建防護機制
- **效能優化**：
  * **頭部壓縮**：QPACK 壓縮演算法
  * **早期資料傳輸**：0-RTT 恢復機制
  * **適應性擁塞控制**：根據網路狀況調整
  * **封包合併**：減少系統呼叫開銷
- **與 TCP 比較**：
  * 更快的連接建立
  * 更好的錯誤恢復
  * 更靈活的協議演進
  * 減少對中間設備的依賴
- **應用場景**：
  * 移動設備網頁瀏覽
  * 高延遲網絡下的應用
  * 需要快速切換網絡的場景
  * 直播和視頻串流
- **實作和支援**：
  * **瀏覽器支援**：Chrome、Firefox、Safari、Edge
  * **伺服器支援**：nginx、Apache、Cloudflare
  * **程式庫**：quiche (Cloudflare)、msquic (Microsoft)

# Apache Kafka Protocol 

- **Kafka 架構演進**：
  * **早期版本**：依賴 Apache ZooKeeper 進行中繼資料管理
  * **KRaft 模式**：Kafka 2.8+ 內建共識機制，移除 ZooKeeper 依賴
- **核心概念**：
  * **Topic**：消息類別，邏輯上的訊息分組
  * **Partition**：Topic 的分片，實現並行處理和水平擴展
  * **Producer**：發送消息到 Topic 的應用程式
  * **Consumer**：從 Topic 消費消息的應用程式
  * **Consumer Group**：多消費者共享消費負載，實現水平擴展
  * **Broker**：Kafka 伺服器節點，處理讀寫請求
- **分區機制**：
  * **並行處理**：每個分區可獨立處理
  * **水平擴展**：增加分區數量提高吞吐量
  * **分區策略**：Round-robin、Key-based、Custom partitioner
  * **分區重平衡**：Consumer Group 成員變化時重新分配
- **複製機制**：
  * **分區副本**：每個分區有多個副本提供容錯
  * **Leader 和 Follower**：Leader 處理讀寫，Follower 同步資料
  * **ISR (In-Sync Replicas)**：與 Leader 保持同步的副本集合
  * **最小 ISR**：保證資料安全的最少同步副本數
- **消息投遞語義**：
  * **At most once**：最多一次，可能丟失但不重複
  * **At least once**：至少一次，可能重複但不丟失
  * **Exactly once**：恰好一次，使用事務和冪等性保證
- **偏移量管理**：
  * **消費者責任**：消費者負責跟踪已消費位置
  * **自動提交**：定期自動提交偏移量
  * **手動提交**：消費者顯式提交偏移量
  * **偏移量存儲**：存儲在 __consumer_offsets 內部 Topic
- **效能優化**：
  * **批次處理**：批量發送和接收消息
  * **壓縮**：支援 GZIP、Snappy、LZ4、ZSTD
  * **零拷貝**：sendfile() 系統呼叫減少 CPU 使用
  * **頁面快取**：利用作業系統頁面快取
- **進階功能**：
  * **Kafka Streams**：串流處理框架
  * **Kafka Connect**：資料整合框架
  * **Schema Registry**：Schema 管理和演進
  * **事務支援**：跨多個 Topic 的 ACID 事務
- **監控與管理**：
  * **JMX 指標**：豐富的效能和健康指標
  * **管理工具**：Kafka Manager、Kafdrop、Confluent Control Center
  * **日誌壓縮**：Log Compaction 保留最新狀態