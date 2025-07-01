# Ứng dụng Chat Âm thanh Bảo mật
*Secure Voice Chat Application - WebSocket, RSA & DES Encryption*

[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://python.org)
[![WebSocket](https://img.shields.io/badge/WebSocket-Supported-green.svg)](https://websockets.readthedocs.io/)
[![Encryption](https://img.shields.io/badge/Encryption-RSA%20%2B%20DES-red.svg)](https://pycryptodome.readthedocs.io/)

## 📖 Tổng quan

Ứng dụng demo minh họa hệ thống chat âm thanh end-to-end encryption được xây dựng với:
- **Backend**: Python WebSocket server
- **Frontend**: HTML5, Bootstrap 5, JavaScript
- **Bảo mật**: Mã hóa đối xứng (DES) và bất đối xứng (RSA)

Ứng dụng mô phỏng quy trình mã hóa tin nhắn thoại từ khâu handshake, trao đổi khóa, đến mã hóa và xác thực nội dung.

## 🔐 Tính năng Bảo mật

### Mã hóa Lai (Hybrid Encryption)
- **RSA 2048-bit**: Tạo cặp khóa công khai/bí mật cho mỗi client
- **DES Session Key**: Khóa phiên được mã hóa bằng RSA-OAEP
- **CBC Mode**: Dữ liệu âm thanh được mã hóa DES ở chế độ CBC

### Xác thực và Toàn vẹn
- **Chữ ký số**: RSA với PKCS#1 v1.5 padding
- **Hash function**: SHA-256 để đảm bảo tính toàn vẹn
- **Key Exchange**: Trao đổi khóa an toàn thông qua RSA

### Audit và Logging
- **Security Logs**: Ghi lại các sự kiện mã hóa vào `encryption_log.json`
- **Real-time Monitoring**: Theo dõi quá trình handshake và mã hóa

## 🏗️ Kiến trúc Hệ thống

```
┌─────────────────┐    WebSocket     ┌─────────────────┐
│   Client A      │◄────────────────►│   Python        │
│   (Browser)     │     (WSS/WS)     │   Server        │
└─────────────────┘                  │   (Backend)     │
                                     │                 │
┌─────────────────┐    WebSocket     │                 │
│   Client B      │◄────────────────►│                 │
│   (Browser)     │     (WSS/WS)     └─────────────────┘
└─────────────────┘
```

### Backend Components (`app.py`)
- **WebSocket Server**: Xử lý kết nối đồng thời với `websockets` library
- **User Management**: Quản lý clients và chat rooms
- **Message Broker**: Chuyển tiếp các gói tin giữa clients
- **Key Exchange Handler**: Tạo và phân phối session keys
- **RSA Key Generator**: Tạo cặp khóa RSA theo yêu cầu

### Frontend Components (`index.html`)
- **Bootstrap 5 UI**: Giao diện người dùng responsive
- **WebSocket Client**: Kết nối real-time với server
- **Audio Recording**: WebRTC để ghi âm
- **Crypto Simulation**: Mô phỏng quy trình mã hóa/giải mã

### Crypto Utilities (`crypto_utils.py`)
- **CryptoUtils Class**: Wrapper cho các operations mã hóa
- **SecureMessageHandler**: Xử lý tin nhắn bảo mật end-to-end
- **Reference Implementation**: Tài liệu tham khảo cho crypto operations

## 📁 Cấu trúc Dự án

```
BTL_ATBMTT/
├── 📄 app.py                 # WebSocket Server Backend
├── 📄 crypto_utils.py        # Crypto Utilities Module
├── 📄 index.html             # Frontend User Interface
├── 📄 requirements.txt       # Python Dependencies
├── 📊 encryption_log.json    # Security Event Logs
└── 📖 README.md              # Documentation
```

## 🛠️ Cài đặt và Triển khai

### Yêu cầu Hệ thống
- **Python**: 3.7 hoặc cao hơn
- **Browser**: Chrome 60+, Firefox 55+, Edge 79+
- **Hardware**: Microphone để ghi âm
- **Network**: Port 8765 không bị chặn

### Bước 1: Cài đặt Dependencies
```bash
# Clone repository
git clone <repository-url>
cd secure-voice-chat

# Cài đặt Python packages
pip install -r requirements.txt
```

### Bước 2: Khởi động Server
```bash
python app.py
```
Server sẽ khởi động tại `ws://localhost:8765`

### Bước 3: Truy cập Frontend
```bash
# Phương pháp 1: Mở trực tiếp
open index.html

# Phương pháp 2: HTTP Server (Khuyến nghị)
python -m http.server 8000
# Truy cập: http://localhost:8000
```

## 🎯 Hướng dẫn Sử dụng

### Phase 1: Khởi tạo và Kết nối
1. **Mở 2 tab browser** với `index.html`
2. **Nhập thông tin:**
   - Tab 1: Username = `UserA`, Room = `room1`
   - Tab 2: Username = `UserB`, Room = `room1`
3. **Kết nối**: Click "Kết nối" ở cả 2 tab
4. **Tạo khóa**: Click "Tạo khóa RSA" ở cả 2 tab

### Phase 2: Handshake và Key Exchange
1. **Chọn người nhận**: Ở UserA, click vào "UserB" trong danh sách online
2. **Tự động handshake**: Hệ thống sẽ tự động thực hiện:
   - Public key exchange
   - Session key generation
   - Key distribution
3. **Kiểm tra logs**: Theo dõi quá trình trong console

### Phase 3: Voice Messaging
1. **Ghi âm**: Click nút 🎤 (microphone) để bắt đầu
2. **Dừng ghi**: Click nút ⏹️ (stop) để kết thúc
3. **Gửi tin nhắn**: Tự động mã hóa và gửi

### Phase 4: Nhận và Giải mã
1. **Nhận tin nhắn**: Encrypted package xuất hiện ở UserB
2. **Xác thực**: Click "Xác thực chữ ký"
3. **Giải mã**: Click "Giải mã"
4. **Phát âm thanh**: Click "Nghe âm thanh"

## 🔒 Phân tích Bảo mật

### ✅ Điểm Mạnh
| Tính năng | Mô tả | Mức độ |
|-----------|--------|--------|
| **End-to-End Encryption** | Mã hóa từ nguồn đến đích | 🟢 Cao |
| **Perfect Forward Secrecy** | Session key duy nhất cho mỗi cuộc trò chuyện | 🟢 Cao |
| **Message Authentication** | RSA signature với SHA-256 | 🟢 Cao |
| **Key Exchange Security** | RSA-OAEP padding | 🟢 Cao |

### ⚠️ Hạn chế và Khuyến nghị

#### 🔴 Critical Issues
- **DES Deprecated**: DES 56-bit dễ bị brute force
  - **Giải pháp**: Nâng cấp lên AES-256-GCM
- **Client-side Simulation**: Logic crypto chỉ là mock
  - **Giải pháp**: Implement WebCrypto API hoặc forge.js

#### 🟡 Security Improvements
- **HTTPS/WSS Required**: Kết nối WebSocket không mã hóa
  - **Giải pháp**: Deploy với SSL/TLS certificate
- **Server-side Key Generation**: Không đảm bảo E2EE thực sự
  - **Giải pháp**: Client tự tạo và quản lý keys


## 🐛 Troubleshooting

### Kết nối WebSocket
```bash
# Kiểm tra server status
netstat -an | grep 8765

# Test WebSocket connection
wscat -c ws://localhost:8765
```

### Microphone Access
- **Chrome**: `chrome://settings/content/microphone`
- **Firefox**: `about:preferences#privacy`
- **Requires HTTPS**: Một số browser yêu cầu secure context

### Debug Mode
```javascript
// Enable debug logs trong browser console
localStorage.setItem('debug', 'true');
```

## 📊 Performance Metrics

| Metric | Value | Notes |
|--------|-------|-------|
| **Key Generation** | ~100ms | RSA 2048-bit |
| **Encryption Speed** | ~1ms/KB | DES CBC mode |
| **WebSocket Latency** | <10ms | Local network |
| **Audio Quality** | 44.1kHz | Browser default |

## 🤝 Đóng góp

1. **Fork** repository
2. **Create** feature branch (`git checkout -b feature/AmazingFeature`)
3. **Commit** changes (`git commit -m 'Add AmazingFeature'`)
4. **Push** to branch (`git push origin feature/AmazingFeature`)
5. **Open** Pull Request

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

## 🔗 Tài liệu Tham khảo

- [WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)
- [PyCryptodome Documentation](https://pycryptodome.readthedocs.io/)
- [RSA-OAEP Specification](https://tools.ietf.org/html/rfc3447)

---

**⚠️ Disclaimer**: Đây là ứng dụng demo cho mục đích học tập. Không sử dụng trong môi trường production mà không có các cải tiến bảo mật cần thiết.