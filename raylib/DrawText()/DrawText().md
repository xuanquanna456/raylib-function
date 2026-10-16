`DrawText` vẽ một dòng chữ lên màn hình

```c
void DrawText(const char *text , int posX , int posY , int fontSize , Color color);
```

|Tham số|Ý nghĩa|
|---|---|
|`text`|Chuỗi cần vẽ|
|`posX`, `posY`|Tọa độ **góc trên bên trái** của chữ|
|`fontSize`|Cỡ chữ (pixel)|
|`color`|Màu|


```cpp
BeginDrawing();
    ClearBackground(BLACK);
    DrawText("PONG", 590, 20, 40, WHITE);
EndDrawing();
```

Hàm này dùng font mặc định của raylib, không cần load gì thêm. Giống các hàm vẽ khác, phải gọi giữa `BeginDrawing()` và `EndDrawing()`.


**Vẽ số (ví dụ điểm):** `DrawText` chỉ nhận chuỗi kiểu C, không nhận `int` trực tiếp. Có hai cách:


```cpp
int score = 5;

// Cách 1: TextFormat của raylib (giống printf)
DrawText(TextFormat("Score: %d", score), 100, 20, 30, WHITE);

// Cách 2: to_string của C++, nhớ .c_str()
DrawText(to_string(score).c_str(), 100, 20, 30, WHITE);
```

`TextFormat` còn dùng được với `%f`, `%s`, `%02d`..., ví dụ `TextFormat("FPS: %.1f", 1.0f / GetFrameTime())`. Lưu ý kết quả của `TextFormat` chỉ tồn tại tạm thời (vài lần gọi gần nhất dùng chung bộ đệm), nên hãy dùng ngay, đừng lưu con trỏ lại.

**Căn giữa chữ:** vì tọa độ là góc trên bên trái, muốn chữ nằm giữa thì đo độ rộng bằng `MeasureText`:

cpp

```cpp
const char* msg = "Player 1 wins!";
int w = MeasureText(msg, 40);                       // độ rộng chữ ở cỡ 40
DrawText(msg, SCREEN_WIDTH / 2 - w / 2, 300, 40, YELLOW);
```

**Áp dụng cho Pong:** hiển thị điểm hai bên đường kẻ giữa:

cpp

```cpp
DrawText(TextFormat("%d", score1), SCREEN_WIDTH / 4,     20, 60, WHITE);
DrawText(TextFormat("%d", score2), SCREEN_WIDTH * 3 / 4, 20, 60, WHITE);
```

**Các hàm liên quan:**

|Hàm|Dùng để|
|---|---|
|`DrawFPS(x, y)`|Hiện FPS hiện tại, tiện để kiểm tra hiệu năng|
|`DrawTextEx(font, text, pos, size, spacing, color)`|Vẽ bằng font tự load (`LoadFont`), có chỉnh khoảng cách chữ|
|`MeasureText(text, fontSize)`|Đo độ rộng chữ, để căn giữa hoặc căn phải|

Font mặc định không hiển thị tiếng Việt có dấu. Nếu cần chữ có dấu thì phải tự load font hỗ trợ và nạp thêm các ký tự đó, còn chữ không dấu hoặc tiếng Anh thì dùng `DrawText` bình thường.