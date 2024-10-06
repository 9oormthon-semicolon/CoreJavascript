# this

## 상황에 따라 달라지는 this

이 챕터는 JS에서 가장 혼란스러울 수 있는 개념인 this를 설명하는 챕터입니다.

### 전역 공간에서의 this

전역공간에서의 `this`는 `window`객체를 가리킵니다.

```jsx
console.log(this === window) //true
```

### 메서드로서 호출할 때 그 메서드 내부에서의 this

함수를 실행하는 방법은 여러가지가 있는데 함수로서 호출, 메서드로서 호출하는 경우 2가지로 나뉩니다.

```jsx
var func = function (x) {
		console.log(this, x) 
} //Window { ... }

var obj = {
		method: func
}

obj.method(2) // {method : f}

var obj = {
		method: function (x) { console.log(this, x) }
}
obj.method(1) //{method : f} 1
obj['method'](2) //{method : f} 2 
```

위처럼 함수 내에서 실행한 `this`는 전역의 `this`처럼 `Window` **객체**를 가리키지만, `obj`내의 메소드의 `this`는 **해당 메서드를 호출한 객체**를 가리킵니다.

## 명시적으로 this를 바인딩하는 방법

call,apply

```jsx
var func = function (a,b,c) {
		console.log(this.name,a,b,c)
}

func.call({name:'ChulSu'},1,2,3)
func.apply({name:'ChulSu'},[1,2,3])
```

사용하는 이유는 따로 매개변수를 설정하지 않은 메서드에 this를 원하는 값으로 설정하기 위함인데 이렇게 만듬으로써 함수에 재사용성을 늘리기 위함이라고 생각합니다. 

예를 들어서 여러 형식의 class들이 name이라는 속성을 가지고 있다고 가정하겠습니다.
모든 클래스의 name을 반환하는 메서드 또는 프로토 타입을 일일이 만들어주기 보다는 call이나 apply를 이용한 name을 반환해주는 함수 하나를 만드는게 더 효율적입니다. 이러한 목적을 가지고 call과 apply를 만든 것 같다고 생각합니다.

### bind

`bind(this)`는 함수가 실행될 때 **`this`가 특정 객체**(주로 클래스의 인스턴스)를 가리키도록 **고정**하는 역할을 합니다.

아래는 제가 프런트엔드 공부를 처음 시작할 때 유튜브에서 참고하여 작성했던 코드입니다:

```jsx
jsx
코드 복사
class Main {
    constructor() {
        this.canvas = document.body.appendChild(document.createElement('canvas'));
        this.ctx = this.canvas.getContext('2d');

        this.resize();

        // 바인딩
        window.addEventListener('resize', this.resize.bind(this), false);
    }

    // 리사이즈 이벤트
    resize() {
        this.stageWidth = window.innerWidth;
        this.stageHeight = window.innerHeight;

        this.canvas.width = this.stageWidth * 2;
        this.canvas.height = this.stageHeight * 2;

        this.ctx.scale(2, 2);
    }
}

```

당시에는 `bind`의 개념을 제대로 이해하지 못하고 사용했지만, 지금은 그 의미를 이해할 수 있습니다.

```jsx
window.addEventListener('resize', this.resize.bind(this), false)
```

여기서 `this.resize`를 바로 사용했다면, `this`는 `window` 객체를 가리켰을 겁니다. 그러면 `this.resize`는 `window.resize`로 인식되어 실행되지 않았을 것입니다. 이를 `bind(this)`로 **현재 실행 중인 클래스 인스턴스**를 바인딩하여, `Main` 클래스 안에 있는 `resize` 메서드를 실행하도록 만든 것입니다.

### 챕터보고 느낀점

지금은 프런트엔드에서 리액트가 대세이기 때문에 함수형으로 쓰는게 익숙하여 클래스의 문법과 메서드의 의미를 많이 모르고 또 잊었습니다.

몰라도 프런트 개발을 할 수 있습니다만, 개인적으로 후에 대세가 되는 프레임워크가 클래스형으로 바뀔 수도 있고, 또 과거 코드를 유지보수 할 때 짜여진 코드가 클래스형 컴포넌트일 수도 있어 배워서 나쁠 것 없어 보입니다.