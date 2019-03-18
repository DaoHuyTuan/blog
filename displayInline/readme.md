# Spacing trong display:inline-block

Hẳn ở đây chẳng ai xa lạ gì với 
``display: inline-block`` nhưng sẽ có rất nhiều người (trong đó có tui) không hiểu rõ về nó và hôm qua tui có gặp vấn đề và đã được **"mặt trời chân lý chói qua tim"** cùng xét qua vd nhé

```css
.main {
    width:800px;
}
```
tôi có 1 div main cha lớn với width 800px

```css
.item {
    width:200px;
}
```
và class con với width 200px

```html
<div class="main">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
</div>
```
Cấu trúc HTML của tôi thế này 1 div cha và có 4 div con bên trong

> Với việc mỗi .item có width là 200px tôi dễ dàng đoán được chúng vừa khít với .main
```html
<div class="main">
    <div class="item red">1</div>
    <div class="item green">2</div>
    <div class="item yellow">3</div>
    <div class="item blue">4</div>
</div>
```
```css
.red {
    background-color:red;
}
.green {
    background-color:green;
}
.yellow {
    background-color:yellow;
}
.blue {
    background-color:blue;
}
```
Thêm 1 chút màu sắc cho dễ phân biệt 
![First png](first.png)

> Giờ tôi muốn chúng phải thẳng hàng với nhau dĩ nhiên tôi phải set display: inline-block cho .item

```css
.item {
    width:200px;
    display: inline-block;
}
```

Và bùm 

![Second png](second.png)

Ơ chúng có thẳng hàng thật nhưng .item cuối cùng bị nhãy xuống dưới 1 dòng ??? mặc dù width của div cha là 800px ?? . Và nếu để ý chúng ta dễ dàng thấy có 1 khoản trống nhỏ giữa các block với nhau. 
Rỏ ràng tôi không set margin, padding hay left/right cho item vậy lý do là gì ??

Nguyên do là cái ``display: inline-block`` kia có cách hiển thị khác 1 tí 

> **inline-block**:
This value causes an element to generate an inline-level block container. The inside of an inline-block is formatted as a block box, and the element itself is formatted as an atomic inline-level box.

Theo bác w3.org

Nhưng hiểu đơn giản ``display:inline-block`` là sẽ hiển thị các phần tử theo kiểu "block of word" dịch tiếng việt chắc là "khối từ" cũng giống như việc bạn gõ chữ các từ được cách nhau với **khoản trắng** và ``display:inline-block`` cũng thế các class .item cách nhau đúng 1 khoảng cách dù bạn có tạo hay không.

> Điều này sẽ làm khó cho bạn nếu bạn muốn làm slider hay các custom khác với các element thẳng hàng nhưng dính sát vào nhau

###Giải quyết

Có rất nhiều cách để giãi quyết vấn đề này mình sẽ liệt kê bên dưới:
![Three png](three.png)
**Thánh "Chuyên Gia"**

```css
.main {
    width:800px;
    display:flex;
}
```
Sử dụng flexbox là 1 trong những cách đầu tiên cho vấn đề này, nhưng flexbox chỉ hổ trợ các trình duyệt "xịn",
 những trình duyệt cũ hơn sẽ không áp dụng được [(Xem chi tiết Flexbox ở đây)](https://caniuse.com/#search=flex)

**Người Bình Thường**
```css 
.item {
    width:200px;
    display: block;
    float: left;
}
```
Đổi **.item** thành ``display:block`` và cho ``float:left`` cũng là cách khá đơn giản

**UFO**

```css 
.item {
    width:200px;
    display: inline-block;
    margin-right:-4px;
}
```
Lần đầu thấy margin âm ??
**Khủng Long**
```html
<div class="item red">1</div><div class="item green">2</div><div class="item yellow">3</div><div class="item blue">4</div>
```
Lúc trên tôi nói nó có spacing mà đúng không =))) vậy đừng để có spacing nữa =))) (đéo đùa đâu chạy thật đấy =))) )

**Bánh Bèo**

```html 
    <div class="item red">1</div><!---->
    <div class="item green">2</div><!---->
    <div class="item yellow">3</div><!---->
    <div class="item blue">4</div><!---->
```

Trên là những hướng giãi quyết khác nhau cho vấn đề này không rõ còn những cách khác không. Hy vọng mấy bác sẽ chém mạnh mạnh tay tí cho tại hạ tỉ thí phân trình cao thấp =)))
Viết bài lần đầu có gì sai sót mấy bác bỏ qua

**Thanks các bác đã bỏ time ngồi đọc #TIL**