### 1. Khái niệm

`LoadImage` đọc file ảnh từ ổ đĩa, giải mã và lưu dữ liệu pixel vào **RAM (CPU)**. Hàm chỉ nạp dữ liệu, không hiển thị gì lên màn hình.

cpp

```cpp
Image LoadImage(const char *fileName);
```

Tham số là đường dẫn file. Kiểu ảnh được nhận dạng theo **phần mở rộng** (`.png`, `.jpg`...).

### 2. Struct `Image`

Hàm trả về một struct gồm vài trường:

cpp

```cpp
typedef struct Image {
    void *data;      // con trỏ tới dữ liệu pixel trong RAM
    int width;       // chiều rộng
    int height;      // chiều cao
    int mipmaps;     // số mipmap (thường là 1)
    int format;      // định dạng pixel (PixelFormat)
} Image;
```

Bạn đọc được `img.width`, `img.height` ngay sau khi load, ví dụ để căn giữa hoặc tính tỉ lệ.

### 3. Định dạng file hỗ trợ

Thường gặp là **PNG, JPG, BMP, TGA, GIF, QOI, PSD, HDR**, ngoài ra có thêm vài định dạng nén cho GPU (DDS, KTX, ASTC...). Danh sách thực tế phụ thuộc vào cấu hình lúc biên dịch raylib, nhưng bản cài thông thường đã có PNG và JPG.

- **PNG** hợp cho sprite vì hỗ trợ **nền trong suốt** (kênh alpha).
- **JPG** không có alpha nên không làm nền trong suốt được.

### 4. Không cần cửa sổ

`LoadImage` chỉ làm việc trên RAM, nên **gọi trước `InitWindow()` vẫn được**. Điều này khác với `LoadTexture`, vì texture nằm trên GPU nên bắt buộc có cửa sổ trước.

### 5. Quản lý bộ nhớ

Dữ liệu pixel được cấp phát động, nên **phải giải phóng bằng `UnloadImage`**, nếu không sẽ rò rỉ bộ nhớ.

cpp

```cpp
Image img = LoadImage("assets/apple.png");
// ... dùng img ...
UnloadImage(img);
```

Các lưu ý quan trọng:

- Sau `LoadTextureFromImage(img)`, texture đã có **bản sao riêng trên GPU**, nên bạn có thể `UnloadImage` ngay.
- **Không gọi `UnloadImage` hai lần** cho cùng một ảnh.
- Gán `Image b = a;` chỉ sao chép **con trỏ**, không sao chép pixel. Hai biến cùng trỏ vào một vùng nhớ, nên unload một cái thì cái kia hỏng. Muốn bản sao thật thì dùng `ImageCopy(a)`.

### 6. Khi load thất bại

Nếu sai đường dẫn hoặc file hỏng, raylib **không dừng chương trình**. Nó chỉ in cảnh báo ra console và trả về `Image` rỗng (`data` là `NULL`, kích thước 0). Nên kiểm tra:

cpp

```cpp
Image img = LoadImage("assets/apple.png");
if (!IsImageValid(img)) {
    TraceLog(LOG_ERROR, "Khong load duoc anh");
}
```

Lỗi hay gặp nhất là đường dẫn sai vì raylib tìm theo **thư mục làm việc hiện tại** (working directory), không phải thư mục chứa file `.cpp`.

### 7. Quy trình thông dụng

```
LoadImage → chỉnh sửa (ImageXxx) → LoadTextureFromImage → UnloadImage → DrawTexture
```

cpp

```cpp
InitWindow(800, 600, "Demo");

Image img = LoadImage("assets/apple.png");
ImageResizeNN(&img, 30, 30);                    // đổi cỡ, giữ nét pixel
ImageFlipHorizontal(&img);                      // lật ngang
Texture2D tex = LoadTextureFromImage(img);
UnloadImage(img);
```

Các hàm chỉnh sửa nhận **con trỏ** `Image*`, nên bạn truyền `&img`.

### 8. Các hàm xử lý ảnh hay dùng

|Nhóm|Hàm|
|---|---|
|Đổi kích thước|`ImageResize` (mượt), `ImageResizeNN` (sắc nét, hợp pixel art)|
|Cắt, lấy một phần|`ImageCrop`, `ImageFromImage(img, rect)` (lấy một ô từ sprite sheet)|
|Biến đổi|`ImageFlipHorizontal/Vertical`, `ImageRotateCW/CCW`|
|Màu|`ImageColorTint`, `ImageColorGrayscale`, `ImageColorInvert`|
|Đọc pixel|`GetImageColor(img, x, y)`, `LoadImageColors` (kèm `UnloadImageColors`)|
|Vẽ lên ảnh|`ImageDrawText`, `ImageDrawRectangle`, `ImageDraw`|
|Lưu ra file|`ExportImage(img, "out.png")`|

### 9. Các hàm cùng họ

|Hàm|Dùng để|
|---|---|
|`LoadImageAnim(file, &frames)`|Đọc ảnh động (GIF), các frame nối nhau trong `data`|
|`LoadImageFromMemory(".png", data, size)`|Đọc ảnh từ bộ nhớ thay vì file|
|`LoadImageRaw(...)`|Đọc file dữ liệu pixel thô|
|`LoadImageFromTexture(tex)`|Lấy ngược dữ liệu từ GPU về RAM|
|`LoadImageFromScreen()`|Chụp màn hình thành `Image`|

### 10. Một số ứng dụng

- **Đặt icon cửa sổ:** `SetWindowIcon(img)`. Đây là trường hợp phải dùng `Image` chứ không phải `Texture2D`.
- **Cắt sprite sheet** thành từng ô rồi tạo texture cho từng ô.
- **Phát hiện va chạm theo pixel** hoặc đọc bản đồ từ ảnh: mỗi pixel là một ô địa hình, đọc bằng `GetImageColor`. Cách này tiện để vẽ map tower defense bằng một file ảnh nhỏ.

### 11. Lưu ý hiệu năng

Đừng gọi `LoadImage` hoặc `LoadTexture` **bên trong vòng lặp game**. Đọc file và giải mã rất chậm, làm game giật. Hãy load một lần trước vòng `while`, rồi unload sau vòng lặp.

Với đa số trường hợp bạn chỉ cần vẽ ảnh, dùng thẳng `LoadTexture("file.png")` là đủ. `LoadImage` có ích khi cần **chỉnh sửa ảnh trước khi vẽ** hoặc **đọc dữ liệu pixel**.