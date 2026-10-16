`GetFrameTime()` trả về **thời gian (tính bằng giây) mà frame trước đó đã mất để chạy xong** tức là khoảng thời gian giữa 2 frame (khung hình) liên tiếp , kiểu `float`.

Nếu game chạy 60 FPS thì mỗi frame mất khoảng 1/60 ≈ **0.0167 giây**, nên `GetFrameTime()` trả về xấp xỉ 0.0167. Giá trị này thường được gọi là **delta time** (`dt`).

**Vì sao cần nó:** giả sử bạn viết `x += 5;` mỗi frame.

- Máy chạy 60 FPS: nhân vật đi 5 × 60 = 300 pixel/giây
- Máy chạy 144 FPS: đi 5 × 144 = 720 pixel/giây
- Máy yếu chỉ 30 FPS: đi 150 pixel/giây

Tức là cùng một game nhưng nhanh chậm khác nhau tùy máy. Khi nhân với `dt` thì tốc độ được tính theo **giây thực** chứ không theo frame: