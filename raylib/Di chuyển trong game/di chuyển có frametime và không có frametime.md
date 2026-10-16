+ Bình thường khi chúng ta chỉ cộng trừ toạ độ bình thường như sau:

```c
    while(!WindowShouldClose()){

        // 1. Event Handling
        if (IsKeyDown(KEY_RIGHT)){
            ballX += 3;
        }
        BeginDrawing();
        ClearBackground(GREEN);
        DrawCircle(ballX , ballY , 20 , RED);
        // 2. Updating Positions
        // 3. Drawing
        EndDrawing();

    }
    CloseWindow();
```

+ Tức là mỗi khung hình frame nó chỉ di chuyển sang phải 3 pixel. tức là với những người máy mạnh thì với 60 FPS trên 1 giây nó đã di chuyển sang phải được 180 pixel trong khi những người máy yếu chỉ di chuyển được 90 pixel trong một giây -> điều này gây ra sự mất cân bằng do đó chúng ta sẽ phải tìm một cách thống nhất sao cho máy yếu hay máy mạnh đều đi được quãng đường như nhau trong 1s.
-> Chúng ta phải cần đến khoảng thời gian giữa 2 khung hình liên tiếp còn được gọi là `FrameTime` . Frame time chính là khoảng thời gian giữa 2 khung hình. Nếu chúng ta lấy:

```c
tốc độ * FrameTime = quãng đường di chuyển của nhân vật.
```

- Từ đó chúng ta sẽ thống nhất được vấn đề di chuyển giữa các máy yếu , máy mạnh khác nhau. Tức là nó sẽ không còn phụ thuộc vào số Frame 1 giây nữa mà phụ thuộc vào khoảng cách giữa 2 Frame liên tiếpk

- Muốn lấy `FrameTime` , nếu muốn chuẩn xác thì dùng hàm [[GetFrameTime()]]



