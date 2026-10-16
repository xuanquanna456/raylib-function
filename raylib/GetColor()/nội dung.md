hàm `GetColor()` là hàm lấy màu từ các mã màu.

**Định dạng là `0xRRGGBBAA`** (8 chữ số hex), khác với CSS/web chỉ có 6 chữ số:

| Phần | Ý nghĩa    |
| ---- | ---------- |
| `RR` | Đỏ         |
| `GG` | Xanh lá    |
| `BB` | Xanh dương |
| `AA` | Alpha      |

```c
cpp
Color c = GetColor(0xFF0000FF);   // đỏ đặc
DrawCircle(200, 200, 40, GetColor(0x3498DBFF));  // xanh dương kiểu flat UI
```

Cạm bẫy hay gặp: nếu copy mã màu từ web như `#3498DB` rồi viết `GetColor(0x3498DB)`, thì alpha sẽ thành `00` (trong suốt hoàn toàn) và bạn không thấy gì. Phải thêm `FF` ở cuối: `GetColor(0x3498DBFF)`.

Ngoài ra, bạn **không viết được** chuỗi kiểu `"#3498DB"` trực tiếp vào hàm vẽ; phải dùng số `0x...` qua `GetColor`.

