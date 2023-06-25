# Managing State

- [Managing State](#managing-state-1)
  - [🤷‍♀️ 질문](#️-질문)
- [Reacting to Input with State](#reacting-to-input-with-state)
  - [🤷‍♀️ 질문](#️-질문)
- [Choosing the State Structure](#choosing-the-state-structure)
- [Sharing State Between Components](#sharing-state-between-components)
- [Preserving and Resetting State](#preserving-and-resetting-state)
- [Extracting State Logic into a Reducer](#extracting-state-logic-into-a-reducer)
- [Passing Data Deeply with Context](#passing-data-deeply-with-context)
- [Scaling Up with Reducer and Context](#scaling-up-with-reducer-and-context)

> **Note**
>
> 'Managing State' 챕터는 다음과 같은 내용을 다룹니다.
>
> - UI 변화를 상태 변화로 생각하는 방법
> - 상태를 잘 구조화하는 방법
> - 컴포넌트 간 상태를 공유하기 위해 '상태를 상위로 올리는' 방법
> - 상태의 보존 또는 초기화 여부를 제어하는 방법
> - 복잡한 상태 로직을 함수로 통합하는 방법
> - 'Prop drilling' 없이 정보를 전달하는 방법
> - 앱이 성장함에 따라 상태 관리를 확장하는 방법

## Managing State

1. > 이는 디자이너가 UI를 바라보는 방식과 비슷합니다.
   - 재밌고 공감가는 표현!
2. > 각 action에 대한 state 업데이트 방법은 파일 맨 마지막 부분의 reducer 함수에 명시되어 있습니다.
   - useReducer를 제대로 사용해본 적이 없는 것 같아요..! 리덕스랑 사용 방식이 비슷하군요..!!
3.

```jsx
import { useContext } from "react";
import { LevelContext } from "./LevelContext.js";

export default function Heading({ children }) {
  const level = useContext(LevelContext);
  switch (level) {
    case 0:
      throw Error("Heading must be inside a Section!");
    case 1:
      return <h1>{children}</h1>;
    case 2:
      return <h2>{children}</h2>;
    case 3:
      return <h3>{children}</h3>;
    case 4:
      return <h4>{children}</h4>;
    case 5:
      return <h5>{children}</h5>;
    case 6:
      return <h6>{children}</h6>;
    default:
      throw Error("Unknown level: " + level);
  }
}
```

- 컴포넌트 본문이 switch/case로 처리된 방식을 처음 봤어요. 신선하네요!

### 🤷‍♀️ 질문

- Q: 개발 할 때 useReducer 많이들 사용하시나요?
- A: 회사에서 한번도 사용해본적이 없는것같아요

## Reacting to Input with State

1. > 운전기사에게 어디서 꺾어야 할지 알려주는게 아니라 가고 싶은 곳을 말한다고 생각해 보세요. 당신을 거기까지 데려다주는 것은 운전기사의 일이고 운전기사는 어쩌면 당신이 몰랐던 지름길을 알고 있을지도 모릅니다!

- 역시 일은 전문가에게 👍

2. > 여러가지 “state”를 가지고 있는 “상태 기계”라는 것을 컴퓨터 과학에서 들어본 적이 있을 것입니다. 그리고 디자이너와 일한다면 다양한 “시각적 state”에 관한 모형을 본 적이 있을 것입니다.
   - 챗지피티 답변: https://chat.openai.com/share/3307058a-dc9a-4e21-894f-0fdf6cb132b7
3. > 목표는 state가 사용자에게 유효한 UI를 보여주지 않는 경우를 방지하는 것입니다. (예를 들면 오류 메시지가 나타났는데 인풋이 비활성화 돼 있어 유저가 오류를 수정할 수 없는

- UX 관련 언급도 있어서 재밌네요..! ㅎㅎㅎ

4. > 리듀서를 사용하면 여러 state 변수를 하나의 객체로 통합하고 관련된 모든 로직도 합칠 수 있습니다!
   - 아직 회사에서 개발할때 리듀서를 사용해 본 적이 없는데 다음 스터디 때 리듀서와 좀 더 친해져서 잘 활용하게 될 수 있기를…바라며..
   - 저도 잘 써보지 못했어요! 커스텀 훅이랑 어떤 차이가 있는지 각각 언제 써야하는지 궁금해지네요. 아마 Escape Hatches에서 나오겠죠?
5. > 선언형 프로그래밍은 UI를 세밀하게 직접 조작하는 것(명령형)이 아니라 각각의 시각적 state로 UI를 묘사하는 것을 의미합니다.
   - 개인적으로 '명령형' 이라는 말이 잘 와닿지 않았는데 micromanaging 이라고 하니 확실히 알겠어요..!

### 🤷‍♀️ 질문

- Q: 혹시 state machine 에 대해 들어보신분이 계실까요?
- A: 전 입사 하고 첫 프로젝트에서 xstate라는 js 상태머신 라이브러리 사용하는걸 곁눈질로 봤었어요! 상태관리 목적으로 사용했었는데 러닝커브가 좀 있긴 하더라구요…

## Choosing the State Structure

1. > 부모 구성 요소가 다른 값을 전달하는 경우messageColor나중에(예:'red'대신에'blue'),color 상태 변수업데이트되지 않습니다!상태는 첫 번째 렌더링 중에만 초기화됩니다.
   - 이렇게 prop을 바로 state에 넣어서 사용해 본 적은 없는것 같아요..(그냥 우연의 일치인듯) 그래서 이 설명대로 진짜 업데이트가 안되나 하고 해보니까 정말 안되네요!

```jsx
import * as React from "react";
import "./style.css";

export default function App() {
  const [color, setColor] = React.useState("red");

  return (
    <div>
      <button
        onClick={() => {
          setColor("blue");
        }}
      >
        change color
      </button>
      <div>color in parent: {color}</div>
      <Message messageColor={color} />
    </div>
  );
}
function Message({ messageColor }) {
  const [color, setColor] = React.useState(messageColor);
  return (
    <div>
      <div>-------------------------</div>
      <div>this is Message component!</div>
      <div>color: {color}</div>
    </div>
  );
}
```

2. > 규칙에 따라 소품 이름을 다음으로 시작합니다.initial또는default새 값이 무시됨을 명확히 하기 위해:
   - 컨벤션 기억해두기
3. > 상태는 다음과 같이 복제되었습니다.
   > items = [{ id: 0, title: 'pretzels'}, ...]
   > selectedItem = {id: 0, title: 'pretzels'}
   > 그러나 변경 후 다음과 같습니다.
   > items = [{ id: 0, title: 'pretzels'}, ...]
   > selectedId = 0
   - 이전 코드처럼 선언된 상태 꽤 자주 본 것 같고 자주 그렇게 썼던 것 같네요. 싱크는 어떻게 맞췄던 거지 😅
4. > Immer
   - immer가 굉장히 적극적으로 언급되어 재밌어요 ㅋㅋㅋㅋ 처음 나왔을 땐 분명 저희 모두 필요성을 느끼지 못했는데.. 적극적으로 영업하는 리액트 독스..

## Sharing State Between Components

1. > State 끌어올리기 (lifting state up)
   - 많이 사용해왔는데 뭐라고 부르는지는 처음 알았네요 ㅎㅎㅎㅎ
2. > 각 state가 어디에 “생존”해야 할지 고민하는 동안 state를 아래로 이동하거나 다시 올리는 것은 흔히 있는 일입니다. 이건 모두 과정의 일부입니다!
   - 작업하면서 state를 얘가 갖고 있어야하는지 아니면 위로 올려서 prop으로 받을지 이리저리 옮기던 지난날 저의 모습이… 앞에 나왔던 state 흐름도를 그려가며 작업을 시작한다면 좀 덜 헤매지않을까 싶습니다

## Preserving and Resetting State

1. > Browsers use many tree structures to model UI. The DOM represents HTML elements, the CSSOM does the same for CSS. There’s even an Accessibility tree!
   - DOM도 알고 CSSOM도 존재는 알고 있었는데 Accessibility tree라는 게 있는 줄은 몰랐어요. 링크를 보니까 스크린 리더기 기능을 제공할 때 필요하군요!
2. > React also uses tree structures to manage and model the UI you make. React makes UI trees from your JSX.
   - [질문] UI 트리는 가상 돔을 뜻하는 걸까요? 공식 문서에 virtual DOM 검색 결과가 없긴 합니다.
   - A: 그 다음에 오는 문장을 보면 ui 트리와 브라우저 dom이 일치하도록 브라우저 dom을 업데이트한다는걸 보니까 그런것 같기도 해요.. 좀 더 검색해볼게요😅
   - (혹시 아시는 분 계시면 알려주시옵소서...🙇)
3. > You might expect the state to reset when you tick checkbox, but it doesn’t! This is because both of these `<Counter />` tags are rendered at the same position. React doesn’t know where you place the conditions in your function. All it “sees” is the tree you return.
   - 완전 의외의 결과였어요! React가 구분하는 건 컴포넌트가 아니라 위치라는 걸 잘 기억해둬야 겠네요.
   - 위치 기준이라는 건 참 생소하네요..!
4. > Remember that keys are not globally unique. They only specify the position within the parent.
   - 리스트처럼 sibling 사이에서만 유니크하면 되는 건가 생각했는데 동일하네요.
   - 토스트(스낵바) 라이브러리를 사용하면서 매번 새로운 토스트를 띄워야 할 때 key를 활용했었어요!
5. > Preserving state for removed components
   - 이 부분을 보니까 vue에서 keepAlive라는 기능을 사용했던게 기억났어요! keepAlive로 감싸진 컴포넌트는 캐싱되어서 해당 컴포넌트의 state를 보존할수 있어요! 예전에 리액트로 작업하다가 리액트는 keepAlive 같은게 없나 하고 찾아봤던 기억이 나네요,,
     ```javascript
     <KeepAlive>
         <component :is="view"></component>
     </KeepAlive>
     ```
   - 리액트 사용하면서는 한 번도 생각해보지 못했네요. 항상 컴포넌트랑 상태가 같이 간다고 생각했어요. react-hook-form에서 shouldUnregister라는 속성이 있는데 언마운트된 필드의 값을 유지시켜주는 걸 본 적이 있습니다.
   - 리액트 메인테이너 분이 메모리 문제로 이런 기능을 도입할 생각이 없다고 하신 걸 찾았어요. [링크](https://github.com/facebook/react/issues/12039#issuecomment-411621949)

## Extracting State Logic into a Reducer

1. > Instead of telling React “what to do” by setting state, you specify “what the user just did” by dispatching “actions” from your event handlers. (The state update logic will live elsewhere!)
   - 리듀서가 추상화를 위한 것이었구나. 컴포넌트를 살펴 볼 때 세부 구현 사항을 신경 쓸 필요가 없네요.
   - 저도 이 부분을 표시했었는데요! 개발자가 로직을 구성할 때 하나하나 컨트롤 하려는 생각보다는 액션 위주로 (유저가 '저장'을 눌렀다, '취소'를 눌렀다) 생각하며 구성하는게 좋은걸까 하는 생각이 들었어요
2. > Because the reducer function takes state (tasks) as an argument, you can declare it outside of your component.
   - 상태는 컴포넌트에 속하는 것이 아니라 React가 관리한다는 걸 다시 한 번 상기시켜 주네요.
3. > We recommend wrapping each case block into the { and } curly braces so that variables declared inside of different cases don’t clash with each other.
   - 부끄럽지만 얼마 전에야 case문에서 괄호를 사용할 수 있다는 걸 알게 되었답니다. 😅
   - 저두..
4. > Why are reducers called this way?
   - 저도 이게 궁금해서 검색했던 기억이 나요ㅎㅎ 뭘 줄인다는 건가 싶었는데 reduce 메소드의 동작 방법에서 유래한거군요!
5. > The function you pass to reduce is known as a “reducer”. It takes the result so far and the current item, then it returns the next result. React reducers are an example of the same idea: they take the state so far and the action, and return the next state. In this way, they accumulate actions over time into state.
   - 한 번도 배열의 reduce와 연관 짓지 않았는데 왜 그랬을까요 ㅎㅎ 영어를 잘 하는 분들은 자연스럽게 알았겠죠?
6. > Component logic can be easier to read when you separate concerns like this.
   - 이건 리액트에만 국한된건 아닌 것 같아요! 관심사의 분리를 항상 염두하며 코딩해야겠습니다!
7. > Now the event handlers only specify what happened by dispatching actions, and the reducer function determines how the state updates in response to them.
   - 이 문장을 보니까 타입이 왜 add가 아니라 added인지 약간 이해가 됩니다.
8. > Reducers are not without downsides! Here’s a few ways you can compare them
   - 정확히 궁금했던 부분을 마지막에서 정리해주어서 좋아요! useReducer를 쓸 일이 없었던 이유가 useState와 동등하게 변환될 수 있어서 였다는 것도 알게 되었어요. 근데 이 페이지 쭉 보니까 복잡해지면 안 쓸 이유가 없는 것 같아요.
9. > Testing: A reducer is a pure function that doesn’t depend on your component. This means that you can export and test it separately in isolation.
   - 회사에서 이런 비슷한 말을 들은 적이 있어요! 컴포넌트에 다 섞지 말고 함수로 잘 분리해놓으면 테스트하기도 용이할거라고.. (혹시 다른분들 회사에서 테스트 코드도 작성 하시는지 궁금하네요👀)
   - A: 테스트 코드는 작성하지 못하고 있답니다. 바쁜 것도 사실이지만 레거시 코드는 도무지 테스트할 수 있는 상태가 아니고 테스트 코드 작성 경험이 있는 분도 없어서 쉽사리 시작하기가 어렵네요 ㅎㅎ
   - 유데미 테스트 관련 [강의](https://www.udemy.com/course/react-testing-library/)

## Passing Data Deeply with Context

1. > Wouldn’t it be great if there were a way to “teleport” data to the components in the tree that need it without passing props? With React’s context feature, there is!
   - 이 부분 보면서 공식 문서보다는 광고나 마케팅 문구 같다는 생각이 들었어요 ㅋㅋ 근데 딱딱하지 않아서 좋아요.
2. > Context: an alternative to passing props

   - vue에도 이런게 있나 싶어서 찾아봤는데 있네요! 예전엔 상태관리 라이브러리를 사용했기 때문에 사용해본적 없는 기능이지만요..!

     ```javascript
     // 값을 공유하려는 컴포넌트 내에서 아래처럼 값을 설정합니다
     import { provide } from "vue";
     provide(/* key */ "message", /* value */ "hello!");

     // 공유된 값을 사용해야 하는 컴포넌트 내에서 아래처럼 사용합니다.
     import { inject } from "vue";
     const message = inject("message");
     ```

3. > Since context lets you read information from a component above, each Section could read the level from the Section above, and pass level + 1 down automatically. Here is how you could do it:

   - 와 가장 가까운 Context에서 값을 가져오는 걸 이렇게 활용할 수가 있네요! 코딩테스트 문제 끙끙거리면서 풀고 해답 보면 다른 분이 단 몇 줄로 깔끔하게 짠 코드 보는 기분이에요 🥲

4. > Context lets you write components that “adapt to their surroundings” and display themselves differently depending on where (or, in other words, in which context) they are being rendered.
   - Context라는 이름도 정말 신중하게 지은 거군요.
   - context라는 단어는 아직도 참 어렵네요.. 실행 컨텍스트도 그렇고 대체 컨텍스트란..!!
5. > How context works might remind you of CSS property inheritance.
   - 이렇게 유사한 개념을 연결시켜주는 것이 이번 문서의 큰 장점이라고 생각합니다!
6. > Current account: Many components might need to know the currently logged in user.
   - 지금 회사에서 next로 만들고 있는 프로덕트가 있는데 로그인 인증 처리를 위해 [next-auth](https://next-auth.js.org/)라는 라이브러리를 사용하고 있어요 거기서 SessionProvider로 앱 최상단을 감싸주는데 이 부분 보고 혹시? 싶어서 찾아 봤더니 [내부에는 SessionContext.provider 로 이루어져 있었네요](https://github.com/nextauthjs/next-auth/blob/1b80a18dd41c9c6d36e8b114db00b25e53f2cc3c/packages/next-auth/src/react/index.tsx#L492)..!!
7. > If you pass a different value on the next render, React will update all the components reading it below!
   - 근데 이러면 원하지 않는 컴포넌트도 죄다 업데이트 된다던지하는 부작용도 있지 않을까 싶어요..
   - 맞아요. Consumer로서 상태를 사용하지 않은 컴포넌트도 다 리렌더링된다는 것 같네요. 다른 상태 라이브러리 중에 이런 문제를 보완한 게 있다고 들은 적이 있는데 기억이 안나서 😅좀 더 찾아볼게요!
   - ([recoil](https://recoiljs.org/ko/docs/introduction/motivation/) <- 요 링크 보시면 하단에 `상태를 사용하는 컴포넌트를 수정하지 않고도 상태를 파생된 데이터로 대체할 수 있다.` 라는 부분이 나와있어서 recoil 인가 싶은데 혹시 다른 상태 관리 라이브러리 아는 분 계시면 알려주세여~ 🙇)
8. > [트위터](https://twitter.com/oliverloops/status/1672048391314976771?s=12&t=fIUAOjUF3fd_8NToV5L_tQ)에서 이런걸 봤어요.. 이런 경우에 리듀서를 활용할 수 있지않을까 싶네요😈

## Scaling Up with Reducer and Context

1. > You can think of TasksProvider as a part of the screen that knows how to deal with tasks, useTasks as a way to read them, and useTasksDispatch as a way to update them from any component below in the tree.
   - 왜 두 개의 Context를 사용하는지 궁금했는데 조회와 수정을 구분하기 위해서였네요.
2. > You can also export functions that use the context from TasksContext.js
   - 이거 보고 또 혹시? 했는데요.. next에서 useRouter 라는 라우터 관련 훅이 있는데 이것도 결국 context로 만든거였네요 [(깃헙 링크)](https://github.com/vercel/next.js/blob/875b205b57a4bb3e3016279268e334fee5692c29/packages/next/client/router.ts#L125)
     ```javascript
     export function useRouter() {
       return React.useContext(RouterContext);
     }
     ```
   - 이렇게 라이브러리들에서 context를 사용하는걸 보면, 자주 안 바뀌는 어떤 값을 전역적으로 사용해야 할 때 context 를 사용하나 봅니다
