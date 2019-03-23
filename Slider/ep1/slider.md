##Series: Tạo độ khó: Carousel

Hẳn với rất nhiều bác ở đây làm web dev cũng đều biết đến Carousel, hay có tên gọi khác như slidershow. Nhưng có mấy người thật sự hiểu cách hoạt động của carousel ??
Bài viết này gồm 2 phần tương đương với 2 cách implement carousel thông dụng nhất 

- **Clone Element** (cách mà các lib carousel rất hay sử dụng vd:Slick,Owl ...)
- **Cách Bootstrap làm carousel** (cách mà vài thư viện css đình đám hay sử dụng vd: bootstrap,foundation)


##Clone Element
##1. Demo 
![Image of Slider](https://media.giphy.com/media/2dlm0ZA53Tv9necMcy/giphy.gif)
##2. Setup project

**HTML**
```html
index.html

<div class="main">
    <button id="preButton" class="btn"></button>
    <div id="slider">
        <div class="item red">1</div>
        <div class="item blue">2</div>
        <div class="item yellow">3</div>  
    </div>
    <button id="nextButton" class="btn"></button>
</div>
```
- Ta có 1 khối main lớn nhất bao tất cả slider của ta bên trong, 1 div slider chứa các item slide, và item là div chứa nội dung bạn muốn hiển thị trên mỗi slide
- nextButton và preButton cho 2 arrow control slider 

**Javascript**
```js 
main.js

var nextBtn = document.getElementById("nextButton");
var preBtn = document.getElementById("preButton");
var slider = document.getElementById("slider");
var widthItem = document.querySelector(".item").offsetWidth;
var numItem = slider.querySelectorAll(".item").length;
var currentPosition = 0;
// 1 biến để giúp ta control được slider đang ở đâu
var widthSlider = numItem * widthItem;
// ta sẽ lấy số lượng item trong slide nhân với width của mỗi item để ra toàn bộ width của slider 
nextBtn.addEventListener("click",nextSlider);
preBtn.addEventListener("click",backSlider);
```

**CSS**
```css
style.css 

* {
    box-sizing: border-box; 
}
.main {
    width:600px;
    height:300px;
    overflow:hidden;
    position:relative;
}
#slider {
    width:4000px;
    height:300px;
    display:flex;
    overflow:hidden;
    position: absolute;
    transition: 0.5s;
    left:-600px;
}
.item {
    width:600px;
    height:300px;
    display:inline-block;
    background-color:red;
}
.btn {
    outline: transparent;
}
#preButton {
    width:20px;
    height:20px;
    border-radius:100%;
    position:absolute;
    top:130px;
    left:20px;
    background-color:#eeeded;
    z-index:1;
    cursor:pointer;
    
}
#preButton:after {
    content:"";
    width:50%;
    height:50%;
    background-image: url("./left.svg");
    display: block;
    position: absolute;
    top:5px;
    left:5px;
}
#nextButton {
    width:20px;
    height:20px;
    border-radius:100%;
    position:absolute;
    background-color:#eeeded;
    z-index:1;
    bottom:150px;
    right:20px;
    cursor:pointer;
    
}
#nextButton:after {
    content:"";
    background-image: url("./right.svg");
    width:50%;
    height:50%;
    display: block;
    position: absolute;
    top:5px;
    left:5px;
}
.red {
    background-color:red;
}
.blue {
    background-color:blue;
}
.yellow {
    background-color:yellow;
}
.pink {
    background-color:pink;
}
```
##3. Cách thức hoạt động 
- Tạo các clone element 1 cách tự động vì không thể nào người dùng phải tự gõ để tạo ra clone elements
- Sẽ phải có sự kiện khi click vào các arrow để thực hiện translate
- Chúng ta sẽ có 3 slide item để làm hiệu ứng slide ta sẽ dùng ``transform:translateX()`` + ``transition:0.5s``

- Ta cần làm phần loop ta sẽ dùng clone elements cho hiệu ứng này 
> ta sẽ giãi quyết từng case nêu trên

##4. Get Thing Done 
####**Clone Elements**

```js 
const createClone = function genClone() {
    const numItems = slider.getElementsByClassName("item").length;
    const firstChild = slider.getElementsByClassName("item").item(0); // xác định first slide
    const lastChild = slider.getElementsByClassName("item").item(numItems -1); // xác định last slide
    const cloneLastChild = lastChild.cloneNode(true); // thực hiện tạo ra 1 last slide mới 
    const cloneFirstChild = firstChild.cloneNode(true); // thực hiện tạo ra 1 first slide mới 
    cloneLastChild.classList += " clone"; // 
    cloneFirstChild.classList += " clone"; // add class clone (bạn có thể add bất cứ class gì bạn muốn)
    slider.insertBefore(cloneLastChild,firstChild); // thêm last slide vào phía trước slide đầu tiên 
    slider.insertBefore(cloneFirstChild,lastChild.nextSibling); // thêm first slide vào cuối slider 
    numItem += 2;
}
createClone(); // gọi hàm createClone đầu tiên để slide khởi tạo sẽ tạo clone trước 
```

Và đây là cây dom sau khi gọi ``createClone`` khi inspect console

![Dom after gen](https://media.giphy.com/media/LZkOAlPD7PmkPHzuuC/giphy.gif)

####**Slide Effect**

```js
function nextSlider(){
    currentPosition -= widthItem;
    checkPosition(currentPosition);
}

function backSlider(){
    currentPosition += widthItem;
    checkPosition(currentPosition); 
}
// currentPosition là vị trí hiện tại của slider
// mỗi lần slide , slider sẽ di chuyển 1 đoạn đúng bằng với width của 1 slide item (widthItem)
// hàm checkPosition thực hiện di chuyển slider 

function checkPosition(newValue) {
    distantSlide = "translate("+ (newValue) + "px)";
    slider.style.transform = distantSlide;
    // xử lý mỗi lần di chuyển sẽ gán giá trị mới cho thuộc tính transform của slider
}
```

#####**Loop**
Điểm này rất quan trọng mọi logic của slide ở đây  
**1. Next slide**
```js
function checkPosition(newValue) {
    distantSlide = "translate("+ (currentPosition) + "px)";
    slider.style.transform = distantSlide;

   if(currentPosition == -(widthItem*(numItem -2)))  {
        slider.style.transform = "translateX(" + currentPosition + "px)";   
        setTimeout(function(){
            slider.style.transition = "0s";
            currentPosition = 0;
            slider.style.transform = "translateX(" + currentPosition + "px)"; 
        }, 400);
   }else {
        slider.style.transition = "0.5s";
   }
    // khi slider tới vị trí của slide cuối cùng nếu bấm next slide tiếp thì sẽ thực hiện translate đến vị trí của clone element cuối slide 
    // clone element cuối slide ở đây là 
    // <div class="item red clone">1</div>
    // ngay khi slide đến clone element cuối ngay lập tức remove transition của slider và dịch chuyển về slide đầu tiên của slider 
    // và sau khi dịch chuyển về slide đầu tiên add transition trở lại slider
    // hình dưới đây sẽ giãi thích vì sao cần remove transition
```

![Dom after gen](https://media.giphy.com/media/RekMhNS08UO0dqEXuG/giphy.gif)


---
**2. Previous Slide**
```js
    if(currentPosition == widthItem) {
        console.log(currentPosition)
        slider.style.transform = "translateX(" + currentPosition + "px)";
        setTimeout(function(){
            slider.style.transition = "0s";
            currentPosition = -(widthItem*(numItem -3));
            slider.style.transform = "translateX(" + currentPosition + "px)"; 
        }, 400);
    }
    else {
        slider.style.transition = "0.5s";
    }
    // khi slider lùi tới vị trí của slide đầu tiên  nếu bấm back slide tiếp thì sẽ thực hiện translate đến vị trí của clone element đầu slide
    // clone element cuối cùng ở đây là:
    // <div class="item yellow clone">3</div>
    // ngay khi slide đến clone element đầu slide ngay lập tức remove transition của slider và dịch chuyển về slide cuối  của slider 
    // và sau khi dịch chuyển về slide cuối add transition trở lại slider
    // hình dưới đây sẽ giãi thích vì sao cần remove transition
}
```
![gen slide](https://media.giphy.com/media/5yYxLyzV288s1rcVoL/giphy.gif)



**Total**
Kết hợp 2 hàm if trên ta sẽ có được slider loop infinity
Cái nhìn bao quát nhất:
![total](https://media.giphy.com/media/CjVah1zZ1qUHZBuro7/giphy.gif);

##5. Góc câu like

- Gạch đá thoải mái, càng ném tác giả càng ra phần 2 nhanh hơn =))), Nhưng xin phép không bàn về "nhân phẩm" của tác giả =)))
- Bằng 1 cách có 1 0 2 tác giả đã tự tạo độ khó cho mình bằng cách viết ra bài chia sẽ này =)).
- Thực tế cái slider này chỉ tốn 1h của tác giả nhưng bài viết này tốn hết 1 tuần lễ của tác giả =)))).
- Có thể tăng thêm độ khó cho đủ đô bằng cách add thêm dots, slide vertical, lazy load, hay bắt user đăng nhập mới thấy slide =))) or tạo luôn 1 framework slider mới :sunglasses: :fire: :fire: :fire: :fire:


**Thanks Mọi Người đã dũng cảm đọc tới đây** :clap: :clap: :clap: :clap:




**Phần 2: Series: Tạo độ khó: Cách Bootstrap làm carousel (incomming...)**









