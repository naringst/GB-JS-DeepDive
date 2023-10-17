# 28.1 Number 생성자 함수

---

- 표준 빌트인 객체인 Number 객체는 생성자 함수 객체다. 따라서 new 연산자와 함께 호출해 Number 인스턴스를 생성 가능하다.
- Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼객체를 생성한다. (??)

```jsx
const numObj = new Number();
console.log(numObj); // Number{[[PrimitiveValue]] : 0}
```

→ 크롬 개발자 도구에서 실행해보면 [[PrimitiveValue]]라는 접근할 수 없는 프로퍼티가 보인다. 이는 [[NumberData]] 내부 슬롯을 가리킨다.

ES5에서는 [[NumberData]]를 [[PrimitiveValue]]라고 불렀다.

- Number 생성자 함수의 인수로 숫자를 전달하며 new연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다.

  ```jsx
  const numObj = new Number(10);
  console.log(numObj); // Number{[[PrimitiveValue]] : 10}
  ```

- Number 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환후 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다. 인수를 숫자로 변환할 수 없다면 NaN을 할당한 객체를 생성한다.
- new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다.
-

```jsx
Number("0"); // 0
```

# 28.2 Nubmer 프로퍼티

---

## 28.2.1 Number.EPSILON

- ES6 도입
- 1과 1보다 큰 숫자 중 가장 작은 숫자와의 차이와 같다.
- 약 2.2204… \* 10 ^ -16
- 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

```sql
function isEqual(a,b) {
	//a와 b를 뺀 값의 절댓값이 Number.EPSILON보다 작으면 같은 수로 인정한다.
	return Math.abs(a-b) < Number.EPSILON;
}

isEqual(0.1+ 0.2, 0.3) // true
```

## 28.2.2 Number.MAX_VALUE

- 자바스크립트에서 표현할 수 있는 가장 큰 양수 값(1.79769… \* 10^308)
- 이보다 큰 숫자는 Infinity이다.

## 28.2.3 Number.MIN_VALUE

- 자바스크립트에서 표현할 수 있는 가장 작은 양수 값(5\* 10^(-324) )
- 이보다 작은 숫자는 0이다.

## 28.2.4 Number.MAX_SAFE_INTEGER

- 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값
- 9007199254740991

## 28.2.5 Number.MIN_SAFE_INTEGER

- 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값
- - 9007199254740991

## 28.2.6 Number.POSITIVE_INFINITY

- 양의 무한대를 나타내는 숫자값 Infinity와 같다.

## 28.2.7 Number.NEGATIVE_INFINITY

- 음의 무한대를 나타내는 숫자값 -Infinity와 같다.

## 28.2.8 Number.NaN

- 숫자가 아님을 나타내는 숫자값.
- Number.NaN === window.NaN

# 28.3 Number 메서드

---

## 28.3.1 Number.isFinite

- ES6 도입
- 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity or -Infinity가 아닌지 검사해 그 결과를 불리언으로 반환.
- 인수가 NaN이면 언제나 false
- 전역함수 isFinite와 다르게 암묵적 타입변환하지 않는다. 따라서 숫자 아니면 모두 false 반환

## 28.3.2 Number.isInteger

- ES6 도입
- 인수로 전달된 숫자값이 정수인지 검사해 그 결과를 불리언으로 반환.
- 검사 전 인수를 숫자로 암묵적 타입변환하지 않는다.

## 28.3.3 Number.isNaN

- 인수로 전달된 숫자값이 NaN인지 검사해 그 결과를 불리언으로 반환.
- 이는 빌트인 전역함수 isNaN과 차이가 있다.
  - 전역함수 : 전달 받은 인수를 숫자로 암묵적 타입 변환
  - Number : 숫자로 타입 변환 안함. 따라서 숫자 아니면 모두 false 반환

## 28.3.4 Number.isSafeInteger

- ES6도입
- 인수로 전달된 숫자값이 안전한 정수인지 검사해 그 결과를 불리언으로 반환.
- 안전한 정수값은 -(2^ 53 -1) 과 2^ 53 -1 사이의 정수값.
- 검사 전 인수를 숫자로 암묵적 타입 변환하지 않는다.

## 28.3.5 Number.prototype.toExponential

- 숫자를 지수 표기법으로 변환해 문자열로 반환.
- 추가

## 28.3.6. Number.prototype.toFixed

- 숫자를 반올림하여 문자열로 반환
- 반올림하는 소숫점 이하 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달 가능.
- 인수 생략 시 기본값 0으로 지정.

## 28.3.7. Number.prototype.toPrecision

- 전달받은 인수를 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림해 문자열로 반환.
- 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과 반환.
- 전체 자릿수를 나타내는 0~21 사이의 정수 값을 인수로 전달할 수 있따. 인수 생략 시 기본값 0이 지정.

## 28.3.6. Number.prototype.toString

- 숫자를 문자열로 반환
- 진법을 나타내는 2~36 사이의 정수 값을 인수로 전달할 수 있다. 인수를 생략하면 기본값 10진법

```jsx
(16).toString(2); //16이라는 십진법 수를 2진법으로 반환 "10000"
```
