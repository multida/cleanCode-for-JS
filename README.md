# cleanCode-for-JS
강의보고 자바스크립트 정리하자.

---

## 전역 공간 사용 최소화
- 어디서나 접근이 쉬워지고 스코프 분리할때 위험하기 때문에 전역 공간을 더럽히지 않는 것이 중요하다.

1. 전역변수 ❌
2. 지역변수 ⭕️
3. window, global 조작 ❌
4. const, let ⭕️
5. IIFE, Module, Closure를 이용해서 스코프 나누기


## 임시 변수 제거
- 명령형으로 가득한 로직이 나오고, 어디서 어떻게 잘못되었는지 디버깅이 힘들다. 
- 추가적인 코드를 작성하고 싶은 유혹에 빠진다. -> 코드 유지보수가 어렵다.

1. 함수 나누기 
2. 바로 반환(return)
3. 고차함수(map, filter, reduce)
4. 선언형 코드로 바꾸기

---

## 호이스팅
- 런타임시기에 선언과 할당이 분리된 것을 뜻함.
  - 런타임시기란? 프로그램이 동작할 때를 의미한다. 보통 코드를 작성할 때 스코프를 예상하고 짜는데, 막상 런타임을 하게 되면 전혀 다른 동작이 나온다. 이러한것 중 하나가 호이스팅이다.
호이스팅은 var로 선언한 변수가 초기화가 제대로 되어 있지 않을때 undefined로 최상단에 끌어올려줄 수 있는 것. ( 선언부만 최상단으로 옮기는 것 )
- 함수도 호이스팅이 된다. 

```javascript
var global = 0;

function outer() {
	console.log(global); //undefined
	var global = 5;
	
	function inner() {
		var global = 10;
	
		console.log(global); //10
	}	
	
	inner();
	
	global = 1;
	
	console.log(global); //1
}

outer();
```

```javascript
function duplicatedVar() {
	var a;
	console.log(a);  //undefined

	var a = 100;
	console.log(a); //100
}

duplicatedVar()  //undefined
```



### 호이스팅에서 벗어나는 방법!
- 변수선언 → 할당 → 초기화 완료 → 정확한 분리
```javascript
const sum = function() { // 함수 표현식 : 익명함수를 넣어서 변수에 할당.
	return 1 + 2;
}

console.log(sum());//3
```

const를 이용하여 함수를 만드는 것이 좋다. 
```javascript 
const sum = function() { // 함수 표현식 : 익명함수를 넣어서 변수에 할당.
	return 1 + 2;
}

console.log(sum());//3
```

> 런타임시 → 선언을 최상단으로 끌어올려 주는 것.
문제는 코드를 작성할 때 예측하지 못한 실행 결과가 노출되는 것 → 🐶발이 힘들어짐.
이런 예측하지 못하는 상황들을 탈피하기 위해 var를 지양해야한다.
함수도 호이스팅 되기 떄문에 함수표현식을 사용하는 것이 좋다.

---

## eqeq 줄이기

느슨한 동등자인 == 보다는 엄격한 동등자인 ===를 사용하자. 
==는 암묵적인 형 변환이 일어나기 때문에 명시적으로 변환해주는 것이 좋다. 
String(11 + '문자와 결합')
Number('11')
parseInt('9.9999', 10)

[https://eslint.org/docs/latest/rules/eqeqeq](https://eslint.org/docs/latest/rules/eqeqeq)
[https://dorey.github.io/JavaScript-Equality-Table/](https://dorey.github.io/JavaScript-Equality-Table/)


## isNaN
is not a number ->  숫자가 아니다?
헷갈리는 구문이고 느슨한 검사이기 때문에 엄격하게 사용하자. 
isNaN(11)보다는 Number.isNaN(11)로 사용합시다!


## 경계 다루기
### min / max
  - 최소값과 최대값을 다룬다.
  - 최소값과 최대값 포함 여부를 결정해야 한다. ( 이상-초과 / 이하-미만 )
   - MIN_NUMBER / MAX_NUMBER 는 포함을 하는지 안하는지를 모른다.
   - MIN_NUMBER_LIMIT = 1; // 미만
   - MAX_IN_MUNBER = 20 // 이상?
	
### begin / end
체크인/체크아웃 (beginDate / endDate)

### first / last
포함된 양 끝을 의미한다. ( --- 부터 ~~~ 까지 )
  
### prefix suffix
- 접두사/접미사의 규칙성을 붙여 보기 쉽게 관리할 수 있다.
- 코드를 읽는 일관성을 가질 수 있는 좋은 방법이다.

### 매개변수의 순서가 경계다. 
호출하는 함수의 네이밍과 인자의 순서의 연관성을 고려한다. 

1. 매개변수를 2개가 넘지 않도록 만든다. 
2. 이름으로 추론할 수 없다면 arguments, rest parameter 사용
3. 매개변수를 객체에 담아서 넘긴다.
4. 랩핑하는 함수를 만든다.

```javascript
getRandomNumber(1, 50)//1부터 50까지의 숫자를 가지고 랜덤하게 뽑나?
getDates('2023-01-01', '2023-01-31')// 1월 31일까지의 날짜를 얻어오나보다. 

//ES2015+
function someFunc(someArg1, someArg2, someArg3, someArg4){}
function getFunc(someArg1, someArg3){
	someFunc(someArg1, undefined, someArg3); 
}
```

---
## 분기 다루기

### 값식문?

값으로 귀결되는 식

### 삼항연산자

3개의 “피”연산자 

`조건 ?  참(식) : 거짓(식)`

- 값을 만들고 그걸 변수로 담아낼때 사용 
- 함수가 내뱉는 값이 바로 값을 반환할때 사용한다.
