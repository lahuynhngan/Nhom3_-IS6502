# Linear Regression

Linear Regression, hay hồi quy tuyến tính, là một thuật toán học có giám sát được sử dụng để dự đoán một giá trị số liên tục dựa trên biến đầu vào. Thuật toán này giả định rằng giữa biến đầu vào và biến cần dự đoán tồn tại một mối quan hệ gần tuyến tính.

Ví dụ, nếu cần dự đoán giá nhà dựa trên diện tích, diện tích là biến đầu vào còn giá nhà là biến cần dự đoán. Linear Regression sẽ cố gắng tìm ra một phương trình tuyến tính mô tả mối quan hệ giữa biến đầu vào và giá trị đầu ra.

## Ý tưởng chính

Ý tưởng chính của Linear Regression là tìm một đường thẳng sao cho đường đó nằm gần các điểm dữ liệu thật nhất có thể.

Với Simple Linear Regression, mô hình chỉ có một biến độc lập. Công thức có dạng:

```text
y = b0 + b1*x
```

Trong đó:

- `y` là giá trị cần dự đoán.
- `x` là biến đầu vào.
- `b0` là hệ số chặn, tức giá trị dự đoán khi `x = 0`.
- `b1` là hệ số góc, cho biết khi `x` tăng thêm 1 đơn vị thì `y` thay đổi bao nhiêu.

## Cách Linear Regression học

Linear Regression học bằng cách tìm các hệ số `b0` và `b1` sao cho sai số giữa giá trị dự đoán và giá trị thực tế là nhỏ nhất.

Với mỗi điểm dữ liệu, mô hình tạo ra một giá trị dự đoán:

```text
predicted_y = b0 + b1*x
```

Sau đó, sai số được tính bằng hiệu giữa giá trị thực tế và giá trị dự đoán:

```text
error = actual_y - predicted_y
```

Mục tiêu của thuật toán là làm cho tổng sai số của toàn bộ dữ liệu nhỏ nhất. Thông thường, Linear Regression sử dụng tổng bình phương sai số vì cách này giúp tránh trường hợp sai số âm và sai số dương triệt tiêu lẫn nhau.

```text
sum_squared_error = sum((actual_y - predicted_y)^2)
```

Nói đơn giản, thuật toán sẽ tìm bộ hệ số làm cho các dự đoán càng gần dữ liệu thật càng tốt.

## Simple Linear Regression

Trong Simple Linear Regression, vì chỉ có một biến đầu vào nên ta có thể tính trực tiếp hai hệ số `b0` và `b1`.

Trước tiên, thuật toán tính giá trị trung bình của `X` và `Y`. Sau đó, nó đo xem `X` và `Y` thay đổi cùng nhau như thế nào. Nếu `X` tăng và `Y` cũng thường tăng, hệ số `b1` sẽ có giá trị dương. Nếu `X` tăng nhưng `Y` thường giảm, hệ số `b1` sẽ có giá trị âm.

Sau khi tính được `b1`, thuật toán dùng giá trị trung bình của `X` và `Y` để tính `b0`. Khi đã có `b0` và `b1`, mô hình có thể dự đoán giá trị mới bằng công thức:

```text
predicted_y = b0 + b1*x_new
```

Về mặt tính toán, Simple Linear Regression thường tính hệ số theo các bước sau:

1. Tính số lượng quan sát `n`.
2. Tính tổng các giá trị của `X` và `Y`.
3. Tính giá trị trung bình của `X` và `Y`:

```text
mean_x = sum_x / n
mean_y = sum_y / n
```

4. Tính mức độ thay đổi chung giữa `X` và `Y`. Phần này được gọi là tử số. Với mỗi điểm, ta nhân độ lệch của `X` với độ lệch của `Y` so với trung bình: nếu cả hai cùng lớn hơn hoặc cùng nhỏ hơn trung bình, tích này dương, tức `X` và `Y` có xu hướng đồng biến:

```text
numerator = sum((X[i] - mean_x) * (Y[i] - mean_y))
```

5. Tính mức độ biến thiên của riêng `X`. Phần này được gọi là mẫu số. Nó cho biết `X` trải rộng như thế nào so với trung bình, dùng để chuẩn hóa tử số thành hệ số góc thực sự:

