# ⏱️ ch32v003_delay

Thư viện bọc (wrapper) các hàm tạo độ trễ chính xác theo phong cách Arduino (`delayMs`, `delayUs`) dành cho vi điều khiển **CH32V003**. Thư viện hoạt động trên nền framework **ch32v003fun**, tối ưu hóa việc quản lý mã nguồn và cho phép bật/tắt linh hoạt thông qua cấu hình hệ thống.

---

## 🚀 Tính năng nổi bật

- **API kiểu Arduino:** Cung cấp các hàm `delayMs()` và `delayUs()` quen thuộc, dễ đọc và dễ tích hợp.
- **Chính xác cao:** Tận dụng trực tiếp bộ đếm SysTick của `ch32v003fun` chạy ở tần số tối đa 48MHz.
- **Siêu nhẹ & Tối ưu Flash:** Thiết kế wrapper siêu nhẹ, không làm tốn tài nguyên bộ nhớ của dòng chip dung lượng nhỏ.

---

## 📦 Cài đặt

Tích hợp thư viện vào dự án PlatformIO bằng cách thêm Git URL vào mục `lib_deps` trong file `platformio.ini`:

```ini
[env:CH32V003J4M6]
platform = ch32v
framework = ch32v003fun
board = genericCH32V003J4M6
monitor_speed = 115200
lib_deps =
    https://github.com/voc-vach-iot-ch32v003/ch32v003_delay.git

```

---

## ⚙️ Cấu hình môi trường (`funconfig.h`)

Để hàm `delayMs()` hoạt động chính xác, tệp `funconfig.h` trong thư mục `include/` của dự án phải bật 2 cờ cấu hình bắt buộc sau:

```c
#ifndef FUNCONFIG_H
#define FUNCONFIG_H

/**
 * @brief Cho phép SysTick sử dụng trực tiếp tần số của lõi HCLK (48MHz)
 *        giúp hàm delayMs() đạt độ chính xác cao.
 */
#define FUNCONF_SYSTICK_USE_HCLK 1

#endif // FUNCONFIG_H

```

---

## 📝 Quick Start

Dưới đây là đoạn code mẫu nhanh minh họa cách dùng `delayMs()` để tạo độ trễ 1 giây và in log ra màn hình Serial Monitor qua chân SWIO:

```c
#include "ch32fun.h"
#include "ch32v003_delay.h"
#include "ch32v003_debug.h" // Để bật tính năng Debug Printf thì nhớ thêm cờ vào funconfig.h #define FUNCONF_USE_DEBUGPRINTF  1

int main(void)
{
    SystemInit();
    delayMs(1000); // Khoảng chờ an toàn tránh kẹt chip khi nạp

    printf("CH32V003 Debug Printf Initialized!\r\n");

    uint32_t count = 0;
    while (1)
    {
        printf("Count: %ld | Hex: 0x%lx\r\n", count, count);
        count++;
        delayMs(1000);
    }
}

```

---

## 📑 Tra cứu API

| Hàm xử lý     | Tham số                                   | Giá trị trả về | Mô tả                                                                                              |
| ------------- | ----------------------------------------- | -------------- | -------------------------------------------------------------------------------------------------- |
| `delayMs(ms)` | `uint32_t ms`: Thời gian chờ (mili-giây)  | `void`         | Tạo độ trễ block luồng xử lý tính bằng mili-giây.                                                  |
| `delayUs(us)` | `uint32_t us`: Thời gian chờ (micro-giây) | `void`         | Tạo độ trễ cực ngắn tính bằng micro-giây, phù hợp cho giao tiếp nhịp dòng lệnh (1-Wire, DHT11...). |

---

## 📄 Giấy phép (License) & Tác giả

- **Tác giả:** [Vọc Vạch IoT](https://github.com/voc-vach-iot)
- **Giấy phép:** Phát hành theo giấy phép [MIT License](LICENSE).
