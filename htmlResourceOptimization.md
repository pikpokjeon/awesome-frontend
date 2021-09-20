# 리소스 로드 성능최적화

## 중요도에 따른 리소스적용
리소스(자바스크립트,스타일,폰트,이미지..)를 웹사이트를 로드할 때 한번에 불러오곤한다.
이렇게 하는경우, 첫 페인팅시 (캐싱된 리소스가 없이) 느려지게된다.
DOM과 CSSDOM을 차레로 읽으며 파싱트리, 렌더트리를 반복적으로 구성하며 attachment(시각적 요소 연경)를 실행하고 렌더링하는 과정에서, 블로킹 없이 리소스를 중요도에 따라 어떻게 불러들일지 고민해봐야 한다.

#### 중요한 리소스
- 페이지의 핵심적인 기능이 포함됨
#### 덜 중요한 리소스
- 페이지가 로드 된 후나, 사용자 인터렉션에 관여하는 경우


## 중요한 CSS
- 여기서 중요한 CSS란, 페이지 컨텐츠의. 초기 로드시 보이는 부분(상단)에 관여하는 스타일이다.
### 방법
- 초기 로드시 보이는 부분의 CSS코드를 불러오는 CSS파일에서 없애고, HTML의 ```<head>```에 ```<style>```태그 안에 바로 넣어준다.
- 직접 추가하게되면, 해당 스타일이 구문 분석되고 첫번째 페인트에(초기 로드)바로 적용된다.

## 덜 중요한 CSS
- 추가 스타일을 로드하는 경우. 렌더 블록킹을 일으키지 않고 웹사이트를 사용 가능케한다.
``` html
<head>
  <!-- ... -->

    <link crossorigin rel="preload" href="/path/to/styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
    <noscript><link rel="stylesheet" href="/path/to/styles.css"></noscript>

  <!-- ... -->
</head>
```

```link rel="preload" as="style"``` 렌더 블록킹을 일으키지않고 CSS파일을 로드한다.

```onload="this.onload=null;this.rel='stylesheet'"``` 
CSS파일이 구문 분석되고, 사이트가 로드된 후 로드되는 것을 확인한다.

```noscript``` 자바스크립트를 사용할 수 없는 경우 표준 방식으로 로드한다.
- google font 예시
```html
<link crossorigin rel="preload" href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600&display=swap" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600&display=swap"></noscript>
```

## 중요한 JS
- 컨텐츠를 렌더링하는 것을 블록킹 하지않고, 모든 DOM노드가 생성되었을 때. 자바스크립트가 접근 가능하게 한다
- ```</body>```태그가 닫히기 전에 ```<script>```를 인라인으로 위치해준다.

## 덜 중요한 JS
- defer : DOM이 전체 렌더링 되었을 때 필요하거나, 상대적인 실행 순서를 보장해야 하는경우. 페이지가 전체 렌더링 된 후, 백그라운드에서 스크립트를 로드 하도록 한다.
- async : 순서에 상관없이 독립적으로 실행되어야 하는경우. 병렬적으로 스크립트를 실행할 수 있다. (다른 스크립트가 전체 실행되는 것을 기다리지 않는다.)
