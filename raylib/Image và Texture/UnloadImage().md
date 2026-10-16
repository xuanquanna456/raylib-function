`UnloadImage` là hàm của raylib dùng để **giải phóng bộ nhớ RAM của một `Image`**, tức là phần dữ liệu pixel được cấp phát khi bạn gọi `LoadImage` hoặc các hàm `GenImage...`.

cpp

```cpp
void UnloadImage(Image image);
```

cpp

```cpp
Image img = LoadImage("assets/apple.png");   // cấp phát RAM
Texture2D tex = LoadTextureFromImage(img);   // sao chép lên GPU
UnloadImage(img);                            // trả RAM lại, tex vẫn dùng được
```

**Vì sao cần:** `Image` giữ pixel bằng một con trỏ cấp phát động (`img.data`). Không gọi `UnloadImage` thì vùng nhớ đó không được trả lại, gây rò rỉ bộ nhớ. Nó giống `delete` đối với `new`, hoặc `fclose` đối với `fopen`.

**Quy tắc cần nhớ:**

- Gọi **đúng một lần** cho mỗi ảnh. Gọi hai lần có thể làm chương trình sập.
- Không dùng `Image` sau khi đã unload.
- Chỉ giải phóng RAM. Texture trên GPU phải giải phóng riêng bằng `UnloadTexture`.
- `Image b = a;` chỉ sao chép con trỏ, nên unload `a` thì `b` cũng hỏng. Muốn bản sao độc lập thì dùng `ImageCopy(a)`.

**Liên hệ với câu hỏi về destructor vừa rồi:** `UnloadImage` thuộc nhóm hàm “dọn dẹp” mà bạn thường đặt trong destructor của class:

cpp

```cpp
class Sprite {
    Texture2D tex;
public:
    Sprite(const char* file) {
        Image img = LoadImage(file);
        tex = LoadTextureFromImage(img);
        UnloadImage(img);          // ảnh RAM chỉ cần lúc khởi tạo
    }
    ~Sprite() { UnloadTexture(tex); }   // texture mới là thứ sống lâu, nên hủy ở đây
};
```

Ở ví dụ này `UnloadImage` được gọi ngay trong constructor vì `Image` chỉ là bước trung gian, còn destructor lo cho `Texture2D`.