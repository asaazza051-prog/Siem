# An toan thong tin kma
SIEM
# 🔒 Real-Time SIEM Alerting System for Small Businesses

**Hệ thống giám sát và cảnh báo mã độc tự động thời gian thực dành cho doanh nghiệp nhỏ sử dụng công cụ mã nguồn mở**

## Mục tiêu dự án
- Xây dựng hệ thống SIEM realtime với chi phí thấp cho doanh nghiệp nhỏ (SME).
- Tự động phát hiện thay đổi file bất thường (FIM).
- Tích hợp Threat Intelligence (VirusTotal API) để kiểm tra hash file.
- Cảnh báo tức thì (< 15 giây) qua Telegram khi phát hiện mã độc.

## Kiến trúc hệ thống
Wazuh Agents (Windows/Linux endpoints)
↓ (encrypted syslog)
Wazuh Server → Splunk (indexing + realtime alert)
↓ (webhook POST JSON)
Python Flask Webhook → VirusTotal API (scan hash)
↓
Telegram Bot → Admin nhận 2 tin nhắn:

Sự kiện thay đổi file (path, hash, host, time)
Kết quả VirusTotal (số engine phát hiện + link)

text## 🚀 Tính năng chính
- Realtime File Integrity Monitoring (FIM) bằng Wazuh
- Tự động query VirusTotal khi phát hiện file mới/thay đổi
- Cảnh báo tức thì qua Telegram (2 giai đoạn)
- 100% mã nguồn mở, dễ mở rộng

## ⚙️ Công nghệ sử dụng
- **Wazuh 4.x** – Agent & Server (FIM, log collection)
- **Splunk Free/Enterprise** – Central log storage & alerting
- **Python 3.11 + Flask** – Webhook xử lý alert
- **VirusTotal API** – Threat intelligence
- **Telegram Bot** – Kênh cảnh báo realtime

## 🛠️ Hướng dẫn chạy nhanh (Quick Start)

### 1. Prerequisites
- Máy chủ Ubuntu/CentOS để cài Wazuh Server + Splunk
- Splunk Free/Enterprise đã cài đặt và chạy
- VirusTotal API key (free tại https://www.virustotal.com/)
- Telegram Bot Token & Chat ID

### 2. Clone repository
```bash
git clone https://github.com/asaazza051-prog/Siem.git
cd Siem
3. Cấu hình Webhook
Bashcp .env.example .env
nano .env
Điền các giá trị:
VT_API_KEY=your_virustotal_api_key
TELEGRAM_TOKEN=your_bot_token
TELEGRAM_CHAT_ID=your_chat_id
4. Chạy Webhook (Python trực tiếp hoặc Docker)
Cách 1: Chạy trực tiếp (khuyến nghị dev)
Bashpip install flask requests python-dotenv
python webhook.py
# Webhook sẽ chạy tại http://localhost:8080/siem
5. Cài đặt & cấu hình Wazuh (tóm tắt quan trọng)

Cài Wazuh All-in-one (hoặc riêng Server): https://documentation.wazuh.com/current/quickstart.html
Cấu hình FIM trong /var/ossec/etc/ossec.conf trên Agent/Server:

XML<syscheck>
  <directories realtime="yes" report_changes="yes">/path/to/monitor</directories>
  <whodata>yes</whodata>
</syscheck>

Forward alert Wazuh đến Splunk 

6. Cấu hình Splunk Alert → Webhook

Vào Splunk → Search & Reporting
Tạo saved search phát hiện FIM event (rule.id: 550-559 hoặc "File Integrity Monitoring")
Tạo Alert → Trigger Actions → Webhook
URL: http://your-webhook-server:8080/siem

7. Test hệ thống

Trên endpoint có Wazuh Agent: tạo/sửa file trong thư mục đang monitor
Chờ <15 giây → nhận 2 tin Telegram.
<img width="1001" height="451" alt="image" src="https://github.com/user-attachments/assets/ed4996f5-3633-4f5b-9608-181221069812" />
<img width="804" height="624" alt="image" src="https://github.com/user-attachments/assets/4d6e3964-188c-4501-b891-1f14ba55a775" />



