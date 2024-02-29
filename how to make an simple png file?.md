# 16x16.png

## Vấn đề
Vẽ một hình tròn màu vàng trên nền màu đỏ
> Kích thước 16x16
> Làm từ con số 0

## Về biểu diễn dữ liệu tệp PNG

### Giá trị màu (Color values)
Các màu (Colors) có thể được biểu diễn bằng thang độ xám (grayscale) hoặc RGB (đỏ, lục, lam) dữ liệu mẫu (sample data). **Tìm hiểu chi tiết [tại đây](http://www.libpng.org/pub/png/spec/1.2/png-1.2.pdf)**

### Bố cục ảnh (Image layout)
Một ảnh PNG là một mảng các pixel hình chữ nhật. Kích thước của mỗi pixel được xác định bởi độ sâu của bit (bit depth). Độ sâu của bit (bit depth) là số bit được sử dụng cho mỗi mẫu (sample). **Tìm hiểu chi tiết [tại đây](http://www.libpng.org/pub/png/spec/1.2/png-1.2.pdf)**

## Về cấu trúc tệp PNG
Một tệp PNG luôn bắt đầu với một chữ kí (signature) và theo sau là các đoạn (chunks).

### Chữ kí của tệp PNG (PNG file signature)
Tám bytes đầu tiên của tệp PNG luôn chứa các giá trị `137 80 78 71 13 10 26 10` là chữ kí của tệp PNG.

### Bố cục các đoạn (Chunk layout)
Mỗi đoạn (chunk) luôn chứa bốn phần:

```
Length (4 bytes) | Chunk Type (4 bytes) | Chunk Data | CRC (4 bytes)
```

Length (4 bytes): độ dài của dữ liệu của đoạn (Chunk Data) không bao gồm loại của đoạn (Chunk Type), CRC và chính nó.
Chunk Type (4 bytes): có nhiều loại, tuy nhiên trong bài viết này chủ yếu chỉ nói đến các loại: IHDR, IDAT, IEND.
Chunk Data: dữ liệu của đoạn, nó phụ thuộc vào các loại đoạn khác nhau (Chunk Type).
CRC: được tính theo thuật toán CRC với đầu vào tính loại đoạn (Chunk Type) đến hết dữ liệu đoạn (Chunk Data).

**Tìm hiểu chi tiết [tại đây](http://www.libpng.org/pub/png/spec/1.2/png-1.2.pdf)**

### Thuật toán CRC (CRC algorithm)
Các phần CRC được tính bằng cách sử dụng phương thức tiêu chuẩn được định nghĩa trong ISO 3309 [ISO-3309] hoặc ITU-T V.42 [ITU-T-V42].
Đẳng thức CRC `x32 +x26 +x23 +x22 +x16 +x12 +x11 +x10+x8 +x7 +x5 +x4+x2 +x+1`

**Tìm hiểu chi tiết [tại đây](https://en.wikipedia.org/wiki/Cyclic_redundancy_check)**

## Tạo 16x16.png
### Đoạn IHDR (IHDR chunk)
Đoạn IHDR dùng để chứa các thông tin cơ bản của hình ảnh. Chi tiết ở sẽ được nêu ở phía dưới.

Dữ liệu của đoạn IHDR chứa các trường dữ liệu:
    - Chiều ngang (Width): 4 bytes
    - Chiều cao (Height): 4 bytes
    - Độ sâu của bit (Bit depth): 1 bytes
    - Loại màu (Color type): 1 bytes
    - Phương thức nén (Compression method): 1 bytes
    - Phương thức lọc (Filter method): 1 bytes
    - Phương thức đan xen (Interlace method): 1 bytes

Như vấn đề đã nêu, kích thước ảnh là 16x16 nên trường dữ liệu chiều ngang và chiều cao sẽ là 16.

Độ sâu của bit (Bit depth) là một số nguyên chỉ số bit được sử dụng cho một mẫu (sample). Các giá trị có thể chọn cho trường dữ liệu này là 1, 2, 3, 4, 8, 16. Để đơn giản chúng ta chọn giá trị này là 8.

Lọai màu (Color type) là một số nguyên dùng để mô tả dữ liệu hình ảnh. Các giá trị có thể chọn cho trường dữ liệu này bao gồm: 0, 1, 2, 3, 4 và 6 **(Tìm hiểu chi tiết [tại đây](http://www.libpng.org/pub/png/spec/1.2/png-1.2.pdf))**
. Ở đây chúng ta chọn giá trị này là 2. Để hình ảnh có thể hiện được các màu sắc.

Phương thức nén (Compression method) là một số nguyên để chỉ phương thức nén sẽ được sử dụng để nén dữ liệu ảnh. Hiện tại, chỉ có thể chọn giá trị 0 cho trường dữ liệu này.

Phương thức lọc (Filter method) là một số nguyên để chỉ phương thức tiền xử lí được áp dụng cho hình ảnh trước khi được nén. Hiện tại, chỉ có thể chọn giá trị 0 cho trường dữ liệu này.

Phương thức đan xen (Interlace method) là một số nguyên cho biết thứ tự truyền dữ liệu hình. Có hai giá trị có thể chọn là 0 và 1 **(Tìm hiểu chi tiết [tại đây](http://www.libpng.org/pub/png/spec/1.2/png-1.2.pdf))**. Để đơn giản chúng ta sẽ chọn giá trị là 0.

Đây là thông tin của đoạn IHDR của chúng ta:
    - Width: 16
    - Height: 16
    - Bit depth: 8
    - Color type: 2
    - Compression method: 0
    - Filter method: 0
    - Interlace method: 0

Giờ, từ các thông tin trên chúng ta sẽ xây dựng một chuỗi mã cho phần dữ liệu đoạn (Data chunk) cho đoạn IDHR này. Ở đây để thuận tiện cho việc trình bày chúng ta sẽ thể hiện dữ liệu bằng mã hexa. Theo đúng thứ tự từ trên xuống dưới chúng ta nối các dữ liệu với được chuỗi dữ liệu như sau:

```
0000 0010 0000 0010 0802 0000 00
```

Tiếp đến chúng ta sẽ nối thêm phần loại đoạn (Chunk type) vào phần dữ liễu trên.

```
4948 4452 0000 0010 0000 0010 0802 0000 00
```

Tiếp theo là phần độ dữ liệu. Dễ thấy dữ liệu có độ dài là 13. Nên chúng ta được dữ liệu:

```
0000 000D 4948 4452 0000 0010 0000 0010 0802 0000 00
```

Cuối cùng là tính CRC. CRC được tính từ phần dữ liệu của Chunk type đến kết thúc Chunk data. Sau khi tính toán chúng ta nhận được kết quả là `9091 6836`. Nối kết quả vào các phần dữ liệu khác được:

```
0000 000D 4948 4452 0000 0010 0000 0010 0802 0000 0090 9168 36
```

Đây cũng là đoạn dữ liệu IHDR hoàn chỉnh của chúng ta.

Tiếp đến sẽ xây dựng các đoạn cần thiết còn lại là IDAT và IEND.

### Đoạn IDAT (IDAT chunk)

Đây là đoạn chứa các dữ liệu hình ảnh thực tế. Để tạo được dữ liệu cho đoạn này cần:
1. Xây dựng dữ liệu thô như phần bố cục ảnh (image layout) đã nói ở phía trên theo các thông số chiều rộng, chiều dài ở đoạn IHDR.
2. Tiền sử xử lí dữ liệu theo phương thức lọc đã chọn ở đoạn IHDR.
3. Nén dữ liệu hình ảnh theo các phương thức nén đã chọn ở đoạn IHDR.

Đầu tiên, chúng ta sẽ xây dựng dữ liệu thô theo yêu cầu của vấn đề. Chúng ta sẽ xây dựng một mảng hình vuông với số dòng tương ứng với chiều cao, ở đây là 16 và số cột tương ứng với chiều rộng, ở đây cũng là 16.

Dễ thấy sẽ chỉ hai màu được sử dụng là màu đỏ và màu vàng. Do độ sâu của bit đã chọn là 8 và loại màu là 2. Nên chúng ta sẽ biểu diễn mỗi pixel của theo RGB với ba mẫu (đỏ, lục lam), mỗi mẫu có 8 bit. Vậy, chúng ta có thể nhận được mã màu tương ứng với hai màu đỏ và vàng là "FF0000" và "FFFF00" (do màu vàng là kết hợp tối đa màu đỏ và màu xanh).

Việc tiếp theo, cần xác định mỗi pixel nên được tô màu vàng hay màu đỏ. Để dễ dàng, chúng ta chỉ cần xác định rằng, nếu pixel đang xét thuộc hình tròn thì tô màu vàng còn ngược lại thì tô màu đỏ. Chúng ta có thể xác định một pixel có thuộc hình tròn hay không bằng cách tính khoảng cách của pixel đến tâm hình tròn theo công thức Pytharo.

```
d = sqrt((xp - xc)^2 + (yp - yc)^2)
```

Nếu khoảng cách tính được lớn hơn bán kính hình tròn thì pixel không thuộc hình tròn và ngược lại (Chúng ta có thể giản lược phép tính bằng cách so sánh độ dài trước khi bị lấy căn bậc 2 với bình phương bán kính để tiện cho việc tính toán).

Sau khi, tính toán chúng ta có thể nhận được một mảng hình vuông các pixel như sau:

```
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
```

Sau khi có được dữ liệu thô, chúng ta sẽ tiền xử lí dữ liệu bằng cách chỉ định cách lọc cho mỗi dòng dữ liệu. Có nhiều chỉ định, tuy nhiên để đơn giản chúng ta sẽ chọn chỉ định là loại 0, với loại này dữ liệu sẽ không bị thay đổi. **Tìm hiểu chi tiết [tại đây](http://www.libpng.org/pub/png/spec/1.2/png-1.2.pdf)**

Chúng ta chỉ cần chèn chỉ định vào đầu mỗi dòng dữ liệu. Sau tiền xử lí, dữ liệu của chúng ta sẽ như sau:

```
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FFFF00 FFFF00 FFFF00 FFFF00 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FFFF00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
00 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000 FF0000
```

Cuối cùng chúng ta sẽ nén dữ liệu theo phương thức nén đã chọn ở đoạn IHDR (hiện tại chỉ có một lựa chọn duy nhất). PNG nén dữ liệu theo thuật toán Deflate của zlip **(Tìm hiểu chi tiết [tại đây](https://www.zlib.net/feldspar.html))**. Thế nên chúng ta sẽ nén dữ liệu hình ảnh trên bằng zlib. Sau khi nén xong, dữ liệu mà chúng ta nhận được sẽ như thế này:

```
789c 63f8 cfc0 4012 224d f500 6bf8 4fbc 86ff a888 8086 ffd8 10f5 3460 558d aa87 de4e 2227 94c8 8907 7a25 0d1c 0800 5d05 2fdf
```

Thế là chúng ta đã có được dữ liệu cho đoạn này. Tiếp theo chúng ta sẽ xây dựng đoạn này bằng cách nối dữ liệu trên với các phần còn lại (tương tự như đoạn IHDR)

Đầu tiên là nối loại đoạn. Loại đoạn của đoạn này có mã là `4944 4154`. Đoạn mã trở thành:

```
4944 4154 789c 63f8 cfc0 4012 224d f500 6bf8 4fbc 86ff a888 8086 ffd8 10f5 3460 558d aa87 de4e 2227 94c8 8907 7a25 0d1c 0800 5d05 2fdf
```

Tiếp đến là phần kích thước dữ liệu. Độ dài dữ liệu ở đây là 32. Nên đoạn mã trở thành:

```
0000 0032 4944 4154 789c 63f8 cfc0 4012 224d f500 6bf8 4fbc 86ff a888 8086 ffd8 10f5 3460 558d aa87 de4e 2227 94c8 8907 7a25 0d1c 0800 5d05 2fdf
```

CRC của đoạn mã sau khi tính là `d75a 7c32`.

Cuối cùng, đoạn IDAT hoàn chỉnh sẽ như sau:

```
0000 0032 4944 4154 789c 63f8 cfc0 4012 224d f500 6bf8 4fbc 86ff a888 8086 ffd8 10f5 3460 558d aa87 de4e 2227 94c8 8907 7a25 0d1c 0800 5d05 2fdf d75a 7c32
```

### Đoạn IEND
Đoạn IEND (IEND chunk) dùng để báo kết thúc một tệp PNG. Đoạn này không có dữ liệu, nên chỉ cần xây dựng bằng cách nối ba phần còn lại.

Độ dài dữ liệu sẽ là 0. Loại đoạn có mã là `4945 4E44`. CRC sao khi tính toán sẽ là `AE42 6082`.

Đoạn IEND hoàn chỉnh sẽ như sau:

```
0000 0000 4945 4E44 AE42 6082
```

### Hoàn chỉnh tệp 16x16.png

Giờ, bằng cách nối các đoạn trên lại cùng với chữ kí của tệp PNG chúng ta sẽ được một tệp PNG hoàn chỉnh.

Các phần sẽ được nối theo biểu diễn phía dưới:

```
[PNG file signiture] [IHDR chunk] [IDAT chunk] [IEND chunk]
```

Cuối cùng tệp PNG hoàn chỉnh của chúng ta sẽ có dữ liệu như sau:

```
8950 4E47 0D0A 1A0A 0000 000D 4948 4452 0000 0010 0000 0010 0802 0000 0090 9168 36 0000 0032 4944 4154 789c 63f8 cfc0 4012 224d f500 6bf8 4fbc 86ff a888 8086 ffd8 10f5 3460 558d aa87 de4e 2227 94c8 8907 7a25 0d1c 0800 5d05 2fdf d75a 7c32 0000 0000 4945 4E44 AE42 6082
```

## Tham khảo
1. http://www.libpng.org/pub/png/spec/1.2/png-1.2.pdf
2. https://en.wikipedia.org/wiki/PNG
3. https://www.zlib.net/feldspar.html
4. https://en.wikipedia.org/wiki/Cyclic_redundancy_check
