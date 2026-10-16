`DrawCircle(int center_x , int center_y , float radius , Color color)` là một hàm dùng để vẽ một hình tròn trên Raylib. 
Với:
+ `x` : là toạ độ trục x
+ `y` : là toạ độ trục y
+ `radius` : là bán kính của hình tròn
- `color`: màu, ví dụ `RED`, `BLUE`, `RAYWHITE`, hoặc tự tạo `Color{255, 100, 0, 255}` (R, G, B, A) hoặc `GetColor()` để nhận dạng = mã màu

**A là Alpha**, tức độ **đục / trong suốt** của màu.

- `0` = hoàn toàn trong suốt (không thấy gì)
- `255` = hoàn toàn đặc (che hẳn thứ nằm dưới)
- Giá trị ở giữa = bán trong suốt, màu trộn với thứ nằm phía sau

Mỗi kênh R, G, B, A là 1 byte nên có giá trị từ 0 đến 255.

cpp

```cpp
Color red_solid = {255, 0, 0, 255};   // đỏ đặc
Color red_half  = {255, 0, 0, 128};   // đỏ bán trong suốt (~50%)
Color red_none  = {255, 0, 0, 0};     // vô hình

DrawCircle(200, 200, 40, BLUE);
DrawCircle(230, 200, 40, red_half);   // thấy được vòng tròn xanh xuyên qua phần đỏ
```

