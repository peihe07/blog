+++
date = '2025-03-31'
draft = false
title = '認證與授權協定'
categories = ['筆記']
tags = ['protocol']
+++

# JDBC (Java Database Connectivity) 

- **核心組件**：
  * **驅動管理器（DriverManager）**：管理數據庫驅動程序
  * **連接對象（Connection）**：表示與數據庫的連接
  * **語句對象（Statement）**：執行 SQL 查詢
  * **結果集（ResultSet）**：查詢結果的表示
- **JDBC 驅動類型**：
  * **Type 1**：JDBC-ODBC 橋接驅動
  * **Type 2**：原生 API 部分 Java 驅動
  * **Type 3**：純 Java 網路協議驅動
  * **Type 4**：純 Java 直接連接驅動（推薦）
- **連接流程**：
  1. 註冊 JDBC 驅動程序（JDBC 4.0+ 自動註冊）
  2. 建立數據庫 URL
  3. 創建連接對象
  4. 創建語句對象
  5. 執行查詢
  6. 處理結果集
  7. 關閉連接
- **語句類型**：
  * **Statement**：基本 SQL 執行
  * **PreparedStatement**：預編譯 SQL，防止 SQL 注入
  * **CallableStatement**：執行存儲過程
- **交易管理**：
  * **自動提交模式**：每條 SQL 自動提交
  * **手動事務控制**：begin/commit/rollback
  * **儲存點（Savepoint）**：部分回滾機制
  * **隔離級別**：READ_UNCOMMITTED、READ_COMMITTED、REPEATABLE_READ、SERIALIZABLE
- **進階功能**：
  * **批處理操作**：高效執行多條 SQL 語句
  * **數據類型映射**：JDBC 類型與 Java 類型對應
  * **連接池**：管理和重用數據庫連接（HikariCP、C3P0）
  * **元數據**：DatabaseMetaData、ResultSetMetaData
- **最佳實踐**：
  * 預處理語句防止 SQL 注入
  * 正確關閉資源防止泄漏（try-with-resources）
  * 有效管理大結果集（分頁查詢）
  * 使用連接池提高效能

# RMI (Remote Method Invocation)

- **基本概念**：Java 特有的遠程過程調用機制
- **核心組件**：
  * **遠程接口（Remote Interface）**：定義可被遠程調用的方法
  * **遠程對象（Remote Object）**：實現遠程接口的對象
  * **註冊表（Registry）**：存儲和查找遠程對象引用的目錄
  * **骨架和存根（Skeleton & Stub）**：處理底層網絡通信
- **RMI 架構層次**：
  1. **應用層**：客戶端和服務端應用
  2. **代理層（Proxy Layer）**：存根和骨架
  3. **遠程引用層（Remote Reference Layer）**：管理遠程對象引用
  4. **傳輸層（Transport Layer）**：處理網絡通信
- **RMI 註冊表**：
  * **預設端口**：1099
  * **命名服務**：存儲和查找遠程對象引用
  * **安全限制**：通常只允許本地註冊
- **參數傳遞機制**：
  * **按值傳遞**：序列化對象副本
  * **按引用傳遞**：遠程對象引用
  * **序列化要求**：參數必須實作 Serializable
- **進階特性**：
  * **動態類加載**：自動下載未知類（Codebase）
  * **安全管理器**：控制代碼訪問權限
  * **分佈式垃圾回收**：自動清理無引用的遠程對象
  * **自定義 Socket 工廠**：SSL、壓縮等
- **與其他 RPC 機制比較**：
  * **平台限制**：只支持 Java 平台
  * **物件導向**：支持完整的對象序列化
  * **內建功能**：分佈式垃圾回收、動態類加載
  * **效能考量**：比二進制協議慢，但開發簡單
