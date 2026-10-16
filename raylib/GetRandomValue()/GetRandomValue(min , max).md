`GetRandomValue(min, max)` trả về **một số nguyên ngẫu nhiên trong khoảng từ `min` đến `max`, tính cả hai đầu**.

```cpp
int GetRandomValue(int min, int max);
```

Vậy `GetRandomValue(0, 1)` trả về **0 hoặc 1** với xác suất xấp xỉ 50/50, giống tung đồng xu.

cpp

```cpp
GetRandomValue(1, 6);      // giống xúc xắc: 1, 2, 3, 4, 5 hoặc 6
GetRandomValue(-100, 100); // từ -100 đến 100
```

Khác với `rand() % n` của C, hàm này **bao gồm cả `max`**: `GetRandomValue(0, 1)` có thể ra 1, còn `rand() % 1` luôn ra 0.

**Dùng trong Pong:** chọn ngẫu nhiên hướng bay của bóng khi bắt đầu hoặc sau mỗi lần ghi điểm:

cpp

```cpp
void ResetBall() {
    x = GetScreenWidth() / 2;
    y = GetScreenHeight() / 2;

    int dirs[2] = { -1, 1 };
    speed_x *= dirs[GetRandomValue(0, 1)];   // đi trái hoặc phải
    speed_y *= dirs[GetRandomValue(0, 1)];   // đi lên hoặc xuống
}
```

Cách viết khác tương đương:

cpp

```cpp
if (GetRandomValue(0, 1) == 0) speed_x *= -1;
```

Lưu ý: `speed_x *= dirs[...]` chỉ **đổi dấu** chứ không đặt lại độ lớn. Nếu `speed_x` đang âm và bạn nhân với -1 thì ra dương, nên hướng thực tế vẫn ngẫu nhiên nhưng không còn phụ thuộc hướng cũ. Nếu muốn đặt tốc độ cố định rồi chọn hướng, hãy gán thẳng: `speed_x = 500 * dirs[GetRandomValue(0, 1)];`.

**Hạt giống ngẫu nhiên (seed):** nếu mỗi lần chạy game bạn thấy dãy số giống hệt nhau, hãy đặt seed theo thời gian ngay sau `InitWindow`:

cpp

```cpp
#include <ctime>

InitWindow(...);
SetRandomSeed((unsigned int)time(NULL));
```

Tùy phiên bản raylib mà seed có được tự đặt hay không, nên gọi thêm dòng trên là chắc ăn nhất.

**Trong tower defense,** hàm này hợp để tạo sai số cho đạn, chọn ngẫu nhiên loại enemy xuất hiện trong wave, hoặc quyết định enemy rơi vật phẩm. Ví dụ `if (GetRandomValue(1, 100) <= 20)` là xác suất 20%.