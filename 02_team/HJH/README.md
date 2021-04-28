# SASS(SCSS)

## 주요 구현

SassScript는 여러 언어로 구현되었다.

- 최초의 오픈소스는 Ruby로 작성되었다. 그러나 유지보수자의 부족으로 deprecate 되었다.
- 공식 오픈소스는 Dart 로 작성되었다.
- libSass는 C++로 작성되었다.
- npm에 발표된 오프소스는 JavaScript로 작성되었다.

*사견* 이 외에도 다양한 언어로 구현되었지만, 구현 언어들이 객체지향 프로그래밍을 지원하므로 객체지향 프로그래밍의 장점인 캡슐화와 모듈화가 지원되는 형태로 구현된 듯 하다. 기존 CSS에서는 이와 같은 기능이 제한적으로나마 구현가능하지만 그 형태는 불완전하다. 

## CSS Preprocessor란?

- CSS 전(예비)처리기
- 전처리기로 작성하고 CSS로 컴파일해서 동작한다.

## Sass와 SCSS 차이점

Sass(Syntatically Awesome Style Sheets)의 3번전에서 새롭게 등장한 SCSS는 CSS 구문과 완전히 호환되도록 새로운 구문을 토입해 만든 Sass의 모든 기능을 지원하는 CSS의 상위집합(Superset)이다.

- 차이
  - `{}`(중괄호)와 `;`(세미콜론)의 유무
  - Mixins(재사용 가능한 기능을 만드는 방식)

- Sass 예제

```Sass
.list
  width: 100px
  float: left
  li
    color: red
    background: url("./image.jpg")
    &:last-child
      margin-right: -10px // {}와 ; 미포함
      
=border-radius($radius)
  -webkit-border-radius: $raduis
  -moz-border-radius:    $radius
    -ms-border-radius:     $radius
    border-radius:         $radius // = 와 + 기호로 Mixins
```

- SCSS 예제

```scss
.list {
  width: 100px;
  float: left;
  li {
    color: red;
    background: url("./image.jpg");
    &:last-child {
      margin-right: -10px;
    }
  } // {}dhk ; 포함
}

@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  -ms-border-radius: $radius;
  border-radius: $radius; // @mixin과 @include로 Mixins
}

.box { @include border-radius(10px); }
```

## 데이터 종류(Data Types)

| 데이터 | 설명 | 예시 |
| ---- | ---- | ---- |
| Numbers | 숫자 | 1, .82, 20px, 2dm |
| Strings | 문자 | bold, relative, "/imags/a.png", "dotum" |
| Colors | 색상 표현 | red, blue, #FFFF00, rgba(255,0,0,.5) |
| Booleans | 논리 | true, false |
| Nulls | 아무것도 없음 | null |
| Lists | 공백이나 `,`로 구분된 값의 목목 | (apple, orange, banana), apple orange |
| Maps | Lists와 유사하나 값이 Key: Value 형태 | (apple: a, orange: o, banana: b) |


## 중첩(Nesting)

```scss
.section {
  width: 100%;
  .list {
    padding: 20px;
    li {
      float: left;
    }
  }
}
```

compiled to

```css
.section {
  width: 100%;
}
.section .list {
  padding: 20px;
}
.section .list li {
  float: left;
}
```

## Ampersand (상위 선택자 참조)

중첩 안에서 `&` 키워드는 상위(부모) 선택자를 참조하여 치환한다.

```scss
.btn {
  position: absolute;
  &.active {
    color: red;
  }
}

.list {
  li {
    &:last-child {
      margin-right: 0;
    }
  }
}
```

complied to

```css
.btn {
  position: absolute;
}
.btn.active {
  color: red;
}
.list li:last-child {
  margin-right: 0;
}
```

`&` 키워드가 참조한 상위 선택자로 치환되기 때문에 다음과 같이 응용 가능하다.

```scss
.fs {
  &-small { font-size: 12px; }
  &-medium { font-size: 14px; }
  &-large { font-size: 16px; }
}
```

compiled to

```css
.fs-small {
  font-size: 12px;
}
.fs-medium {
  font-size: 14px;
}
.fs-large {
  font-size: 16px;
}
```

