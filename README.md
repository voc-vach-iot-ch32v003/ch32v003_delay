# ⏱️ ch32v003_delay

Thư viện bọc (wrapper) các hàm tạo độ trễ chính xác theo phong cách Arduino (`delayMs`, `delayUs`) dành cho vi điều khiển **CH32V003**. Thư viện hoạt động trên nền framework **ch32v003fun**, tối ưu hóa việc quản lý mã nguồn và cho phép bật/tắt linh hoạt thông qua cấu hình hệ thống.

---

## 🚀 Tính năng nổi bật

- **Phong cách Arduino quen thuộc:** Cung cấp hàm `delayMs()` và `delayUs()` trực quan, giúp dễ dàng chuyển đổi code từ môi trường Arduino sang CH32V003.
- **Độ chính xác cao:** Tận dụng trực tiếp các hàm `Delay_Ms()` và `Delay_Us()` gốc của `ch32v003fun` dựa trên chu kỳ xung nhịp SysTick (48MHz).
- **Tối ưu bộ nhớ (Modular Control):** Hỗ trợ bật/tắt toàn bộ hàm wrapper thông qua file cấu hình `sys_config.h`, tránh lãng phí bộ nhớ Flash nếu dự án không cần dùng.
- **Tự động thẩm thấu cấu hình:** Tự quét và include `sys_config.h` bằng cơ chế `__has_include` an toàn.

---

## 📑 Cấu trúc hàm (API Reference)

### 1. `void delayMs(uint32_t ms)`

Tạo độ trễ dừng luồng (blocking) theo đơn vị mili-giây (ms).

| Tham số | Kiểu dữ liệu | Mô tả                              |
| ------- | ------------ | ---------------------------------- |
| `ms`    | `uint32_t`   | Thời gian chờ tính bằng mili-giây. |

### 2. `void delayUs(uint32_t us)`

Tạo độ trễ dừng luồng (blocking) cực ngắn theo đơn vị micro-giây ($\mu s$).

| Tham số | Kiểu dữ liệu | Mô tả                                                                                        |
| ------- | ------------ | -------------------------------------------------------------------------------------------- |
| `us`    | `uint32_t`   | Thời gian chờ tính bằng micro-giây (phù hợp cho các giao thức One-Wire, DHT11, bit-banging). |

---

## 📝 Code mẫu sử dụng

```c
#include "ch32fun.h"
#include <ch32v003_delay.h>

void setup() {
    // Khởi tạo ứng dụng
}

void loop() {
    // Chờ 500ms (0.5 giây)
    delayMs(500);

    // Chờ 100 microseconds (cho các tác vụ thời gian thực cực ngắn)
    delayUs(100);
}

int main() {
    SystemInit();

    // Delay an toàn 2s khởi động
    delayMs(2000);

    setup();
    while (1) {
        loop();
    }
}

```

---
