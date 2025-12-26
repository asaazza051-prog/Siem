# An-To-n-Th-ng-Tin
SIEM
# 🔒 Real-Time SIEM Alerting System for Small Businesses

**Xây dựng hệ thống giám sát và cảnh báo mã độc tự động thời gian thực cho doanh nghiệp nhỏ sử dụng nền tảng mã nguồn mở**

![Wazuh](https://img.shields.io/badge/Wazuh-4.x-0078D4?style=flat&logo=wazuh)
![Splunk](https://img.shields.io/badge/Splunk-9.x-000000?style=flat&logo=splunk)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat&logo=python)
![VirusTotal](https://img.shields.io/badge/VirusTotal-API-red?style=flat)
![Telegram](https://img.shields.io/badge/Telegram-Bot-2CA5E0?style=flat&logo=telegram)

### 📌 Mục tiêu dự án
- Xây dựng hệ thống SIEM **realtime** phục vụ doanh nghiệp nhỏ (SME) với chi phí thấp.
- Tự động phát hiện thay đổi file (FIM), giám sát bất thường và **cảnh báo tức thì** khi phát hiện mã độc.
- Tích hợp **Threat Intelligence** (VirusTotal API) để đánh giá mức độ nguy hiểm.
- Đáp ứng nhanh chóng (<15 giây) từ lúc sự kiện xảy ra đến khi nhận cảnh báo.

### 🏗️ Kiến trúc hệ thống
Wazuh Agents (Windows/Linux)
↓ (syslog + encrypted)
Wazuh Server → Splunk (indexing & alerting)
↓ (webhook POST JSON)
Python Webhook → VirusTotal API (auto scan hash)
↓
Telegram Bot (realtime alerting)
### 🚀 Tính năng chính
- **Realtime File Integrity Monitoring (FIM)** – Phát hiện thêm/sửa/xóa file ngay lập tức.
- **Automated Malware Detection** – Tự động query VirusTotal với hash file.
- **Instant Alerting** – Gửi Telegram 2 tin:
  1. Thông báo sự kiện (path, hash, host, thời gian)
  2. Cảnh báo mã độc (số engine phát hiện + link VirusTotal)
- **Mã nguồn mở 100%** – Không phụ thuộc tool thương mại.

### ⚙️ Công nghệ sử dụng
- **Wazuh 4.x** – Agent & Server (FIM, vuln detection, log collection)
- **Splunk Free/Enterprise** – Central log storage & realtime search/alert
- **Python 3.11 + Flask** – Webhook xử lý alert, tích hợp VT API
- **VirusTotal API** – Threat intelligence tự động
- **Telegram Bot** – Kênh cảnh báo realtime

### 🛠️ Hướng dẫn chạy nhanh (Quick Start)
```bash
# 1. Clone repo
git clone https://github.com/duongcongdinh/siem-wazuh-splunk.git
cd siem-wazuh-splunk

# 2. Cấu hình environment variables
cp .env.example .env
# Edit .env: VT_API_KEY, TELEGRAM_TOKEN, TELEGRAM_CHAT_ID

# 3. Deploy webhook (Docker khuyến nghị)
docker build -t siem-webhook .
docker run -d -p 8080:8080 --name siem-webhook --env-file .env siem-webhook

# 4. Cấu hình Splunk Alert → Webhook URL: http://your-server:8080/siem
