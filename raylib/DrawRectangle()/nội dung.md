`DrawRectangle(int posX , int posY , int width , int height , Color color)` là Một hàm để vẽ một **hình chữ nhật đặc** (tô kín), giống `DrawCircle` nhưng cho hình chữ nhật.

| Tham số        | Ý nghĩa                                        |
| -------------- | ---------------------------------------------- |
| `posX`, `posY` | Tọa độ **góc trên bên trái** của hình chữ nhật |
| `width`        | Chiều rộng (pixel)                             |
| `height`       | Chiều cao (pixel)                              |
| `color`        | Màu                                            |

```cpp
BeginDrawing();
    ClearBackground(RAYWHITE);
    DrawRectangle(100, 50, 200, 80, BLUE);   // chữ nhật xanh, rộng 200, cao 80
EndDrawing();
```