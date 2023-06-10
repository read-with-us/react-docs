# Adding Interactivity

- [Adding Interactivity](#adding-interactivity-1)
- [Responding to Events](#responding-to-events)
  - [🤷‍♀️ 질문](#️-질문)
- [State: A Component's Memory](#state-a-components-memory)
- [Render and Commit](#render-and-commit)
  - [🤷‍♀️ 질문](#️-질문-1)
  - [💡 정보](#-정보)
- [State as a Snapshot](#state-as-a-snapshot)
- [Queueing a Series of State Updates](#queueing-a-series-of-state-updates)
- [Updating Objects in State](#updating-objects-in-state)
- [Updating Arrays in State](#updating-arrays-in-state)

> **Note**
>
> 'Adding Interactivity' 챕터는 다음과 같은 내용을 다룹니다.
>
> - 사용자의 이벤트에 응답하는 방법
> - 컴포넌트가 정보를 기억하게 하는 방법
> - UI를 화면에 표시하는 두 가지 단계
> - 스냅샷처럼 동작하는 상태
> - 상태 업데이트를 큐에 추가하는 방법
> - 객체 상태를 업데이트하는 방법
> - 배열 상태를 업데이트하는 방법

## Adding Interactivity

1. > Unlike regular JavaScript variables, React state behaves more like a snapshot. Setting it does not change the state variable you already have, but instead triggers a re-render. This can be surprising at first!
   - 버그를 많이 겪었던 부분이라 공감된다.
2. > replacing setScore(score + 1) with setScore(s => s + 1)
   - 두 방식의 차이를 명확하게 몰랐는데 이번에 알게 되었다. 두번째 방식으로 사용할 때에 버그를 피할 수 있어 권장되었다.
3. [Immer?](https://github.com/immerjs/use-immer)
   - 라이브러리 자체는 유용해보이지만 코드를 획기적으로 줄여주지는 않는 듯 보인다. 주변에서 사용하는 케이스를 보지 못함
   - 이제 브라우저에서 객체 깊은 복사를 수행해주는 [structuredClone()](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone)를 제공해주니 사용할 이유가 딱히 없지 않을까

## Responding to Events

1. > By convention, it is common to name event handlers as handle followed by the event name. You’ll often see onClick={handleClick}, onMouseEnter={handleMouseEnter}, and so on.
   - 컨벤션 관련된 내용이 다수 보여 함수 관련하여 convention을 정할 때에 react docs를 레퍼런스 삼을 수 있을 것 같다.
2. > In the second example, the `()` at the end of `handleClick()` fires the function _immediately_ during [rendering](https://react-ko.dev/learn/render-and-commit), without any clicks. This is because JavaScript inside the [JSX `{` and `}`](https://react-ko.dev/learn/javascript-in-jsx-with-curly-braces) executes right away. If you want to define your event handler inline, wrap it in an anonymous function like so:
   - 이벤트 핸들러에서 함수를 실행하는걸 지양해야 한단 걸 무지성으로 알고 있었는데, docs에 명확하게 있으니 이유를 확실히 알 수 있었다.
3. > By convention, event handler props should start with on, followed by a capital letter.
   - handle, on이 혼합되어 사용되고 방대한데 규칙이 없었고 리팩토링 시에도 기준이 없었는데 docs에서 명확하게 구분하여 설명해주니까 현실적으로 도움이 많이 됐다.
4. > We say that an event “bubbles” or “propagates” up the tree: it starts with where the event happened, and then goes up the tree.
   - 면접에서 해당 질문을 많이 받은 것 같다. (이벤트 버블링을 설명하라, 이를 어떻게 처리했는지를 설명하라)
5. > All events propagate in React except onScroll, which only works on the JSX tag you attach it to.
   - 한글 번역이 조금 이상한 느낌...
   - on으로 시작하는 이벤트가 많은데, onClick, onChange 등만 제한적으로 쓰는것 같아서 아쉽다.
   - onScroll, onDrop 과 같은 이벤트는 예외케이스 개발 시 사용하게 되는 듯
6. > By convention, it’s usually called e, which stands for “event”.
   - 그동안 event나 error나 모두 풀네임을 적었는데 docs에서 e로 축약해서 사용하는 내용을 적었다.
   - 다들 e로 축약하여 쓰는 걸 선호하는 편!
7. **e.stopPropagation 사용 사례 공유**
   - 큰 div에 card가 4개 있고, card 내부의 이미지에 클릭 이벤트가 있고 각각의 card에도 클릭 이벤트가 있는 형태
   - 이미지 클릭 이벤트에 e.stopPropagation을 사용했었다
   ```jsx
    <div>
        <Card onClick={} />
        <Card onClick={} />
        <Card onClick={} />
        <Card onClick={} />
    </div>
   ```
8. > In rare cases, you might need to catch all events on child elements, even if they stopped propagation. For example, maybe you want to log every click to analytics, regardless of the propagation logic. You can do this by adding Capture at the end of the event name:
   - Capture가 붙은 이벤트가 어떤 용도인지 몰랐는데, 분석 도구 사용할 때에 유용할 것 같다!

### 🤷‍♀️ 질문

- Q: 회사에서 접근성 어떻게 챙기는지?
- A:
  - 특수하게 챙겨야 하는 경우가 아니면 챙기지 못하게 되는 것 같다.
  - eslint에 [접근성 챙기는 컨벤션](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y)을 정립하니까 Error 표시에 맞춰 수정하면 되어 요긴했다.
  - chrome 라이트하우스에서 간단하게 접근성 체크하는 방식
  - [스토리북 접근성 체크용 addon](https://storybook.js.org/addons/@storybook/addon-a11y)을 사용
- Q: 회사에서 사용하는 분석 도구?
- A:
  - 보통은 GA 사용, vercel analytics 제안 받았으나 금액 부담으로 사용하지 않음. (tagmanager에서 셋팅하던 걸 알아서 해주는 이점이 있는 듯)
  - GA 분석에 대한 책임소재가 명확하지 않아서 혼란스럽기도 하다. (개발측에서 관리해야 하는지 마케팅 측에서 관리 해야 하는지?)
  - 마케팅 파트에서 GA를 다루기 어려워 데이터를 다루는 개발자를 따로 필요로 하기도 한다.

## State: A Component's Memory

1. > 1. A **state variable** to retain the data between renders. 2. A **state setter function** to update the variable and trigger React to render the component again.
   - 관습적으로 사용하던 useState를 2depth로 나누어서 보여주는게 인상 깊었다.
2. > Hooks—functions starting with use —can only be called at the top level of your components or your own Hooks.
   - 커스텀훅이 필수적이라곤 생각을 안했는데 (리팩토링의 일종으로 여김) 뭔가 docs에서 커스텀훅을 적극적으로 언급 하는것 같아서 재밌다.
   - toss의 slash라는 라이브러리에서도 커스텀훅을 공통으로 사용할 수 있는 여지를 제공해주어 잘 쓰면 유용할 것 같다.
3. > 1. **Your component renders the first time.** Because you passed `0` to `useState` as the initial value for `index`, it will return `[0, setIndex]`. React remembers `0` is the latest state value. 2. **You update the state.** When a user clicks the button, it calls `setIndex(index + 1)`. `index` is `0`, so it’s `setIndex(1)`. This tells React to remember `index` is `1` now and triggers another render. 3. **Your component’s second render.** React still sees `useState(0)`, but because React *remembers* that you set `index` to `1`, it returns `[1, setIndex]` instead. Instead, to enable their concise syntax, Hooks rely on a stable call order on every render of the same component. This works well in practice because if you follow the rule above (“only call Hooks at the top level”), Hooks will always be called in the same order. Additionally, a linter plugin catches most mistakes. Internally, React holds an array of state pairs for every component. It also maintains the current pair index, which is set to 0 before rendering. Each time you call useState, React gives you the next state pair and increments the index. You can read more about this mechanism in React Hooks: Not Magic, Just Arrays.
   - 딱히 의문을 가지고 있지 않았는데 처음 알게 된 부분. 렌더링 시 배열 기반의 호출 순서로 렌더링한다는 부분이 인상깊었다.
   - 이런 지식의 차이가 레벨 차이를 만드는 것 같다…!
4. > How does React know which state to return?
   - 처음에 제목이 뭐가 문제인지 파악을 못했는데, 읽어보니 전혀 생각치도 못한 부분이어서 인상 깊었다.
   - [React hooks: not magic, just arrays](https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e) 글의 이미지와 함께 보면 이해가 쉽다.
   - 예제 코드를 보며 상수에 해당하는 영역은 코드 내부의 하단부에 위치시키는 것도 깔끔하다는 생각이 들었다.
5. > Unlike props, state is fully private to the component declaring it. The parent component can’t change it.
   - 캡슐화가 생각난다.
   - 영역에 상관없이 잘 짜인 코드의 원리는 비슷하다

## Render and Commit

1. > 1. **Triggering** a render (delivering the guest’s order to the kitchen) 2. **Rendering** the component (preparing the order in the kitchen) 3. **Committing** to the DOM (placing the order on the table)
   - state 상태를 DOM에 반영하는 로직의 핵심
   - 이 단계와 Real dom에 그려지는 painting 단계를 구분하는게 인상 깊었다. (virtual dom)
2. > “Rendering” is React calling your components.
   - 렌더링을 한 마디로 표현하면 컴포넌트 호출
   - 컴포넌트도 결국 함수니까 이 함수를 호출 -> 얘가 jsx 리턴 -> 화면에 보여짐 요 과정 때문에 '렌더링' 이라고 한 단어로 말하는 건가 싶기도
   - 결국 jsx도 js로 만들어진 함수...
3. > This process is recursive: if the updated component returns some other component, React will render _that_ component next, and if that component also returns something, it will render _that_ component next, and so on.
   - 부모 컴포넌트가 렌더링되면 자식 컴포넌트도 렌더링된다는 건 알고 있었는데 그 이유가 재귀적으로 호출되기 때문이라는 건 몰랐다.
4. > React only changes the DOM nodes if there’s a difference between renders. For example, here is a component that re-renders with different props passed from its parent every second. Notice how you can add some text into the `<input>`, updating its value, but the text doesn’t disappear when the component re-renders:
   - controlled component가 잘못 사용하면 제품에 부하를 일으킬 수 있는 원인이라 리마인드 할 수 있어 좋았다.
   - 번거로워도 Form library같은걸 붙여서 값을 처리해야겠다. (react-hook-form을 주로 사용)
   - 최근 react-hook-form과 외부 라이브러리를 함께 사용하는데, reset 함수 관련하여 동작이 제대로 되지 않아 힘들었다. react-hook-form을 사용하면 까다로운 요구사항이어도 해결 가능하긴 하나 docs가 친절하지 않아 가끔 이렇게 애를 먹기도...
   - MUI와 같은 외부라이브러리와 사용하는 경우엔 [controller](https://www.react-hook-form.com/api/usecontroller/controller/)와 함께 사용하는 편
   - Q: 폼 라이브러리를 사용하는 이유가 무엇일까요?
     - 입력 칸이 여러개일 때, 모든 분기와 케이스를 고려해서 치다보면 state의 양이 너무 커지고, 핸들러의 양이 너무 많아짐
     - customHook을 직접 짜는 것보다 공수가 적게 든다.
     - 폼이 다른 폼에 영향을 받는 식이면 복잡도가 올라가는데 폼 라이브러리 사용 시엔 훨씬 편하게 작성 가능하다.
     - 유효성 검증도 훨씬 작성하기가 쉽다. (custom한 validation도 가능) → 이후의 동작을 formState로 관리하는데 이 점이 유용

### 🤷‍♀️ 질문

- Q: 회사에서 성능 최적화 어떻게 챙기는지?
- A: 개인 역량에 달린 느낌... 개발자가 최적화 했는지 여부를 회사에선 신경써주지 못하는 느낌이다. (필수적인 영역이라고 생각을 못하는 듯)
- Q: 테이블 표현 시 보이지 않는 부분 렌더링 되지 않도록 하는 라이브러리?
- A:
  - https://www.npmjs.com/package/react-virtualized
  - https://www.npmjs.com/package/react-window
  - https://tanstack.com/virtual/v3

### 💡 정보

**react-hook-form의 useFieldArray 사용 사례**

- https://tech.inflab.com/202207-rallit-form-refactoring/react-hook-form/#2-2-2-배열-관리---usefieldarray
- https://youtu.be/4MrbfGSFY2A (7분 50초 쯤)

## State as a Snapshot

## Queueing a Series of State Updates

## Updating Objects in State

## Updating Arrays in State