## at-root (중첩 벗어나기)
```scss
.list {
  $w: 100px;
  $h: 50px;
  li {
    width: $w;
    height: $h;
  }
  @at-root .box {
    width: $w;
    height: $h; // box 클래스에서 list 클래스에서 사용하는 $w, $h가 같은 값이어야 한다면 이는 유용하다.
  }
}
```

compiled to

```css
.list li {
  width: 100px;
  height: 50px;
}
.box {
  width: 100px;
  height: 50px;
}
```

## 중첩된 속성

`font-`, `margin-` 등과 같이 동일한 네임 스페이스를 가지는 속성들은 다음과 같이 사용가능하다.

```scss
.box {
  font: {
    weight: bold;
    size: 10px;
    family: sans-serif;
  };
  margin: {
    top: 10px; // margin-top
    left: 20px; // margin-left
  };
  padding: {
    bottom: 40px; // padding-bottom
    right: 30px; // padding-right
  };
}
```

## 변수(Variables)

반복적으로 사용되는 값을 변수로 지정할 수 있다.
변수 이름 앞에는 `$`을 붙인다.

```scss
$변수이름: 속성값;
```

- 예시
```scss
$color-primary: #e96900;
$url-images: "/assets/images/";
$w: 200px;

.box {
  width: $w;
  margin-left: $w;
  background: $color-primary url($url-iamges + "bg.jpg");
}
```

compiled to

```css
.box {
  width: 200px;
  margin-left: 200px;
  background: #e96900 url("/assets/images/bg.jpg");
}
```

## 변수 유효범위(Variable Scope)

변수는 선언된 블록(`{}`) 내에서만 유효범위를 갖는다.
아래 예시의 `$color`는 `.box1`의 블록 안에서 설정되었으므로 블록 밖의 `.box2`에서는 사용 불가하다.

```scss
.box1 {
  $color: #111;
  background: $color;
}

// Error
.box2 {
  background: $color;
}
```

## 변수 재할당(Variable Reassignment)

다음과 같이 변수에 변수를 할당할 수 있다.

```scss
$red: #FF0000;
$blue: #0000FF;

$color-primary: $blue;
$color-danger: $red;

.box {
  color: $color-primary;
  background: $color-danger;
}
```

compiled to

```css
.box {
  color: #0000FF;
  background: #FF0000;
}
```

## !global (전역 설정)

`!global` 플래그를 사용하면 변수의 유효범위를 전역으로 설정할 수 있다.

```scss
.box1 {
  $color: #111 !global;
  background: $color;
}
// No Error
.box2 {
  background: $color;
}
```

compiled to

```css
.box1 {
  background: #111;
}
.box2 {
  background: #111;
}
```

대신 기존에 사용하던 같은 이름의 변수가 있을 경우 값을 덮어쓸 수 있다.

```scss
$color: #000;
.box1 {
  $color: #111 !global;
  background: $color;
}
.box2 {
  background: $color;
}
.box3 {
  $color: #222;
  background: $color;
}
```

compiled to

```css
.box1 {
  background: #111;
}
.box2 {
  background: #111;
}
/* .box3에서 $color #222로 재할당 */
.box3 {
  background: #222;
}
```

그러나 이런 전역변수 설정은 유지보수를 어렵게 하지 않을까?

## !default (초깃값 설정)

`!default` 플래그는 할당되지 않은 변수의 초깃값을 설정한다. 좀 더 쉽게 설명하면 '기존 변수에 값이 설정되어 있으면 현재 설정하는 변수의 값은 사용하지 않겠다'는 의미로 쓸 수 있다.

```scss
$color-primary: red;

.box {
  $color-primary: blue !default;
  background: $color-primary;
}
```

compiled to

```css
.box {
  background: red; /* 기존에 $color-primary 에 red가 할당되어 있으므로 blue로 새로 할당하더라도 변수 값으로 설정하지 않는다. */
} 
```

## #{} 문자 보간

`#{}`를 이용하여 변수 값을 넣을 수 있다.

