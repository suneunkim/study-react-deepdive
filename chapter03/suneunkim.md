## 03장 - 리액트의 모든 훅 파헤치기

### 3.1.1 useState

리액트의 렌더링은 함수형 컴포넌트에선 return, 클래스형 컴포넌트에선 render 함수를 실행한 다음, 실행 결과를 이전의 리액트 트리와 비교해 리렌더링이 필요한 부분만 업데이트해 이뤄진다.

```jsx
fucntion Component() {
	const [, trggerRender] = useState()

	let state = 'hello'

	function handleBtn() {
		state = 'hi'
		triggerRender() // 리렌더링 발생시키려는 시도
	}

	return (
	<>
		<h1>{state}</h1>
		<button onClick={handleBtn}>hi</button>
	</>
	)
}
```

위 코드는 useState의 첫 번째 원소는 사용하지 않고 자체적으로 변수를 사용하고, 두 번째 원소로 리액트에서 리렌더링이 일어나게끔 하고있지만 버튼을 클릭해도 변경된 state로 리렌더링 되지 않는다.

그 이유는 렌더링이 발생될 때 마다 함수는 다시 새롭게 실행된다. 그렇기에 state의 값을 바꿔도 다시 초기값(’hello’)으로 초기화된다. 그렇기에 리렌더링이 되어도 상태를 유지하려면 useState를 사용해야 한다. 참고로 useState의 초깃값은 최초에만 사용된다.

그렇다면 리렌더링 되어도 useState의 변수가 초기값으로 초기화 되지 않고, 함수가 종료되어도 필요할 때 마다 state로 꺼내 쓸 수 있다는 건 클로저를 이용해 구현되었을 것으로 짐작할 수 있다.

여기서 클로저를 다시 복습! 클로저는 함수와 함수가 선언된 렉시컬 스코프의 조합이다. 같은 어휘적 환경에 있는 변수에 접근할 수 있고, 사용한다면 그 변수의 환경을 기억하고 있기 때문이다. 함수의 실행이 끝났어도 함수가 선언된 환경을 기억할 수 있는 방법이 클로저이다. useState()도 실행이 끝났어도 state에 접근할 수 있는건 클로저 때문이다.

**게으른 초기화**

useState() 인수로 보통 원시값을 받는데, 특정한 값을 넘기는 함수를 인수로 쓰는 경우를 게으른 초기화라고 한다.

언제 쓰는가 ? → 초기값이 복잡하거나 무거운 연산을 포함하고 있을 때!

왜 ? → 리액트에서는 렌더링이 실행될 때 마다 함수형 컴포넌트의 함수가 다시 실행된다는 걸 명심하자. 게으른 초기화 함수는 오로지 state가 처음 만들어질 때만 사용한다. 즉 리렌더링때는 이 함수의 실행이 무시된다.

그렇기에 리렌더링 시에 초기값이 더는 필요없는데도 연산을 하고, 값에 접근하는 낭비가 생기는 경우 사용할 수 있다. 예시로 로컬 스토리지, 세션 스토리지의 접근과 map, filter, find 같은 배열의 접근, 초깃값 계산으로 함수 호출 시, 무거운 연산을 포함한 실행 비용이 많이 드는 경우가 있다.

### 3.1.2 useEffect

첫 번째 인수로 실행할 부수 효과가 포함된 함수를, 두 번째 인수로 의존성 배열을 전달한다. 의존성 배열이 변경 될 때 마다 첫 번째 인수인 콜백을 실행하는 함수이다.

- 두 번째 의존성 배열에 빈 배열을 넣으면 렌더링이 된 후에 실행된다.
- 생명주기 메서드를 위해 만들어진 훅이 아님.
- 컴포넌트의 여러 값들을 활용해 동기적으로 부수 효과를 만드는 메커니즘
- 부수 효과가 ‘언제’ 일어나는지보다 어떤 상태값과 함께 실행되는지 살펴보는게 중요
- 렌더링 할 때 마다 의존성에 있는 값을 보면서 이전과 다른게 있다면 부수 효과를 실행하는 함수

**useEffect의 클린업 함수**

useEffect 내에서 return 되는 함수는 클린업 함수라고 불린다. 일반적으로 이벤트를 등록하고 지울 때 사용.

