# 22장 this

# 22.1 this 키워드

> 객체 : 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적 단위로 묶은 복합적인 자료구조

> 메서드 : 동작을 나타내며 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다.

- 이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.
- 객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다. 예) circle.radius
- 하지만 생성자 함수 방식으로 인스턴스를 생성하면 생성자 함수 내부에서는 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 따라서 이를 위해 this 라는 특수한 식별자를 제공한다.

> this : 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

- this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다.
- 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다. 함수 내부에서 arguments 객체를 지역 변수처럼 사용할 수 있는 것처럼, this도 지역변수처럼 사용 가능.
- this 가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정

<aside>
💡 this 바인딩 : 바인딩이란 식별자와 값을 연결하는 과정. 에를 들어 변수 선언은 변수 이름(식별자)와 확보된 메모리 공간의 주소를 바인딩 하는 것이다. this 바인딩은 this와 this가 가리킬 객체를 바인딩 하는 것.

</aside>

- 생성자 함수 내부의 this → 생성자 함수가 생성할 인스턴스르 가리킨다.
- 자바나 c++ 같은 클래스 기반 언어에서는 this가 언제나 클래스가 생성하는 인스턴스를 가리키지만 자바스크립트의 this는 함수 호출 방식에 따라 this 바인딩이 동적으로 결정된다. strict mode 또한 this 바인딩에 영향을 준다.

- this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다. strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩. 일반 함수 내부에서 this를 사용할 필요가 없기 때문.

# 22.2 함수 호출 방식과 this 바인딩

this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정

렉시컬 스코프와 this 바인딩은 결정 시기가 다르다

함수의 상위 스코프를 결정하는 방식인 **렉시컬 스코프**는 함수 정의가 평가되어 `함수 객체가 생성되는 시점에` 상위 스코프를 결정한다. 하지만 **this 바인딩**은 `함수 호출 시점`에 결정된다.

주의할 점 : 동일한 함수도 다양한 방식으로 호출할 수 있다.

함수 호출 방식

1. 일반 함수 호출 : this는 전역객체 window를 가리킨다.
2. 메서드 호출 : 함수 내부 this는 메서드를 호출한 객체를 가리킨다.
3. 생성자 함수 호출 : 생성자 함수가 생성한 인스턴스를 가리킨다.
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출 : 함수 내부 This는 인수에 의해 결정

## 22.2.1 일반 함수 호출

- 기본적으로 this에는 전역 객체가 바인딩
- 일반 함수에서 this는 의미가 없다.
- 메서드 내에서 정의한 중첩함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.

- 콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩 된다. 어떤 함수라도 일반함수로 호출되면 this에 전역 객체가 바인딩 된다.

- 정리 : 일반 함수로 호출된 모든 함수(중첩함수, 콜백함수 포함)내부의 this에는 전역 객체가 바인딩된다.

- 하지만 메서드 내에서 정의한 중첩 함수나 메서드에게 전달한 콜백함수 내부에서 this가 전역객체를 바인딩 하는 것은 문제가 있다. 이를 해결하기 위해 메서드 내부 중첩 함수나 콜백함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법은 다음과 같다

### 1. that 에 this 바인딩 할당 후 that 참조

```python
var value = 1;

const obj = {
	value : 100,
	foo() {
		//this 바인딩을 변수 that에 할당.
		const that = this;
		setTimeout(function() {
		//콜백 함수 내에서 this 대신 that을 참조
			console.log(that.value);
		}. 100);
	}
}

obj.foo();
```

### 2. this를 명시적으로 바인딩할 수 있는 Function.prototype.apply, Function.prototype.call, Function.prototype.bind 메서드 → 22.2.4절

### 3. 화살표 함수를 사용해 this 바인딩 일치시키기

```python
var value= 1;

const obj = {
	value : 100,
	foo() {

		setTimeout(() => console.log(this.value), 100);
	}
}

obj.foo();
```

## 22.2.2 메서드 호출

- 메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 `메서드 이름 앞의 마침표 연산자 앞에 기술한 객체`가 바인딩
- 주의 할 점 : 메서드 내부의 this는 메서드를 소유한 객체가 아닌 `메서드를 호출한 객체에 바인딩.`
- 예제
-

```python
cosnt person = {
	name : 'Lee',
	getName() {
		//메서드 내부 this는 메서드를 호출한 객체에 바인딩
		return this.name;
	}
};

console.log(person.getName()); // Lee -> person이 메서드를 호출했으니 this는 person
```

→ getName메서드는 person 객체의 메서드로 정의되었다. getName 메서드는 프로퍼티에 바인딩된 함수로, person객체의 getName 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체이다. ??? 351p

```python
const anotherPerson = {
	name : 'Kim'
}

anotherPerson.getName = person.getName;

console.log(anotherPerson.getName()); //Kim -> 호출객체인 anotherPerson이 this

const getName = person.getName;

console.log(getName()): // '' -> 일반 함수로 호출된 getName 함수 내부의 this.name은
													// window.name과 같다.
```

정리하자면 메서드 내부의 this는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고, 메서드를 호출한 객체에 바인딩된다.

- 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.

```python
function Person(name) {
	this.name = name;
}

Person.prototype.getName = function(){
	return this.name;
};

const me = new Person('Lee')

console.log(me.getName()) // Lee -> this는 me 이고 me는 person의 인스턴스이므로 lee

Person.prototype.name = 'Kim'

console.log(Person.prototype.getName()); //Kim -> getName을 호출한 객체는
																				//Person.prototype this는 Person.prototype

```

