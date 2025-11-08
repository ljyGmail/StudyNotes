# WebSquare

## 1. WebSquare5 소개 및 Studio 설명

* 요율적인 메모리를 관리하기 위해 `script`에서 변수, 함수, 이벤트 작성 시 `scwin`네임스페이스를 이용한다.
* WebSquare의 컴포넌트는 `id`중심으로 동작하기 때문에 제어할 컴포넌트는 꼭 id를 설정해야한다.
* InputBox관련 속성:
    - `initValue`: 초기 값
    - `dataType`: number로 설정하면 숫자
    - `displayFormat`: `#,###`로 설정하면 숫자가 3자리 마다 `,`로 나누게 됨
    - `applyFormat`: 위 설정은 InputBox가 포커스되지 않을 때 보여주는 포멧이고, 입력할때는 포멧이 사라진다.  
    입력할때도 포멧을 유지할려면 applyFormat의 값을 `all`로 설정한다.

* 컴포넌트에게 이번트를 적용하는 방식
    - 컴포넌트를 선택하고 `Property`창의 `이벤트` 탭에서 이벤트 추가
    - 컴포넌트를 `우클릭`하고 `이벤트` 항목에서 추가

* 실행순서
    - 우선 `Script`에 있는 소스를 모두 읽어들이고 `Property`창의 `속성` 탭에 있는 값을 설정하고 `scwin.onpageload`의 함수를 실행한다.