- 생명주기 메서드의 언마운트 개념과 조금 차이가 있다
- 클린업 함수는 리렌더링 시 의존성 변화가 있었을 당시 이전의 값을 기준으로 실행
- 즉 이전 상태를 청소해 주는 개념
- 새로운 값을 기반으로 렌더링 뒤에 실행되지만 변경된 값을 읽는게 아니라 이전 값을 보고 실행

즉 실행 될 때마다 이전의 클린업 함수가 존재한다면 그 클린업 함수를 실행한 다음에 콜백을 실행한다. 이렇게 함으로써 특정 이벤트가 무한히 추가되는 것을 방지할 수 있다.

**의존성 배열**

의존성 배열이 없으면 최초 렌더링에만 실행 된다. 의존성이 없기에 비교할 필요 없이 렌더링할 때 실행된다. 그렇다면 useEffect를 안쓰는거와 쓸 때 무슨 차이가 있는가?

- 클라이언트 사이드에서 실행되는 것을 보장한다. 렌더링이 완료 된 이후에 실행되기 때문이다.
- 그렇기에 컴포넌트가 렌더링 된 후에 어떠한 부수 효과를 일으키고 싶을 때 사용한다.

**useEffect 사용할 때 주의할 점**

1. 의존성 배열을 빈 배열로 사용하는 것을 지양하기
   - 대부분 빈 배열을 의존성으로 할 때는 컴포넌트를 마운트하는 시점에만 무언가를 하려는 의도로 작성된다. 이는 클래스형 컴포넌트의 생명주기 메서드인 componentDidMount에 기반한 접근법으로 가급적 사용해선 안 된다.
   - 의존성 배열을 비우고 콜백 함수 내부에서 특정한 값을 사용한다는 것은 state, props의 변경과 useEffect의 부수 효과가 별개로 작동하게 된다. useEffect는 언제가 아니라 어떤 값과 함께 실행되는지 살펴보는게 중요하다는 점에서 저자는 의존성 배열을 비우고 쓰는걸 지양하고 있다.
2. useEffect의 수가 많거나 복잡하다면 익명 함수 대신 기명 함수로 바꿔서 이름에 맞는 목적을 명확히 하기
3. 부수 효과가 클 수록 성능에 악영향을 미칠 수 있으므로 큰 useEffect 보다는 여러 개의 useEffect로 분리하고 의존성 배열을 나눠 갖기.

### 3.1.3 useMemo → 값(변수) 저장

- 비용 큰 연산 결과를 저장, 그 값을 반환하는 훅
- 첫 번째 인수는 값을 반환하는 함수이고 두 번째 인수는 그 함수가 의존하는 값이다.
- 즉 계산된 값을 저장하는 훅이다.

React.memo → 컴포넌트 저장

- useMemo는 계산한 값을 저장하는 용도라면 이것은 컴포넌트가 대상이다.
- 같은 프로퍼티인데 리렌더링 하는 경우가 빈번한 경우 사용하면 좋다

### 3.1.4 useCallback → 함수 저장

- 리액트는 state 바뀌면 리렌더링이 된다. 이 때 함수가 재생성 된다. 이를 방지하기 위한 함수의 메모이제이션.
- 의존성이 변경 됐을 때만 함수의 재생성이 되도록 만들기에 불필요한 리소스 또는 리렌더링을 방지함

### 3.1.5 useRef

- useState처럼 렌더링 후에도 상태값을 저장하나 그 값이 변해도 리렌더링은 하지 않는다.
- 반환하는 객체 내부에 current로 값에 접근하거나 변경할 수 있다
- 렌더링 될 때만 생성된다. 변수에 값을 할당하는 것과 비교하면 렌더링되지 않을 때도 변수가 메모리를 사용하는 것과 차이가 있음.
- DOM에 접근하고 싶을 때 가장 일반적으로 사용한다.

### 3.2 사용자 정의 훅과 고차 컴포넌트 중 무엇을 써야 할까?

사용자 정의 훅

- 다른 컴포넌트 내부에서 같은 로직을 공유하고자 할 때 주로 사용
- use로 시작하는 규칙을 준수해서 리액트 훅이라는 것을 바로 인식

### 3.2.2 고차 컴포넌트

- 컴포넌트 자체의 로직을 재사용하기 위한 방법
- 함수를 인수로 받거나 새로운 함수를 반환해 완전히 새로운 결과를 만들어 낼 수 있음
- 공통화 된 로직을 처리하기에 좋은 방법

렌더링 결과물에 영향을 미치는 공통 로직이라면 고차 컴포넌트를 사용하자!