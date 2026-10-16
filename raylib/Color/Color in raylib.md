Color color = {red , green , blue , alpha} là một object để tạo màu sắc trong raylib

Với màu được tạo từ 3 tổ hợp R , G , B và alpha là độ trong suốt.

Muốn tạo biến làm màu thì làm như sau:

```c
Color green = {20 , 160 , 133 , 255} ;

while(!WindowShouldClose){
	BeginDrawing();
	ClearBackground(green);
	EndDrawing();
}


```

