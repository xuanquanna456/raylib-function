`UnloadTexture` là hàm của raylib dùng để **giải phóng một texture khỏi bộ nhớ GPU (VRAM)**.

cpp

```cpp
void UnloadTexture(Texture2D texture);
```

cpp

```cpp
InitWindow(800, 600, "Snake");
Texture2D tex = LoadTexture("assets/apple.png");   // chiếm VRAM

while (!WindowShouldClose()) {
    BeginDrawing();
        DrawTexture(tex, 100, 100, WHITE);
    EndDrawing();
}

UnloadTexture(tex);    // trả VRAM lại
CloseWindow();
```

**Vì sao cần:** texture nằm trên GPU, và dữ liệu đó không tự mất khi biến `tex` hết phạm vi. Không gọi `UnloadTexture` thì VRAM bị chiếm mãi cho đến khi chương trình kết thúc. Với game nhỏ ít thấy hậu quả, nhưng nếu bạn load texture lặp đi lặp lại (ví dụ trong vòng lặp, hoặc mỗi lần đổi màn chơi) thì VRAM sẽ cạn dần.

**So với `UnloadImage`:**

||`UnloadImage`|`UnloadTexture`|
|---|---|---|
|Giải phóng|RAM (`Image`)|VRAM (`Texture2D`)|
|Gọi khi|Xong bước chỉnh sửa, thường ngay sau `LoadTextureFromImage`|Khi không vẽ texture đó nữa, thường cuối chương trình|
|Cần cửa sổ còn mở|Không|**Có**|

**Quy tắc cần nhớ:**

- Phải gọi **trước `CloseWindow()`**, vì texture phụ thuộc vào context OpenGL mà `CloseWindow()` sẽ hủy. Gọi sau thì có thể lỗi hoặc treo.
- Gọi **đúng một lần** cho mỗi texture. `Texture2D b = a;` chỉ sao chép `id`, nên unload `a` thì `b` cũng không còn dùng được, và unload cả hai là giải phóng hai lần.
- Không vẽ texture sau khi đã unload.
- Load texture nào thì unload texture đó: `LoadTexture` hoặc `LoadTextureFromImage` đều đi với `UnloadTexture`.

Trong class, thường đặt `UnloadTexture` trong destructor (như ví dụ `Sprite` ở câu trước), nhưng nhớ để đối tượng bị hủy **trước** `CloseWindow()`: tạo nó trong `main()` sau `InitWindow()` chứ đừng để làm biến toàn cục.

Nếu bạn có nhiều texture, một cách gọn là gom vào một chỗ và unload cùng lúc trước khi đóng cửa sổ:

cpp

```cpp
UnloadTexture(appleTex);
UnloadTexture(snakeHeadTex);
UnloadTexture(bgTex);
CloseWindow();
```