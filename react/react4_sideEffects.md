1. Side Effects(부수효과)
    - 앱이 제대로 동작하기 위해 실행되어야 하지만 현재의 컴포넌트 렌더링 과정에 직접적인 영향을 미치지 않는 모든 작업
    - 꼭 실행되어야 하지만 현 컴포넌트 렌더링 과정에 별 영향 안 미침

2. useEffect
    - return 안 함
    - 묶어줄 부수효과코드를 묶어줄 함수 필요
    - 의존성 배열 필요
    - useEffect가 의도하는 바: 전달하는 첫 인수가 리액트로 인해 실행되는 시점(앱 컴포넌트 함수가 모두 완료된 이후 실행, jsx코드 반환된 후 시점>useEffect에 넣어놓은 부수 효과 함수(첫 인수)를 실행)
    - 의존성 배열 바뀌어야 useEffect 재실행
    - 앱 컴포넌트 실행되고 추가적으로 실행하는 방식(따라서 남용 좋지 않음)
    - 무한 루프 방지 || 컴포넌트 최소 한 번은 작동 후 작동이 가능한 코드가 있을때만 필요

3. localStorage
    - setItem에 selectedPlaces와 같은 식별자 전달, 2번째 인수 저장되어야 할 값 전달(2번째 인수> String 형태여야 함)

4. 의존성
    - 컴포넌트 함수를 다시 실행하게 만드는 속성 or 상태(useEffect안에서 사용)

5. setTimeOut
    - 브라우저 내장 함수
    - 첫 번째 인자: 함수, 두 번째 인자: 초[ms]

6. useCallback
    - value return(둘러싸둔 함수 반환)
    - 주변 컴포넌트 함수 재실행될 때마다 재-생성x
    - 안에 있는 함수 재생성x, 메모리로서 내부에 전달> 해당 컴포넌트 함수 재 실행 될 때마다 메모리로서 저장된 그 함수 재사용
    - 배열에 안에 들어가 있는 함수의 prop이나 state value값만 추가

7. 클린업함수
    - useEffect 실행 후 종료 안 되지 않은 함수를 종료 시키는 함수
    - useEffect 내부에 return을 넣어서 함수 종료시킴
    

```jsx
// 10초가 걸리는 패치 함수는 어떻게 처리해야 할까?
const [data, setData] = useState([]);

// useEffect(()=>{
    const _data = fetch();
    setData(_data);
// }, []);

return <div>{data}</div>

```