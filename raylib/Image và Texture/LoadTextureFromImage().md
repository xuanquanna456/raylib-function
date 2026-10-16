`LoadTextureFromImage` **chuyển một `Image` (dữ liệu ảnh trong RAM) thành một `Texture2D` (dữ liệu ảnh trên GPU)** để có thể vẽ lên màn hình.

cpp

```cpp
Texture2D LoadTextureFromImage(Image image);
```

cpp

```cpp
InitWindow(800, 600, "Snake");               // phải có cửa sổ trước

Image img = LoadImage("assets/apple.png");   // file → RAM
Texture2D tex = LoadTextureFromImage(img);   // RAM → GPU
UnloadImage(img);                            // bỏ bản RAM, tex vẫn dùng được

// trong vòng lặp:
DrawTexture(tex, 100, 100, WHITE);

// cuối chương trình:
UnloadTexture(tex);
CloseWindow();
```

**Vì sao cần:** `Image` chỉ để xử lý pixel, không vẽ trực tiếp được. Muốn vẽ thì phải đưa lên GPU dưới dạng `Texture2D`, và hàm này làm việc đó.

**Ba điều cần nhớ:**

1. Phải gọi **sau `InitWindow()`**, vì texture cần context OpenGL. Gọi trước thì `tex.id = 0`, ảnh sẽ không hiện.
2. Sau khi tạo, texture là **bản sao riêng** trên GPU. Bạn có thể `UnloadImage` ngay, và sửa `Image` về sau không làm texture đổi theo.
3. Texture phải được giải phóng bằng `UnloadTexture` trước `CloseWindow()`.

**Khi nào dùng:** khi bạn muốn chỉnh ảnh trước khi vẽ (resize, cắt, lật, đổi màu) hoặc tạo ảnh bằng code (`GenImageColor`, `GenImageChecked`...). Nếu chỉ cần vẽ ảnh nguyên bản thì dùng `LoadTexture("file.png")`, vì hàm đó đã làm sẵn cả ba bước load, chuyển và unload.