đây là một hàm kiểm tra xem có cần phải đóng window không , trả về true hoặc false
```cpp
#include<raylib.h>
int main(){
    InitWindow(800 , 800 , "my first raylib game");
    while(WindowShouldClose() == false)
    {

  
  
  

    }
    CloseWindow();
    return 0;
}
```
Ctrình sẽ chạy khi should close = false;
