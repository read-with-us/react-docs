# Installation

- [Start a New React Project](#start-a-new-react-project)
- [Add React to an Existing Project](#add-react-to-an-existing-project)
- [Editor Setup](#editor-setup)
- [React Developer Tools](#react-developer-tools)

<br />

> **Note**
>
> 'Installation' 챕터는 다음과 같은 내용을 다룹니다.
>
> - React를 새롭게 시작하거나 점진적으로 도입하는 방법

<br />

## Start a New React Project

- 리액트 프레임워크 중에 Next 부터 소개되는 것이 SSR이 대세구나 하는 느낌이 들었다.
- npm trends에서도 Next가 압도적인 선택을 받고 있음. (https://npmtrends.com/gatsby-vs-next-vs-remix)
- 라우팅, 데이터 호출, 번들링처럼 손이 많이 가는 설정들을 프레임웍을 사용함으로써 간단하게 구현할 수 있을 듯.
  > As your JavaScript bundle grows with every new feature, you might have to figure out how to split code for every route individually. As your data fetching needs get more complex, you are likely to encounter server-client network waterfalls that make your app feel very slow. As your audience includes more users with poor network conditions and low-end devices, you might need to generate HTML from your components to display content early—either on the server, or during the build time. Changing your setup to run some of your code on the server or during the build can be very tricky. (출처: https://react.dev/learn/start-a-new-react-project#can-i-use-react-without-a-framework)
- 아래 본문은 왜 리액트 프레임웍을 사용해야 하는지? 라는 질무에 대한 모범 답안이라고 생각.
  > React frameworks on this page solve problems like these by default, with no extra work from your side. They let you start very lean and then scale your app with your needs. Each React framework has a community, so finding answers to questions and upgrading tooling is easier. Frameworks also give structure to your code, helping you and others retain context and skills between different projects. Conversely, with a custom setup it’s easier to get stuck on unsupported dependency versions, and you’ll essentially end up creating your own framework—albeit one with no community or upgrade path (and if it’s anything like the ones we’ve made in the past, more haphazardly designed). (출처: https://react.dev/learn/start-a-new-react-project#can-i-use-react-without-a-framework)
- 특히 Next 팀과 긴밀하게 개발하는 것 같고 아래 본문을 봤을 때, 점차 풀스택으로 가는 것이 리액트 팀의 비전으로 보임.
  > Next.js’s App Router is a redesign of the Next.js APIs aiming to fulfill the React team’s full-stack architecture vision. It lets you fetch data in asynchronous components that run on the server or even during the build. (출처: https://react.dev/learn/start-a-new-react-project#nextjs-app-router)
- 서버에서 구동하는 컴포넌트를 간단히 구현하게 해줄 새로운 기능
  > For example, you can write a server-only React component as an async function that reads from a database or from a file. Then you can pass data down from it to your interactive components: (출처: https://react.dev/learn/start-a-new-react-project#which-features-make-up-the-react-teams-full-stack-architecture-vision)

### 🗣️ 이 챕터 관련해서 나눈 이야기

- Q. 다른 분들은 실무할 때, 리액트의 새로운 기능이나 관련 프레임웍 사용 현황이 어떤지?
- A. 파트장 성향이 안정화된(stable) 새로운 기능이 있으면 버전업하는 것이 좋다는 성향이라 새 프로젝트 시작하면 새로운 기능을 적극 도입하는 분위기. 데이터 통신량이 많은 백오피스 프로젝트에서 리액트 서버 컴포넌트를 사용해본 경험이 좋았다.
- A. Gatsby 사용 중인데 Next가 대세인 것 같아 옮길까 고민임.
- Q. Gatsby는 정적 페이지로 된 프로젝트에 특화된 것으로 알고 있는데 맞는지?
- A. 무조건 정적 페이지 생성 기능만 있는건 아니지만 ssr은 지원되지 않는지라 고민 중이다.

- Remix라는 이름을 처음 들은 것이 2021년인데 이번에 공식 문서에 관련 프레임웍으로 두번째에 이름을 올려서 놀랐음. 하지만 아직은 실무에서 많지 사용되진 않는 듯.
- 리액트의 프레임웍이 여러가지인 만큼 직접 경험해보고 내가 하려는 작업에 적절한 프레임웍을 선택하는 것이 중요해 보임.

<br />

## Add React to an Existing Project

- 리액트 프레임웍 없이도 프로젝트에서 리액트를 사용할 수 있다는 것을 설명해준 챕터.
- 예제 코드에 나온 `import { createRoot } from 'react-dom/client';` 부분의 `'react-dom/client'` 를 보며 리액트에서 서버 쪽과 클라이언트 쪽으로 나눠가려는 경향이 보이는 듯 했음.

### 🗣️ 이 챕터 관련해서 나눈 이야기

- 이전 버전의 리액트 문서에서는 vite 말고 webpack이 나왔었는데 이번 새로운 문서에서는 webpack이 아예 사라진 것이 인상적임.
- 메타에서도 이렇게 사용해왔어! 라는 문장이 있었는데 메타도 레거시 코드는 어쩔 수 없구나 하는 생각이 들었음. (_That’s a common way to integrate React—in fact, it’s how most React usage looked at Meta for many years!_)
- Gatsby만 사용하다가 Next를 도입해봐야겠다 싶었던 적이 있었음. Gatsby로 하던 정적 빌드를 Next로도 하려고 하다가 못했었는데 이번 챕터의 예제 코드 중에 `next export output` 을 발견해서 다시 시도해봐야겠다 싶었음. (관련 링크: https://react.dev/learn/add-react-to-an-existing-project#using-react-for-an-entire-subroute-of-your-existing-website)
  - Next에서 ssr, csr, ssg 다 구현가능하며, 이 경우엔 getStaticProps() 한번 살펴보는 것을 추천함.

<br />

## Editor Setup

- 대표적인 에디터 소개 및 설정 예시 소개.

### 🗣️ 이 챕터 관련해서 나눈 이야기

- vim은 매니아 층이 있어보임.
- ㅇㅇ 커스텀하기 좋아서 그런듯.
- 보통 웹개발하면 webstorm을 많이 사용하는데 intelliJ 를 사용하는 경우도 봤고, 유료 버전인 경우 자바스크립트, 타입스크립트도 사용할 수 있는데 편하다고 들음.
- '리팩토링' 책(링크: http://www.yes24.com/Product/Goods/89649360)에서 에디터 마다 자동 리팩토링 기능을 제공한다는 내용을 본 적이 있는데 유료 버전의 에디터들은 그 기능이 좀 더 강력하다더라.
- lint 관련 고민들
  - lint는 공용 패키지를 만들어서 사용하려 하는데 프로젝트 마다 커스텀해서 사용하게 되는데 기준을 어떻게 정해야 할 지 궁금함.
  - 개발자 개인마다 선호하는 방식이 있어서 어려운 듯. 공용 컨벤션에 개인 기호를 다 반영할 수 없을텐데, 개개인이 공용 컨벤션에 맞춰야 하는 것인지 개인 기호에 맞게 컨트롤 해도 되는건지 고민.
  - 혼자 개발하는 경우 혼자 lint 설정하다보니 애매해지는 부분도..
  - 프로젝트는 여러 개인데 담당자는 없을 때, lint를 제일 일반적인 걸로 설정해두고 상황에 맞게 바꾸기도 함. 다만 설정을 매번 바꾸기 어려우니 ignore 를 사용하게 되는 경우도..
  - Husky로 lint 설정을 정리하도록 하였음. (husky: https://www.npmjs.com/package/husky)
  - Husky를 같이 사용하지 않으면 lint가 의미 없는듯.
  - Husky를 사용하다 사용하지 않으니 프로젝트가 무너지는 느낌...🥺

<br />

## React Developer Tools

- 리액트 개발을 도와주는 브라우저 익스텐션 및 패키지 설치에 대한 소개.

### 🗣️ 이 챕터 관련해서 나눈 이야기

- highlight 를 주로 사용하게 됨.
- 이슈가 생겼을 때, highlight로 어디가 문제인지 확인하기 좋음.
- 상태 관리도 debugger나 console.log로 확인..
- safari 브라우저에서는 패키지를 설치해야 한다는 내용이 새로웠음.
