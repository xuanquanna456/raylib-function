
`BeginDrawing()` và `EndDrawing()` trong raylib là cặp hàm đánh dấu điểm bắt đầu và kết thúc của một khung hình (frame) vẽ.

**BeginDrawing()** khởi tạo một canvas mới để vẽ trong frame hiện tại — sau khi gọi hàm này, mọi lệnh vẽ (như `DrawText`, `DrawRectangle`, `DrawCircle`...) sẽ tác động lên buffer đang được chuẩn bị hiển thị.

**EndDrawing()** kết thúc quá trình vẽ của frame đó: nó hoán đổi buffer (swap buffers - kỹ thuật double buffering) để hiển thị những gì vừa vẽ lên màn hình, đồng thời xử lý các sự kiện liên quan đến cửa sổ (window events) và đầu vào (input polling).
Mọi đoạn code vẽ trong raylib phải nằm giữa hai hàm này. Ví dụ:
```c
while (!WindowShouldClose())
{
    BeginDrawing();

        ClearBackground(RAYWHITE);  // xóa nền trước khi vẽ
        DrawText("Hello, raylib!", 190, 200, 20, LIGHTGRAY);

    EndDrawing();
}
```
Một vài lưu ý:
- Thường thì ngay sau `BeginDrawing()`, người ta gọi `ClearBackground()` để xóa nội dung frame trước đó, tránh hiện tượng "bóng ma" (ghosting).
- Cấu trúc này nằm trong vòng lặp game chính (game loop), thường đi cùng `WindowShouldClose()` để kiểm tra khi nào người dùng muốn thoát.
- Cơ chế double buffering giúp tránh hiện tượng xé hình (screen tearing): bạn vẽ vào một buffer "ẩn", rồi `EndDrawing()` mới đưa nó lên màn hình, trong khi buffer hiển thị trước đó được giữ nguyên cho đến lúc hoán đổi.
Nếu bạn dùng các tính năng vẽ 3D, còn có cặp `BeginMode3D()`/`EndMode3D()` lồng bên trong, hoạt động theo nguyên tắc tương tự nhưng chuyển sang không gian 3D với camera.


==Lưu ý vẽ ngoài vùng begin và end vẫn vẽ được nhưng đôi khi sẽ gây lỗi nên không nên vẽ ngoài các vùng này==