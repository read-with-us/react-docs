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
  - [🤷‍♀️ 질문](#️-질문-2)
  - [💡 정보](#-정보-1)
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

1.  > 사진이나 동영상 프레임과 달리 반환하는 UI “스냅샷”은 대화형입니다.

    - 역시 사용자와의 인터랙션을 책임지는 파트라는 게 프론트엔드의 묘미라고 생각
    - 사용자와 가장 맞닿아 있는 직무
    - 유저에 대한 신뢰를 잃어가고 있어요. ㅋㅋ 예상치 못한 동작과 입력들

2.  > React가 컴포넌트를 다시 렌더링할 때,
    >
    > 1. React가 함수를 다시 호출합니다.
    > 2. 함수가 새로운 JSX 스냅샷을 반환합니다.
    > 3. 그러면 React가 반환한 스냅샷과 일치하도록 화면을 업데이트합니다.

    - 직관적으로는 이해가 되지만, 정확하게 무슨 프로세스인지 이해가 되지는 않네요. 여러번 읽으며 생각해봐야겠어요.

3.  > 컴포넌트의 메모리로써 state는 함수가 반환된 후 사라지는 일반 변수와 다릅니다. state는 실제로 함수 외부에 마치 선반에 있는 것처럼 React 자체에 "존재”합니다.
    >
    > <div align="center"><img src="https://ko.react.dev/images/docs/illustrations/i_state-snapshot1.png" alt="state snapshot 1" height="350" hspace="20" /><img src="https://ko.react.dev/images/docs/illustrations/i_state-snapshot2.png" alt="state snapshot 2" height="350" hspace="20" /><img  src="https://ko.react.dev/images/docs/illustrations/i_state-snapshot3.png" alt="state snapshot 3" height="350" hspace="20" /></div>

    - 상태를 컴포넌트 내에 정의하다 보니까 컴포넌트에 종속된다고 생각했는데 React가 가지고 있다는 것이 더 말이 되네요.
    - 그림이 없었으면 React가 자기 선반에 상태를 갖고 있다는 걸 이해하기 어려웠을 것 같다.
    - 다른 공식 문서는 이렇게 일러스트까지 만들면서 공들이는 걸 못 본 것 같다.
    - 문서가 아니라 제품처럼 생각하고 만든 것 같다.
    - React의 입지를 유지하기 위한 노력의 일환이 아닐까?

4.  > 코드에서 state 변수를 해당 값으로 대입하여 이를 시각화할 수도 있습니다.
    >
    > ```jsx
    > <button
    >   onClick={() => {
    >     setNumber(0 + 1);
    >     setNumber(0 + 1);
    >     setNumber(0 + 1);
    >   }}
    > >
    >   +3
    > </button>
    > ```

    - 변수에 값을 대입하는 것이 이해하는 데 큰 도움이 됩니다. 시각화의 강력한 힘을 느꼈어요!
    - 저도 이 부분 보면서 아 이거구나! 하고 감탄했어요.

## Queueing a Series of State Updates

1. > React는 state 업데이트를 하기 전에 이벤트 핸들러의 모든 코드가 실행될 때까지 기다립니다.

   - setState 호출과 렌더링 간의 관계를 잘 모르다보니 여러 setState 함수를 호출하면 뭔가 괜찮나 하는 찜찜함이 있었어요. 그런데 오늘부로 걱정없이 사용해도 될 것 같습니다.

2. > 리렌더링은 모든 setNumber() 호출이 완료된 이후에만 일어납니다.
   > 이는 음식점에서 주문받는 웨이터를 생각해 볼 수 있습니다. 웨이터는 첫 번째 요리를 말하자마자 주방으로 달려가지 않습니다! 대신 주문이 끝날 때까지 기다렸다가 주문을 변경하고, 심지어 테이블에 있는 다른 사람의 주문도 받습니다.
   >
   > <div align="center"><img src="https://ko.react.dev/images/docs/illustrations/i_react-batching.png" alt="react batching" height="350" /></div>

   - 깔끔한 비유라 이해가 잘 되네요.
   - 기억에 남겨두고 싶은 비유다.
   - 마지막으로 가져가는 건 주문이 모두 끝난 뒤라는 걸 한 장면으로 잘 설명했다.

3. > 흔한 사례는 아니지만, 다음 렌더링 전에 동일한 state 변수를 여러 번 업데이트 하고 싶다면 setNumber(number + 1) 와 같은 다음 state 값을 전달하는 대신, setNumber(n => n + 1) 와 같이 이전 큐의 state를 기반으로 다음 state를 계산하는 함수를 전달할 수 있습니다.

   - 공식 문서는 흔한 사례가 아니라고 하지만, 저는 이걸 엄청 상습적으로 사용하고 있어서 반성하게 되네요. 변수 관리 시 반드시 state로 관리해야하는지, 아니면 다른 방법이 있는지 고민해보는 습관을 들여야겠습니다.
   - 비즈니스 로직이 초기 기획과 계속 달라지고 세부 사항이 붙으면서 처음 설계보다 크고 복잡해지면 변수를 하나 하나 고려하기 어려워 일단 state에 때려박기도 했다.

4. > 여기서 n => n + 1 은 업데이터 함수(updater function)라고 부릅니다. 이를 state 설정자 함수에 전달 할 때,
   >
   > 1. React는 이벤트 핸들러의 다른 코드가 모두 실행된 후에 이 함수가 처리되도록 큐에 넣습니다.
   > 2. 다음 렌더링 중에 React는 큐를 순회하여 최종 업데이트된 state를 제공합니다.

   - updater function는 그냥 이전 값을 가져와서 계산하려고 할 때만 사용했는데 렌더링과 관련이 깊었네요. 함수가 그냥 실행된다고 생각했지 큐에 넣는 동작이 비하인드에서 벌어지고 있다는 걸 몰랐고 위의 예제처럼 같은 setState를 여러 번 호출해본 적이 없어서 예상 밖의 결과에 당황했습니다.

5. > 1. setNumber(number + 5): number 는 0 이므로 setNumber(0 + 5)입니다. React는 ”5로 바꾸기” 를 큐에 추가합니다.
   > 2. setNumber(n => n + 1): n => n + 1 는 업데이터 함수입니다. React는 이 함수를 큐에 추가합니다.
   > 3. setNumber(42): React는 ”42로 바꾸기” 를 큐에 추가합니다.

   - 버그가 발생했는데 원인를 파악하기 힘들 때, 이런 식으로 해당 코드가 수행하는 과정을 자연어로 바꾸어 적어보는 것도 좋은 방법인 것 같아요.
   - [Rubber duck debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging)이 떠올랐다.
   - 알 수 없는 오류를 다른 개발자 분에게 설명하려고 작성하다가 갑자기 버그의 원인을 깨닫게 되는 경우도 있다.

6. > 업데이터 함수는 순수해야 하며 결과만 반환 해야 합니다. 업데이터 함수 내부에서 state를 변경하거나 다른 사이드 이팩트를 실행하려고 하지 마세요.

   - 업데이터 함수 내에서 다른걸 하려고 했던 제 자신을 반성합니다..

7. > 명명 규칙
   > 업데이터 함수 인수의 이름은 해당 state 변수의 첫 글자로 지정하는 것이 일반적입니다.
   >
   > ```
   > setEnabled(e => !e);
   > setLastName(ln => ln.reverse());
   > setFriendCount(fc => fc * 2);
   > ```
   >
   > 좀 더 자세한 코드를 선호하는 경우 setEnabled(enabled => !enabled)와 같이 전체 state 변수 이름을 반복하거나, setEnabled(prevEnabled => !prevEnabled)와 같은 접두사를 사용하는 것이 널리 사용되는 규칙입니다.

   - 이런 컨벤션이 있는 줄 처음 알았네요.. 지금까지 setData(prev => !prev) 같은 식으로 많이 사용했는데 좀 더 시멘틱하게 작성할 수 있도록 노력해야겠습니다~
   - 읽기 좋은 코드는 이런 디테일이 모여서 결정된다.
   - prev와 같은 표현을 사용하는 코드를 보고 배워서 그렇게 써왔는데 좋은 컨벤션을 배워갑니다! 함수형 프로그래밍의 컨벤션일 것 같아요.

8. > Challenge 1 of 2: 요청 카운터를 고쳐보세요.

   - 이 챌린지 재밌어보이네요! 스터디 끝나고 혼자 시도해봐야겠습니당

## Updating Objects in State

1. > 하지만 리액트 state의 객체들이 기술적으로 변경 가능할지라도, 숫자, 불리언, 문자열과 같이 불변성을 가진 것처럼 다루어야 합니다. 객체를 변경하는 대신 교체해야 합니다.

   - 중요한 포인트네요.
   - 자바스크립트처럼 참조에 대해 불변성을 유지해주지 않는 경우는 주의해야겠어요.

2. > 다시 말하면, state에 저장한 자바스크립트 객체는 어떤 것이라도 읽기 전용인 것처럼 다루어야 합니다.

   - 읽기전용이라고 하니까 와닿아서 좋아요.

3. > ```jsx
   > <div
   >   onPointerMove={(e) => {
   >     position.x = e.clientX;
   >     position.y = e.clientY;
   >   }}
   > />
   > ```

   - [이런 이벤트](https://developer.mozilla.org/en-US/docs/Web/API/Element/pointermove_event)가 있군요..! 지난 시간에 했던 이야기에 이어, 이런 html기본 이벤트 핸들러를 사용하면 재밌는 걸 많이 만들 수 있을 것 같아요

4. > 하지만 이 코드는 방금 생성한 새로운 객체를 변경하기 때문에 적절합니다.
   >
   > ```js
   > const nextPosition = {};
   > nextPosition.x = e.clientX;
   > nextPosition.y = e.clientY;
   > setPosition(nextPosition);
   > ```

   - setState 안에 들어갈 로직이 조금 복잡하면 가독성을 위해서 위로 뺍니다. 공식 문서가 권장하는 방식이라니 좋네요!

5. > 이것은 “지역 변경 local mutation” 이라고 합니다. 렌더링하는 동안 지역 변경을 할 수도 있으며, 이는 아주 편리합니다!

   - 이미 이런 방식을 많이 사용하고 있었는데 지역 변경 local mutation이라는 명칭이 있었네요. 공식이 허락한 방식이라는 생각에 마음이 편해졌습니다
   - 에디터를 다룰 때 예시 코드와 같은 방식으로 작성했다. 기존 코드와 일관성을 맞추기 위해 따라했는데 왜 문제가 없는지 이해하게 되니까 불안감이 사라졌다.

6. > Using a single event handler for multiple fields
   >
   > 아래에는 이전 예제와 같지만, 세 개의 다른 이벤트 핸들러 대신 하나의 이벤트 핸들러를 사용하는 예제가 있습니다.
   >
   > ```js
   > function handleChange(e) {
   >   setPerson({
   >     ...person,
   >     [e.target.name]: e.target.value,
   >   });
   > }
   > ```

   - react-hook-form에서 name을 항상 작성하도록 하는데 그게 하나의 이벤트 핸들러로 제어하기 위해서였군요.

7. > obj1 객체는 obj2 “안”에 없습니다. obj3 또한 obj1을 “가리킬” 수 있기 때문입니다.

   - 영어 문서에 가보니 역시 point라고 되어 있네요. 포인터 배울 때 머리를 싸맸던 기억이 새록새록 납니다.

8. > 프로퍼티를 통해 서로를 “가리키는” 각각의 객체들입니다.

   - 중첩된게 아니라 서로를 가리키는 것이다 라는 새로운 개념을 얻어가네요!

9. > Immer는 편리하고, 변경 구문을 사용할 수 있게 해주며 복사본 생성을 도와주는 인기 있는 라이브러리입니다. Immer를 사용하면 작성한 코드는 “법칙을 깨고” 객체를 변경하는 것처럼 보일 수 있습니다.

   - 이번에도 immer에 대한 이야기가 나왔는데, 예시 코드와 같은 상황에서라면 꽤 유용해보이네요😲 중첩 객체를 다루는 코드를 useImmer라는 일종의 추상화된 hook을 이용해 관리할 수 있어 코드가 깔끔해지는 것 같습니다

10. > Immer가 제공하는 draft는 Proxy라고 하는 아주 특별한 객체 타입으로, 당신이 하는 일을 “기록” 합니다.

    - react-hook-form에서 formState라는 객체를 제공하는데 이것도 Proxy를 쓴다고 알고 있어요. [design and philosophy 참고](https://react-hook-form.com/get-started#designandphilosophy)
    - 자바스크립트도 공부할게 많음을 깨닫습니다.

11. > Why is mutating state not recommended in React?

    - 딥 다이브 내용을 통해 버그 방지 외에도 불변성을 권장하는 이유를 더 알게 되어 좋았습니다.

12. > 보편적인 리액트 최적화 전략

    - 영어 공식 문서에서는 [memo 페이지](https://ko.react.dev/reference/react/memo)로 링크를 걸어뒀던데 여기서는 아직 페이지가 없나봐요!
    - 링크 깨진 거 PR 올려보면 좋겠네요 ㅎㅎ

13. > Challenge 1 of 3: 잘못된 state 업데이트 고치기
    > Challenge 2 of 3: 변경 사항을 찾아 고치세요

    - 세 번째 챌린지는 immer를 사용하다보니 익숙하지 못해서 풀지 못했는데 나머지 챌린지들 재미있어요.

### 🤷‍♀️ 질문

- Q: 오픈 소스 코드를 뜯어 본 적이 있는지?
- A: 타입스크립트 지원을 안 하는 라이브러리 내부 동작을 확인하고 코드를 작성해본 적이 있다. 정말 이거 없으면 일이 안 된다 하는 경우에 뜯어 보게 된다.
- A: lodash의 debounce나 throttle을 사용할 때 CTO님이 어떻게 만들었는지 알고 쓰는 것이 좋다고 해서 코드를 찾아 본 적이 있다.
- A: 토스의 slash는 react에서 쓸 수 있는 다양한 유틸 컴포넌트 모아둔 느낌이라 필요할 때 조금씩 까서 본다. 국내에서도 이렇게 오픈소스를 공개하고 메인테이너로 참여하는 사람들이 있다니!

### 💡 정보

**[ko.javascript.info](https://ko.javascript.info/)**

- React도 추천하는 자바스크립트 학습 자료
- 번역 프로젝트를 추진한 이보라님의 [소개 글](https://violetboralee.medium.com/%EB%AA%A8%EB%8D%98-javascript-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-4338630fef35)

## Updating Arrays in State

1. > 변경하지 않고 배열 업데이트하기

   - 배열을 state로 가질 때 일어날만한 상황에 대한 예제들이 아래에 잘 나와있어서 많이 참고하게 될 것 같아요!
   - 처음 자바스크립트를 배울 때 이런 케이스를 하나씩 맞닥뜨릴 때마다 구글링하며 풀었던 기억이 나네요.

2. > 다음은 일반적인 배열 연산에 대한 참조 표입니다. React state 내에서 배열을 다룰 땐, 왼쪽 열에 있는 함수들의 사용을 피하는 대신, 오른쪽 열에 있는 함수들을 선호해야 합니다.
   > | | 비선호 (배열을 변경) | 선호 (새 배열을 반환) |
   > | :--: | :-----------------------: | :--------------------------------------------------------------------------------------------------------------------: |
   > | 추가 | push, unshift | concat, [...arr] 전개 연산자 ([예시](https://ko.react.dev/learn/updating-arrays-in-state#adding-to-an-array)) |
   > | 제거 | pop, shift, splice | filter, slice ([예시](https://ko.react.dev/learn/updating-arrays-in-state#removing-from-an-array)) |
   > | 교체 | splice, arr[i] = ... 할당 | map ([예시](https://ko.react.dev/learn/updating-arrays-in-state#replacing-items-in-an-array)) |
   > | 정렬 | reverse, sort | 배열을 복사한 이후 처리 ([예시](https://ko.react.dev/learn/updating-arrays-in-state#making-other-changes-to-an-array)) |

   - 한눈에 볼 수 있게 표로 정리되어 있어서 좋아요! 그러고보니 UI 작업할 때는 오른쪽만 거의 사용했네요.

3. > 또는 두 열의 함수를 모두 사용할 수 있도록 하는 Immer를 사용할 수 있습니다.

   - 공식 문서가 엄청 적극적으로 Immer를 영업하는군요.. Immer 메인테이너가 궁금해져서 찾아봤는데 메타 근무하는 개발자시네요 [Github 링크](https://github.com/mweststrate)
   - 다 이유가 있었군요. 😏 상태에 복잡한 객체를 넣으면 유용할 것 같은데 또 객체가 너무 복잡해지면 안 되는 거 아닌가 하는 생각이 들어요.

4. > 안타깝지만, slice와 splice 함수는 이름이 비슷하지만 몹시 다릅니다.
   >
   > - slice를 사용하면 배열 또는 그 일부를 복사할 수 있습니다.
   > - splice는 배열을 변경합니다. (항목을 추가하거나 제거합니다.)
   >
   > React에서는, state의 객체나 배열을 변경하지 않는 게 좋기 때문에 slice (p가 없습니다!)를 훨씬 더 자주 사용하게 될 것입니다.

   - 이 두개를 저도 종종 헷갈려서 리마인드를 위한 기록차 어노테이션 달아둡니다

5. 중첩된 state를 업데이트할 때, 업데이트하려는 지점부터 최상위 레벨까지의 복사본을 만들어야 합니다.

   - 예전에는 이 작업을 간단하게 하기 위해 lodash의 deepClone같은 메소드를 사용했는데, 요즘에는 거의 지난 시간에 소개드린 structuredClone 메소드로 대체하고 있습니다~

6. map을 사용하면 이전 항목의 변경 없이 업데이트된 버전으로 대체할 수 있습니다.

   - 굳이 깊은 복사까지 고려하지 않아도 손쉽게+안전하게 객체의 불변셩을 유지하며 state를 변경할 수 있는 방법이군요! 관습적으로 이렇게 사용하고 있기는 했는데, 정확한 이유를 알게되어 좋습니당
