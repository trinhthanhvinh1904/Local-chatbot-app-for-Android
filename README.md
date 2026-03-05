# Hướng dẫn cài đặt và sử dụng Android Chat Bot

## Yêu cầu
- Điện thoại Android
- Kết nối Internet (để tải model và các gói cần thiết)

---

## Cài đặt lần đầu

### Bước 1 — Cài đặt ứng dụng

1. Cài đặt file **chat-with-qwen.apk** vào điện thoại.
2. Cài đặt **Termux** từ F-Droid: https://f-droid.org/packages/com.termux/
   > ⚠️ Không cài Termux từ Google Play vì phiên bản đó đã cũ và không được hỗ trợ.

---

### Bước 2 — Thiết lập môi trường trong Termux

Mở Termux và chạy lần lượt các lệnh sau:

**Cập nhật hệ thống và cài đặt công cụ cần thiết:**
```bash
pkg update && pkg upgrade -y
pkg install git make cmake clang wget -y
```

**Tạo thư mục chứa model AI:**
```bash
mkdir -p ~/models
```

**Tải model AI (nhẹ, phù hợp với điện thoại):**
```bash
wget -O ~/models/Qwen3.5-0.8B-Q4_K_M.gguf \
  https://huggingface.co/Qwen/Qwen3.5-0.8B-GGUF/resolve/main/qwen3_5-0_8b-q4_k_m.gguf
```

**Tải và biên dịch llama.cpp:**
```bash
cd
git clone https://github.com/ggerganov/llama.cpp.git
cd llama.cpp
cmake -B build
cmake --build build --config Release -j$(nproc)
```

**Khởi động server AI:**
```bash
./build/bin/llama-server -m ~/models/Qwen3.5-0.8B-Q4_K_M.gguf -ngl 99 -c 2048 --host 0.0.0.0 --port 8080
```

> ⚠️ **Không được tắt Termux** khi đang dùng ứng dụng chat. Server cần chạy liên tục trong nền.

---

### Bước 3 — Mở ứng dụng chat

Sau khi server đã khởi động thành công trong Termux, mở ứng dụng **chat-with-qwen** để bắt đầu trò chuyện.

---

## Các lần sử dụng tiếp theo

Mỗi lần muốn dùng chatbot, chỉ cần:

1. Mở **Termux**, vào thư mục llama.cpp và chạy lại lệnh khởi động server:
   ```bash
   cd llama.cpp
   ./build/bin/llama-server -m ~/models/Qwen3.5-0.8B-Q4_K_M.gguf -ngl 99 -c 2048 --host 0.0.0.0 --port 8080
   ```
2. Mở ứng dụng **chat-with-qwen** và bắt đầu chat.