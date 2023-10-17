- 표준 빌트인 객체인 Date는 날짜와 시간(연, 월, 시, 분, 초, 밀리초)을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다.
- UTC는 국제 표준시를 말한다. UTC는 GMT로 불리기도 한다. 기술적인 표기에선 UTC를 쓴다. (초의 소수점 단위 차이)
- KST(한국 표준시)는 UTC에 9시간을 더한 시간이다. 즉 KST는 UTC보다 9시간이 빠르다.
- 현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템 시계에 의해 결정된다.

# 30.1 Date 생성자 함수

---

- Date는 생성자 함수다.
- 이 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.
- 1970년 1월1일 00:00:00을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다. 예를 들어, 1970년 1월1일 00:00:00 는 정수값 0을 가지며 하루 뒤는 정수값 86,400,000(24h _ 60m _ 60s \* 1000ms)를 갖는다.
- Date 생성자 함수로 생성한 Date 객체는 기본적으로 현재 날짜와 시간을 나타내는 정수값을 갖는다.

## Date 생성자 함수로 객체를 생성하는 네 가지 방법

## 30.1.1 new Date()

- 인수 없이 new 연산자와 함께 호출하는 방법
- 현재 날짜와 시간을 가지는 Date 객체를 반환한다.
- 내부적으로는 정수값을 갖지만 Date 객체를 콘솔에 출력하면 날짜와 시간 정보를 출력한다.

## 30.1.2 new Date(milliseconds)

- 밀리초를 인수로 전달하면 1970년 1월 1일을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date객체를 반환한다.

## 30.1.3 new Date(dateString)

- 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
- 이때 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 한다.

```jsx
new Date("May 26, 2020 10:00:00");
// Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)

new Date("2020/03/26/10:00:00");
// Tue Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

## 30.1.4 new Date(year, month[,day,hour,minute, second, millisecond])

- Date 생성자 함수에 연,월, 시, 분, 초 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date객체를 반환한다.
- 이때 연, 월은 반드시 지정해야 한다. 지정하지 않은 옵션은 0 또는 1로 초기화 된다.

| year        | 연을 나타내는 1900 이후의 정수. 0부터 99는 1900부터 1999로 처리 |
| ----------- | --------------------------------------------------------------- |
| month       | 월을 나타내는 0~11까지의 정수                                   |
| day         | 일을 나타내는 1~31까지의 정수                                   |
| hour        | 시를 나타내는 0~23까지의 정수                                   |
| minute      | 분을 나타내는 0~59까지의 정수                                   |
| second      | 초를 나타내는 0~59까지의 정수                                   |
| millisecond | 밀리초를 나타내는 0~999까지의 정수                              |

- 연,월을 지정하지 않은 경우 1970년 1월 1일 00:00:00을 나타내는 Date 객체를 반환

# 30.2 Date 메서드

---

## 30.2.1 [Date.now](http://Date.now)

- 1970년 1월 1일 00:00:00을 기점으로 현재까지 경과한 밀리초를 숫자로 반환.

## 30.2.2 Date.parse

- 1970년 1월 1일 00:00:00을 기점으로 인수로 전달된 지정 시간(new Date의 인수와 동일한 형식)까지의 밀리초를 숫자로 반환한다.
- 날짜 → 밀리초의 숫자

## 30.2.3 Date.UTC

- 1970년 1월 1일 00:00:00을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.
- 이 메서드는 new Date(year, month[,day,hour,minute, second, millisecond])와 같은 형식의 인수를 사용해야 한다. 이는 로컬타임이 아닌 UTC로 인식된다. month는 월을 의미하는 0~11의 정수. 0부터 시작을 주의

```jsx
Date.UTC(1970, 0, 2); // 86400000
```

## 30.2.4 Date.prototype.getFullYear

- Date 객체의 연도를 나타내는 정수를 반환한다.

```jsx
new Date("2020/07/24").getFullYear(); // 2020
```

## 30.2.5 Date.prototype.setFullYear

- Date 객체에 연도를 나타내는 정수를 설정한다. 연도 이외에 옵션으로 월, 일도 설정 가능

```jsx
const today = new Date();

today.setFullYear(2020);
today.getFullYear(); //2020
```

## 30.2.6 Date.prototype.getMonth

- Date 객체의 월을 나타내는 0~11의 정수를 반환
-

```jsx
new Date("2020/07/24").getMonth(); //6
```

## 30.2.7 Date.prototype.setMonth

- Date 객체의 월을 나타내는 0~11의 정수를 지정
- 월 이외에 옵션으로 일도 설정 가능

```jsx
const today = new Date();

