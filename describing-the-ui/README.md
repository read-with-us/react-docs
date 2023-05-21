# Describing the UI

- [Your first component](#your-first-component)
- [Importing and exporting components](#importing-and-exporting-components)
- [Writing markup with JSX](#writing-markup-with-jsx)
- [JavaScript in JSX with curly braces](#javascript-in-jsx-with-curly-braces)

<br />

> **Note**
>
> 'Describing the UI' 챕터는 다음과 같은 내용을 다룹니다.
>
> - 컴포넌트를 만드는 방법
> - 다중 컴포넌트 파일을 생성하는 시기와 방법
> - JSX로 자바스크립트에 마크업을 더하는 방법
> - JSX에서 `{}`로 자바스크립트 기능에 접근하는 방법

<br />

## Describing the UI
1. >  JSX는 HTML과 매우 비슷해 보이지만 조금 더 '엄격' 하다
   - 당연시 여겨와서 인지하지 못했던 특징
<br />

2. > 이와 같은 기존 HTML이 있다면 converter를 사용하여 수정할 수 있습니다
   - 리액트에서 소개해준 converter 툴: (https://transform.tools/html-to-jsx)
<br />

3. > React에서는 if 문, &&, ? : 연산자 같은 JavaScript 구문을 사용해 조건부로 JSX를 렌더링할 수 있습니다.
   > - (LEARN REACT >DESCRIBING THE UI > Conditional Rendering) 실제로 컴포넌트에서 null을 반환하는 것은 렌더링하려는 개발자를 놀라게 할 수 있기 때문에 일반적이지 않습니다. 부모 컴포넌트의 JSX에 컴포넌트를 조건부로 포함하거나 제외하는 경우가 더 많습니다. 
   - && 연산자를 써서 컴포넌트를 렌더링 하는 것보다 (조건) ? (컴포넌트) : null 로 렌더링 하는 방식이 좋다는 의견을 참조했는데, 공식문서의 관련내용중 이런 방식을 권장하지 않는다는 내용이 있음
   - 이에 관해서는 더 공부하면 좋을것같음!
<br />

4. > prop을 전달하여 이 컴포넌트를 순수하게 만들 수 있습니다.
   - prop을 전달하는 근본적인 의도가 __컴포넌트를 순수__ 하게 만들기 위한 것
   - 순수함수, 함수형 프로그래밍에 관한 이야기도 언급 -> 함수형 프로그래밍이 어떤상황에 쓰이는게 더 좋은지 알아보면 좋을것같음
<br />

## Your first component
1. > 컴포넌트는 다른 컴포넌트를 렌더링할 수 있지만, 그 정의를 중첩해서는 안 됩니다 
 ```
export default function Gallery() {
  // 🔴 Never define a component inside another component!
  function Profile() {
    // ...
  }
  // ...
}
```
   - __베스트 프랙티스__ 🤩

<br />

### 🤷‍♀️ 질문
1. > 프로젝트가 성장함에 따라 이미 작성한 컴포넌트를 재사용하여 많은 디자인을 구성할 수 있으므로 개발 속도가 빨라집니다.
   - Q: 회사에서 디자인 시스템을 구축해서 사용 중인데 개발할 때 최대한 재사용하려고 한다. 하지만 결국 서비스마다 요구사항이 다르다보니 조금씩 디자인이 달라져서 그대로 가져다 쓰기는 어렵다. 디자이너 분들도 제약받는다는 느낌을 받는다, __회사에서 디자인 시스템 사용하시는 분 있는가?__
   - A: 컴포넌트를 정해서 만들어두더라도 결국엔 그 컴포넌트들을 여러 곳에서 사용하는 과정에서 추가적으로 다양한 요구사항이 나와서 반영하다보면 기존에 의도한 컴포넌트보다 커스텀 가능범위가 복잡해져, 요구사항 중 __공통요소로 넣기 타당__ 하다고 생각되는 부분은 __디자이너분들과 회의 통해서 반영__ 하고, 그렇지 않은 경우엔 기존 컴포넌트 수정하지 않고 사용하는 프로젝트에서 __css 오버라이딩 통해서 덮어씌우는 식__ 으로 진행
 <br />
 
2. > Material UI
   - Q: 컴포넌트를 직접 작성하는가? 아니면 MUI 같은 UI 라이브러리 사용하는가?
   - A: __stitches__ (https://stitches.dev/) 사용해서 공통적인 테마만 지정해준뒤 직접 스타일컴포넌트 작성하는 식으로 작업, 요즘 __tailwind__ (https://tailwindcss.com/) 대세인듯 하여 찍먹 고민중
   - A: tailwind 는 이미 정해져있어 커스텀이 어려울 수 있지만, 또한 정해져있기 때문에 접근이 쉬울수 있음 실습링크:(https://play.tailwindcss.com/)
 <br />
 
## Importing and exporting components
1. > 이 예제의 컴포넌트들은 모두 App.js라는 root 컴포넌트 파일에 존재합니다. Create React App에서는 앱 전체가 src/App.js에서 실행됩니다. 설정에 따라 root 컴포넌트가 다른 파일에 위치할 수도 있습니다. Next.js처럼 파일 기반으로 라우팅하는 프레임워크일 경우 페이지별로 root 컴포넌트가 다를 수 있습니다.
   - 프레임워크별 경험들 공유해주심💙 
   - __Next__ : (https://github.com/vercel/next.js/tree/canary/examples) -> next팀에서 만들어놓은 예제들 모음. 위 주소에 나와있는 각 폴더들이 각 Next 프로젝트인데, 아무거나 눌러보시면 그 안의 pages라는 폴더가 있다. next에선 이 __page 폴더를 기준__ 으로 라우팅을 알아서 해줌.(최근에 나온 app router 방식은 제외) 예를 들면 pages/index.tsx 는 메인페이지(example.com/), pages/about.tsx는 example.com/about 처럼 되는 방식
   - __gatsby__ : gatsby에서도 똑같은 방식으로 라우팅, 그래서 처음에 nextjs에서 gatsby로 넘어갈 때 프로젝트의 구조가 비슷해서 러닝커브가 적게 느낌 

<br />

2. > Default vs named exports 
   - 이런 자바스크립트 개념들을 중간에 간단명료하게 짚어 주는 부분이 참 좋다고 생각
   -__하나의 파일에 하나의 컴포넌트__ 를 export default 해서 사용하려고 하고, named export를 사용하는 경우는 __유틸 함수__ 같은거 내보낼때 사용
   - 만약 날짜 관련 함수들을 모아놓은 dateUtils.ts 파일 아래와 같이 사용
   ```
   export const convertDateToYYYYMMDD  () => {...}
   export const convertTimeToString  () => {...} 
   import { convertDateToYYYYMMDD, convertTimeToString } from './dateUtils.ts' 
   ``` 
 <br />
 
### 🤷‍♀️ 질문
1. 
   - Q: (2번관련) 정말 간단한 건 같은 파일에 두지만 대부분 __한 파일에 하나의 컴포넌트만 선언__ 하려고 하는 편, 다른 분들 어떤 방식으로 사용 하는가? 컴포넌트 선언 방법 컨벤션 정해두시는 분이 있는가?
   - A: __유틸함수들은 하나의 파일 안에__ 모아두는 식으로 사용, __컴포넌트 같은 경우엔 정말 간단한 컴포넌트가 아니라면 한 파일에 하나의 컴포넌트만 사용__ 하려 하는 편. 이전에 한 파일에 여러개의 컴포넌트를 정의해두고 사용했던 적이 있었는데 결국 파일을 왔다갔다 보기 힘들었어서 하나하나 분리하는 식으로 수정..!
   - Q: 파일이 많아지더라도 분리하는 방식이 더 편한가?
   - A: 파일명으로 컴포넌트를 바로 파악할 수 있어, 분리하면 더 분리수록 편해진다. 시간이 있다면 더 분리하고 싶다.
 <br />
 
## Writing markup with JSX

1. > 하지만 웹이 더욱 인터랙티브해지면서 로직이 컨텐츠를 결정하는 경우가 많아졌습니다. 그래서 JavaScript가 HTML을 담당하게 되었죠! 이것이 바로 React에서 렌더링 로직과 마크업이 같은 위치의 컴포넌트에 함께 있는 이유입니다.
   - JSX의 탄생 배경을 잘 설명해줌, jQuery를 써본 적은 없지만 굉장히 초창기에 돔을 직접 조작해본 적이 있는데 React에 비하면 쓰기 힘들다.
   - 자바스크립트만으로 돔 조작하는 방식을 경험한 이후에 리액트를 배웠었는데 정말 신세계라고 느낌
   - 현 회사에서 사용하는 입장으로 정말 공감이 가는 내용 ...😥

<br />

2. > 대부분의 경우 React의 화면 오류 메세지는 문제가 있는 곳을 찾는 데 도움이 됩니다. 막혔을 때 읽어주세요!
   - 사람들이 에러 메시지를 그렇게 안 읽나 싶은데🤣. 생각해보면 초보일 때는 제대로 안 읽었던 것 같다고 하심
   - 현재도 그래서 반성하게 되는 내용
   - 친절한 docs..

<br />

3. > JSX는 HTML처럼 보이지만 내부적으로는 JavaScript 객체로 변환됩니다. 하나의 배열로 감싸지 않은 하나의 함수에서는 두 개의 객체를 반환할 수 없습니다.
   - 이걸 알기 전에는 그냥 무지성으로 따랐는데 __JSX가 자바스크립트 함수__ 로 변환된다는 걸 알고 나니까 자연스럽게 이해되어서 좋음!
   - 이 또한 매우 공감 가는 내용

<br />

4. > 역사적인 이유로 aria-* 과 data-*의 어트리뷰트는 HTML에서와 동일하게 대시를 사용하여 작성합니다.
   - 역사적인 이유가 있지만, 이유를 알수는 없었다..

 <br />
 
### 🤷‍♀️ 질문
1. > 변환기를 사용하여 기존 HTML과 SVG를 JSX로 변환하는 것을 추천합니다.
   - Q: 실제로 변환기를 사용하는가?
   - A: 요즘엔 html을 작성할 일이 별로 없어서 처음부터 jsx로 작성하여 사용할 일이 없음
   - A: 기존 html 파일의 경우에 변경을 위해 사용할듯 싶음

 <br />
 
## JavaScript in JSX with curly braces
> 내용없음 😏