```scss
$family: unquote("Droid+Sans"); // Sass의 내장 함수 `unquote()`는 문자에서 따옴표를 제거한다.
@import url("http://fonts.googleapis.com/css?family=#{$family}");
```

compiled to

```css
@import url("http://fonts.googleapis.com/css?family=Droid+Sans");
```

## 가져오기(import)

`@import`로 외부에서 가져온 Sass 파일은 모두 단일 CSS 출력 파일로 병합된다.
또한 가져온 파일에 정의된 모든 변수 또는 Mixins 등을 주 파일에서 사용할 수 있다.

Sass `@import`는 기본적으로 Sass 파일을 가져오는데, 다음의 상황에서는 CSS `@import` 규칙으로 컴파일된다.

- 파일 확장자가 `.css` 일 때
- 파일 이름이 `http://`로 시작하는 경우
- `url()`이 붙었을 경우
- 미디어쿼리가 있는 경우

```css
@import "hello.css";
@import "http://hello.com/hello";
@import url(hello);
@import "hello" screen;
```

## 여러 파일 가져오기

하나의 `@import`로 여러 파일을 가져올 수 있다. 파일 이름은 `,`로 구분한다.

```scss
@import "header", "footer";
```

## 파일 분할(Partials)

프로젝트 규모가 커지면 파일들은 `header`나 `side-menu` 같이 각 기능과 부분으로 나눠 유지보수가 쉽도록 관리하는게 보편적이다.
모든 파일이 컴파일 시 각각의 `~.css` 파일로 나눠서 저장된다면 관리나 성능 차원에서 문제가 될 수 있다.
Sass의 Partials 기능은 파일 이름 팡에 `_`을 붙여(가령, `_header.scss`와 같이) `@import`로 가져오면 컴파일 시 `~.css` 파일로 컴파일되지 않는다.

```bash
Sass-App
  # ...
  |-scss
  | |-header.scss
  | |-side-menu.scss
  | |-main.scss
  # ...
```
`main.scss`로 아래와 같이 나머지 `~.scss` 파일을 가져온다.

```scss
// main.scss
@import "header", "side-menu";
```

이 상황에서 컴파일하면 `scss/`에 있던 파일들을 `css/`안에 각 하나씩의 파일로 컴파일된다.

```bash
Sass-App
  # ...
  ├─css
  │  ├─header.css
  │  ├─side-menu.css
  │  └─main.css
  ├─scss
  │  ├─header.scss
  │  ├─side-menu.scss
  │  └─main.scss
  # ...
```

위와 같은 상황에서 파일이름 앞에 `_`을 붙이면 (_header.scss, _side-menu.scss) 아래처럼 별도의 파일로 컴파일되지 않는다.
```bash
Sass-App
  # ...
  ├─css
  │  └─main.css  # main + header + side-menu
  ├─scss
  │  ├─header.scss
  │  ├─side-menu.scss
  │  └─main.scss
  # ...
```
`Webpack`이나 `Parcel`, `Gulp` 같은 빌드툴에서는 Partials 기능을 사용할 필요 없이, 설정값에 따라 빌드되지만 위의 사항이 권장된다.

## 연산(Operations)

Sass는 다음과 같은 기본적인 연산 기능을 지원한다.

- 산술 연산자

| 종류 | 설명 | 주의사항 |
| ---- | ---- | ---- |
| `+` | 더하기 | |
| `-` | 빼기 | |
| `*` | 곱하기 | 하나 이상의 값이 반드시 숫자(Number)일 것 |
| `/` | 나누기 | 나누는 값(divider)가 반드시 숫자(Number)일 것 |
| `%` | 나머지 | |

- 비교 연산자

| 종류 | 설명 |
| ---- | ---- |
| `==` | 동등 |
| `!=` | 부등 |
| `<` | 대소 / 보다 작은 |
| `>` | 대소 / 보다 큰 |
| `<=` | 대소 및 동등 / 보다 작거나 같은 |
| `>=` | 대소 및 동등 / 보다 크거나 같은 |

- 논리 연산자