today.setMonth(0);
today.getMonth(); //0

today.setMonth(11, 1); // 12.1
today.getMonth(); // 11
```

## 30.2.8 Date.prototype.getDate

- Date 객체의 날짜를 나타내는 정수 반환

```jsx
new Date("2020/07/24").getDate(); //24
```

## 30.2.9 Date.prototype.setDate

- Date 객체의 날짜를 나타내는 정수 설정
  ```jsx
  const today = new Date();

  today.setDate(1);
  today.getDate(); //1
  ```

## 30.2.10 Date.prototype.getDay

- Date 객체의 요일을 나타내는 정수를 반환

| 일  | 0   |
| --- | --- |
| 월  | 1   |
| 화  | 2   |
| 수  | 3   |
| 목  | 4   |
| 금  | 5   |
| 토  | 6   |

## 30.2.11 Date.prototype.getHours

- Date 객체의 시간(0~23)을 나타내는 정수 반환

## 30.2.12 Date.prototype.setHours

- Date 객체에 시간(0~23)을 나타내는 정수 설정
- 시간 이외에 옵션으로 분,초, 밀리초도 설정 가능

## 30.2.13 Date.prototype.getMinutes

- Date 객체의 분(0~59) 를 나타내는 정수를 반환
-

```jsx
new Date("2020/7/24/12:30").getMinutes(); //30
```

## 30.2.14 Date.prototype.setMinutes

- Date 객체의 분(0~59) 를 나타내는 정수를 설정.
- 분 이외의 옵션으로 초, 밀리초도 설정 가능.

## 30.2.15 Date.prototype.getSeconds

- Date 객체의 초(0~59) 를 나타내는 정수를 반환

## 30.2.16 Date.prototype.setSeconds

- Date 객체의 초(0~59) 를 나타내는 정수를 설정
- 초 이외에 옵션으로 밀리초도 설정 가능

## 30.2.17 Date.prototype.getMilliseconds

- Date 객체의 밀리초(0~999) 를 나타내는 정수를 반환

## 30.2.18 Date.prototype.setMilliseconds

- Date 객체의 밀리초(0~999) 를 나타내는 정수를 설정

## 30.2.19 Date.prototype.getTime

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

## 30.2.20 Date.prototype.setTime

- 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 설정한다.

## 30.2.21 Date.prototype.getTimezoneOffset

- UTC와 Date 객체에 지정된 로캘 시간과의 차이를 분 단위로 반환한다. KST는 UTC에 9시간을 더한 시간이다.
- 즉, UTC = KST- 9h

## 30.2.22 Date.prototype.toDateString

- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

## 30.2.23 Date.prototype.toTimeString

- 사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환

```jsx
const today = new Date("2020/7/24/12:30");

toay.toString(); //Fri Jul 24 2020 12:30:00 GMT+0900
today.toTimeString(); // 12:30;00 GMT+0900 (대한민국표준시)
```

## 30.2.24 Date.prototype.toISOString

- ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환
-

```jsx
const today = new Date("2020/7/24/12:30");

toay.toString(); //Fri Jul 24 2020 12:30:00 GMT+0900
today.toISOString(); // 2020-07-24T03:30:00.000Z

today.toISOString().slice(0, 10); // 2020-07-24
```

## 30.2.25. Date.prototype.toLocaleString

- 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환.
- 인수를 생략한 경우 브라우저가 동작중인 시스템의 로캘을 적용한다.
-

```jsx
const today = new Date("2020/7/24/12:30");

today.toString();
today.toLocaleString(); // 2020. 7. 24. 오후 12:30:00
today.toLocaleString("ko-KR"); // 2020. 7. 24. 오후 12:30:00
```

## 30.2.26 Date.prototype.toLocaleTimeString

- 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환.
- 인수를 생략한 경우 브라우저가 동작중인 시스템의 로캘을 적용한다.

```jsx
const today = new Date("2020/7/24/12:30");

today.toString();
today.toLocaleTimeString(); // 오후 12:30:00
today.toLocaleString("ko-KR"); // 오후 12:30:00
```

# 30.3 Date를 활용한 시계 예제

---

- 다음 예제는 현재 날짜와 시간을 초 단위로 반복 출력한다.
