hàm `CheckCollisionCircleRec` . Hàm này kiểm tra hình tròn có va chạm với hình chữ nhật hay không.

```cpp
bool CheckCollisionCircleRec(Vector2 center , float radius , Rectangle rec);
```

|Tham số|Ý nghĩa|
|---|---|
|`center`|Tâm hình tròn (`Vector2`)|
|`radius`|Bán kính|
|`rec`|Hình chữ nhật `Rectangle{x, y, width, height}` (`x, y` là góc trên bên trái)|

Trả về `true` nếu hai hình **chạm hoặc chồng lên nhau**, ngược lại `false`.

**Dùng trong Pong của bạn** (bóng va vợt):


```cpp
Rectangle rec1 = { paddle1.x, paddle1.y, (float)paddle1.width, (float)paddle1.height };
Rectangle rec2 = { paddle2.x, paddle2.y, (float)paddle2.width, (float)paddle2.height };

if (CheckCollisionCircleRec({ ball.x, ball.y }, ball.radius, rec1) ||
    CheckCollisionCircleRec({ ball.x, ball.y }, ball.radius, rec2)) {
    ball.speed_x *= -1;   // bóng bật ngược lại
}
```

Cú pháp `{ ball.x, ball.y }` tự tạo ra một `Vector2` ngay tại chỗ.