## Javasciprt Deep Dive  
https://github.com/hy57in/study-javascript-deep-dive

## 📚 즉시 실행 함수 (IIFE, Immediately-invoked function expression)
* 함수 표현(Function expression)은 함수를 정의하고, 변수에 함수를 저장하고 실행하는 과정을 거칩니다.
* 하지만 즉시 실행 함수는 함수를 정의하고 바로 실행하여 이러한 과정을 거치지 않는 특징이 있습니다.
* 함수를 정의하자마자 바로 호출하는 것을 즉시 실행 함수입니다.

```js
-- 기본 형태
(function () {
  // statements
})()
```

### 즉시 실행 함수를 사용하는 이유
* 즉시 실행 함수는 한 번의 실행만 필요로 하는 초기화 코드 부분에 많이 사용됩니다.
#### 그렇다면 왜 초기화 코드 부분에 많이 사용 할까요? 
1. 초기화
* 변수를 전역(global scope)으로 선언하는 것을 피하기 위해서 입니다. 
* 전역에 변수를 추가하지 않아도 되기 때문에 코드 충돌 없이 구현 할 수 있어, 플러그인이나 라이브러리 등을 만들 때 많이 사용됩니다.

2. 라이브러리 전역 변수의 충돌
* jQuery나 Prototype 라이브러리는 동일한 $라는 전역 변수를 사용합니다. 만약, 이 두개의 라이브러리를 같이 사용한다면 $ 변수 충돌이 생기게 됩니다.
* 즉시 실행 함수를 사용하여 $ 전역 변수의 충돌을 피할 수 있습니다.
* 즉시 실행 함수 안에서 $는 전역변수가 아닌 jQuery object의 지역 변수가 되어, Prototype 라이브러리의 $와 충돌 없이 사용할 수 있습니다.
```js
(function ($) {
    // $ 는 jQuery object
})(jQuery);
```

### 샘플 코드
```js
-- backgroundChanger.js
(function($) {
  $.fn.changeBackground = function(color) {
    return this.each(function() {
      $(this).css('background-color', color);
    });
  };
})(jQuery);
```
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="./jquery/jquery-3.7.1.min.js"></script>
    <script src="./backgroundChanger.js"></script>
</head>
<body>
    <div id="test"></div>

    <script>
        $('#test').html('제이쿼리 테스트').css('border','1px solid blue');
        $('#test').changeBackground('yellow');
    </script>
</body>
</html>
```

</br>

## clearInterval() VS clearTimeout()
### clearInterval()
* clearInterval()은 setInterval()로 설정한 주기적인 반복을 취소합니다. 
* setInterval()은 일정 시간 간격으로 함수를 반복 실행하는 타이머를 설정하는데, 
* clearInterval()은 이 반복을 멈추는 데 사용됩니다.

```js
let intervalId = setInterval(function() {
  console.log("이 메시지는 1초마다 출력됩니다.");
}, 1000);

// 5초 후에 interval을 정지시킵니다.
setTimeout(function() {
  clearInterval(intervalId);
  console.log("interval이 취소되었습니다.");
}, 5000);
```

### clearTimeout()
* clearTimeout()은 setTimeout()으로 설정한 타이머를 취소합니다. 
* setTimeout()은 한 번만 실행되는 타이머를 설정하는데, 
* clearTimeout()을 사용하면 설정된 타이머의 실행을 취소할 수 있습니다.
```js
let timeoutId = setTimeout(function() {
  console.log("이 메시지는 3초 후에 출력됩니다.");
}, 3000);

// 1초 후에 timeout을 취소합니다.
setTimeout(function() {
  clearTimeout(timeoutId);
  console.log("timeout이 취소되었습니다.");
}, 1000);
```
#### clearInterval()은 setInterval()로 설정한 주기적인 타이머를 취소합니다.
#### clearTimeout()은 setTimeout()으로 설정한 한 번 실행되는 타이머를 취소합니다.
