# Quick Start

- [Tutorial: Tic-Tac-Toe](#tutorial-tic-tac-toe)
- [Thinking in React](#thinking-in-react)

<br />

> **Note**
>
> 'Quick Start' 챕터는 다음과 같은 내용을 다룹니다.
>
> - 컴포넌트를 만들고 중첩하는 방법
> - 마크업과 스타일을 추가하는 방법
> - 자바스크립트 데이터를 표시하는 방법
> - 조건에 따라 또는 리스트를 렌더링하는 방법
> - 이벤트에 응답하고 화면을 업데이트하는 방법
> - 컴포넌트 간에 데이터를 공유하는 방법

<br />

## Quick Start
  1. > React components are JavaScript functions that return markup:
        - 컴포넌트가 자바스크립트 함수라는 점이 핵심인 문장으로, 함수라는걸 상기시키는 문장!   
        -(https://www.youtube.com/watch?v=MSq_DCRxOxw&t=141s) SOLID 관련영상 컴포넌트 정리시 참고하기 좋은 영상으로 공유해주심💙
       
  2. > JSX lets you put markup into JavaScript. Curly braces let you “escape back” into JavaScript so that you can embed some variable from your code and display it to the user.
     > For example, this will display user.name:
        - JSX를 사용하는건 결국 js확장파일, 결국 리액트에서 js로 바꿔는주는것      

### 🤷‍♀️ 토의 
- Q: '? {isLoggedIn && <AdminPanel />}' 처럼 조건부 구문이 다른 프레임워크에서는 어떤형식으로 사용 되는가?
- A: (Vue) 에서는 v-if, v-else-if 이런식으로 사용
- Q: 학습방법은 어떤식인가?
- A: 대부분 따라치는것을 우선으로 하고 그후에 이론을 확인하는 방식, 이론먼저 읽기엔 부담스러운면이 있음
      
<br /> 
<br />

## Tutorial: Tic-Tac-Toe
  1. > In React, a component is a piece of reusable code that represents a part of a user interface. Components are used to render, manage, and update the UI elements in your application.
        - 자바스크립트에서 UI처럼 재사용한것이 혁신적  
        - 컴포넌트란 무엇인가에 대해 생각해 본 적이 없는데, 간단명료한 정리라서 인상 깊음  
     
  2. > In React, it’s conventional to use onSomething names for props which represent events and handleSomething for the function definitions which handle those events.
        - 스터디 1주차에 각기 달라 컨벤션을 맞추기 어렵다고 언급했는데, 공식문서에 이런 부분(on, handle)을 정해줘서 이 기준을 따르면 좋겠다고 생각
        - 다른 사람과 협업하여 함수명 짤 때 매번 on과 handle이 섞이게 되어서 둘 중 하나로 통일해야 하나 고민했었는데, 앞으로는 이걸 기준으로 두고 작업해야겠다 생각
      
  3. > Immutability makes it very cheap for components to compare whether their data has changed or not
     > avoid redundant state. Simplifying what you store in state reduces bugs and makes your code easier to understand.
        - 불변성의 장점 2가지로 최적화 와 더 쉬운 코드의 이해가 있음
        - 불변성을 지키면서 작업해야 한다고 인식은 하고 있었는데, 그 이유 중 하나가 최적화였단 걸 알게됨
       
  4. > JavaScript supports closures which means an inner function (e.g. handleClick) has access to variables and functions defined in a outer function (e.g. Board).
     > The handleClick function can read the squares state and call the setSquares method because they are both defined inside of the Board function. 
        - 클로저를 지원하므로 내부함수가 외부 함수에 정의된 변수 및 함수에 엑세스할 수 있음
        - 클로저를 명확히 설명해줘서 좋음 
      
  5. > You don’t need useState for this—you already have enough information to calculate it during rendering:
        - 개발을 진행하다 보면 useState를 남발하는 경우 가 발생하는데, 남발없이 하는방법을 고려하는것이 최적화에 좋다고 생각
        
  6. > There’s no reason for you to store both of these in state. In fact, always try to avoid redundant state.       
        - 상태의 중복을 피하는게 간단한 최적화 방법이라고 생각
<br /> 
<br />

## Thinking in React
  1. > One such technique is the single responsibility principle, that is, a component should ideally only do one thing.
        - 급하게 작업시 한 컴포넌트에 여러가지를 몰아 넣느라 이런 원칙을 깨는 경우가 많다고 느낌, 리팩토링 단계에서 이를 개선함

  2. > It’s often easier to build the static version first and add interactivity later. Building a static version requires a lot of typing and no thinking, 
       but adding interactivity requires a lot of thinking and not a lot of typing.
        - UI를 만들고 하는게 통상적인 순서구나..  
  
  3. > If your JSON is well-structured, you’ll often find that it naturally maps to the component structure of your UI. 
     > That’s because UI and data models often have the same information architecture—that is, the same shape. Separate your UI into components, 
     > where each component matches one piece of your data model.
        - api가 화면에 최적화 되서 전달 받지 않아서 컴포넌트 구조와 자연스럽게 매핑이 되나? 라는 생각이 듬

  4. > - Does it remain unchanged over time? If so, it isn’t state.
     > - Is it passed in from a parent via props? If so, it isn’t state.
     > - Can you compute it based on existing state or props in your component? If so, it definitely isn’t state!
        - 리액트에서 상태라는건 중요한 개념 이걸 잘 구분한다면 최소한의 최적화된 코드를 맞출 수 있다고 생각

### 🤷‍♀️ 토의 
- Q: (1번 관련) 어떤식으로 코드를 작성 하는가?
- A: 대부분 코드 작성 후에 쪼개는편, 작게 만든후 조립하는 방식이 오히려 어렵게 느껴질때가 있음
- A: > You can either build “top down” by starting with building the components higher up in the hierarchy (like FilterableProductTable) 
     > or “bottom up” by working from components lower down (like ProductRow). In simpler examples, it’s usually easier to go top-down, 
     > and on larger projects, it’s easier to go bottom-up.
     > 이런 방법도 존재하고, 코드 작성을 해보니 하향식이 더쉽다고 느껴짐
- Q: (2번 관련) 다른분들은 어떤 순서로 진행 되는가?
- A: Api가 없는상태에서 디자인하고 벡엔드 개발 완료 후 기능 이어 붙임
- Q: (3번 관련) 백엔드와 협업하여 api와 화면을 맞추는 편인가? 아니면 그냥 주는대로 하는 편인가?
- A: 필요한값만 주면 그거에 맞춰서 가공하는편이다. 
- A: 백엔드 개발자가 화면을 얼나마 고려해서 주느냐에 따라서 다르다. 프로젝트 하면서 백엔드 개발자와 많은 협업이 필요하다.
<br />      
💡 발표자님 Tip. 
  - 공식문서 중간중간 모르는 내용이 있더라고 뒤쪽에서 설명해주는 경우가 많으니 넘어가는것도 좋다!