## 22.2.3 생성자 호출

- 생성자 함수 내부의 this에는 생성자 함수가 생성할 `인스턴스`가 바인딩

```python
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function() {
		return 2* this.radius;
	};
}

const circle1 = new Circle(5);

console.log(circle1.getDiameter()) // 10
```

→ 이때 getDiameter 내부의 this는 생성한 circle1인스턴스가 된다. 따라서 10이 출력된다.

```python
const circle3= Circle(15);
console.log(circle3);//undefined
console.log(radius); //15

```

생성자 함수로 호출되지 않으면 일반 함수로 인식되어 반환문이 없으므로 암묵적으로 undefined를 반환한다.

일반 함수로 호출된 Circle내부의 this는 전역 객체를 가리킨다.

## 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- apply,call,bind 메서드는 Function.prototype의 메서드다. 즉 이들 메서드는 모든 함수가 상속받아 사용 할 수 있다.
- Function.prototype.apply, [Function.prototype.call](http://Function.prototype.call) 메서드는 `this로 사용할 객체`와 `인수 리스트`를 인수로 전달받아 함수를 호출한다.
- apply,call의 사용법

```jsx
/**
* 주어진 this 바인딩, 인수 리스트 배열을 사용해 함수를 호출한다.
* @param thisArg : this로 사용할 객체
@param argsArray : 함수에게 전달할 인수 리스트이 배열 또는 유사 배열 객체
@returns 호출된 함수의 반환값
*/

Function.prototype.apply(thisArg[,argsArray])

/**
* 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용해 함수를 호출한다.
* @param thisArg : this로 사용할 객체
@param arg1, arg2, ... : 함수에게 전달할 인수 리스트
@returns 호출된 함수의 반환값
*/

Function.prototype.call(thisArg[,arg1[,arg2[, ...]]])

```

예제

```jsx
function getThisBinding() {
  return this;
}

const thisArg = { a: 1 }; //this로 사용할 객체

console.log(getThisBinding());

//getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding함수의 this에 바인딩
console.log(getThisBindint.apply(thisArg)); //{a:1} -> thisArg를 this로 지정
console.log(getThisBindint.call(thisArg)); //{a:1} -> thisArg를 this로 지정
```

- apply,call 메서드의 본질적인 기능은 함수를 호출하는 것이다.
- apply,call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this로 바인딩
- apply,call 매서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.

- getThisBinding함수를 호출하면서 인수를 전달해보자.

```jsx
function getThisBinding() {
  console.log(arguments);
  return this;
}

//this로 사용할 객체
const thisArg = { a: 1 };

//getThisBinding함수를 호출하면서 인수로 전달할 객체를 getThisBinding 함수의 this에 바인딩
// apply메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
//Arguments(3) [ 1,2,3, callee : f, Symbol(Symbol.iterator) : f]
// {a:1}

//call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
cosnole.log(getThisBinding.call(thisArg, 1, 2, 3));
//Arguments(3) [1,2,3, callee : f, Symbol(Symbol.iterator) : f]
//{a:1}
```

- apply, call은 호출할 함수에 인수를 전달하는 방식만 다를 뿐 this로 사용할 객체를 전달하면서 함수를 호출하는 것은 동일
- apply,call의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다. arguments객체는 배열이 아니기 때문에 Array.prototype.slice 같은 배열의 메서드를 사용할 수 없으나 apply와 call 메서드를 이용하면 가능하다.

Function.prototype.bind 메서드는 apply,call과 달리 함수를 호출하지 않는다. 다맛 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```jsx
function getThisBinding() {
  return this;
}

//this로 사용할 객체
const thisArg = { a: 1 };

//bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된다.
// getThisBinding함수를 새롭게 생성해 반환한다.
cosnole.log(getThisBinding.bind(thisArg)); //getThisBinding

//bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)());
```

- bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다. 다음 예제를 살펴보자.

```jsx
const person = {
  name: "Lee",
  foo(callback) {
    //1
    setTimeout(callback, 100);
  },
};

person.foo(function () {
  console.log(`Hi my name is ${this.name}`); //2 Hi my name is
  //일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 windiow.name과 같다.
  //브라우저에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ""이다.
  //Nodejs환경에서 this.name은 undefined
});
```

- 콜백함수 호출 이전 시점인 1에서의 this는 foo메서드를 호출한 .앞의 person객체이다.
- 그러나 person.foo의 콜백 함수가 일반 함수로서 호출된 2의 시점에서 this는 전역 객체 window를 가리킨다.
- 이때 person.foo의 콜백함수는 외부함수를 돕는 헬퍼함수 역할을 하기 때문에 맥락상 같은 this를 가리켜야 한다. 이때 bind메서드를 사용해 this를 일치시킬 수 있다.

```jsx
const person = {
  name: "Lee",
  foo(callback) {
    // bind메서드로 callback함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(`Hi my name is ${this.name}`); // Hi my name is Lee
});
```

정리

| 함수 호출 방식                                             | this바인딩                                                            |
| ---------------------------------------------------------- | --------------------------------------------------------------------- |
| 일반 함수 호출                                             | 전역 객체                                                             |
| 메서드 호출                                                | 메서드를 호출한 객체                                                  |
| 생성자 함수 호출                                           | 생성자 함수가 생성할 인스턴스                                         |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달할 객체 |
