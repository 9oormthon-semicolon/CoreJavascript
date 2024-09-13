# 2~3 VariableEnvironment & LexicalEnvironment

### `VariableEnvioment`란?

**실행 컨텍스트의 일부로** 현재 컨텍스트 내에 식별자들에 대한 정보 + 외부 환경 정보, 선언시점의 스냅샷입니다. 간단하게 요약하면 실행 컨텍스트 내의 **변수와 함수 선언에 대한 정보**를 담고 있는 구조입니다.

특징은 초기 정보를 가지고 있을 뿐 변경사항은 반영되지 않습니다. 

### `LexicalEnvironment`란?

특정 코드 블록 내에서 정의된 변수를 포함하고, 해당 블록 외부에서는 접근할 수 없는 변수를 관리하는 방식입니다. 간단하게 함수나 코드 블록 내에서 사용 가능한 변수의 범위를 정의하는 것입니다.

### `EnvironmentRecord`와 호이스팅

enviromentRecord에는 현재 컨텍스트와 관련된 코드의 정보들이 저장됩니다. 텍스트를 구성하는 함수에 지정된 메개변수 식별자 선언한 함수가 있을 경우 그 함수자체 var로 선언된 변수의 식별자 등이 식별자에 해당 됩니다.

예를 들어서 간단한 예시를 들고 호이스팅을 해보겠습니다.

```jsx
(()=>{
		var x = 1
		console.log(x)
		var x 
		console.log(x)
		var x = 2
		console.log(x)
})()
```

```jsx
//호이스팅

(()=>{
		var x
		var x
		var x

		var x = 1
		console.log(x)
		console.log(x)
		var x = 2
		console.log(x)
})()

//1
//1
//2
```

호이스팅 하기 전에는 두 번째 `x`가 `undefined`로 나올 것 같지만, 호이스팅을 이해하고 나면 두 번째 `x`가 `1`로 출력된다는 것이 명확해집니다. 

이를 통해, 변수 선언이 코드 상단으로 끌어올려지고, 초기화는 실제 코드 흐름에 따라 이루어진다는 사실을 알 수 있습니다. 개인적으로 원하는 코드를 작성하기 위해서는 **호이스팅 개념을 완벽하게 이해하는 것이 중요해보입니다.**

### 스코프 스코프체인

스코프란 식별자의 유효범위

A 내부에서 선언한 변수는 오직 A에서만 유효합니다.

```jsx
var a = 1
(function outer(){
    (function inner(){
        console.log(a)
        var a = 3
    })()
    console.log(a)
})()
console.log(a)

//undifined
//1
//1
```

호이스팅

```jsx
var a = 1;
(function outer(){
	  var = a
    (function inner(){
        var a
        console.log(a)
        a = 3
    })()
    console.log(a)
})()

console.log(a)

//undifined
//1
//1
```

**후기**

코어 자바스크립트에서는 정적 환경이라는 단어로 번역했다는데 이해가 잘 되지 않았다.

설명에는 `VariableEnvironment` 와 달리 변경사항을 반영한다고 했다.

당장 SSG(**Static Site Generation**)만 보더라도 한국어로 **정적 사이트 생성**인데(정적 사이트는  빌드하고 유동적으로 API를 가져오는게 불가능하다.) 나만 그런건지 모르겠는데 보편적인 정적이라는 단어가 주는 이미지가 고정적이고 변화하지 않는 상태를 말하는건데, `LexicalEnvironment` 을 정적 환경이라고 잘 번역했다는게 상당히 열받고 처음엔 이해가 되지 않았다.