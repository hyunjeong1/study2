1. Props
    - Children prop(아름 변경 불가)
        - 커스텀 컴포넌트 사이에 어떤 것을 넣어도 무시(리액트가 어디에 넣을지 모르기 때문에)
        - 단 props.children 사용시 이용 가능
            - ex) <TabButton>components</TabButton>
            1. export default function TabButton({children}) {
                return <button>{children}</button>
            }
            2. export default function TabButton(props) {
                return <button>{props.chldren}</button>
            }
    - 컴포넌트 합성
        - 컴포넌트가 다른 컴포넌트나 내용을 감싸서 컴포넌트를 구축하는 것
    - children 사용과 attributes 사용 둘 중 경우에 따라 선택

2. 이벤트
    - 이벤트 처리
        - vanilla JS
            - ex)document.querySelector('button').addEventListener('cllick', () => {

            })
        - 리액트
            - prop 사용(on + Something)
                - on~~ 함수 가리킴
                - ex) export default function TabButton({Children}) {
                    function handleClick() {
                        console.log('Hello World!')
                    }
                    return (
                        <button onClick ={handleClick}>{children}</button>
                    )
                }
                    - 주로 함수 이름은 handle+이벤트trigger or 이벤트이름+handler(ex clickHandler)
                    - {}안에 함수 뒤에 ()추가하지 않기(추가시 화면에 렌더링시 함수 바로 실행)
3. 함수 Prop값으로 전달
    - 커스텀 컴포넌트에 속성으로 이벤트 추가
    - 이벤트가 실행시키는 함수가 실제 컴포넌트 설정 안에 있어야 함
        - 이벤트에 의해 실행되는 함수를 받아들이는 속성의 이름: 주로 on + Something
    - App.jsx에서 속성으로 함수 전달 > 커스텀한 컴포넌트 prop으로 이벤트 받아옴> on~~ 이벤트 발생시 prop으로 받아온 이벤트 사용하도록 설정
        -  ex)
        <App.jsx>
            function handleClick() {
                console.log('누름');
            }

            <TapButton onSelect = {handleClick}></TapButton>
        <Component>
            export default function TapButton({onSelect}){
                return(
                    <button onClick = {onSelect}></button>
                )
            }
        - Q. 그냥 App.jsx에서 onClick = {handleClick}눌러서 처리하면 안 돼? 굳이 버튼 클릭하면 app.jsx의 onSelect를 실행시켜서 handleClick함수를 실행해야해?

4. 이벤트 함수에 custom 인자 전달
    - ex)
    <App.jsx>
        function handleSelect(selectedButton){
            //selectedButton에 들어갈 수 있는 것: components, jsx, props, state
            console.log(selectedButton);
        }
        <TabButton onSelect{()=> handleSelect('components')}>Components</TapButton>
        //함수로 바꾸면 좀 더 확실하게 함
        //이 경우에는 렌더링 될 때 화살표 함수만 정의(화살표 함수 내부 내용은 실행 안 함)해서 화살표 함수의 우측 함수 바로 실행 안 함. 따라서 ()사용
        //탭 버튼의 컴포넌트에 따라서 onClick 실행되면 onSelect실행되어 그때 handleSelect() 실행됨
        //따라서 handleSelect함수 자동적으로 실행할 수 있고, 컨트롤 할 수도 있음(함수안에 인자 넣을수도)
    <TapButton.jsx>
        export default TaButton({children, onSelect}){return(
            <li> <button onClick={onSelect}>{children}</button></li>
        )}
5. 버튼 클릭시 출력 내용 바꾸기
    - Hooks
        - 리액트에서 use~~로 사용되는 것
        - 기술적으로는 일반 함수 but 다른 리액트 컴포넌트 함수에서 호출되거나 다른 리액트 훅에서 호출되어야 함
        - 중첩된 함수 안에서 호출되면 안 됨(컴포넌트 함수의 최상위 레벨에서 호출되어야 함)(if, loop도 안 됨)
            - ex) function APP(){
                useState(); //O
            }
             function APP(){
                function handleSelect(){
                    useState(); //X
                }
             }
        - useState()
            - 데이터 변경시 자기가 속한 컴포넌트 함수 활성화>리액트에 의해 재검토 됨
<App.jsx> - 실패본(리액트 컴포넌트 한 번만 실행하여 이렇게는 화면의 내용이 바뀌지 않음-다시 렌더링 안되므로)
    let tabContent = "처음 내용";

    function handleSelect(selectedButton){
            tabContent = selectedButton;
        }

    <TabButton onSelect{()=> handleSelect('components')}>Components</TapButton>
    
    {tabContent}



//let이나 const나 상관 없음(데이터 바뀌면 다시 실행되므로 굳이 변수 사용할 필요 없음. 상수로도 ok)
---
<App.jsx> - 성공본(State)
import {useState} from 'react';

const [selectedTopic, setSelectedTopic] = useState('처음 내용'); 

function handleSelect(selectedButton){
            setSelectedTopic(selectedButton);
        }

    <TabButton onSelect{()=> handleSelect('components')}>Components</TapButton>
    
    {selectedTopic}

