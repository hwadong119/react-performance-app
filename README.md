# 리액트 앱 성능 개선하기

<br>

<a href='https://github.com/hwadong119/react-performance-app/tree/main'>useMemo를 이용한 최적화</a>

<br>

# useCallback을 이용한 최적화
컴포넌트가 렌더링 될 때 그 안에 있는 함수도 다시 만들게 됨

하지만 똑같은 함수를 컴포넌트가 리렌더링 된다고 해서 계속 만드는 것은 비효율적

컴포넌트가 리렌더링될 때마다 함수를 계속 다시 만들어 이 함수가 자식 컴포넌트에 props로 내려준다면 함수를 포함하고 있는 컴포넌트가 리렌더링될 때마다 자식 컴포넌트도 함수가 새롭게 만들어지니 계속 리렌더링 하게됨

<br><br>

# 함수 생성하기

```javascript 
const testFunction = () => {};
```

<br><br>

# props로 함수 넘겨주기

```javascript
return (
    <div>
      <h1>B Component</h1>
      <Message message={message} />
      <List posts={posts} testFunction={testFunction}/>
    </div>
  )
```

<br><br>
<br><br>

![Alt text](image/image.png)

원래는 React.memo로 감싸줘서 리렌더링 되지 않았던 컴포넌트들이 한글자 입력할 때마다 List 컴포넌트까지 다시 리렌더링되는 걸 볼 수 있음

<br><br>

# useCallback으로 문제 해결

useCallback은 메모이제이션된 함수를 반환하는 함수 

useCallback 안에 콜백함수와 의존성 배열을 순서대로 넣는다

```javascript
const testFunction = useCallback(() => {}, [])
```

함수 내 참조하는 state와 props가 있다면 의존성 배열에 추가

useCallback으로 인해 의존성 배열에 추가해준 state 혹은 props가 변하지 않는다면 함수는 새로 생성 되지 않음

새로 생성되지 않기에 메모리에 새로 할당되지 않고 동일 참조 값을 사용하게 됨

의존성 배열에 아무것도 없다면 컴포넌트가 최초 렌더링 시에만 함수가 생성되며 그 이후에는 동일한 참조값을 사용하는 함수가 됨
