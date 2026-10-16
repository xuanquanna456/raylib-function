`ClearBackground(Color color)` **xóa toàn bộ màn hình (khung hình hiện tại) và tô kín bằng một màu**, thường gọi ngay đầu mỗi frame.

```c
BeginDrawing();
    ClearBackground(RAYWHITE);   // xóa frame cũ, nền trắng
    DrawCircle(480, 270, 30, RED);
EndDrawing();
```

