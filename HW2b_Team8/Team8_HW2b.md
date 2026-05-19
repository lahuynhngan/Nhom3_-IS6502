# Linear Regression

Linear Regression, hay hồi quy tuyến tính, là một thuật toán học có giám sát được sử dụng để dự đoán một giá trị số liên tục dựa trên một hoặc nhiều biến đầu vào. Thuật toán này giả định rằng giữa biến đầu vào và biến cần dự đoán tồn tại một mối quan hệ gần tuyến tính.

Ví dụ, nếu cần dự đoán giá nhà, các biến đầu vào có thể là diện tích, số phòng, vị trí hoặc tuổi của căn nhà. Giá nhà là biến cần dự đoán. Linear Regression sẽ cố gắng tìm ra một phương trình tuyến tính mô tả mối quan hệ giữa các biến đầu vào và giá trị đầu ra.

## Ý tưởng chính

Ý tưởng chính của Linear Regression là tìm một đường thẳng, hoặc một mặt phẳng trong trường hợp có nhiều biến, sao cho đường hoặc mặt phẳng đó nằm gần các điểm dữ liệu thật nhất có thể.

Với Simple Linear Regression, mô hình chỉ có một biến độc lập. Công thức có dạng:

```text
y = b0 + b1*x
```

Trong đó:

- `y` là giá trị cần dự đoán.
- `x` là biến đầu vào.
- `b0` là hệ số chặn, tức giá trị dự đoán khi `x = 0`.
- `b1` là hệ số góc, cho biết khi `x` tăng thêm 1 đơn vị thì `y` thay đổi bao nhiêu.

Với Multiple Linear Regression, mô hình có nhiều biến độc lập. Công thức có dạng:

```text
y = b0 + b1*x1 + b2*x2 + ... + bp*xp
```

Trong đó `x1, x2, ..., xp` là các biến đầu vào, còn `b1, b2, ..., bp` là các hệ số tương ứng với từng biến. Mỗi hệ số thể hiện mức độ ảnh hưởng của biến đó đến giá trị dự đoán, khi các biến còn lại được giữ nguyên.

## Cách Linear Regression học

Linear Regression học bằng cách tìm các hệ số `b0, b1, ..., bp` sao cho sai số giữa giá trị dự đoán và giá trị thực tế là nhỏ nhất.

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

## Multiple Linear Regression

Multiple Linear Regression mở rộng từ Simple Linear Regression bằng cách sử dụng nhiều biến đầu vào thay vì chỉ một biến. Về bản chất, cách tính vẫn nhằm tìm ra các hệ số tốt nhất để giảm sai số dự đoán.

Tuy nhiên, khi có nhiều biến, việc viết công thức riêng cho từng hệ số sẽ dài và khó theo dõi. Vì vậy, dữ liệu thường được biểu diễn dưới dạng ma trận. Mỗi dòng của ma trận `X` là một mẫu dữ liệu, mỗi cột là một biến đầu vào.

Ví dụ, nếu mỗi căn nhà có 2 thuộc tính là diện tích và số phòng, ma trận `X` trông như sau:

```text
X = [
    [50, 2],    # căn nhà 1: 50m², 2 phòng
    [60, 3],    # căn nhà 2: 60m², 3 phòng
    [80, 4],    # căn nhà 3: 80m², 4 phòng
    [70, 3],    # căn nhà 4: 70m², 3 phòng
]
```

Phương trình dự đoán có dạng `y = b0 + b1*x1 + b2*x2`. Nếu viết lại dưới dạng nhân vector:

```text
y = [b0, b1, b2] · [1, x1, x2]
```

`b0` cần nhân với `1` để xuất hiện trong phép tính. Vì vậy ta tạo ra `X_new` bằng cách thêm một cột toàn số `1` vào đầu ma trận `X`, gọi là **bias column**. Nhờ đó `b0` được tính chung với các hệ số còn lại trong cùng một công thức ma trận:

```text
X gốc (4×2):       X_new (4×3, thêm cột 1):

[ 50  2 ]          [ 1  50  2 ]
[ 60  3 ]    →     [ 1  60  3 ]
[ 80  4 ]          [ 1  80  4 ]
[ 70  3 ]          [ 1  70  3 ]
                       ↑
                   cột mới, luôn = 1
```

`X_new` là 4×3 — **không vuông**, không tính được nghịch đảo trực tiếp.

Khi đó, toàn bộ các hệ số được gom lại thành một vector:

```text
B = [b0, b1, b2, ..., bp]
```

Trước khi đi vào công thức, cần hiểu hai phép toán trên ma trận:

**Transpose (chuyển vị)** là phép đổi hàng thành cột và cột thành hàng. Phần tử ở hàng `i` cột `j` sẽ chuyển sang hàng `j` cột `i`:

```text
X_new (4×3):                  XT (3×4):

[ 1   50   2 ]                [ 1   1   1   1  ]
[ 1   60   3 ]      →         [ 50  60  80  70 ]
[ 1   80   4 ]                [ 2   3   4   3  ]
[ 1   70   3 ]
```

