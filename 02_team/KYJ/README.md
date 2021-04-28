# Sass(Syntacically Awesome Style Sheets)

## 개요

Sass는 마치 프로그래밍 언어처럼 Css를 사용할 수 있게 만들어주는 전처리기다. 변수를 사용하거나, 중첩 구조를 사용해 생상선과 가독성을 높일수 있다. Scss의 원리는 작성된 Sass 문법을 컴파일러가 css로 변환하고, 변환된 css를 브라우저가 해석하는 방식이다. scss를 어떻게 활용할 수 있을지 집중적으로 학습해서 이 문서에 기록해 보려고 한다.

## Sass와 SCSS(Sassy CSS)

SCSS는 Sass의 3번째 버전으로 CSS문법을 Sass 문법에 적용한 모델이다. 즉 기존 CSS문법으로 작성된 문서는 SCSS의 범주에 속하게 된다.

### Sass 문법

```sass
ul.list
    overflow: hidden
    float: left
    width: 200px
    padding: 20px 30px
```

### SCSS 문법

```scss
ul.list {
  overflow: hidden;
  li {
    float: left;
  }
}
```

## 컴파일 방법

1. [scssmeister](https://www.sassmeister.com/)
2. parcel modul

## 데이터 종류

css의 데이터 종류에 추가로 booleans, null, lists, maps가 있다.

| 데이터   | 타입                                    | 예시                                     |
| -------- | --------------------------------------- | ---------------------------------------- |
| Numbers  | 숫자                                    | 1, .82, 20px, 2em...                     |
| Strings  | 문자                                    | bold, relative, "/images/a.png", "dotum" |
| Colors   | 색상 표현                               | red, blue, #FFFF00, rgba(255,0,0.5)      |
| Booleans | 논리                                    | true, false                              |
| Nulls    | 빈 값                                   | null                                     |
| Lists    | 공백이나 ,로 구분된 값의 목록           | (apple, orange, banana), apple orange    |
| Maps     | Lists와 유사하나 값이 `Key: Value` 형태 | (apple: a, orange: o, banana: b)         |

> 참고: Lists 타입은 () 괄호가 없어도 동작하나, Maps 타입은 반드시 ()를 붙여야 한다.

## 중첩

css에서 부모 요소를 통해 자식요소를 선택하려면 다음 과 같이 부모 요소를 계속 선택해 주어야 한다.

```css
.container .item1 {
  width: 200px;
}
.container .item2 {
  width: 300px;
}
```

scss의 중첩 문법을 사용하면 부모요소를 중복 작성할 필요가 없어 생산성이 높아진다.

scss

```scss
.container {
  .item1 { /*li item1*/
    width: 200px;
  }
  .item2 { /*li item2*/
    width: 300px;
  }
}
```

### 중첩 문법의 활용

#### 상위 선택자 `&`

`&` 기호는 상위 선택자의 이름을 나타낸다.

활용 1. 일치 선택자를 선언할 수 있다.

```scss
li {
  &.item1 { /*li.item1*/
    width: 200px;
  }
  &.item2 { /*li.item2*/
    width: 300px;
  }
}
```

활용 2. 공통된 이름을 가진 선택자를 선언할 수 있다.

```scss
.container {
  &-item1 { /* container-item1 */
    width: 200px;
  }
  &-item2 { /* container-item2 */
    width: 300px;
  }
}
```

#### @at-root
선택자 내부에 변수를 정의한 경우 그 밖에서는 해당 변수를 사용할 수 없다. 이 때 선택자 내부에서 @at-root를 사용하면 중첩을 벗어나 정의할 수 있다.
```scss
.list{
	$size: 200px;
  @at-root{  
		item1{ /* item1: 200px */
			width: size;
		}
	}
}
```

#### 개별속성 정의
font-weight, font-size 등 font를 정의하는 개별속성이 여럿있다. 이런 개별 속성들을 중첩 문법으로 정의할 수 있다.

```scss
li{
	font:{
		weight: 600; /* font-weight: 600; */
		size: 2em; /* font-size: 2em; */
	}
}
```

### 변수
scss의 가장 큰 장점은 변수를 사용해서 값을 저장하고 재활용할 수 있다는 점이다.

#### 문법
`&변수이름: 속성값;`

활용1. 특정 디렉토리의 경로를 변수에 저장해서 지속적으로 활용할 수 있다.
```scss
$color-primary: #e96900;
$url-images: "/assets/images/";
.box{
	background-image: url($url-images + "bg.jpg")
}
```

#### 유효범위
Scope에 대해서 알고있다면 이해가 쉽다. 모든 변수는 lexical scope를 따르며 
외부 scope에서 내부 scope에 접근할 수 없지만, 내부에서 외부는 가능하다.

#### 예시
내부 scope에서 외부 scope 변수에 접근 가능하다.
```scss
$color-primary: #ccc;
.box{
	color: $color-primary; /* color: #ccc; */
}
```
외부 scope에서 내부 scope에 접근하면 오류가 발생한다.
```scss
.box{
	$color-primary: #ccc;
	...
}
color-primary: $color-primary;
```
#### global
내부 scope에서 global 변수에 값을 할당하고 싶다면 `!global` 식별자를 사용해보자.
```scss
$gray: #ccc;
li{
	$gray: #ddd !global;
	color: $gray; /* color = #ddd */
}

img{
	&--x1{
		color: $gray; /* img--x1{ color: #ddd } */
	}
}
```

#### default
외부에서 선언한 같은 이름의 변수가 있을 때 내부에서 선언한 변수를 무시하고 싶다면 `!default` 식별자를 사용해보자.

```scss
$gray: #ccc;
.gray{
	$gray: #ddd !default;
	color: $gray; /* color: #ccc */
}
```

##### 활용
- bootstrap같은 css 프래임워크의 경우 개발자가 선언한 변수에 우선순위를 부여하기 위해 !default를 사용하고있다.

#### {}(string format)
문자 내부에 변수 값을 넣을 수 있다.

```scss
$result: pass;
`@import url("http://www.naver.com/{result}")`;
```

#### import 가져오기

##### 문법
url 함수를 사용하지 않고 문자만 사용한다.
`@import "hello.css"`

특징:
1. Scss파일을 가져오면 가져온 문서의 모든 변수를 사용할 수있다.
2. css파일을 가져올 경우에는 단순히 Scss문법을 css문법으로 바꾸기만 한다.
3. 파일앞에 `_`를 붙이면 열결된 파일을 개별파일이 아닌 결합된 파일로 반환한다.

아래 예제는 모두 css문법으로 변환된다.
```scss
@import "hello.css";
@import "http://hello.com/hello";
@import url(hello);
@import "hello" screen;
```

#### 연산
Sass는 연산 기능을 지원한다.

##### 산술연산
1. +
2. -
3. \* : 하나 이상의 값이 반드시 숫자여야 한다.
4. / : 오른쪽 갑이 반드시 숫자여야 한다.

###### 나누기 연산 주의 사항
css는 속성 값의 숫자를 분리하는 방법으로 /를 허용하기 때문에 다음 방법으로 연산해야 한다.

1. 변수에 저장한 값과 함께 연산한다.
2. 값이 ()로 묶는다.
3. 다른 산술 표현식의 일부로 사용한다.

##### 비교연산
1. ==
2. !=
3. <
4. \>
5. <=
6. >=

##### 논리(불린) 연산자

1. and
2. or
3. not

##### 문자연산
`""`가 있어도 문자이며 없어도 문자로 취급한다.

첫번째 연산 값에 따옴표가 포함되어있다면 따옴표로 연산된 결과를 반환하고, 아닌 경우는 따옴표가 없는 결과를 반환한다.


#### 재활용(Mixins)

##### 선언
```scss
@mixin size($w: 100px, $h: 100px){
	width: $w;
	height: $h;
}
```

##### 사용
```scss
@include size(100px, 200px)
```

### 내장함수(Built-in Functions)
Sass는 기본적으로 유용한 내장함수를 제공한다.

#### 특징
- 여타 언어와 다르게, 1부터 순서를 매긴다.
- `[]`는 선택 가능한 인수로 필수로 입력하지 않아도 된다.

#### 문법
```scss
@use "sass:color;" /* 함수 사용 선언 */
.button {
	$primary-color: #6b717f;
	color: color.scale($primary-color, $lightness: 20%); /* 함수 사용 */
}
```

#### 종류

- `sass:math`: math 모듈은 사칙연산 등의 숫자에 관련된 함수를 제공한다.
- `sass:string`: string 모듈은 문자를 합치거나, 분리하는 등의 문자 관련 함수를 제공한다.
	- `quote(string)`: 입력된 값을 string 형식으로 변환한다. // ex) quote(hello world) => Result: "hello world"
	- `str-index(string, substring)`: 입력된 문자열 내에서 부분 문자가 포함되어 있다면 index를 반환한다. // ex) str-index("Hello world!", "H") => Result: 1
	- `str-slice(string, start, end)`: 입력된 문자열에서 시작 index 부터 끝 index까지에 해당하는 부분 문자열을 반환한다. // ex) str-slice("Hello world!", 2, 5) => Result: "ello"
- `sass:color`: color 모듈 색상 관련 함수를 제공한다.
	- `mix(color1, color2, weight)`: 두 개의 색상을 섞는다.
	- `red(color)`: 색상의 빨간색 값을 반환한다. // ex) red(#7fffd4); => Result: 127
	- `invert(color, weight)`: 반대되는 색을 반환한다. // ex) invert(white);
Result: black
- `sass:list`: list 모듈을 두 개의 list를 합치는 등의 list 관련 함수를 제공한다. 단 모든 list의 내장 함수는 기존 list의 데이터를 갱신하지 않고, 새로운 list를 반환한다.
- `sass:map`: map 모듈은 key와 value의 형태의 자료를 저장하고 있는 map 자료구조를 다루기 위한 연산들의 집합이다.
	- `map-get(map, key)`: key에 해당하는 value를 반환한다. // ex) map-get($font-sizes, "small") => Result: 12px
	- `map-keys(map)`: map의 모든 key list를 반환한다. // ex) map-keys($font-sizes) =>Result: "small", "normal, "large"
- `sass:selector`: selector 모듈은 선택자를 통해 새로운 선택자를 연산후 반환합니다.
	- `append("a", ".disabled")`: "a" 선택자와 ".disabled" 선택자를 합친후 값을 반환한다 // "a.disabled"
	- `extend("a.disabled", "a", ".link")`: "a.disabled" 선택자에서 "a"를 찾아 ".link"로 대체한다. // ".link.disabled"
	- `simple-selector("div.myInput")`: 입력된 선택자를 개별적인 작은 단위의 선택자로 나누어 list로 반환한다. // div, .myInput, :before


#### 참고자료:
- https://www.w3schools.com/sass/sass_functions_selector.asp
- https://sass-lang.com/documentation/modules/
- https://heropy.blog/2018/01/31/sass/