# Java Timezone Offset 포멧팅


- [1. 개념](#1-개념)
- [2. 표현 방법](#2-표현-방법)
	- [2.1. offset `X` and `x`](#21-offset-x-and-x)
	- [2.2. offset `Z`](#22-offset-z)
- [3. 결과](#3-결과)
- [4. 참고](#4-참고)

<br>

일하다가 DateTimeFormatter에서 제공하는 포멧과 아주 유사하지만 다른 포멧을 커스터마이징해서 사용해야하는 상황이 생겼다.

ISO_OFFSET_DATE_TIME 
ex) 2011-12-03T10:15:30+01:00
이런 포멧이지만 밀리세컨드를 3자리 더 표현해야했다.

<br><br>

## 1. 개념

timezone offset: UTC(Coordinated Universal Time) 기준시간과의 차이를 나타낸 것
ex) 한국시간을 나타낼때 UTC +09:00 이라고 할 때 "+09:00"이 오프셋이다.
(한국외에도 일본, 인도네시아 등 여러 나라에서 사용)

<br>

## 2. 표현 방법

일단 기존에 사용하던 `LocalDateTime`의 경우, timezone offset 포멧을 위한 심볼에 대한 정보를 가지고 있지 않기 때문에 표현이 불가능하다.

그래서 timezone offset 포멧을 표현할 수 있는 `ZonedDateTime` 클래스를 사용했고, timezone offset 포멧을 나타내는 심볼은 X 혹은 Z 이다.

<br>

### 2.1. offset `X` and `x`
- 패턴문자(X나 x)의 갯수에 따라서 다르게 표현된다.
- 오프셋이 0인 경우, X(대문자)는 `'Z'`를 출력하고, x(소문자)는 `'+00'`, `'+0000'`, `'+00:00'`과 같이 숫자로 출력된다.
- 패턴문자가 하나인 경우, 시간만 출력
    - 분 단위가 0인 경우 `'+01'`
    - 분 단위가 0이 아닌 경우에는 `'+0130'`
- 패턴문자가 둘인 경우, 시간과 분을 콜론(:) 없이 출력
    - 분 단위에 관계없이 `'+0130'`
- 패턴문자가 셋인 경우, 시간과 분을 콜론과 함께 출력
    - `'+01:30'`
- 패턴문자가 넷인 경우, 시간과 분,초(0이 아닌 경우만)를 콜론 없이 출력
    - `'+013015'`
- 패턴문자가 다섯인 경우, 시간과 분, 초(0이 아닌 경우만)를 콜론과 함께 출력
    - `'+01:30:15'`

<br>

### 2.2. offset `Z`
- 패턴문자(Z)의 갯수에 따라서 다르게 표현된다.
- 패턴문자가 하나, 둘 혹은 셋인 경우, 시와 분을 콜론 없이 출력
    - `'+0130'`
    - 오프셋이 0인 경우 `'+0000'`
- 패턴문자가 넷인 경우, 오프셋의 full 포멧을 출력
    - `'GMT+08:00'`
- 패턴문자가 다섯인 경우, 시와 분, 초(0이 아닌 경우만)를 콜론과 함께 출력
    - `'+01:30'`, `'+01:30:15'`
    - 오프셋이 0인 경우 `'Z'` 출력

<br><br>

## 3. 결과

결과적으로, 

yyyy-MM-dd'T'HH:mm:ss.SSSXXX

yyyy-MM-dd'T'HH:mm:ss.SSSZZZZZ

와 같은 포멧을 구성해서 목적했던 포멧을 만들어낼 수 있었다.


<br><br>

## 4. 참고
- https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html
- https://stackoverflow.com/questions/25561377/format-localdatetime-with-timezone-in-java8
- https://stackoverflow.com/questions/55599436/how-to-parse-offset-with-colon-using-datetimeformatter