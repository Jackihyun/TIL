## 리액트의 생명주기

- 컴포넌트는 생성(mounting), 업데이트(updating), 제거(unmounting)의 생명주기를 갖는다.
- 리액트 클래스 컴포넌트는 라이프 사이클 메소드를 사용 / 함수형 컴포넌트는 Hook을 사용한다.
<br/>

### 1. Class Component 생명주기

#### Mounting

컴포넌트가 생성될 때 발생하는 생명주기
1. constructor
  - 컴포넌트 생성자 메소드, 컴포넌트가 생성되면 가장 먼저 실행.
2. getDerivedStateFromProps
  - props로부터 파생된 state를 가져온다. props로 받아온 것을 state에 넣어주고 싶을 때 사용
3. render
  - 컴포넌트를 렌더링하는 메소드
4. componentDidMount
  - 컴포넌트가 마운트 됨이라는 말 그대로 첫 렌더링이 끝나면 호출되는 메소드이다.
  - 이 메소드가 호출되는 시점에는 화면에 컴포넌트가 나타난 상태입니다.
  - 여기서 주로 DOM을 사용해야 하는 외부 라이브러리 연동, 해당 컴포넌트에서 필요로하는 데이터를 ajax로 요청(등)하는 행위를 한다.

<br/>

#### Updating

컴포넌트가 업데이트 되는 시점에 어떤 생명주기 메소드들이 호출되는가.
1. getDerivedStateFromProps
- 컴포넌트의 props나 state가 바뀌었을 경우에도 이 메소드가 호출된다.
2. shouldComponentUpdate
- 컴포넌트가 Rerendering할 지 말지 를 결정하는 메소드이다. (React.memo?)
3. componentDidUpdate
- 컴포넌트가 업데이트 되고 난 후 발생한다.

<br/>

#### Unmounting

언마운트는 컴포넌트가 화면에서 사라지는 것을 의미하는데, 언마운트와 관련된 라이프사이클 메소드는 componentWillUnmount 하나이다.
1. componentWillUnmount
- 컴포넌트가 화면에서 사라지기 직전에 호출된다.
- 주로 DOM에 직접 등록했었던 이벤트를 제거하고, 만약 setTimeout을 걸은 것이 있다면 clearTimeout을 통해 제거한다.
- 예를 들어 추가 라이브러리가 있다면, 또 그 라이브러리의 dispose 기능이 있다면 여기서 호출한다.

<br/>

### 2. Functional Component 생명주기

리액트에서 Hook은 함수형 컴포넌트에서 React state와 생명주기 기능을 연동할 수 있게 해주는 함수이다.
Hook은 클래스 안에서 동작하지 않는다. 또한 class없이 React를 사용할 수 있게 해준다.

#### 왜 리액트 훅을 도입했을 까?
- 기존의 생명주기 메소드 기반이 아닌 로직 기반으로 나눌 수 있어서 컴포넌트를 잘게 쪼갤 수 있다는 장점(아토믹?)
- 생명주기 메소드에서는 관련 없는 로직이 자주 섞여 들어가는데, 이로인해 버그가 쉽게 발생해 무결성을 쉽게 해친다.

<br/>

#### Hook 사용 규칙 2개
1. 최상위에서만 Hook을 호출해야한다!!
- 반복문, 조건문, 중첩된 함수 내에서 Hook 실행 불가
- 이 규칙으로 인해 컴포넌트가 렌더링될 때마다 항상 동일한 순서로 Hook이 호출되는 것이 보장.

2. 리액트 함수 컴포넌트에서만 Hook을 호출해야한다
- 일반 JS함수에서는 Hook을 호출해서는 안된다.

<br/>

### Hook의 종류

**useState**
- 상태 관리 / [state이름, setter이름] 순으로 반환받아서 사용

  ```jsx
    const [state, setState] = useState(initialState);
  ```

**useEffect**
- 화면에 렌더링이 완료된 후에 수행. 위의 클래스 컴포넌트에서 사용되는 componentDidMount와 componentDidUpdate, componentWillUnmount가 합쳐진 것
- useEffect안에서 return은 정리 함수를 사용하기 위해 사용됨.

```jsx
   useEffect(() => {}); // 렌더링 결과가 실제 돔에 반영된 후마다 호출
   useEffect(() => {}, []); // 컴포넌트가 처음 나타날 때 한 번 호출
   useEffect(() => {}, [의존성1, 의존성2, ..]); // 조건부 effect 발생. 의존성 중 하나가 변경되면 effect는 항상 재생성
```

**useContext** 
- Context API를 통해 만들어진 Context에서 제공하는 value를 가져올 수 있다.
```jsx
  const value = useContext(MyContext);
```
컴포넌트에서 가장 가까운 <MyContext.Provider>가 갱신되면 이 Hook은 그 MyContext porvider에게 전달된 가장 최신의 context value를 사용하여 렌더러를 트리거한다.

**useRef**
- 특정 DOM을 선택할 때 주로 쓰이며, .current 프로퍼티로 전달된 인자로 초기화된 변경가능한 ref 객체를 반환한다. 반환된 객체는 컴포넌트의 전 생애주기를 통해 유지된다.
```jsx
  const ref = useRef(null);
```

**useMemo**
- memoization된 값을 반환. 이미 연산된 값을 리렌더링 시 다시 계산되지 않도록한다.
- 의존성이 변경되었을 경우에만 memoization된 값만 다시 계산한다. 의존성 배열이 없는 경우 모든 렌더링 때마다 새 값을 계산.
```jsx
  const momoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

**useCallback**
- memoization된 콜백을 반환한다. useMemo와 유사하게 이용, 함수에 적용한다.
- 의존성이 변경되었을 때만 변경된다. 따라서 특정 함수를 새로 만들지 않고 재사용가능.
```jsx
  const memoizedCallback = useCallback(
    () => {
      doSomething(a, b);
    },
    [a, b],
  );
```
  






