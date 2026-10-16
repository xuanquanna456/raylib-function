`DrawLine(int startPosX , int startPosY , int endPosX , int endPosY , Color color)`

`DrawLine` vẽ một **đoạn thẳng** từ điểm này đến điểm kia. Tên đúng viết hoa chữ **L**: `DrawLine`.

cpp

```cpp
void DrawLine(int startPosX, int startPosY, int endPosX, int endPosY, Color color);
```

|Tham số|Ý nghĩa|
|---|---|
|`startPosX`, `startPosY`|Tọa độ điểm đầu|
|`endPosX`, `endPosY`|Tọa độ điểm cuối|
|`color`|Màu|

cpp

```cpp
BeginDrawing();
    ClearBackground(RAYWHITE);
    DrawLine(100, 100, 400, 250, RED);   // đoạn thẳng đỏ từ (100,100) đến (400,250)
EndDrawing();
```

**Hạn chế:** `DrawLine` luôn vẽ đường **mảnh 1 pixel**, không chỉnh được độ dày. Nên có các biến thể:

| Hàm                                                              | Dùng để                                       |
| ---------------------------------------------------------------- | --------------------------------------------- |
| `DrawLineV(Vector2 start, Vector2 end, Color)`                   | Giống `DrawLine` nhưng nhận `Vector2`         |
| `DrawLineEx(Vector2 start, Vector2 end, float thick, Color)`     | Vẽ đường có **độ dày** tùy chỉnh              |
| `DrawLineBezier(Vector2 start, Vector2 end, float thick, Color)` | Vẽ đường cong mềm (Bezier)                    |
| `DrawLineStrip(Vector2* points, int count, Color)`               | Nối nhiều điểm liên tiếp thành đường gấp khúc |