```text
denominator = sum((X[i] - mean_x)^2)
```

6. Tính hệ số góc `b1`:

```text
b1 = numerator / denominator
```

7. Tính hệ số chặn `b0`:

```text
b0 = mean_y - b1 * mean_x
```

Sau khi có `b0` và `b1`, mô hình có thể dự đoán giá trị mới bằng cách thay `x_new` vào phương trình tuyến tính. Vì vậy, phần tính toán của Simple Linear Regression gồm hai giai đoạn: học hệ số từ dữ liệu ban đầu và dùng hệ số đó để dự đoán dữ liệu mới.

### Ví dụ tính tay

Giả sử có 5 căn nhà với diện tích (`X`, đơn vị m²) và giá (`Y`, đơn vị tỷ đồng) như sau:

| Căn nhà | Diện tích X | Giá Y |
| ------- | ----------- | ----- |
| 1       | 50          | 2.0   |
| 2       | 60          | 2.5   |
| 3       | 70          | 3.0   |
| 4       | 80          | 3.5   |
| 5       | 90          | 4.0   |

**Bước 1–3:** Tính trung bình.

```text
n = 5
sum_x = 50 + 60 + 70 + 80 + 90 = 350  →  mean_x = 350 / 5 = 70
sum_y = 2 + 2.5 + 3 + 3.5 + 4 = 15    →  mean_y = 15 / 5 = 3
```

**Bước 4:** Tính tử số — đo mức độ `X` và `Y` thay đổi cùng nhau.

```text
numerator = (50-70)(2-3) + (60-70)(2.5-3) + (70-70)(3-3) + (80-70)(3.5-3) + (90-70)(4-3)
          = (-20)(-1)   + (-10)(-0.5)     + (0)(0)       + (10)(0.5)      + (20)(1)
          = 20 + 5 + 0 + 5 + 20
          = 50
```

**Bước 5:** Tính mẫu số — đo mức độ phân tán của riêng `X`.

```text
denominator = (-20)^2 + (-10)^2 + 0^2 + 10^2 + 20^2
            = 400 + 100 + 0 + 100 + 400
            = 1000
```

**Bước 6–7:** Tính hệ số.

```text
b1 = 50 / 1000 = 0.05
b0 = 3 - 0.05 * 70 = 3 - 3.5 = -0.5
```

Phương trình thu được: `y = -0.5 + 0.05 * x`

Nghĩa là mỗi m² diện tích tăng thêm, giá nhà tăng thêm 0.05 tỷ đồng.

**Dự đoán:** Căn nhà mới có diện tích 75 m²:

```text
predicted_y = -0.5 + 0.05 * 75 = -0.5 + 3.75 = 3.25 tỷ đồng
```

### Mã giả Simple Linear Regression

```text
FUNCTION SimpleLinearRegression(X, Y):

    n = length(X)  # n = số mẫu dữ liệu, ví dụ 5 căn nhà thì n = 5

    sum_x = 0
    sum_y = 0

    FOR i = 0 TO n - 1:       # duyệt qua từng mẫu, cộng dồn X và Y
        sum_x = sum_x + X[i]
        sum_y = sum_y + Y[i]

    mean_x = sum_x / n         # tính trung bình X
    mean_y = sum_y / n         # tính trung bình Y


    numerator = 0    # tử số: đo mức độ X và Y thay đổi cùng nhau
    denominator = 0  # mẫu số: đo mức độ phân tán của X

    FOR i = 0 TO n - 1:
        x_diff = X[i] - mean_x          # độ lệch của X[i] so với trung bình
        y_diff = Y[i] - mean_y          # độ lệch của Y[i] so với trung bình

        numerator   = numerator   + x_diff * y_diff  # cộng dồn tích hai độ lệch
        denominator = denominator + x_diff * x_diff  # cộng dồn bình phương độ lệch X


    b1 = numerator / denominator    # hệ số góc: X tăng 1 đơn vị thì Y tăng bao nhiêu
    b0 = mean_y - b1 * mean_x      # hệ số chặn: giá trị Y khi X = 0

    RETURN b0, b1


FUNCTION PredictSimpleLinearRegression(b0, b1, x_new):

    y_predicted = b0 + b1 * x_new  # thay x_new vào phương trình để dự đoán

    RETURN y_predicted
```