| 종류 | 설명 |
| ---- | ---- |
| `and` | 그리고 |
| `or` | 또는 |
| `not` | 부정 |

*파이썬*과 유사하다.

## 숫자(Numbers)

- 상대적 단위 연산

일반적으로 절대값을 나타내는 `px` 단위로 연산을 하지만 상대적 단위(`%`, `em`, `vw` 등)의 연산의 경우 CSS calc()로 연산헤야 한다.

```scss
width: 50% - 20px; // Incompatible units error
width: calc(50% - 20px);
```

- 나누기 연산의 주의사항

CSS는 속성 값의 숫자를 분리하는 방법으로 `/`를 허용하기 때문에 `/`가 나누기 연산으로 사용되지 않을 수 있다.
예를 들어, `font: 16px / 22px serif;` 같은 경우 `font-size: 16px`와 `line-height: 22px`의 속성값 분리를 위해서 `/`를 사용한다.
아래 예제를 보면 나누기 연산자는 연산되지 않고 그래도 컴파일된다.

```scss
div {
  width: 20px + 20px;
  height: 40px - 10px;
  font-size: 10px * 2;
  margin: 30px / 2;
}
```

complied to

```css
div {
  width: 40px;
  height: 30px;
  font-size: 20px;
  margin: 30px / 2;
}
```

위와 같은 상황을 방지하기 위해 아래의 사항을 준수해야 한다.

- 값 또는 그 일부가 변수에 저장되거나 함수에 의해 반환되는 경우
- 값이 `()`로 묶여있는 경우
- 값이 다른 산술 표현식의 일부로 사용되는 경우

```scss
div {
  $x: 100px;
  width: $x / 2;
  height: (100px / 2);
  font-size: 10px + 12px / 3;
}
```

complied to

```css
div {
  width: 50px;
  height: 50px;
  font-size: 14px;
}
```

## 문자(Strings)

문자 연산에는 `+`가 사용된다.
문자 연산의 결과는 첫 번째 피연산자를 기준으로 한다. 첫 번째 피연산자에 따옴표가 붙어있다면 연산 결과를 따옴표로 묶는다.
반대로 첫 번째 피연산자에 따옴표가 붙어있지 않다면 연산 결과도 따옴표로 처리하지 않는다.

```scss
div::after {
  content: "Hello" + World;
  flex-flow: row + "-revers" + " " + wrap;
}
```

compiled to

```css
div::after {
  content: "Hello World";
  flex-flow: row-reverse wrap;
}
```

## 색상(Colors)

색상도 연산할 수 있다.

```scss
div {
  color: #123456 + #345678;
  // R: 12 + 34 = 46
  // G: 34 + 56 = 8a
  // B: 56 + 78 = ce
  background: rgba(50, 100, 150, .5) + rgba(10, 20, 30, .5);
  // R: 50 + 10 = 60
  // G: 100 + 20 = 120
  // B: 150 + 30 = 180
  // A: Alpha channels must be equal
}
```

compiled to

```css
div {
  color: #468ace;
  background: rgba(60, 120, 180, 0.5);
}
```

RGBA에서 Alpha 값은 연산되지 않으므로 서로 동일해야 다른 값의 연산이 가능하다.
Alpha 값을 연산하기 위해 opacify(), transparentize()과 같은 색상 함수(Color Functions)를 사용할 수 있다.

```scss
$color: rgba(10, 20, 30, .5);
div {
  color: opacify($color, .3);  // 30% 더 불투명하게 / 0.5 + 0.3
  background-color: transparentize($color, .2);  // 20% 더 투명하게 / 0.5 - 0.2
}
```

complied to

```css
div {
  color: rgba(10, 20, 30, 0.8);
  background-color: rgba(10, 20, 30, 0.3);
}
```

## 논리(Boolean)

Sass의 `@if` 조건문에서 사용되는 논리(Boolean) 연산에는 `and`. `or`, `not`이 있다.

```scss
$width: 90px;
div {
  @if not ($width > 100px) {
    height: 300px;
  }
}
```

complied to

```css
div {
  height: 300px;
}
```

## 재활용(Mixins)

