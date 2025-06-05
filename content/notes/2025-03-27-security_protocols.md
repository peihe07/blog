+++
date = '2025-03-27'
draft = false
title = '安全協定'
categories = ['筆記']
tags = ['protocol']
+++

# TLS/SSL (Transport Layer Security) 

- **TLS 握手過程**：
  1. **ClientHello**：客戶端發送支援的加密算法和隨機數
  2. **ServerHello**：伺服器選擇加密算法並發送隨機數和證書
  3. **證書驗證**：客戶端驗證伺服器證書
  4. **金鑰交換**：使用非對稱加密建立共享密鑰
  5. **完成握手**：雙方使用對稱加密保護後續通訊
- **加密類型**：
  * **對稱加密**：AES、ChaCha20 等，用於資料傳輸
  * **非對稱加密**：RSA、ECC，用於金鑰交換
  * **雜湊函數**：SHA-256、SHA-384，用於訊息完整性
- **TLS 版本演進**：
  * **TLS 1.0/1.1**：已棄用，存在安全漏洞
  * **TLS 1.2**：目前廣泛使用，支援多種加密套件
  * **TLS 1.3**：最新版本，大幅改進安全性和效能
- **Perfect Forward Secrecy (PFS)**：即使私鑰洩露，之前的通訊仍安全
- **TLS 1.3 改進**：
  * 減少握手往返次數（1-RTT）
  * 移除不安全的加密算法
  * 握手加密
  * 0-RTT 恢復功能（權衡安全性與性能）
- **證書驗證機制**：
  * **證書鏈**：根 CA → 中間 CA → 終端實體證書
  * **CRL (Certificate Revocation List)**：憑證撤銷清單
  * **OCSP (Online Certificate Status Protocol)**：線上憑證狀態協定
  * **SNI (Server Name Indication)**：支援單一 IP 多域名證書
- **安全增強功能**：
  * **HSTS (HTTP Strict Transport Security)**：強制瀏覽器使用 HTTPS
  * **Certificate Pinning**：證書釘選，防止憑證攻擊

# SSH (Secure Shell) 

- **SSH 版本差異**：
  * **SSH-1**：已棄用，存在安全漏洞
  * **SSH-2**：目前標準版本，安全性大幅改善
- **認證方式**：
  * **密碼認證**：簡單但安全性較低
  * **公鑰認證**：高安全性，使用密鑰對
  * **雙因素認證**：結合密鑰和密碼/驗證碼
- **密鑰類型比較**：
  * **RSA**：傳統算法，密鑰長度 2048/4096 bits
  * **DSA**：已不推薦使用
  * **ECDSA**：橢圓曲線算法，較短密鑰長度
  * **Ed25519**：新一代橢圓曲線算法，推薦使用
- **連接建立流程**：
  1. TCP 連接建立（預設端口 22）
  2. 協議版本協商
  3. 演算法協商
  4. 金鑰交換
  5. 用戶認證
- **主機身份驗證**：
  * **known_hosts 檔案**：儲存已知主機的公鑰指紋
  * **主機密鑰檢查**：防止中間人攻擊
  * **首次連接警告**：新主機連接時的安全提示
- **SSH 隧道**：
  * **本地轉發**：-L 選項，將本地端口轉發到遠端
  * **遠程轉發**：-R 選項，將遠端端口轉發到本地
  * **動態轉發**：-D 選項，建立 SOCKS 代理
- **SSH 配置與管理**：
  * `~/.ssh/config` 文件配置
  * 主機別名設定
  * 密鑰管理與 ssh-agent
  * 連接池和復用 (ControlMaster)
  * 免密碼登入設定

# IPSec (Internet Protocol Security) 

- **協議組件**：
  * **AH (Authentication Header)**：僅提供認證和完整性，不加密
  * **ESP (Encapsulating Security Payload)**：提供認證、完整性和加密
- **運行模式**：
  * **傳輸模式**：僅加密 IP 封包的負載，保留原始 IP 標頭
  * **隧道模式**：加密整個 IP 封包並添加新的 IP 標頭
- **安全關聯 (SA)**：定義加密算法、密鑰和安全參數
- **IKE (Internet Key Exchange)**：
  * **IKEv1**：傳統版本，較複雜的協商過程
  * **IKEv2**：改進版本，更簡潔且支援移動性
  * **第一階段**：建立 ISAKMP SA，保護第二階段
  * **第二階段**：建立 IPSec SA
- **NAT 穿透**：
  * **NAT-T (NAT Traversal)**：解決 NAT 環境下的 IPSec 連接問題
  * **UDP 封裝**：將 ESP 封包封裝在 UDP 中通過 NAT
- **實際應用**：
  * **站點對站點 VPN**：連接不同辦公室網路
  * **遠程訪問 VPN**：員工在外部存取公司網路
  * **L2TP/IPSec VPN**：結合 L2TP 隧道和 IPSec 加密
- **配置考量**：
  * **加密算法選擇**：AES、3DES 等對稱加密
  * **認證方式**：PSK (Pre-Shared Key) 或憑證
  * **完整性驗證**：SHA-1、SHA-256 等雜湊算法