`DrawTexture` **vẽ một texture lên màn hình** tại vị trí cho trước, giữ nguyên kích thước gốc của ảnh.

cpp

```cpp
void DrawTexture(Texture2D texture, int posX, int posY, Color tint);
```

|Tham số|Ý nghĩa|
|---|---|
|`texture`|Texture đã load (`LoadTexture` hoặc `LoadTextureFromImage`)|
|`posX`, `posY`|Tọa độ **góc trên bên trái** của ảnh|
|`tint`|Màu phủ lên ảnh|

cpp

```cpp
InitWindow(800, 600, "Snake");
Texture2D apple = LoadTexture("assets/apple.png");

while (!WindowShouldClose()) {
    BeginDrawing();
        ClearBackground(BLACK);
        DrawTexture(apple, 200, 150, WHITE);   // vẽ quả táo tại (200, 150)
    EndDrawing();
}

UnloadTexture(apple);
CloseWindow();
```

**Tham số `tint` (màu phủ):**

- `WHITE` giữ nguyên màu gốc của ảnh, đây là giá trị bạn dùng nhiều nhất
- Màu khác sẽ nhân với màu ảnh: `RED` làm ảnh ngả đỏ, `GRAY` làm ảnh tối đi
- Alpha trong `tint` điều chỉnh độ trong suốt: `Fade(WHITE, 0.5f)` cho ảnh mờ 50%

cpp

```cpp
DrawTexture(apple, 200, 150, Fade(WHITE, 0.5f));   // táo bán trong suốt
```

**Lưu ý:**

- Tọa độ là **góc trên bên trái**, giống `DrawRectangle`, khác với `DrawCircle` dùng tâm. Muốn đặt tâm ảnh tại `(cx, cy)` thì vẽ ở `cx - tex.width/2`, `cy - tex.height/2`.
- Hàm này **không co giãn, không xoay**. Ảnh 128×128 sẽ chiếm đúng 128×128 pixel. Nếu ô lưới của bạn chỉ 30×30 thì dùng `DrawTexturePro`, hoặc thu nhỏ ảnh trước bằng `ImageResize`.
- Không hiện ảnh thường do load sai đường dẫn, hoặc `LoadTexture` được gọi trước `InitWindow`.
- Gọi giữa `BeginDrawing()` và `EndDrawing()`, và cái vẽ sau sẽ đè lên cái vẽ trước.
- `tex.width` và `tex.height` cho biết kích thước ảnh gốc.

**Các hàm cùng họ:**

|Hàm|Dùng để|
|---|---|
|`DrawTextureV(tex, Vector2 pos, tint)`|Vị trí dạng `Vector2`|
|`DrawTextureEx(tex, pos, rotation, scale, tint)`|Xoay và phóng to/thu nhỏ đều|
|`DrawTextureRec(tex, source, pos, tint)`|Vẽ **một phần** ảnh (cắt sprite sheet)|
|`DrawTexturePro(tex, source, dest, origin, rotation, tint)`|Đầy đủ nhất: cắt, đổi kích thước, xoay, đặt điểm neo|

Trong game rắn, mỗi ô lưới là `cell` pixel thì cách vẽ táo vào đúng ô là:

cpp

```cpp
Rectangle src  = { 0, 0, (float)apple.width, (float)apple.height };
Rectangle dest = { gridX * (float)cell, gridY * (float)cell, (float)cell, (float)cell };
DrawTexturePro(apple, src, dest, { 0, 0 }, 0.0f, WHITE);
```