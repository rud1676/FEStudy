# React Hooks(feat 리액트역사)

> author: @rud1676

- [1.React의\_등장](#react의-등장)
- [2.ReactHooks등장](#react-hooks의-도입)
- useCallback
- useMemo
- ..(계속 추가할 예정)

## React의 등장

### 리액트의 역사

React는 페이스북이 2011년 개발하고 2013년에 공개한 JavaScript 라이브러리로, 사용자 인터페이스를 구축하기 위해 설계되었습니다. React의 탄생 배경은 **대규모**, **동적인 웹 애플리케이션의 복잡성을 관리**하기 위해 등장. 특히, **UI의 상태를 효율적으로 관리**하고, **DOM의 변화를 최소화하여 성능을 향상시키려는 목표**. React는 **컴포넌트 기반 아키텍처**를 도입함으로써 UI를 재사용 가능한 독립적인 단위로 나누고, 이를 통해 개발의 복잡성을 줄이고 유지보수가 쉽게 만들었다. 초기에는 주로 페이스북 내부에서 사용되다가 점차 다른 개발자들도 사용하기 시작했다. React의 성공은 그것이 제공하는 **선언적 프로그래밍 모델**, **효율적인 DOM 업데이트 메커니즘**, 그리고 **컴포넌트 기반의 설계** 덕분이다.

### 리액트 등장배경

당시 기존의 프레임워크나 라이브러리로는 페이스북의 복잡한 UI와 데이터 상태 관리 요구사항을 만족시키기 어려웠다.(실제로 한번 떠올려보면 페이스북은 엄청난 데이터와 인터페이스 등이 있었다.)
React의 아이디어는 페이스북의 소프트웨어 엔지니어인 **조던 워크(Jordan Walke)**에 의해 제안되었다 조던 워크는 XHP라는 PHP용 HTML 컴포넌트 라이브러리에 영감을 받아 React를 개발했다.

## React Hooks의 도입

React Hooks의 등장은 클래스 기반 컴포넌트와의 차별화된 접근 방식을 제공하고, React 16.8 버전에서 처음 등장했다. Hooks는 기능적 컴포넌트에서 상태 관리 및 다른 React 기능들을 사용할 수 있게 하는 새로방법론을 제시했다.

Hooks는 조금더 현대적인 접근 방식을 제공하는데, 구문구조, 재사용성, 상태관리, 학습 곡선등 클래스와는 차별화 되어 제공된다.

구체적인 예시로 customHooks를 제작할 수 있는데, 이는 코드의 재 사용성이 유용하게 늘어난다.

```js
import { useState, useEffect } from "react";

function useFetchData(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url);
        const data = await response.json();
        setData(data);
        setLoading(false);
      } catch (error) {
        setError(error);
        setLoading(false);
      }
    };
    fetchData();
  }, [url]);

  return { data, loading, error };
}

export default useFetchData;
```

fetch하는 hooks를 예시로 들었는데 이것을 classComponent로바꾸면

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      data: null,
      loading: true,
      error: null,
    };
  }

  componentDidMount() {
    fetch("https://api.example.com/data")
      .then((response) => response.json())
      .then((data) => this.setState({ data, loading: false }))
      .catch((error) => this.setState({ error, loading: false }));
  }

  render() {
    const { data, loading, error } = this.state;

    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error.message}</div>;

    return (
      <div>
        {data.map((item) => (
          <div key={item.id}>{item.title}</div>
        ))}
      </div>
    );
  }
}

export default MyComponent;
```

와 같이 변한다. 즉, 재사용성이 조금 떨어진다.

### 그렇다면 왜 Hooks의 등장이 필요했을까?

과거에 React는 **mixins**, **고차 컴포넌트(Higher Order Components, HOCs)**, 그리고 **렌더 프롭스(Render Props)**와 같은 다양한 방법을 통해 컴포넌트 간의 로직을 재사용하려고 했으나 이러한 접근법들은 각각의 한계와 복잡성이 있는데, 예를 들어, mixins는 충돌의 위험이 있었고, HOCs는 컴포넌트 계층을 복잡하게 만들었다. 이러한 문제를 해결하기 위해 Hooks가 도입이 되었고, Hooks는 함수형 컴포넌트에서 상태 관리와 사이드 이펙트를 처리할 수 있게 해주며, 컴포넌트 간의 로직을 재사용하기 더 쉽게 만들어주었다.

## Hooks를 뜯어보자!

### Hooks를 분석하려면..

오픈소스의 코드를 살펴봐도 좋다 주요로 살펴봐야될 파일은 3개이다

- ReactFiberHooks.js: 이 파일은 Hooks의 주요 로직을 담고 있으며, 여러 Hooks (useState, useEffect, useContext 등)의 구현을 포함한다.
- Scheduler 패키지: Hooks의 비동기 동작과 관련된 로직있고, React의 효율적인 상태 업데이트와 렌더링을 관리한다.
- ReactFiberReconciler.js: 이 파일은 React의 리컨실레이션(reconciliation) 알고리즘을 다룬다. 리액트의 가상 DOM과 실제 DOM 간의 비교 및 업데이트 과정을 이해하는데 중요.

### Hooks가 뭐가있는지 살펴보기

1. useCallback에 대해
   > CHATGPT: useCallback을 사용하면, 의존성 배열 내의 값이 변경될 때에만 함수가 다시 생성됩니다. 이를 통해 불필요한 렌더링을 줄이고, 성능을 최적화할 수 있습니다.

차이점을 직접 실행시키며 보자

[useCallback](https://codepen.io/KyungJin-Joo/pen/QWoQNgQ?editors=1111)

React.memo는 props가 이전과 같다면 이전에 랜더링 결과를 재사용하는 함수! - 리액트에서 제공하는 고차 컴포넌트이다

그렇다면 무조건 useCallback을 쓰는게 유리할까?

> ChatGPT의 답변
>
> - 오버헤드: useCallback은 메모이제이션(memoization)을 수행합니다. 이 과정에서 함수를 메모리에 저장하고, 의존성 배열을 추적하는 추가적인 오버헤드가 발생할 수 있습니다. 때문에 매우 간단한 함수나, 렌더링 최적화가 크게 필요하지 않은 경우에 useCallback을 사용하면 오히려 성능이 저하될 수 있습니다.
> - 의존성 배열 관리: useCallback은 의존성 배열에 따라 함수를 재생성합니다. 이 배열을 관리하는 과정에서 불필요한 재생성이 발생할 수 있으며, 이는 성능에 부정적인 영향을 미칠 수 있습니다.
> - 컴포넌트 복잡성: useCallback은 컴포넌트의 복잡성을 증가시킬 수 있습니다. 코드가 복잡해지면 성능 최적화가 오히려 어려워질 수 있습니다.

실제로 useCallback을 무한 반복했다가 피봤다. 의존성을 추적하기 때문에 오히려 오버헤드가 발생해 앱전체가 랙을 엄청먹었었다.

2. useMemo에 대해
