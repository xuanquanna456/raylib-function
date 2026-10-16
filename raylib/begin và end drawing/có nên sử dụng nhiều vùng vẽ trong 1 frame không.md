### 1. Có thể gọi nhiều cặp `BeginDrawing()` / `EndDrawing()` trong một frame không?
**Không nên.**
Ví dụ:

```cpp
BeginDrawing();
DrawCircle(100, 100, 20, RED);
EndDrawing();
BeginDrawing();
DrawCircle(200, 200, 20, BLUE);
EndDrawing();
```
Đoạn này có thể chạy trên một số hệ thống, nhưng **không phải cách sử dụng được khuyến nghị**. Mỗi lần `EndDrawing()` thường sẽ kết thúc frame hiện tại (swap buffer, đồng bộ FPS...), nên thông thường **mỗi vòng lặp chỉ nên có một cặp**: