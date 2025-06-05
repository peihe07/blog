+++
date = '2025-04-03'
draft = false
title = '多媒體傳輸協定'
categories = ['筆記']
tags = ['protocol']
+++

# RTP (Real-time Transport Protocol)

**深入學習筆記：**
- **協定定位**：
  * **應用層協定**：運行在 UDP 之上
  * **即時傳輸**：專為音視頻設計
  * **配對協定**：通常與 RTCP 一起使用
- **主要功能**：
  * 實時傳輸音頻和視頻資料
  * 提供時間戳和序列號
  * 支持多種媒體格式和編碼器
  * 處理網路抖動和封包亂序
- **協議結構**：
  * **RTP 標頭**：12 字節固定頭部 + 可選擴展
  * **有效載荷（Payload）**：實際的媒體數據
- **關鍵標頭字段**：
  * **版本號（V）**：RTP 版本，通常為 2
  * **填充位（P）**：指示是否有填充字節
  * **有效載荷類型（PT）**：標識媒體編碼格式
  * **序列號（Sequence Number）**：檢測丟包和重排序
  * **時間戳（Timestamp）**：媒體同步和抖動計算
  * **SSRC**：同步源標識符，唯一識別資料流
- **RTCP (RTP Control Protocol)**：
  * **功能**：提供傳輸統計和控制信息
  * **QoS 回饋**：報告封包遺失、延遲、抖動
  * **媒體同步**：多媒體串流間的同步
  * **參與者資訊**：會話參與者的描述資訊
- **RTCP 封包類型**：
  * **SR (Sender Report)**：發送者統計和時間同步
  * **RR (Receiver Report)**：接收者統計和 QoS 回饋
  * **SDES (Source Description)**：源描述信息
  * **BYE**：離開會話通知
  * **APP**：應用特定功能擴展
- **有效載荷格式**：
  * **音頻**：G.711、G.722、Opus、AAC
  * **視頻**：H.264、H.265、VP8、VP9
  * **動態有效載荷**：通過 SDP 協商
- **擴展協議和相關技術**：
  * **SRTP (Secure RTP)**：加密版本，提供機密性和認證
  * **RTSP (Real Time Streaming Protocol)**：控制串流媒體會話
  * **RTMP (Real-Time Messaging Protocol)**：Adobe Flash 串流協定
  * **WebRTC**：瀏覽器端即時通信，使用 RTP
- **QoS 和網路適應**：
  * **抖動緩衝**：平滑播放延遲變化
  * **錯誤恢復**：FEC (Forward Error Correction)
  * **適應性串流**：根據網路狀況調整品質
- **應用場景**：
  * **VoIP 通話**：Skype、WhatsApp 語音通話
  * **視頻會議**：Zoom、Teams、Google Meet
  * **直播串流**：YouTube Live、Twitch
  * **IPTV**：數位電視服務