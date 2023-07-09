# Escape Hatches

- [Referencing Values with Refs](#referencing-values-with-refs)
- [Manipulating the DOM with Refs](#manipulating-the-dom-with-refs)
- [Synchronizing with Effects](#synchronizing-with-effects)
- [You Might Not Need an Effect](#you-might-not-need-an-effect)
- [Lifecycle of Reactive Effects](#lifecycle-of-reactive-effects)
- [Separating Events from Effects](#separating-events-from-effects)

> **Note**
>
> 'Escape Hatches' 챕터는 다음과 같은 내용을 다룹니다.
>
> - 리렌더링 없이 정보를 "기억"하는 방법
> - 리액트가 관리하는 DOM 엘리먼트에 접근하는 방법
> - 컴포넌트를 외부 시스템과 동기화하는 방법
> - 불필요한 Effects를 제거하는 방법
> - Effect의 라이프사이클이 컴포넌트의 라이프사이클과 어떻게 다른지
> - 어떤 값이 Effects를 다시 트리거하지 못하게 막는 방법
> - Effects의 재실행 빈도를 줄이는 방법
> - 컴포넌트 간에 로직을 공유하는 방법

## Escape Hatches

1.  > Effects are an escape hatch from the React paradigm. They let you “step outside” of React and synchronize your components with some external system. If there is no external system involved (for example, if you want to update a component’s state when some props or state change), you shouldn’t need an Effect. Removing unnecessary Effects will make your code easier to follow, faster to run, and less error-prone.

    - 리액트 성능 최적화의 키포인트 중 하나라는 생각이 드네요.

2.  > 외부 시스템이 관여하지 않는 경우(예: 일부 prop이나 state가 변경될 때 컴포넌트의 state를 업데이트하려는 경우)에는 Effect가 필요하지 않습니다.
    - 이렇게 사용한 경우 정말 많았는데 요즘엔 조심하고 있어요. You Migh Not Need an Effect 챕터 가장 기대됩니다.
3.  > - You don’t need Effects to transform data for rendering.
    > - You don’t need Effects to handle user events.

    - 그 동안 좀 복잡해진다 싶으면 해결책으로 useEffect를 남용한 적이 많았었는데 반성하게 되네요..!

4.  > Effects have a different lifecycle from components. Components may mount, update, or unmount. An Effect can only do two things: to start synchronizing something, and later to stop synchronizing it.
    - 클래스형 컴포넌트일 때 라이프사이클 외우던 게 생각나요. Effect의 동작도 라이프사이클로 설명하는 군요.
    - Slash 라이브러리의 [useDidUpdate](https://slash.page/ko/libraries/react/react/src/hooks/usedidupdate.i18n/)
5.  > Move the code reading theme out of your Effect into an Effect Event
    - useEffectEvent라는 훅은 정말 처음 들어봤어요. 그래서 이런 경우에 그냥 의존성 배열에서 빼버렸던 것 같네요.
    - Q. 실험적인 API라 당장 쓰기 어려울 것 같은데 다른 분들도 그냥 의존성 배열에서 제거하셨나요? 아니면 더 좋은 방법이 있을까요?
    - A. 오 이런 훅이 있었군요…. 저는 저런 경우엔 roomId에 대한 커넥션이 진행중이면 return 시키는 식으로 꾸역꾸역 처리했던것 같은데 (+아니면 useRef를 이상하게 엮어서 사용하는 식…) 저렇게 처리하면 훨씬 개선될 것 같은데 experimental_useEffectEvent 이라 하니 조금 무섭긴 하네요 흐흐…
    - A. experimental 은 프로덕션에서 사용하지 않는것이 좋지 않을까요!
6.  > 이 목록에 무엇을 넣을지는 의도적으로 선택하지 않습니다. 이 목록은 당신의 코드를 기술합니다. 의존성 목록을 변경하려면, 코드를 변경하세요.
    - 그동안 완전히 반대로 생각하고 있었네요. 의존성 배열에 props나 state를 넣어서 Effect 내의 코드를 실행시킨다고 생각했어요.

## Referencing Values with Refs

1.  > 컴포넌트가 특정 정보를 ‘기억’하도록 하고 싶지만 해당 정보가 새 렌더링을 촉발하지 않도록 하려는 경우
    - 용도 위주로만 기억해서 useRef가 어떤 친구인지 두루뭉실하게만 알고 있었는데 이번에 제대로 알게 되네요..!
2.  > state와 달리 ref는 읽고 수정할 수 있는 current 프로퍼티를 가진 일반 자바스크립트 객체입니다. (ko.react.dev)
    - 참조형 데이터인 일반 자바스크립트 객체이기 때문에 current 값이 리렌더링과 상관없이 유지되는거군요. 당연한 사실인데, 오늘에서야 퍼뜩 깨달음이 오네요.
3.  > state와 ref를 비교하면 다음과 같습니다: [(비교해놓은 표)](https://react-ko.dev/learn/referencing-values-with-refs#differences-between-refs-and-state)

    - 면접 단골 질문 중 하나?

4.  > You shouldn’t read (or write) the current value during rendering.
    - Q. return (…) 안에서 사용하지 말라는 뜻인거죠..?.?
    - A. 저도 이 렌더링 중에 읽거나 쓰지 말라는 부분이 헷갈리는데 렌더링 중이라는 건 컴포넌트 함수를 호출했다는 거고 아마 return 전 함수 본문에서 바로 쓰지 말라고 얘기하는 것 같아요.
    - A. 저도 렌더링 중이라는 표현이 잘 안와닿았는데 다음 내용들 읽어보니 이수님 말씀하신 내용대로 이해가 됐습니다! (별도의 훅 없이 사용 지양)
    - (스터디 후 추가 🙇) 이 챕터의 다다음 챕터인 Synchronizing with Effects 챕터의 [예제](https://react-ko.dev/learn/synchronizing-with-effects#step-1-declare-an-effect)를 보면 렌더링 중에 ref에 바로 접근해서 에러가 나는 경우가 나와 있어요. 이런 문제에 대한 해결책으로 `useEffect로 감싸 렌더링 계산 밖으로 옮기는 것` 이 소개됩니다.
      >
5.  > React 내부에서 useRef가 이렇게 구현되는 것을 상상할 수 있습니다.
    - useRef가 useState를 기반으로 만들어져 있다니 둘이 완전 별개인줄 알았어요. state를 set 함수를 사용하지 않고 mutable하게 바꾸기 때문에 React가 객체 속성의 변경을 감지하지 못해서 렌더링이 일어나지 않는 거군요!
6.  > ref를 설정자가 없는 일반 state 변수라고 생각하면 됩니다.

    - const [ref, unused] = useState({ current: initialValue }); 와 함께 이렇게 보니까 훨씬 이해가 쉽네요..!

7.  > 또한 ref로 작업할 때 mutation 방지에 대해 걱정할 필요가 없습니다. (ko.react.dev)

    - 링크가 잘못 걸려있네요..! https://ko.react.dev/learn/updating-objects-in-state

8.  > 이에 대한 자세한 내용은 ref로 DOM 조작하기에서 확인할 수 있습니다. (ko.react.dev)

    - 여기도 링크 오류가.. 한글 문서는 오류(=PR찬스)가 많군요. 😌 https://ko.react.dev/learn/manipulating-the-dom-with-refs

9.  > React 내부에서 useRef는 다음과 같이 구현된다고 상상할 수 있습니다:

    - useRef가 useState를 기반으로 만들어져 있다니 둘이 완전 별개인줄 알았어요. state를 set 함수를 사용하지 않고 mutable하게 바꾸기 때문에 React가 객체 속성의 변경을 감지하지 못해서 렌더링이 일어나지 않는 거군요!

10. > ref를 설정자가 없는 일반 state 변수라고 생각하면 됩니다.
    - const [ref, unused] = useState({ current: initialValue }); 와 함께 이렇게 보니까 훨씬 이해가 쉽네요..!

## Manipulating the DOM with Refs

1.  > This is called a ref callback. React will call your ref callback with the DOM node when it’s time to set the ref, and with null when it’s time to clear it.

    - ref 콜백에 대해 알게 된 지 얼마 안 됐는데 DOM 엘리먼트가 마운트되기 전에 너비를 재야하는 경우 유용할 것 같더라구요.
    - (추가) 정확히 기억나지 않지만 약간 차이를 볼 수 있는 예시를 만들어보았습니다.

      ```jsx
      import { useEffect, useRef, useState } from "react";

      export default function Form() {
        const inputRef = useRef(null);
        const [inputWidth, setInputWidth] = useState(0);

        useEffect(() => {
          if (!inputRef.current) {
            return;
          }

          console.log("App inputWidth", inputWidth);
        }, [inputWidth]);

        return (
          <>
            <input ref={(el) => el && setInputWidth(el.clientWidth)} />
            <span> width: {inputWidth}</span>
          </>
        );
      }
      ```

      - 콘솔에 로그가 찍히지 않습니다.
      - width: 옆의 숫자가 0에서 173으로 깜빡이지 않습니다.

      ```jsx
      import { useEffect, useRef, useState } from "react";

      export default function Form() {
        const inputRef = useRef(null);
        const [inputWidth, setInputWidth] = useState(0);

        useEffect(() => {
          setInputWidth(inputRef.current.clientWidth);
        }, []);

        useEffect(() => {
          if (!inputRef.current) {
            return;
          }

          console.log("App2 inputWidth", inputWidth);
        }, [inputWidth]);

        return (
          <>
            <input ref={inputRef} />
            <span> width: {inputWidth}</span>
          </>
        );
      }
      ```

      - 콘솔에 로그가 찍힙니다.
        - App2 inputWidth 0 (x2)
        - App2 inputWidth 173
      - width: 옆의 숫자가 0에서 173으로 깜빡입니다.

2.  > 또 다른 해결책은 ref 어트리뷰트에 함수를 전달하는 것입니다. 이것을 “ref 콜백”이라고 합니다. React는 ref를 설정할 때 DOM 노드와 함께 ref 콜백을 호출하며, ref를 지울 때에는 null을 전달합니다. 이를 통해 자체 배열이나 Map을 유지하고, 인덱스나 특정 ID를 사용하여 어떤 ref에든 접근할 수 있습니다.
    - 이 부분의 작동 원리가 잘 이해가 안되네요. 여러번 읽어봐야 할 것 같습니다.
3.  > const MyInput = forwardRef((props, ref) => {
    > return <input {...props} ref={ref} />;
    > });

        - 매번 prop으로 ref를 넘겨주는 식으로 처리했었는데 이런 방법이 있었군요…!

4.  > MyInput 컴포넌트는 forwardRef를 통해 선언되었습니다. 이것은 props 다음에 선언된 두 번째 ref 인수를 통해 상위의 inputRef를 받을 수 있도록 합니다.
    - 요즘 모노레포에서 사용할 공통 UI 컴포넌트 작업 관련 내용이 파트에서 오가고 있어 종종 코드를 뜯어보는데, 디자인 시스템처럼 광범위하게 사용될 수 있는 컴포넌트를 작성할 때에는 forwardRef로 감싸주는게 필수인 것 같아요.
5.  > On the other hand, high-level components like forms, lists, or page sections usually won’t expose their DOM nodes to avoid accidental dependencies on the DOM structure.
    - 그런데 MUI에서는 보통 root 엘리먼트를 ref에 담아준다고 되어 있었어요. 부득이하게 접근이 필요한 사람들을 위해서겠죠?
6.  > 반면 폼, 목록 혹은 페이지 섹션 등의 고수준 컴포넌트에서는 의도하지 않은 DOM 구조 의존성 문제를 피하고자 일반적으로 DOM 노드를 노출하지 않습니다.
    - low-level component와 high-level component의 비교가 인상깊네용
7.  > In uncommon cases, you may want to restrict the exposed functionality. You can do that with useImperativeHandle
    - 이 훅이 이런 데 사용하는 거였군요. focus때문에 ref 쓰는 곳이 많은데 외부에서 쓰는 건 아니니까 이런 제한을 걸어둔 적은 없거든요. 제한을 꼭 걸어야 하는 경우를 위해서 기억해둬야 겠어요.
    - 오 저는 이 훅을 처음 들어봤는데… 당장은 필요한 경우가 있을까 싶으면서도 복잡한 협업 시에 유용할 것 같네요…!
8.  > 드문 경우지만 노출되는 기능을 제한하고 싶을 수도 있을 것입니다. 그럴 땐 useImperativeHandle을 사용하면 됩니다:
    - 와 이건 정말 처음봐요. 너무 신기하고 유용해보이네요! ref에 참조한 DOM이 포함하고 있는 메소드 중 필요한 것만 남길 수 있군용
9.  > Usually, you will access refs from event handlers.
    - 이 부분이 제가 앞서 올린 질문에 대한 답이지 않으려나…아닌가… (이 질문이었습니다 -> return (…) 안에서 사용하지 말라는 뜻인거죠..?.?)
    - (스터디 후 추가 🙇) 아니었던걸로..
10. > So the time you scroll the list to its last element, the todo has not yet been added. This is why scrolling always “lags behind” by one item.
    - 비슷한 문제를 겪었을 때 상태를 하나 추가해서 useEffect로 해결했던 기억이 나는데 대신 flushSync를 사용하면 되는군요. 근데 동기적으로 업데이트를 강제한다고 하니까 조심해서 써야 하는 함수일 것 같아요.
    - 오오 저도 최근에 비슷한 걸 겪어서 굉장히 이상한(?) 방식으로 처리했던 경험이 있는데… flushSync 사용하면 확실히 코드가 훨씬 깔끔해질 것 같아요
11. > the last todo will already be in the DOM by the time you try to scroll to it:
    - vue에도 비슷한게 있어요 flushSync와 일대일 대응된다고는 할 수 없겠지만… 다음 dom 업데이트가 완료되면 `nextTick` 내 콜백을 실행합니다(또는 await를 붙여서 비동기적으로 처리하거나요!) 프레임워크를 떠나서 state 업데이트가 된 후에 뭔갈 하고싶은 욕구(?)는 동일한가 봅니다ㅎㅎ
    ```javascript
    function nextTick(callback?: () => void): Promise<void>
    ```
12. > 하지만 DOM을 직접 수정하는 시도를 한다면 React가 만들어 내는 변경 사항과 충돌을 발생시킬 위험을 감수해야 합니다.
    - ref를 이용하여 DOM 조작하는 것을 escape hatches 용도로만 사용해야 하는 이유
13. > 그렇다고 전혀 할 수 없다는 건 아니고, 주의가 필요하다는 의미입니다.
    - 이번 내용들 굉장히 하지 못하는건 아닌데 주의해라~ 하는 내용이 많군요 ㅎㅎㅎ 무섭….

## Synchronizing with Effects

1. > Effect를 사용하면 특정 이벤트가 아닌 렌더링 자체로 인해 발생하는 사이드 이펙트를 명시할 수 있습니다.

   - 이펙트가 하는 역할 한 줄 설명
   - 초반부에 useEffect를 쓰지 않아도 되는 부분~ 과 같은 내용에서 확 와닿지 않았는데 여기 사이드 이펙트를 명시한다는 부분이 이름이랑도 연관지어져서 와닿네요..! 그리고 여태까지 남용한 부분들에 대해서 확실히 반성을 하게 됩니다..

2. > If your Effect only adjusts some state based on other state, you might not need an Effect.
   - 무지성 Effect 사용을 반성합니다.. 🫡
3. > In other words, useEffect “delays” a piece of code from running until that render is reflected on the screen.
   - props나 state가 바뀔 때 useEffect를 부르는 게 아니라 오히려 실행을 지연한다는 관정에서 바라봐야 하는군요!
4. > 이 코드가 올바르지 않은 이유는 렌더링 중에 DOM 노드로 무언가를 시도하기 때문입니다. React에서 렌더링은 JSX의 순수한 계산이어야 하며 DOM 수정과 같은 사이드 이펙트를 포함해서는 안됩니다.
   - 오 여기 보니까 렌더링 중에 뭐뭐 하지 말라는 게 Effect처럼 이스케이프하지 않고 함수 본문에서 계산 외 다른 작업을 하지 말라고 하는 뜻 맞는 것 같아요.
5. > 더구나 VideoPlayer가 처음 호출될 때 DOM은 아직 존재하지 않습니다!

   ```javascript
   function VideoPlayer({ src, isPlaying }) {
     const ref = useRef(null);

     if (isPlaying) {
       ref.current.play(); // Calling these while rendering isn't allowed.
     } else {
       ref.current.pause(); // Also, this crashes.
     }

     return <video ref={ref} src={src} loop playsInline />;
   }
   ```

   - 위 예제 코드에서 VideoPlayer 컴포넌트의 if 부분을 if(ref.current) {...} 로 감싸니까 되기는하네요,,,
   - 위 예제 코드의 `// Calling these while rendering isn't allowed.` 주석을 보니까 렌더링 중에 ref 접근하지 말라는 말이 이해되는것 같아요!
   - 이 경우에 사용하기 좋은게 ref callback 인것같기도! (시간 되시면 코드 공유해주시기..!💕)
   - (스터디 후 추가🙇) 한번 해봤는데 아래처럼 하면 되려나요..!

     ```javascript
     function VideoPlayer({ src, isPlaying }) {
       // const ref = useRef(null);

       const handleRefCb = (node) => {
         // if문 두 번 들어가는건 모른 척 해주시기
         if (node) {
           if (isPlaying) {
             node.play();
           } else {
             node.pause();
           }
         }
       };

       return <video ref={handleRefCb} src={src} loop playsInline />;
     }
     ```

6. > 해결책은 사이드 이펙트를 useEffect로 감싸 렌더링 계산 밖으로 옮기는 것입니다.
   - 이것도 비슷한 의미
7. > 동영상 플레이어 제어는 실제로는 훨씬 더 복잡하다는 점에 유의하세요. play() 호출이 실패할 수도 있고,
   - 최근에 외주사 프로젝트에 sentry(에러추적라이브러리)를 달아봤더니 동영상 play() 호출 실패로 하루에 에러를 천개단위로 뱉고 있더라구요 ㅠㅠ 별거 아니지만 넘 공감가는 줄이네요..
8. > You will get a lint error if the dependencies you specified don’t match what React expects based on the code inside your Effect.
   - Q. 다들 exhaustive-deps eslint rule 켜놓고 사용하시나요..? 저는 warning으로 켜놓고 쓰긴 하는데, 가끔 귀찮을 때가 있어서 어떻게 하면 좋을지 고민입니다. 🤔 (의도적으로 특정 상태만을 구독해서 useEffect를 사용할 때라던가..)
9. > 의존성 배열이 없는 경우와 비어 있는 [] 의존성 배열이 있는 경우의 동작은 다릅니다:
   - 의존성 배열을 안 넣고 useEffect를 사용해본 경험이 없어서 두번째 인자가 optional인줄도 몰랐네요…!
   - 의존성 배열 없이 useEffect 사용할 일이 드물어서 그런 것 같아요
10. > 예를 들어, 부모 컴포넌트에서 ref가 전달된 경우 의존성 배열에 이를 지정해야 합니다. 부모 컴포넌트가 항상 동일한 ref를 전달하는지, 아니면 여러 ref 중 하나를 조건부로 전달하는지 알 수 없기 때문에 이 방법이 좋습니다.
    - 이런 경우까지 린터에서 잡아준다는 거겠죠? 사실 린터 쓰면서도 좀 무지성으로 넣으라는 거 다 넣고 문제 없으면 넘어가긴 하는데 ㅎㅎ
    - 린터가 잡아줄까욧? (실험 해봅시다)
11. > Bugs like this are easy to miss without extensive manual testing. To help you spot them quickly, in development React remounts every component once immediately after its initial mount.
    - 이전에 immutable 확인을 위해서도 두 번 마운트 시킨다고 했는데 cleanup 함수가 필요한지 확인을 위해서도 유용하군요.
12. > So there would be only one active subscription at a time. This has the same user-visible behavior as calling addEventListener() once, as in production.
    - 두 번 마운트시키는 동작이 cleanup이 제대로 이루어지는지 확인하기 위한 용도로도 사용되는군요. 여러모로 react 개발시에는 strict mode가 필수적이네용
13. > 개발 중인 두 번째 요청이 귀찮은 경우 가장 좋은 방법은 요청을 중복 제거하고 컴포넌트 간에 응답을 캐시하는 솔루션을 사용하는 것입니다:
    - 개발 환경에서 데이터 호출 두 번 하더라도 그냥 두는데 역시 그러면 안 되겠죠? ㅎㅎ 서비스 하나에 React Query 도입하니까 코드량도 줄고 편하더라구요.
    - Q. 혹시 React Query 쓰시는 분? 어느 정도 깊이 있게 써보셨나요?
14. > Effect 내에 fetch 호출을 작성하는 것은 특히 클라이언트 측에서만 작성된 앱에서 데이터를 페치하는 인기 있는 방법입니다. 그러나 이것은 매우 수동적인 접근 방식이며 상당한 단점이 있습니다.
    - React Query 쓰는 곳 제외하고 저희 코드는 거의 다 이렇게 구현되어 있어요. 처음 배우는 방법이기도 하고
15. > If you use a framework, use its built-in data fetching mechanism. Modern React frameworks have integrated data fetching mechanisms that are efficient and don’t suffer from the above pitfalls.
    - 혹시 Next.js에서 빌트인으로 제공하는 데이터 페칭 방법이 있나요?
    - [찾아보니까 있습니다!](https://nextjs.org/docs/app/building-your-application/data-fetching)
16. > Otherwise, consider using or building a client-side cache. Popular open source solutions include React Query, useSWR, and React Router 6.4+.
    - Next.js의 SSR과 같은 방법은 사용할 수 있는 환경에 한계가 있다 보니, 요즘에는 클라이언트 캐시 구축이 보편적으로 많이 사용되는 것 같아요. React Query, SWR, Apollo/client 등등..
17. This helps you find Effects that need cleanup and exposes bugs like race conditions early.
    - 최근에 이 두 번의 effect 호출로 인하여 데이터 처리 로직에 문제가 생긴 경우가 있었는데, 개발 환경에서도 useEffect를 한번만 실행하는 hook을 만들어 처리했거든요. 그리 좋은 방법이 아니었던 것 같아 반성하게 되네요.

## You Might Not Need an Effect

1. > You don’t need Effects to transform data for rendering.
   - Effect가 불필요한 첫 번째 경우
2. > You don’t need Effects to handle user events.
   - Effect가 불필요한 두 번째 경우
3. > When something can be calculated from the existing props or state, don’t put it in state. Instead, calculate it during rendering.
   - 예전에 이렇게 할 수 있다는 걸 모르고 무조건 state나 ref만 사용해야 하는 줄 알았다가 알게 됐을 때 좀 충격 받았어요 ㅋㅋ
4. > 계산이 비싼지는 어떻게 알 수 있나요?
   - 비싸다는 기준은 무엇인지 궁금했는데 속시원하게 알려주니 좋네요
   - 친절한 deep dive..
5. > If the overall logged time adds up to a significant amount (say, 1ms or more), it might make sense to memoize that calculation.
   - 구체적으로 어느 정도 시간이 넘으면 메모이제이션이 필요한지 알려주어서 좋아요.
6. > Chrome에서는 CPU 스로틀링 옵션을 제공합니다.
   - 이런 옵션이 있다니! 네트워크 옵션만 알고 있었는데 크롬 개발자 도구를 좀 더 알아가야 겠네요.
7. > useMemo는 첫 번째 렌더링을 더 빠르게 만들지 않습니다. 업데이트 시 불필요한 작업을 건너뛰는 데만 도움이 됩니다.
   - useMemo의 기능 핵심 요약!
8. >
   ```javascript
   // Better: Adjust the state while rendering
   // 더 나음: 렌더링 중에 state 조정
   const [prevItems, setPrevItems] = useState(items);
   if (items !== prevItems) {
     setPrevItems(items);
     setSelection(null);
   }
   ```
   - 위에 있는 것보다 읽기 더 불편한 코드라고 생각했는데 성능 면에서는 이게 낫다는거군요🤔
9. > 이 패턴은 Effect보다 효율적이지만, 대부분의 컴포넌트에는 필요하지 않습니다. 어떻게 하든 props나 다른 state들을 바탕으로 state를 조정하면 데이터 흐름을 이해하고 디버깅하기 어려워질 것입니다. 항상 key로 모든 state를 재설정하거나 렌더링 중에 모두 계산할 수 있는지를 확인하세요. 예를 들어, 선택한 item을 저장(및 재설정)하는 대신, 선택한 item의 ID를 저장할 수 있습니다:

   - 1. 데이터에 id 역할을 할 수 있는 값를 만들고, 해당 값의 변경을 이용해 초기화
   - 2. 렌더링 중 state 조정
   - 3. 불필요한 상황에 Effect 사용 … 순으로 better practice라는 거군요. 리마인드 목적으로 어노테이션 달아둡니다.
   - [useId](https://react.dev/reference/react/useId#useid) 라는 훅도 있네요 (다만 논의 내용을 복기하며 레퍼런스 문서를 조금 더 읽어보니 dom 요소의 id 속성에 사용하면 좋을 것 같아요. [list의 key 값으로는 사용하지 말라](https://react.dev/reference/react/useId#caveats)고 하네용)

10. > 일반적으로 React는 같은 컴포넌트가 같은 위치에서 렌더링될 때 state를 유지합니다. userId를 key로 Profile 컴포넌트에 전달하는 것은 곧, userId가 다른 두 Profile 컴포넌트를 state를 공유하지 않는 별개의 컴포넌트들로 취급하도록 React에게 요청하는 것입니다. React는 (userId로 설정한) key가 변경될 때마다 DOM을 다시 생성하고 state를 재설정하며, Profile 컴포넌트 및 모든 자식들의 state를 재설정할 것입니다.
    - key를 활용하는건 이전 챕터에서도 나왔던 내용이지만 이 부분에서 설명을 깔끔하게 잘 해줘서 표시해봤습니다!
11. > When you update a component during rendering, React throws away the returned JSX and immediately retries rendering. To avoid very slow cascading retries, React only lets you update the same component’s state during a render.

    - 지난 번에 ref 예시를 작성하면서 왜 렌더링이 두 번 일어나지 않는지 설명을 할 수 없었는데 렌더링 중 업데이트가 일어나면 마지막 상태에 대한 렌더링만 커밋되는 거였군요!

12. > 어떤 코드가 Effect에 있어야 하는지 이벤트 핸들러에 있어야 하는지 확실하지 않은 경우 이 코드가 실행되어야 하는 이유를 자문해 보세요. 컴포넌트가 사용자에게 표시되었기 때문에 실행되어야 하는 코드에만 Effect를 사용하세요.
    - 이 안티패턴을 사용한 코드가 있는데, 리팩토링 방법을 계속 고민중입니다. 오늘 시도해봐야겠네요..
13. > if (typeof window !== 'undefined') ...
    - 이런 코드를 100vh 문제 때문에 구글링하며 본 적이 있었어요! 그 땐 꼼수인가 싶었는데 공식문서에서도 나오네요😲
    - 100 vh를 모바일 / 웹 환경에 다르게 적용할 때 사용할 수 있는 css 속성이 있다고 합니다. `dvh`
    - https://css-tricks.com/the-large-small-and-dynamic-viewports/
14. > 자식 컴포넌트가 Effect에서 부모 컴포넌트의 state를 업데이트하면, 데이터 흐름을 추적하기가 매우 어려워집니다. 자식과 부모 컴포넌트 모두 동일한 데이터가 필요하므로, 대신 부모 컴포넌트가 해당 데이터를 페치해서 자식에게 전달하도록 하세요:
    - 리액트가 단방향 데이터 흐름으로 디자인된 이유가 이것 같기도 하네요.. 중요한 포인트라는 생각이 듭니다.
15. > 이를 위해 Effect를 사용하는 것이 일반적이지만, React에는 외부 저장소를 구독하기 위해 특별히 제작된 훅이 있습니다. Effect를 삭제하고 useSyncExternalStore호출로 대체하세요:
    - 새로운 hook useSyncExternalStore
    - nextjs에서 라우터의 이벤트를 구독하는 부분이 있는데 이 경우에 `useSyncExternalStore`를 사용할 수 있을지 살펴봐야겠어요
      ```javascript
      useEffect(() => {
        router.events.on("routeChangeStart", handleRouterLoadingStart);
        router.events.on("routeChangeComplete", handleRouterLoadingComplete);
        router.events.on("routeChangeError", handleRouterLoadingComplete);
        return () => {
          router.events.off("routeChangeStart", handleRouterLoadingStart);
          router.events.off("routeChangeComplete", handleRouterLoadingComplete);
          router.events.off("routeChangeError", handleRouterLoadingComplete);
        };
      }, [router]);
      ```
16. > React에는 외부 저장소를 구독하기 위해 특별히 제작된 훅이 있습니다. Effect를 삭제하고 useSyncExternalStore호출로 대체하세요
    - 이 훅도 처음 보는데 [링크](https://react-ko.dev/reference/react/useSyncExternalStore) 들어가서 보니까 이제보니 레퍼런스도 굉장히 상세하네요. 사용법이랑 트러블슈팅이 있어서 유용해보여요!

## Lifecycle of Reactive Effects

1. > Previously, you were thinking from the component’s perspective. When you looked from the component’s perspective, it was tempting to think of Effects as “callbacks” or “lifecycle events” that fire at a specific time like “after a render” or “before unmount”. This way of thinking gets complicated very fast, so it’s best to avoid.
   - 계속 이렇게 이해하고 있었는데 이렇게 바라보면 안 된다니. 그런데 아직 시작/중지 방식으로 바라보는 게 어떤 이점이 있는지 와닿지 않아요 😅
2. > 컴포넌트를 마운트, 업데이트 또는 마운트 해제하는 것은 중요하지 않습니다.
   - useEffect를 componentDidMount 같은 생명주기 함수들의 대체제로 생각하곤 했는데.. 거기에서 빠져나와야겠네요
3. > This might remind you how you don’t think whether a component is mounting or updating when you write the rendering logic that creates JSX. You describe what should be on the screen, and React figures out the rest.
   - 앗 이 문장을 보니까 컴포넌트 라이프사이클에 신경쓰지 말라는 말이 좀 이해가 됩니다.
4. > You already have an Effect that depends on roomId, so you might feel tempted to add the analytics call there:
   - 생각없이 이렇게 한 경우가 많았던 것 같은데 반성합니다.
5. > 반면 일관된 로직을 별도의 Effect로 분리하면 코드가 “더 깔끔해” 보일 수 있지만 유지 관리가 더 어려워집니다. 따라서 코드가 더 깔끔해 보이는지 여부가 아니라 프로세스가 동일한지 또는 분리되어 있는지를 고려해야 합니다.
   - 리팩토링 포인트
6. > Props, state, and other values declared inside the component are reactive because they’re calculated during rendering and participate in the React data flow.
   - 1\) 렌더링 중에 계산되고 2\) React의 데이터 흐름에 참여하는 것을 반응형이라고 하는군요
7. > 하지만 Effect의 관점에서 생각하면 마운트 및 마운트 해제에 대해 전혀 생각할 필요가 없습니다. 중요한 것은 Effect가 동기화를 시작하고 중지하는 작업을 지정한 것입니다.
   - 이번 챕터는 사고방식 자체를 가이드 해주는 목적이네요🤔
8. > Can global or mutable values be dependencies?
   - 의존성이 될 수 없는 값과 그 이유를 명확하게 알려주어서 좋아요.
9. > 버그를 수정하려면 린터의 제안에 따라 Effect의 의존성 요소로 roomId 및 serverUrl을 지정하세요:
   - 어제 토스 Next 챌린지를 봤는데, 토스에서도 이 deps 관련 린트 rule을 error로 설정해두었더라고요. 역시 해당 옵션 자체에 문제가 있는 게 아니라 effect를 오용하고 있는 내 코드가 문제였구나.. 라는 생각을 했습니다.
10. > 의존성을 “선택”할 수는 없습니다. 의존성에는 Effect에서 읽은 모든 반응형 값이 포함되어야 합니다. 린터가 이를 강제합니다. 때때로 이로 인해 무한 루프와 같은 문제가 발생하거나 Effect가 너무 자주 다시 동기화될 수 있습니다. 린터를 억제하여 이러한 문제를 해결하지 마세요!
    - 린터를 억제하여 의존성 관련 문제를 해결하려 하지 말 것!

## Separating Events from Effects

1. > 이와 같은 반응형 값은 리렌더링으로 인해 변경될 수 있습니다. 예를 들어, 사용자가 message를 수정하거나 드롭다운에서 다른 roomId를 선택할 수 있습니다. 이벤트 핸들러와 Effect는 변경 사항에 다르게 반응합니다:
   - 반응형 값의 정의 내지 부연설명
2. > You can think of Effect Events as being very similar to event handlers. The main difference is that event handlers run in response to a user interactions, whereas Effect Events are triggered by you from Effects. Effect Events let you “break the chain” between the reactivity of Effects and code that should not be reactive.
   - 아직 잘 이해가 안 되는데 여러번 읽어봐야겠어요.
3. > 이 비반응형 로직을 Effect에서 추출하려면 useEffectEvent라는 특수 Hook을 사용합니다:

   - 유용해보이는 훅인데 아직 Experimental이라 아쉽습니다.. 🥲
   - 공감합니다!
   - 그쵸.. 만약 useEffectEvent 없이 이 예제에서 나온 문제를 해결하려면 ref를 활용해야할것 같네요!
   - (숙제 제출합니다...( ͡° ͜ʖ ͡°) )

     ```javascript
     function ChatRoom({ roomId, theme }) {
       const themeRef = useRef(theme); // 이래도 되는걸까? 이 부분이 조금 마음에 걸려요..

       useEffect(() => {
         const connection = createConnection(serverUrl, roomId);
         connection.on("connected", () => {
           showNotification("Connected!", themeRef.current);
         });
         connection.connect();
         return () => connection.disconnect();
       }, [roomId]);

       useEffect(() => {
         themeRef.current = theme;
       }, [theme]);

       return <h1>Welcome to the {roomId} room!</h1>;
     }
     ```

4. > Effect Event로 최근 props와 state 읽기
   - experimental이지만 이렇게 정성들여 설명하는 것을 보면 곧 stable로 올라올 것 같기도 하고.. 존버해야겠네요
5. > useEffectEvent가 React의 안정적인 기능이 되면 린터를 절대로 억제하지 않을 것을 추천합니다.
   - 하지만 아직은 어쩔 수 없는 경우가 있을 수 있다는 뜻이군요…
6. > You can think of Effect Events as being very similar to event handlers. The main difference is that event handlers run in response to a user interactions, whereas Effect Events are triggered by you from Effects. Effect Events let you “break the chain” between the reactivity of Effects and code that should not be reactive.
   - 아직 잘 이해가 안 되는데 여러번 읽어봐야겠어요.
   - (추가) ko.react.dev에서는 이렇게 번역되어 있네요. `Effect Event가 이벤트 핸들러와 아주 비슷하다고 생각할 수 있습니다. 이벤트 핸들러는 사용자의 상호작용에 대한 응답으로 실행되는 반면에 Effect Event는 Effect에서 직접 트리거 된다는 것이 주요한 차이점입니다. Effect Event를 사용하면 Effect의 반응성과 반응형이어서는 안 되는 코드 사이의 “연결을 끊어줍니다”.`
7. > By passing url as an argument to your Effect Event, you are saying that visiting a page with a different url constitutes a separate “event” from the user’s perspective.
   - 이벤트라는 단어의 의미를 새삼 다시 생각해볼 수 있었습니다. 어떤 순간에 발생하는 사건이라는 점에서
8. > useEffectEvent를 사용하면 Linter에 “거짓말”을 할 필요가 없으며 코드가 예상대로 작동합니다:

   - Q. 린터 따라가다 보면 아래와 같은 방식으로도 동작을 하긴 하는데 이 방식은 대안일까요? 아니면 꼼수일까요?
   - (추가) 밑에 글 읽다보니까 한 번만 Effect가 실행되길 원하는 건데 이 방법을 따랐을 때 canMove가 바뀔 때마다 실행되서 바라는 결과가 아닌 것 같습니다.

     ```javascript
     const handleMove = useCallback(
       (e) => {
         if (canMove) {
           setPosition({ x: e.clientX, y: e.clientY });
         }
       },
       [canMove]
     );

     useEffect(() => {
       window.addEventListener("pointermove", handleMove);
       return () => window.removeEventListener("pointermove", handleMove);
     }, [handleMove]);
     ```

9. > In the above sandbox, you didn’t want the Effect’s code to be reactive with regards to canMove. That’s why it made sense to extract an Effect Event.
   - Q. canMove 상태와 상관없이 마운트한 시점에만 Effect가 한 번 실행되기는 바란다는 걸로 해석했는데 맞을까요?
   - 그렇다고 생각해요! `위의 샌드박스에서는 Effect의 코드가 canMove에 반응하길 원하지 않았습니다. 그러므로 Effect Event로 추출하는 것이 합리적이었습니다.` -> ko.react.dev에서는 이렇게 해석되어 있는데 좀 더 수월하게 읽히는것 같아요
10. > - Only call them from inside Effects.
    > - Never pass them to other components or Hooks.

    - Effect Event의 제약 사항