// 반환되는 값은 배열임(요소 2개 있음)
// 보통 2번째 요소는 첫번째 요소에 set을 붙임
// 첫번째 요소 처음 렌더링 될 때 들어감
// 두 번째 요소 저장된 값 업데이트 됨& 두 번째 함수 호출 시 속한 컴포넌트 함수 다시 실행시킴
------
<App.jsx> - 좀 긴 내용 출력
import {Examples} from './src/data.js';
import {useState} from 'react';

const [selectedTopic, setSelectedTopic] = useState('components'); 

function handleSelect(selectedButton){
            setSelectedTopic(selectedButton);
        }

    <TabButton onSelect{()=> handleSelect('components')}>Components</TapButton>
    
    <div id="tab-content">
    <h3>{Examples[selectedTopic].title}</h3>
    <p>{Examples[selectedTopic].description}</p>
    <code>{Examples[selectedTopic].code}</code>
    </div>

<data.js>
export const Examples = {
    components: {
        title: 'Components',
        description: 'component description',
        code: 'component code'
    };
    jsx:{
        title: 'Jsx',
        description: 'jsx description',
        code: 'jsx code'
    }
}
--------
<App.jsx> - 초기값 변화/조건부로 출력
import {Examples} from './src/data.js';
import {useState} from 'react';

const [selectedTopic, setSelectedTopic] = useState('');  // null이나 공백도 OK

function handleSelect(selectedButton){
            setSelectedTopic(selectedButton);
        }

    <TabButton onSelect{()=> handleSelect('components')}>Components</TapButton>
    
    {selectedTopic === undefined ? <p>Please select a topic.</p> : <div id="tab-content">
    <h3>{Examples[selectedTopic].title}</h3>
    <p>{Examples[selectedTopic].description}</p>
    <code>{Examples[selectedTopic].code}</code>
    </div> : null}
    // 또는 {!selectedTopic ? <p>Please select a topic.</p> : <div id="tab-content"></div> }
    // 또는 {!selectedTopic && <p>select a topic </p>} {selectedTopic && (<div id="tab-content"></div>)}
    // 또는 변수 사용 
        let tabContent = <p>Select a topic</p>;
        if(selectedTopic){
            tabContent = (<div id = "tab-content"></div>)
        }
        <TabButton onSelect={() => handleSelect('components')}>Components</TabButton>
        {tabContent}

<data.js>
export const Examples = {
    components: {
        title: 'Components',
        description: 'component description',
        code: 'component code'
    };
    jsx:{
        title: 'Jsx',
        description: 'jsx description',
        code: 'jsx code'
    }
}

6. CSS 스타일링 및 동적 스타일링
    - class 사용을 하려면 class가 아니라 className을 속성에 추가해야함
    - 조건문 사용
        ex) 
        <TabButton.jsx>
        export default function TabButton({children, onSelect, isSelected}){
            return(
        <button className = {isSelected? 'active' :undefined} onClic = {onSelect}></button>
            );
        }

        <App.jsx>
        <TabButton isSelected={selectedTopic ==='components' onSelect={() => handleSelect('components')}></TabButton>

7. 리스트 데이터 동적 출력(반복-map)
<data.js>
import componenetsImg from './assets/components.png';
import jsxImg from './assets/jsx-ui.png';

export const CORE_CONCEPTS = [
    {
        image: componentsImg,
        title: 'Components',
        descrpition: 'desc'
    },
    {
        image: jsxImg,
        title: 'JSX',
        description: 'desc'
    }
]

<App.jsx>
import CORE_CONCEPTS from './assets/data.js';
<section id="core-concepts">
<ul>
{CORE_CONCEPTS.map((conceptItem) => <CoreConcept key ={conceptItem.title} {...conceptItem}/>)}
</ul>
</section>


Tip
- 컴포넌트 나눠놓기
    - src 폴더 밑에 components 폴더 만들기
    - 다른 파일의 컴포넌트 사용하기위해 export [default] function 사용해야 함
        - Q. export default와 export 차이?
        - 사용하려는 컴포넌트 임포트 해주기
- 코드 나눠놓기
    - CSS파일 각 컴포넌트의 css 파일로 쪼개기
    (컴포넌트 폴더 안에 넣기)
    - 컴포넌트 파일에 css 임포트해도 컴포넌트 파일에 적용되는 것이 아님
        - 전체에 적용됨
- 컴포넌트와 그 컴포넌트에 적용되는 css 파일 폴더 따로 만들어서 저장하는 것 권장(찾기 쉬움)
- 리액트 JSX코드 보고 현재 렌더링된 UI와 비교>UI 업데이트 하기 위해서 코드 리액트에 의해 재평가 되어야 함
- 기본적으로 리액트는 컴포넌트 함수 코드 내에서 처음 발견했을 때 한 번만 실행
- 계산: state나 props값을 기반으로 다른 값을 도출하는 작업을 의미(UI 동적으로 업데이트 하는데 사용)
- Derived State(파생된 state): 컴포넌트 내에서 기존 state나 props를 기반으로 값을 계산하는 것
- on 실제 이벤트 연결될 때
- handle 이벤트 호출되는 내용 설명
- 고차함수





우수 프로젝트
    - 기획배경
        - 통계자료 이용(논리성 증가)
        - 타겟 층을 명확하게(좁히기)

- S3(동영상 보관)
