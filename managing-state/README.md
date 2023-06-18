# Managing State

- [Managing State](#managing-state-1)
   - [🤷‍♀️ 질문](#️-질문)
- [Reacting to Input with State](#reacting-to-input-with-state)
  - [🤷‍♀️ 질문](#️-질문)
- [Choosing the State Structure](#choosing-the-state-structure)
- [Sharing State Between Components](#sharing-state-between-components)

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
```
import Heading from './Heading.js';
import Section from './Section.js';

export default function Page() {
  return (
    <Section>
      <Heading>Title</Heading>
      <Section>
        <Heading>Heading</Heading>
        <Heading>Heading</Heading>
        <Heading>Heading</Heading>
        <Section>
          <Heading>Sub-heading</Heading>
          <Heading>Sub-heading</Heading>
          <Heading>Sub-heading</Heading>
          <Section>
            <Heading>Sub-sub-heading</Heading>
            <Heading>Sub-sub-heading</Heading>
            <Heading>Sub-sub-heading</Heading>
          </Section>
        </Section>
      </Section>
    </Section>
  );
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
    -  챗지피티 답변: https://chat.openai.com/share/3307058a-dc9a-4e21-894f-0fdf6cb132b7
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
```
     import * as React from 'react';
import './style.css';

export default function App() {
  const [color, setColor] = React.useState('red');

  return (
    <div>
      <button
        onClick={() => {
          setColor('blue');
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
items = [{ id: 0, title: 'pretzels'}, ...]
selectedItem = {id: 0, title: 'pretzels'}
그러나 변경 후 다음과 같습니다.
items = [{ id: 0, title: 'pretzels'}, ...]
selectedId = 0
   - 이전 코드처럼 선언된 상태 꽤 자주 본 것 같고 자주 그렇게 썼던 것 같네요. 싱크는 어떻게 맞췄던 거지 😅
4. >  Immer
   - immer가 굉장히 적극적으로 언급되어 재밌어요 ㅋㅋㅋㅋ 처음 나왔을 땐 분명 저희 모두 필요성을 느끼지 못했는데.. 적극적으로 영업하는 리액트 독스..
## Sharing State Between Components
1. > State 끌어올리기 (lifting state up)
   - 많이 사용해왔는데 뭐라고 부르는지는 처음 알았네요 ㅎㅎㅎㅎ
2. > 각 state가 어디에 “생존”해야 할지 고민하는 동안 state를 아래로 이동하거나 다시 올리는 것은 흔히 있는 일입니다. 이건 모두 과정의 일부입니다!
   - 작업하면서 state를 얘가 갖고 있어야하는지 아니면 위로 올려서 prop으로 받을지 이리저리 옮기던 지난날 저의 모습이… 앞에 나왔던 state 흐름도를 그려가며 작업을 시작한다면 좀 덜 헤매지않을까 싶습니다