Sass Mixins는 스타일 시트 전체에서 *재사용 할 CSS 선언 그룹*을 정의하는 기능이다.
mixin 프로그래밍은 기능이 클래스에서 정의되고 다른 클래스들과 기능이 융합되는 방식의 프로그래밍 기법이다.
따라서 

- 선언: `@mixin`
- 사용: `@include`

### @mixin

```scss
@mixin large-text {
  font-size: 22px;
  font-weight: bold;
  font-family: sans-serif;
  color: orange;
}
```

Mixin은 선택자를 포함하고 상위(부모) 요소 참조(`&`)도 가능하다.

```scss
@mixin large-text {
  font: {
    size: 22px;
    weight: bold;
    family: sans-serif;
  }
  color: orange;

  &::after {
    content: "!!";
  } // 여기서 &는 이 mixin을 사용하는 요소의 상위 요소의 참조이다.

  span.icon {
    background: url("/images/icon.png");
  }
}
```

### @include

선언된 Mixin을 사용하기 위해서는 `@include`가 필요하다.
위에서 선언한 mixin `large-text`를 사용한 예는 다음과 같다.

```scss
h1 {
  @include large-text;
}
div {
  @include large-text;
}
```

compiled to

```css
h1 {
  font-size: 22px;
  font-weight: bold;
  font-family: sans-serif;
  color: orange;
}
h1::after {
  content: "!!";
}
h1 span.icon {
  background: url("/images/icon.png");
}

div {
  font-size: 22px;
  font-weight: bold;
  font-family: sans-serif;
  color: orange;
}
div::after {
  content: "!!";
}
div span.icon {
  background: url("/images/icon.png");
}
```

mixin을 사용하고 mixin 속성에 다른 효과를 추가할 수 있다.
이는 객체 지향 프로그래밍의 성격으로 볼 수 있을 듯 하다.

## 인수(Arguments)

Mixin은 함수처럼 인수를 가질 수 있다.
하나의 Mixin으로 다양한 결과를 만들 수 있다.

```scss
@mixin dash-line($width, $color) {
  border: $width dashed $color;
}

.box1 { @include dash-line(1px, red); }
.box2 { @include dash-line(4px, blue); }
```

complied to

```css
.box1 {
  border: 1px dashed red;
}
.box2 {
  border: 4px dashed blue;
}
```

- 인수의 기본값 설정

인수는 기본값을 가질 수 있으므로, `@include` 포함 단계에서 별도의 인수가 전달되지 않으면 기본값이 사용된다.

```scss
@mixin dash-line($width: 1px, $color: black) {
  border: $width dashed $color;
}

.box1 { @include dash-line; }
.box2 { @include dash-line(4px); }
```

compiled to

```css
.box1 {
  border: 1px dashed black;
}
.box2 {
  border: 4px dashed black;
}
```

## 내장 함수(Built-in Functions)

### 모듈

Sass는 유용한 기능(믹스인 포함)을 포함한 내장 모듈을 제공한다. 이 모듈들은 다른 사용자 정의 stylesheet와 같이 `@use rule`로 가져올 수 있고 그 기능들은 다른 모듈 멤버와 동일한 방식으로 호출할 수 있다.
모든 내장 모듈은 `sass:`로 시작하는 URL로 시작한다.

> Sass의 모듈 시스템이 도입되기 전에, 모든 Sass 함수들은 전역 범위에서 사용 가능했다. 많은 함수들이 여전히 전역 이름을 가지고 있다.
> Sass 개발팀은 전역 함수의 사용을 권장하지 않고 머지않아 그들을 deprecate할 예정이다. 그러나 현재는 Sass의 예전 버전과 LibSass(아직 모듈 시스템을 지원하지 않는다.)과 호환을 위해 남아있다.
> 
> 새로운 모듈 시스템에서는 몇 함수만이 `if()`와 같은 특별한 평가 프로시저가 포함되거나 `rgb()`와 `hsl()` 같은 내장 CSS 함수를 이용하기 때문에 전역에서 사용가능할 것이다.
> 이 함수들은 deprecate 되지 않을 것이며 자유롭게 사용 가능하다.

