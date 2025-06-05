+++
date = '2025-04-06'
draft = false
title = '網路協定分類'
categories = ['筆記']
tags = ['protocol']
+++

# 網路協定分類概要

## OSI 模型與常見協定對照

| OSI 層級 | TCP/IP 模型 | 代表協定 |
|---------|------------|---------|
| 應用層-會話層 | 應用層 | HTTP, SMTP, FTP, DNS, SSH |
| 傳輸層 | 傳輸層 | TCP, UDP |
| 網路層 | 網路層 | IP, ICMP |
| 資料連結層-實體層 | 網路介面層 | Ethernet, WiFi |

## 主要協定分類

### 1. 基礎網路通訊協定(/notes/2025-03-27-tcp_udp_ip.md)
- **TCP** - 可靠傳輸，適用於網頁、檔案傳輸
- **UDP** - 快速傳輸，適用於串流、遊戲
- **IP** - 網際網路基礎協定，負責資料路由

### 2. 網路管理協定(/notes/2025-03-28-dns_dhcp.md)
- **DNS** - 域名解析系統，將網址轉換為 IP 位址
- **DHCP** - 自動分配 IP 位址和網路設定

### 3. 安全協定(/notes/2025-03-27-security_protocols.md)
- **TLS/SSL** - 網路通訊加密，保護資料傳輸
- **SSH** - 安全遠端登入和管理
- **IPSec** - IP 層加密，常用於 VPN

### 4. Web 協定(/notes/2025-03-28-web_protocols.md)
- **HTTP/HTTPS** - 網頁瀏覽協定，現代網路基礎
- **WebSocket** - 即時雙向通訊，用於聊天、遊戲
- **WebRTC** - 點對點音視訊通訊

### 5. API 通訊協定(/notes/2025-03-29-api_protocols_corrected.md)
- **REST** - 基於 HTTP 的 API 設計風格
- **GraphQL** - 靈活的查詢語言和 API 規範
- **gRPC** - 高效能 RPC 框架，適合微服務

### 6. 訊息傳遞協定(/notes/2025-03-29-messaging_protocols_corrected.md)
- **MQTT** - 輕量級發布/訂閱協定，適合物聯網
- **AMQP** - 企業級訊息佇列協定
- **QUIC** - 新一代傳輸協定，HTTP/3 基礎
- **Kafka** - 高吞吐量分散式訊息系統

### 7. 認證與授權協定(/notes/2025-03-30-oauth.md)
- **OAuth 2.0** - 授權框架，允許第三方應用安全存取資源

### 8. 資料庫與數據協定(/notes/2025-03-31-db_protocols.md)
- **JDBC** - Java 資料庫連接標準
- **RMI** - Java 遠端方法調用

### 9. Web 服務協定(/notes/2025-03-31-web_service_protocols.md)
- **SOAP** - 基於 XML 的 Web 服務協定
- **XMPP** - 可擴展訊息與狀態協定，用於即時通訊

### 10. 多媒體傳輸協定(/notes/2025-04-03-transport.md)
- **RTP/RTCP** - 即時音視訊傳輸協定

### 11. 物聯網專用協定(/notes/2025-04-04-物聯網專用協定.md)
- **CoAP** - 受限應用協定，適用於低功耗物聯網設備
- **LoRaWAN** - 長距離低功耗廣域網協定
- **NB-IoT** - 窄頻物聯網，基於蜂窩網路的 LPWAN 技術
- **Zigbee** - 低功耗短距離無線通信，常用於智慧家居
- **Thread** - 新興的 IPv6 基礎網狀網路協定
- **Matter** - 統一智慧家居設備的應用層協定

### 12. 邊緣計算協定(/notes/2025-04-04-邊緣計算協定.md)
- **K8s Edge** - Kubernetes 邊緣計算擴展
- **ETSI MEC** - 歐洲電信標準協會多接入邊緣計算標準
- **OpenFog** - 開放霧計算架構參考模型

### 13. 區塊鏈共識協定(/notes/2025-04-04-區塊鏈共識協定.md)
- **PoW** - 工作量證明，比特幣使用的共識機制
- **PoS** - 權益證明，以太坊 2.0 採用的機制
- **DPoS** - 委託權益證明，提高交易處理速度
- **PBFT** - 實用拜占庭容錯，適用於聯盟鏈

### 14. 5G 與未來網路協定(/notes/2025-04-05-5G 與未來網路協定.md)
- **QUIC** - 基於 UDP 的新一代傳輸協定，HTTP/3 基礎
- **Network Slicing** - 5G 網路切片技術
- **SBA** - 基於服務的 5G 核心網架構
- **O-RAN** - 開放無線接入網標準

---