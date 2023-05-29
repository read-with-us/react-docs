# Describing the UI

- [Describing the UI](#describing-the-ui)
  - [Describing the UI](#describing-the-ui-1)
  - [Your first component](#your-first-component)
    - [🤷‍♀️ 질문](#️-질문)
  - [Importing and exporting components](#importing-and-exporting-components)
    - [🤷‍♀️ 질문](#️-질문-1)
  - [Writing markup with JSX](#writing-markup-with-jsx)
    - [🤷‍♀️ 질문](#️-질문-2)
  - [JavaScript in JSX with curly braces](#javascript-in-jsx-with-curly-braces)
  - [Passing Props to a Component](#passing-props-to-a-component)
  - [Conditional Rendering](#conditional-rendering)
  - [Rendering Lists](#rendering-lists)
  - [Keeping Components Pure](#keeping-components-pure)
    - [💡 정보](#-정보)

<br />

> **Note**
>
> 'Describing the UI' 챕터는 다음과 같은 내용을 다룹니다.
>
> - 컴포넌트를 만드는 방법
> - 다중 컴포넌트 파일을 생성하는 시기와 방법
> - JSX로 자바스크립트에 마크업을 더하는 방법
> - JSX에서 `{}`로 자바스크립트 기능에 접근하는 방법
> - props 인수로 컴포넌트 함수를 조절하는 방법
> - 조건에 따라 다른 컨텐츠를 렌더링하는 방법
> - 리스트를 효율적으로 렌더링하는 방법
> - 컴포넌트를 순수 함수로 만드는 방법

<br />

## Describing the UI

1. > JSX는 HTML과 매우 비슷해 보이지만 조금 더 '엄격' 하다

   - 당연시 여겨와서 인지하지 못했던 특징
     <br />

2. > 이와 같은 기존 HTML이 있다면 converter를 사용하여 수정할 수 있습니다

   - 리액트에서 소개해준 converter 툴: (https://transform.tools/html-to-jsx)
     <br />

3. > React에서는 if 문, &&, ? : 연산자 같은 JavaScript 구문을 사용해 조건부로 JSX를 렌더링할 수 있습니다.
   >
   > - (LEARN REACT >DESCRIBING THE UI > Conditional Rendering) 실제로 컴포넌트에서 null을 반환하는 것은 렌더링하려는 개발자를 놀라게 할 수 있기 때문에 일반적이지 않습니다. 부모 컴포넌트의 JSX에 컴포넌트를 조건부로 포함하거나 제외하는 경우가 더 많습니다.

   - && 연산자를 써서 컴포넌트를 렌더링 하는 것보다 (조건) ? (컴포넌트) : null 로 렌더링 하는 방식이 좋다는 의견을 참조했는데, 공식문서의 관련내용중 이런 방식을 권장하지 않는다는 내용이 있음
   - 이에 관해서는 더 공부하면 좋을것같음!
     <br />

4. > prop을 전달하여 이 컴포넌트를 순수하게 만들 수 있습니다.
   - prop을 전달하는 근본적인 의도가 **컴포넌트를 순수** 하게 만들기 위한 것
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

- **베스트 프랙티스** 🤩

<br />

### 🤷‍♀️ 질문

1. > 프로젝트가 성장함에 따라 이미 작성한 컴포넌트를 재사용하여 많은 디자인을 구성할 수 있으므로 개발 속도가 빨라집니다.

   - Q: 회사에서 디자인 시스템을 구축해서 사용 중인데 개발할 때 최대한 재사용하려고 한다. 하지만 결국 서비스마다 요구사항이 다르다보니 조금씩 디자인이 달라져서 그대로 가져다 쓰기는 어렵다. 디자이너 분들도 제약받는다는 느낌을 받는다, **회사에서 디자인 시스템 사용하시는 분 있는가?**
   - A: 컴포넌트를 정해서 만들어두더라도 결국엔 그 컴포넌트들을 여러 곳에서 사용하는 과정에서 추가적으로 다양한 요구사항이 나와서 반영하다보면 기존에 의도한 컴포넌트보다 커스텀 가능범위가 복잡해져, 요구사항 중 **공통요소로 넣기 타당** 하다고 생각되는 부분은 **디자이너분들과 회의 통해서 반영** 하고, 그렇지 않은 경우엔 기존 컴포넌트 수정하지 않고 사용하는 프로젝트에서 **css 오버라이딩 통해서 덮어씌우는 식** 으로 진행
     <br />

2. > Material UI
   - Q: 컴포넌트를 직접 작성하는가? 아니면 MUI 같은 UI 라이브러리 사용하는가?
   - A: **stitches** (https://stitches.dev/) 사용해서 공통적인 테마만 지정해준뒤 직접 스타일컴포넌트 작성하는 식으로 작업, 요즘 **tailwind** (https://tailwindcss.com/) 대세인듯 하여 찍먹 고민중
   - A: tailwind 는 이미 정해져있어 커스텀이 어려울 수 있지만, 또한 정해져있기 때문에 접근이 쉬울수 있음 실습링크:(https://play.tailwindcss.com/)
     <br />

## Importing and exporting components

1. > 이 예제의 컴포넌트들은 모두 App.js라는 root 컴포넌트 파일에 존재합니다. Create React App에서는 앱 전체가 src/App.js에서 실행됩니다. 설정에 따라 root 컴포넌트가 다른 파일에 위치할 수도 있습니다. Next.js처럼 파일 기반으로 라우팅하는 프레임워크일 경우 페이지별로 root 컴포넌트가 다를 수 있습니다.
   - 프레임워크별 경험들 공유해주심💙
   - **Next** : (https://github.com/vercel/next.js/tree/canary/examples) -> next팀에서 만들어놓은 예제들 모음. 위 주소에 나와있는 각 폴더들이 각 Next 프로젝트인데, 아무거나 눌러보시면 그 안의 pages라는 폴더가 있다. next에선 이 **page 폴더를 기준** 으로 라우팅을 알아서 해줌.(최근에 나온 app router 방식은 제외) 예를 들면 pages/index.tsx 는 메인페이지(example.com/), pages/about.tsx는 example.com/about 처럼 되는 방식
   - **gatsby** : gatsby에서도 똑같은 방식으로 라우팅, 그래서 처음에 nextjs에서 gatsby로 넘어갈 때 프로젝트의 구조가 비슷해서 러닝커브가 적게 느낌

<br />

2. > Default vs named exports
   - 이런 자바스크립트 개념들을 중간에 간단명료하게 짚어 주는 부분이 참 좋다고 생각 -**하나의 파일에 하나의 컴포넌트** 를 export default 해서 사용하려고 하고, named export를 사용하는 경우는 **유틸 함수** 같은거 내보낼때 사용
   - 만약 날짜 관련 함수들을 모아놓은 dateUtils.ts 파일 아래와 같이 사용
   ```
   export const convertDateToYYYYMMDD  () => {...}
   export const convertTimeToString  () => {...}
   import { convertDateToYYYYMMDD, convertTimeToString } from './dateUtils.ts'
   ```
   <br />

### 🤷‍♀️ 질문

1.  - Q: (2번관련) 정말 간단한 건 같은 파일에 두지만 대부분 **한 파일에 하나의 컴포넌트만 선언** 하려고 하는 편, 다른 분들 어떤 방식으로 사용 하는가? 컴포넌트 선언 방법 컨벤션 정해두시는 분이 있는가?
    - A: **유틸함수들은 하나의 파일 안에** 모아두는 식으로 사용, **컴포넌트 같은 경우엔 정말 간단한 컴포넌트가 아니라면 한 파일에 하나의 컴포넌트만 사용** 하려 하는 편. 이전에 한 파일에 여러개의 컴포넌트를 정의해두고 사용했던 적이 있었는데 결국 파일을 왔다갔다 보기 힘들었어서 하나하나 분리하는 식으로 수정..!
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
   - 이걸 알기 전에는 그냥 무지성으로 따랐는데 **JSX가 자바스크립트 함수** 로 변환된다는 걸 알고 나니까 자연스럽게 이해되어서 좋음!
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

## Passing Props to a Component

1. > You can think of props like “knobs” that you can adjust. They serve the same role as arguments serve for functions—in fact, props are the only argument to your component! React component functions accept a single argument, a props object:

   - props가 하는 역할을 잘 설명해주고 있다. 다이얼로 조정한다는 표현이 인상깊다.
   - 개정된 React docs에는 비유적인 표현이 많이 사용되어 있다고 느낌. (이해하기 좋음)

   <br />

2. > Use spread syntax with restraint. If you’re using it in every other component, something is wrong. Often, it indicates that you should split your components and pass children as JSX.
   >
   > > spread 문법은 제한적으로 사용하세요. 다른 모든 컴포넌트에 이 구문을 사용한다면 문제가 있는 것입니다.

   - 전개 구문(spread 문법)을 언제 사용하지 말아야하는지, 그리고 대신 어떻게 해야 하는지 대안까지 알려주는 점이 좋다.
   - **Q**: 개발 작업은 결국 스프레드 문법의 사용 여부와 같은 의사 판단의 연속이라고 생각한다. 문서의 이 부분은 그 의사 판단의 기준을 어떻게 세우면 좋은지 가이드 라인을 잡아주는 역할. 하지만 결국 디테일한 판단은 개발자의 주관에 달려 있다. 다른 분들은 이러한 의사 판단을 무엇을 기준으로 하시는지 궁금하다.
   - **A1**: 어떠한 것을 선택했을 때 코드를 읽기 편한지.
   - **A2**: 스프레드 문법을 잘 사용하지 않는디. 예외적으로 사용하는 경우가 디자인 시스템을 사용할 때. 커스텀한 props 외, default로 지정되어있는 props 값을 넘길 때에는 spread로 사용.

   <br />

3. > ```
   > function Avatar(props) {
   >    let person = props.person;
   >    let size = props.size;
   >    // ...
   > }
   > ```

   - **Q**: 저는 props 구조분해할당시 보통 다음과 같은 방식을 사용하는데, props 가져와서 쓰는 것도 여러가지 방식들이 있겠구나 싶다. 다른 분들은 어떻게 하는지?
     ```ts
     function Avatar(props) {
       const { person, size } = props;
     }
     ```
   - **A1**: 문서에 기재된 것 처럼 props를 받아올 때 소괄호 안에서 구조분해할당해서 끌어오는 방식을 선호함.
     ```ts
     function Avatar({ person, size }) {
       // ...
     }
     ```
   - **A2**: 어떠한 방식을 선택하느냐는 취향의 문제이지만, 작업자끼리 컨벤션으로 공통 방식을 합의하는 것이 중요하다고 생각. 코드의 일관성을 유지할 수 있도록.

   <br />

4. > `children` prop을 가지고 있는 컴포넌트는 부모 컴포넌트가 임의의 JSX로 “채울” 수 있는 “구멍”을 가진 것이라고 생각할 수 있습니다. 패널, 그리드 등의 시각적 래퍼에 종종 `children` prop를 사용합니다.

   - typescript 사용 시 children을 props로 받는 컴포넌트는 `PropsWithChildren` 타입을 사용해주면 조금 더 명시적으로 타입 명세 가능 (react18 추가 타입)
   - 참고 링크 : [https://trend21c.tistory.com/2280](https://trend21c.tistory.com/2280)

   <br />

5. > There’s nothing wrong with repetitive code—it can be more legible. But at times you may value conciseness.

   - 반복에 가독성이라는 장점이 있다고는 미처 생각하지 못했다. 새로운 관점을 배웠다.
   - 반복 코드와 중복 코드는 다르다는 생각을 했음.

   <br />

6. > However, props are immutable—a term from computer science meaning “unchangeable”. When a component needs to change its props (for example, in response to a user interaction or new data), it will have to “ask” its parent component to pass it different props—a new object! Its old props will then be cast aside, and eventually the JavaScript engine will reclaim the memory taken by them.
   >
   > > 그러나 props는 불변으로, 컴퓨터 과학에서 “변경할 수 없다”는 뜻의 용어입니다. 컴포넌트가 props를 변경해야 하는 경우(예: 사용자의 상호작용이나 새로운 데이터에 대한 응답으로), 부모 컴포넌트에 다른 props, 즉,새로운 객체를 전달하도록 “요청”해야 합니다! 그러면 이전의 props는 버려지고(참조를 끊는다), 결국 JavaScript 엔진은 기존 props가 차지했던 메모리를 회수(가비지 컬렉팅. GC)하게 됩니다.

   - props가 어떤 식으로 변경되는지 궁금하긴 해도 찾아보진 않았는데 새로 알아간다.

   <br />

## Conditional Rendering

1. > This style works well for simple conditions, but use it in moderation. If your components get messy with too much nested conditional markup, consider extracting child components to clean things up. In React, markup is a part of your code, so you can use tools like variables and functions to tidy up complex expressions.

   ```ts
   function Item({ name, isPacked }) {
     return (
       <li className="item">{isPacked ? <del>{name + " ✔"}</del> : name}</li>
     );
   }
   ```

   - 예시로 나온 위 코드를 다음과 같이 정리하면 조금 더 읽기 수월하다고 생각함.

   ```ts
   function Item({ name, isPacked }) {
     return (
       <li className="item">{isPacked ? <PackedItem /> : <UnpackedItem />}</li>
     );
   }
   ```

   - [del이라는 html 태그](https://developer.mozilla.org/ko/docs/Web/HTML/Element/del)는 처음 봤음. html도 꾸준히 발전하고 새로운 요소가 추가되고 있구나..
   - 중단선을 표현해야 하는 경우 통상적으로 `text-decoration: line-through`같은 css를 사용해왔음

   <br />

2. > In practice, returning null from a component isn’t common because it might surprise a developer trying to render it. More often, you would conditionally include or exclude the component in the parent component’s JSX.

   - 컴포넌트는 항상 무언가를 반환할거라 생각하는데 null을 반환하게 되면 개발자가 컴포넌트 내부 구현을 보지 않는 이상 null을 반환한다는 걸 모를 수도 있다. 그래서 '놀라게 된다'는 것 같다. 그래서 이런 방식보다는 컴포넌트는 항상 무언가를 반환하되 부모 컴포넌트에서 렌더링 여부를 제어하도록 하는 방법을 추천.

   <br />

3. > If your components get messy with too much nested conditional markup, consider extracting child components to clean things up. In React, markup is a part of your code, so you can use tools like variables and functions to tidy up complex expressions.

   - 전에 리팩토링 책을 읽으며 복잡한 표현식이나 로직을 변수나 함수로 추출하고 적합한 이름을 부여해 추상화하는 방식을 봤는데 이와 비슷한 것 같다.
   - 상태명이나 변수명을 시멘틱하게 짓는 것이 중요. 이름 짓기 어렵다.. -> 요즘 chatGPT에게 이름 짓기를 많이 위임하고 있는데 편하고 좋다.

   <br />

4. > React considers false as a “hole” in the JSX tree, just like null or undefined, and doesn’t render anything in its place.

   - 지난 시간에 삼항 연산자로 null을 반환하는 방식이 더 낫다는 의견을 들었다고 공유했는데, 다행히 React의 작동 원리상 논리 연산자를 사용하는 방식과 차이가 없는 것 같다.

   <br />

5. > Don’t put numbers on the left side of &&.

   - 0으로 계산된 값이 false로 넘어가면서 코드가 꼬였던 경험이 있어서 공감되네요.. (+ 여러 명의 공감)
   - 이런 경우를 미연에 방지하기 위해 반드시 해당 값이 의도대로 들어가는지 테스트해보는 편
   - `??`, `||`, `&&`를 사용할 때, string을 number로 변경할 때 등 형변환 관련된 자바스크립트 이슈가 예상되는 경우 주의가 필요하다.
     - string을 number로 변경할 때 `Number()`, `parseInt()`중 무엇을 사용하느냐에 따라 결과값이 달라지는 경우가 꽤 잦다.

   <br />

## Rendering Lists

1. > ```tsx
   > import { people } from "./data.js";
   > import { getImageUrl } from "./utils.js";
   >
   > export default function List() {
   >   const chemists = people.filter(
   >     (person) => person.profession === "chemist"
   >   );
   >   const listItems = chemists.map((person) => (
   >     <li>
   >       <img src={getImageUrl(person)} alt={person.name} />
   >       <p>
   >         <b>{person.name}:</b>
   >         {" " + person.profession + " "}
   >         known for {person.accomplishment}
   >       </p>
   >     </li>
   >   ));
   >   return <ul>{listItems}</ul>;
   > }
   > ```

   - **Q**: 예시 코드와 같은 경우 저는 ListItems를 List 컴포넌트 외부에 별도 선언하여 추상화하는 방향을 개인적으로 선호한다. 다른 분들은 어떠신지 궁금.
   - 👇 이런 느낌..

     ```tsx
     import { people } from "./data.js";
     import { getImageUrl } from "./utils.js";

     const listItems = (person) => (
         <li>
            <img src={getImageUrl(person)} alt={person.name} />
            <p>
               <b>{person.name}:</b>
               {" " + person.profession + " "}
               known for {person.accomplishment}
            </p>
         </li>;
      )

     export default function List() {
       const chemists = people.filter(
         (person) => person.profession === "chemist"
       );

       return <ul>{chemists.map((person) => <listItems person={person} />)}</ul>

     }
     ```

   - **A**: 동의한다. 오히려 docs의 예시와 같은 형태를 처음 봐서 신기했음.

   <br />

2. > Keys tell React which array item each component corresponds to, so that it can match them up later. This becomes important if your array items can move (e.g. due to sorting), get inserted, or get deleted. A well-chosen key helps React infer what exactly has happened, and make the correct updates to the DOM tree.
3. > Locally generated data: If your data is generated and persisted locally (e.g. notes in a note-taking app), use an incrementing counter, [crypto.randomUUID()](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID) or a package like [uuid](https://www.npmjs.com/package/uuid) when creating items.
   >
   > > 로컬에서 생성된 데이터: 데이터가 로컬에서 생성되고 유지되는 경우(예: 메모 작성 앱의 메모), 항목을 만들 때 증분 카운터, [crypto.randomUUID()](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID) 또는 [uuid](https://www.npmjs.com/package/uuid)와 같은 패키지를 사용하세요.

   - 얼마 전에 데이터베이스가 아닌 프론트에서 행을 추가할 일이 있어 어떻게 고유 아이디를 부여할지 고민하다가 uuid 패키지를 사용하는 것으로 해결다. 워낙 데이터베이스에서 id를 주는데 익숙해져 있어서 어떻게 해야할지 바로 떠오르지 않았음.
   - uuid 패키지 외에도 다양한 방법이 있다는 걸 알았다. 특히 `crypto.randomUUID()`는 처음 보는 웹 API라서 신기함!
   - 패키지 없이 uuid를 생성할 수 있는 `crypto.randomUUID()`의 존재를 최근에 알게되었는데 (긍정적) 충격이었다..
   - 이 때, map으로 렌더링한 list에서 즉석으로 생성하는 것이 아니라, 로컬에서 데이터에 고유한 id를 부여하는 작업이 선행되어야 한다는 것이 포인트라고 생각
   - 대충 index값 넣을 때가 많았는데 반성하게 된다..

   <br />

4. > 잘 선택한 key는 배열 내 위치보다 더 많은 정보를 제공합니다. 만약 재정렬로 인해 어떤 항목의 위치가 변경되더라도, 해당 항목이 사라지지 않는 한, React는 key를 통해 그 항목을 식별할 수 있습니다.

   - 최근에 key에 대한 질문을 받았을 때 제대로 대답하지 못했음. 두루뭉술하게만 알고 있었는데 확실히 알아간다.

   <br />

5. > 배열에서 항목의 인덱스를 key로 사용하고 싶을 수도 있습니다.

   - 상단 내용 보고 반성하고 있는데 마침 이런 글이...

   <br />

6. > Similarly, do not generate keys on the fly, e.g. with key={Math.random()}. This will cause keys to never match up between renders, leading to all your components and DOM being recreated every time. Not only is this slow, but it will also lose any user input inside the list items. Instead, use a stable ID based on the data.
   >
   > > 마찬가지로 key={Math.random()}과 같이 즉석에서 key를 생성하지 마세요. 이렇게 하면 렌더링될 때마다 key가 일치하지 않아 매번 모든 컴포넌트와 DOM이 다시 생성됩니다. 속도가 느려질 뿐만 아니라 목록 항목 내부의 사용자 입력도 손실됩니다. 대신 데이터에 기반한 안정적인 ID를 사용하세요.

   - 바로 이전 문단에서 왜 렌더링 중에 키를 생성하면 안된다고 이야기하는지 궁금했는데 조금 텀을 두고 설명해주니까 잠시 혼자 생각해볼 수 있었다. 문서를 구성할 때 고민을 많이 한 것 같다.

   <br />

## Keeping Components Pure

1. > Detecting impure calculations with StrictMode
   >
   > ```tsx
   > import * as React from "react";
   > import * as ReactDOM from "react-dom/client";
   > import { StyledEngineProvider } from "@mui/material/styles";
   > import Demo from "./demo";
   >
   > ReactDOM.createRoot(document.querySelector("#root")).render(
   >   <React.StrictMode>.....</React.StrictMode>
   > );
   > ```

   - index 파일에 `StrictMode`로 감싸져있으면 컴포넌트가 두 번 렌더링 된다는 건 알았는데 왜 그러는지는 몰랐음. 이번에 알았네요.
   - `StrictMode`에서 두 번 호출한다는 건 알고 있었는데 그게 컴포넌트의 순수성을 테스트한다는 건 몰랐다. 그리고 순수성을 테스트하기 위해 어려운 검증을 거치는 게 아니라 단순히 두 번 호출해본다는 점이 재미있음.

   <br />

2. > It minds its own business. It does not change any objects or variables that existed before it was called.
   > Same inputs, same output. Given the same inputs, a pure function should always return the same result.
   >
   > > 자신의 일에만 신경씁니다. 호출되기 전에 존재했던 객체나 변수를 변경하지 않습니다.
   > > 동일 입력, 동일 출력. 동일한 입력이 주어지면 항상 동일한 결과를 반환해야 합니다.

   - 순수 함수의 특징

   <br />

3. > 컴포넌트를 레시피라고 생각할 수 있습니다. 레시피를 따르고 요리 과정에서 새로운 재료를 넣지 않으면 매번 같은 요리를 얻을 수 있습니다.

   - 리액트 개념에 함수형 프로그래밍이 자주 언급되어서 흥미로움! 이 문장도 함수형 프로그래밍 잠깐 찍먹할 때 봤던 문장인데 그대로 들어가있어서 재밌다.

   <br />

4. > 함수형 프로그래밍은 순수성에 크게 의존하지만, 언젠가는, 어딘가에서, 무언가가 바뀌어야 합니다. 그것이 프로그래밍의 요점입니다!

   - [한탄] 프론트엔드에서는 특히 이 순수성을 보장하기가 어렵지 않나요..ㅠㅠ
   - 공감..

   <br />

5. > However, it’s fine because you’ve created them during the same render, inside `TeaGathering`. No code outside of `TeaGathering` will ever know that this happened. This is called “local mutation”—it’s like your component’s little secret.
   >
   > > 하지만 `TeaGathering` 내부에서 동일한 렌더링 중에 생성했기 때문에 괜찮습니다. `TeaGathering` 외부의 어떤 코드도 이런 일이 일어났다는 것을 알 수 없습니다. 이를 “지역 변이”라고 하며, 컴포넌트의 작은 비밀과 같습니다.

   - 지역 변이라는 개념 자체는 처음 봤는데 지금까지 무의식적으로 사용한 것 같다. 배열이 담긴 상태가 있을 때 그 배열을 필터링한 값을 변수에 담아서 렌더링한다던가.

   <br />

6. > In React, side effects usually belong inside event handlers. Event handlers are functions that React runs when you perform some action—for example, when you click a button. Even though event handlers are defined inside your component, they don’t run during rendering! So event handlers don’t need to be pure.
   >
   > > React에서 사이드 이펙트는 보통 이벤트 핸들러에 속합니다. 이벤트 핸들러는 사용자가 어떤 동작을 수행할 때(예를 들어, 버튼을 클릭할 때) React가 실행하는 함수입니다. 이벤트 핸들러가 컴포넌트 내부에 정의되어 있긴 하지만 렌더링 중에는 실행되지 않습니다! 따라서 이벤트 핸들러는 순수할 필요가 없습니다.

   - 좀 어려워서 곱씹는 중인데 렌더링 중에 실행되는 것이 아니면 순수할 필요가 없다는 점을 계속 생각하게 된다.

   <br />

7. > If you’ve exhausted all other options and can’t find the right event handler for your side effect, you can still attach it to your returned JSX with a useEffect call in your component. This tells React to execute it later, after rendering, when side effects are allowed. However, this approach should be your last resort.

   - useEffect가 하는 일을 잘 풀어서 설명해주었다. 항상 그냥 사이드 이펙트를 처리하는 훅이라고만 생각했음.
   - 리액트 clean code의 포인트 중 하나가 useEffect 남용으로 인한 사이드 이펙트를 얼마나 최소화하느냐라고 생각한다.

   <br />

### 💡 정보

- 한국에서 커뮤니티 활동을 활발히 하는 FE 시니어 개발자 중 테오라는 분이 함수형 코딩에 대한 블로그 글을 종종 작성해주심. 프론트엔드 영역에 적용할 수 있는 함수형 프로그래밍에 대한 관심이 있다면 테오님 블로그 참고 추천합니다.
- [https://velog.io/@teo](https://velog.io/@teo)
- [함수형 프로그래밍을 배워보자!](https://velog.io/@teo/functional-programming-study)