```scss
@use "sass:color";

.button {
  $primary-color: #6b717f;
  color: $primary-color;
  border: 1px solid color.scale($primary-color, $lightness: 20%);
}
```

Sass는 다음과 같은 내장 모듈을 지원한다. 각 모듈에 내장된 함수의 디테일한 사용법은 [sass-lang 공식 문서](https://sass-lang.com/documentation/modules) 를 참고하도록 한다.

- `sass:math` 모듈: type `numbers`와 관련된 함수를 제공한다.
- `sass:string` 모듈: type `strings`를 쉽게 결함하고 검색하고 나눌 수 있는 함수를 제공한다.
- `sass:color` 모듈: type `colors`와 관련된 함수를 제공하는데 색상 테마를 쉽게 만들게 해주며 기존의 색상을 기반으로 새로운 색상을 만드는 기능을 제공한다.
- `sass:list` 모튤: type `lists`의 값에 접근하고 수정하는 함수를 제공한다.
- `sass:map` 모듈: type `map`의 key와 연결된 value를 보고 추적하는 등의 함수를 제공한다.
- `sass:selector` 모듈: Sass의 강력한 셀렉팅 엔진에 접근가능하게 해준다.
- `sass:meta` 모듈: Sass의 디테일한 내부 작업 구조를 보여준다.

### 전역 함수

- hsl(), hsla()
```scss
hsl($hue $saturation $lightness)
hsl($hue $saturation $lightness / $alpha)
hsl($hue $saturation $lightness, $alpha: 1) // default alpha: 1
hsla($hue $saturation $lightness)
hsla($hue $saturation $lightness / $alpha)
hsla($hue $saturation $lightness, $alpha: 1) // default alpha: 1
```

위의 함수는 `hue`(색상; 범위: 0deg ~ 360deg), `saturation`(채도; 범위: 0% ~ 100%), `lightness`(명도; 범위: 0% ~ 100%) 그리고 `alpha`(알파채널, 투명도; 범위: 0(0%) ~ 1(100%))로 하나의 색상을 반환한다.

> Sass에는 나누기 연산과 관련한 파싱 예외가 있기 때문에 `hsl($hue $saturation $lightness / $alpha)` 대신 `hsl($hue, $saturation, $lightness, $alpha)` 사용을 권장한다.

- if()
```scss
if($condition, $if-true, $if-false)
```

위의 함수는 `$condition`이 true이면 `$if-true`를, false이면 `$if-false`를 반환한다.
이 함수는 반환되지 않는 인수를 평가하지 않는다는 점에서 특별하다. 그래서 사용되지 않은 인수가 에러를 발생시키더라도 안전하게 호출될 수 있다.

```scss
@debug if(true, 10px, 15px); // 10px
@debug if(false, 10px, 15px); // 15px
@debug if(variable-defined($var), $var, null); // null; variable-defined(*args)가 정의되지 않아도 안전하게 null을 반환한다.
```

- rgb(), rgba()

```scss
rgb($red $green $blue)
rgb($red $green $blue / $alpha)
rgb($red, $green, $blue, $alpha: 1)
rgb($color, $alpha)
rgba($red $green $blue)
rgba($red $green $blue / $alpha)
rgba($red, $green, $blue, $alpha: 1)
rgba($color, $alpha) //=> color 
```

위의 함수는 `$red`, `$green`, `$blue` 그리고 선택적으로 `$alpha`이 전달되면 하나의 색상을 반환한다.

각각의 인수 자리에는 0부터 255까지 단위가 없는 숫자나 0%와 100% 사이의 퍼센트 수가 올 수 있다. 알파 채널은 0과 1사이의 단위가 없는 수나 0%와 100% 사이의 퍼센트 수가 올 수 있다.

```scss
@debug rgb(0 51 102); // #036
@debug rgb(95%, 92.5%, 89.5%); // #f2ece4
@debug rgb(0 51 102 / 50%); // rgba(0, 51, 102, 0.5)
@debug rgba(95%, 92.5%, 89.5%, 0.2); // rgba(242, 236, 228, 0.2)
```