**Inverse (nghịch đảo)** sinh ra từ bài toán: làm sao tìm được `B` khi biết `XTX * B = XTY`?

Với số thường, bài toán tương tự là `3 * b = 6`. Để tìm `b`, ta chia cả hai vế cho `3`:

```text
3 * b = 6
(1/3) * 3 * b = (1/3) * 6
1 * b = 2
b = 2
```

Lý do `(1/3) * 3 = 1` là vì `1/3` là nghịch đảo của `3`. Nhân một số với nghịch đảo của nó luôn ra `1`, và nhân `1` với bất kỳ số nào thì không đổi — nên `b` đứng một mình.

Với ma trận, ta làm y hệt. `inverse(XTX)` là nghịch đảo của `XTX`, với tính chất:

```text
inverse(XTX) * XTX = I    (ma trận đơn vị, giống số 1 trong thế giới ma trận)
```

Trong đó `I` là ma trận có `1` trên đường chéo và `0` ở các ô còn lại. Nhân bất kỳ ma trận nào với `I` thì không đổi, giống như nhân số với `1`.

Áp dụng vào bài toán tìm `B`:

```text
XTX * B = XTY
inverse(XTX) * XTX * B = inverse(XTX) * XTY    ← nhân cả hai vế với inverse(XTX)
I * B = inverse(XTX) * XTY                       ← inverse(XTX) * XTX = I
B = inverse(XTX) * XTY                           ← I * B = B
```

Chỉ ma trận vuông (số hàng bằng số cột) mới có nghịch đảo, đó là lý do ta cần `XTX` vuông trước khi tính.

Mục tiêu là tìm `B` sao cho `X_new * B = Y`. Dùng lại ví dụ 4 căn nhà với 2 biến (diện tích và số phòng):

```text
X_new (4×3):          Y:
[ 1   50   2 ]        [ 1.7 ]
[ 1   60   3 ]        [ 2.0 ]
[ 1   80   4 ]        [ 2.5 ]
[ 1   70   3 ]        [ 2.2 ]
```

Mở ra ta thấy đây là 4 phương trình nhưng chỉ có 3 ẩn (`b0`, `b1`, `b2`):

```text
1*b0 + 50*b1 + 2*b2 = 1.7
1*b0 + 60*b1 + 3*b2 = 2.0
1*b0 + 80*b1 + 4*b2 = 2.5
1*b0 + 70*b1 + 3*b2 = 2.2
```

Muốn giải thì cần "chia" cả hai vế cho `X_new`, tức nhân với `inverse(X_new)`. Nhưng `X_new` là 4×3, **không vuông**, nên không có nghịch đảo — máy tính sẽ báo lỗi nếu cố tính.

**Thủ thuật:** nhân cả hai vế với `XT` (kích thước 3×4):

```text
XT (3×4)  *  X_new (4×3)  =  XTX (3×3)   ← vuông!
XT (3×4)  *  Y (4×1)      =  XTY (3×1)
```

`XTX` luôn có kích thước `(số biến + 1) × (số biến + 1)` — vuông bất kể có bao nhiêu mẫu. Lúc này hệ phương trình chỉ còn **3 phương trình, 3 ẩn** — giải được bằng nghịch đảo:

```text
B = inverse(XTX) * XTY
```

Tóm lại chuỗi lý do:

```text
X_new không vuông
  → không tính được inverse(X_new)
  → không giải được B trực tiếp
  → nhân cả hai vế với XT
  → XTX luôn vuông (bất kể bao nhiêu mẫu)
  → tính được inverse(XTX)
  → giải ra B
```

Công thức đầy đủ này còn được gọi là **Normal Equation**:

```text
B = inverse(transpose(X) * X) * transpose(X) * Y
```

### Ví dụ tính tay

Giả sử có 4 căn nhà với diện tích và số phòng như sau:

| Căn nhà | Diện tích (m²) | Số phòng | Giá (tỷ) |
| ------- | -------------- | -------- | -------- |
| 1       | 50             | 2        | 1.7      |
| 2       | 60             | 3        | 2.0      |
| 3       | 80             | 4        | 2.5      |
| 4       | 70             | 3        | 2.2      |

**Bước 1:** Chuẩn bị ma trận `X` và vector `Y`.

```text
X = [ [50, 2],          Y = [1.7,
      [60, 3],               2.0,
      [80, 4],               2.5,
      [70, 3] ]              2.2]
```

**Bước 2:** Thêm cột `1` vào đầu `X` để tính được `b0`. `X_new` lúc này là 4×3 — không vuông.

```text
X_new (4×3) = [ [1,  50,  2],
                [1,  60,  3],
                [1,  80,  4],
                [1,  70,  3] ]
```

**Bước 3:** Chuyển vị `X_new` — đổi hàng thành cột. `XT` là 3×4.

```text
XT (3×4) = [ [1,   1,   1,   1 ],
              [50,  60,  80,  70],
              [2,   3,   4,   3 ] ]
```

**Bước 4:** Tính `XTX = XT * X_new`. Kết quả là 3×3 — vuông, tính được nghịch đảo.

