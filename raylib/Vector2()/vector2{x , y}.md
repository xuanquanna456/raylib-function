
`Vector2` là kiểu dữ liệu của raylib chứa hai số `float` : `x` và  `y` . Nó thường để lưu vị trí , vận tốc hoặc hướng trong 2D

```c
typedef struct Vector2 { float x; float y; } Vector2;
```

**Khi viết `Vector2()` với ngoặc tròn:**

cpp

```cpp
Vector2 a = Vector2();        // tạo vector {0, 0}
Vector2 b = Vector2{3, 4};    // tạo vector {3, 4}, luôn đúng
Vector2 c = Vector2(3, 4);    // tạo {3, 4}, nhưng chỉ biên dịch được từ C++20
```

- `Vector2()` không có đối số thì luôn được, cho ra vector `{0, 0}`.
- `Vector2(3, 4)` kiểu gọi hàm chỉ chạy khi bạn biên dịch với **C++20 trở lên**. Bản cũ hơn (C++11/14/17) sẽ báo lỗi, vì `Vector2` là struct kiểu C không có constructor. Ở câu trước mình nói là “không dùng được”, chính xác hơn là “không dùng được ở C++ cũ”.
- `Vector2{3, 4}` với ngoặc nhọn thì chạy ở mọi phiên bản C++ từ C++11, nên đây là cách an toàn nhất.

**Các cách tạo vector trong thực tế:**

cpp

```cpp
Vector2 p = { 100, 200 };

p.x = 150;        // đổi 100 thành 150
p.y = 300;        // đổi 200 thành 300

p.x += 10;        // cộng thêm 10 vào x
p.y -= 5;         // trừ 5 ở y

float a = p.x;    // đọc giá trị
```

Muốn dùng `Vector2(3, 4)` thì thêm `set(CMAKE_CXX_STANDARD 20)` vào CMakeLists. Nếu không thì cứ dùng ngoặc nhọn.