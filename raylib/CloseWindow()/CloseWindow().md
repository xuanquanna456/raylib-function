`CloseWindow()` là hàm dọn dẹp của raylib , gọi khi game kết thúc. 

Nó làm gì:

+ Đóng cửa sổ game
+ Giải phóng context OpenGL và các tài nguyên của cửa sổ
+ Kết thúc phiên làm việc của raylib.

Nó đi cặp với `InitWindow()`: mở cửa sổ ở đầu chương trình, đóng ở cuối.

```cpp
InitWindow(960, 540, "TD");   // mở cửa sổ

while (!WindowShouldClose()) {
    // update + draw
}

CloseWindow();                // đóng cửa sổ, dọn dẹp
```

**Phân biệt với `WindowShouldClose()`:** hai hàm này hay bị nhầm.

- `WindowShouldClose()` chỉ **kiểm tra** xem người chơi có yêu cầu thoát không (bấm nút X hoặc phím ESC). Nó trả về `true`/`false`, không đóng gì cả.
- `CloseWindow()` mới là hàm **thực sự đóng** cửa sổ.


Mặc dù khi không dùng `CloseWindow()` chương trình vẫn có thể tự kết thúc do kết thúc chương trình nhưng chúng ta nên dùng `CloseWindow()` để giải phóng hết tất cả các bộ nhớ còn sót lại.

