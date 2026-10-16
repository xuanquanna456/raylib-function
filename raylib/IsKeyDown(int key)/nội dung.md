`IsKeyDown(int key)` kiểm tra xem một phím **có đang được giữ** ở thời điểm hiện tại hay không. Trả về `true` trong **mọi frame** mà phím còn bị nhấn.

cpp

```cpp
if (IsKeyDown(KEY_RIGHT)) player.x += 200 * GetFrameTime();
if (IsKeyDown(KEY_LEFT))  player.x -= 200 * GetFrameTime();
```

Giữ phím thì nhân vật di chuyển liên tục, thả ra thì dừng.

**Phân biệt với các hàm cùng nhóm:** đây là chỗ hay bị nhầm.

|Hàm|Trả về `true` khi|
|---|---|
|`IsKeyDown(key)`|Phím **đang được giữ** (nhiều frame liên tiếp)|
|`IsKeyPressed(key)`|Phím **vừa mới nhấn xuống** (chỉ đúng 1 frame)|
|`IsKeyReleased(key)`|Phím **vừa được thả ra** (chỉ đúng 1 frame)|
|`IsKeyUp(key)`|Phím **đang không bị nhấn**|

**Khi nào dùng cái nào:**

- Di chuyển liên tục (đi, chạy, kéo camera): `IsKeyDown`
- Hành động một lần (nhảy, bắn một phát, mở menu, pause): `IsKeyPressed`

cpp

```cpp
if (IsKeyPressed(KEY_SPACE)) Shoot();    // mỗi lần nhấn bắn 1 viên
if (IsKeyDown(KEY_SPACE))    Shoot();    // giữ là bắn mỗi frame, 60 viên/giây
```

**Tên phím** là hằng số `KEY_...`: `KEY_A`...`KEY_Z`, `KEY_SPACE`, `KEY_ENTER`, `KEY_ESCAPE`, `KEY_UP/DOWN/LEFT/RIGHT`, `KEY_LEFT_SHIFT`, `KEY_ONE`...

**Với chuột** có bộ hàm tương tự: `IsMouseButtonDown`, `IsMouseButtonPressed`, `IsMouseButtonReleased`, với `MOUSE_BUTTON_LEFT` / `MOUSE_BUTTON_RIGHT`. Trong tower defense, bạn sẽ dùng `IsMouseButtonPressed(MOUSE_BUTTON_LEFT)` để đặt tower: nếu dùng `Down` thì giữ chuột một lúc sẽ đặt ra nhiều tower chồng lên nhau.

Hãy nhân tốc độ với `GetFrameTime()` như ví dụ trên để tốc độ di chuyển không phụ thuộc vào FPS.