```text
XTX[0][0] = 1+1+1+1                     = 4
XTX[0][1] = 50+60+80+70                 = 260
XTX[0][2] = 2+3+4+3                     = 12
XTX[1][0] = 260  (= XTX[0][1], XTX là ma trận đối xứng)
XTX[1][1] = 50²+60²+80²+70²             = 17400
XTX[1][2] = 50*2+60*3+80*4+70*3         = 810
XTX[2][0] = 12   (= XTX[0][2])
XTX[2][1] = 810  (= XTX[1][2])
XTX[2][2] = 2²+3²+4²+3²                 = 38

XTX = [ [4,    260,   12 ],
         [260,  17400, 810],
         [12,   810,   38 ] ]
```

**Bước 5:** Tính `XTY = XT * Y`.

```text
XTY[0] = 1.7+2.0+2.5+2.2                        = 8.4
XTY[1] = 50*1.7+60*2.0+80*2.5+70*2.2            = 85+120+200+154 = 559
XTY[2] = 2*1.7+3*2.0+4*2.5+3*2.2                = 3.4+6.0+10.0+6.6 = 26.0

XTY = [8.4, 559, 26.0]
```

**Bước 6:** Tính `B = inverse(XTX) * XTY`. `XTX * B = XTY` là hệ phương trình viết gọn dưới dạng ma trận. Mở ra:

```text
hàng 0:   4*b0 +   260*b1 +   12*b2 =   8.4
hàng 1: 260*b0 + 17400*b1 +  810*b2 = 559.0
hàng 2:  12*b0 +   810*b1 +   38*b2 =  26.0
```

3 phương trình, 3 ẩn. Giải bằng nghịch đảo:

```text
B = inverse(XTX) * XTY = [0.5, 0.02, 0.1]
```

Tức là: `b0 = 0.5`, `b1 = 0.02`, `b2 = 0.1`

Phương trình thu được: `giá = 0.5 + 0.02 * diện_tích + 0.1 * số_phòng`

Kiểm tra lại với căn nhà 1: `0.5 + 0.02*50 + 0.1*2 = 0.5 + 1.0 + 0.2 = 1.7 tỷ` ✓

**Dự đoán:** Căn nhà mới có diện tích 75m², 4 phòng:

```text
predicted_y = 0.5 + 0.02*75 + 0.1*4 = 0.5 + 1.5 + 0.4 = 2.4 tỷ đồng
```

### Giới hạn của Multiple Linear Regression

Normal Equation yêu cầu ma trận `XTX` phải khả nghịch, tức là phải tồn tại ma trận nghịch đảo của nó. Điều này có thể không thỏa mãn trong hai trường hợp: khi số lượng mẫu dữ liệu ít hơn số lượng biến đầu vào, hoặc khi có hiện tượng **đa cộng tuyến** (multicollinearity), nghĩa là một số biến đầu vào có tương quan tuyến tính cao với nhau. Khi đó, mô hình sẽ không thể tính được nghiệm hoặc nghiệm trở nên không ổn định.

### Mã giả Multiple Linear Regression

```text
FUNCTION MultipleLinearRegression(X, Y):

    n = số hàng của X      # n = số mẫu dữ liệu, ví dụ 3 căn nhà thì n = 3
    p = số cột của X       # p = số biến đầu vào, ví dụ diện tích + số phòng thì p = 2

    # Tạo ma trận X_new bằng cách thêm cột 1 vào đầu X
    # Mục đích: để b0 được tính chung với b1, b2,... trong cùng một công thức
    # Ví dụ: [[50,2],[60,3]] → [[1,50,2],[1,60,3]]
    X_new = ma trận mới có n hàng và p + 1 cột

    FOR i = 0 TO n - 1:               # duyệt qua từng mẫu dữ liệu
        X_new[i][0] = 1               # cột đầu tiên luôn là 1
        FOR j = 0 TO p - 1:           # duyệt qua từng biến đầu vào
            X_new[i][j + 1] = X[i][j] # sao chép giá trị từ X sang, dịch sang phải 1 cột

    XT = transpose(X_new)  # đổi hàng thành cột: [[1,1,1],[50,60,80],[2,3,4]]

    XTX = XT * X_new       # nhân XT với X_new → tạo ra ma trận vuông để có thể nghịch đảo
    XTY = XT * Y           # nhân XT với Y → vế phải của phương trình

    B = inverse(XTX) * XTY # giải phương trình → tìm ra b0, b1, b2,...

    RETURN B               # trả về [b0, b1, b2,...] là các hệ số của mô hình


FUNCTION PredictMultipleLinearRegression(B, x_new):

    y_predicted = B[0]                    # bắt đầu bằng b0 (hệ số chặn)

    FOR j = 1 TO length(B) - 1:          # duyệt qua từng hệ số b1, b2,...
        y_predicted = y_predicted + B[j] * x_new[j - 1]  # cộng thêm bj * xj

    RETURN y_predicted  # kết quả dự đoán: b0 + b1*x1 + b2*x2 + ...
```
