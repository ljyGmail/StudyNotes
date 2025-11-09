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

## 2. 컴포넌트 소개 및 사용방법

* `InputCalender`
    - 날짜 형식은 속성 `calendarValueType`으로 바꿀 수 있다.
* `TextBox`
    - `tagname`속성으로 HTML태그를 지정할 수 있다.
    - `TextBox`내 텍스트를 여러줄로 보여줄때 입력창에 `Alt+Enter`로 줄을 바꾸고, 속성에 `escape`의 값을 `false`로 설정해야 화면에서 줄이 바뀐다.

## 3. 화면 그리기

* `TableLayout`의 `th`열 너비를 조절할려면 열 더블클릭하고 너비 값을 지정 후, 그 뒤에 있는 `td`의 값을 없애면 된다.
* `TableLayout`에서 테이블 속성`adaptive`의 값을 `layout`으로 설정하면 화면 너비에 따라서 반응한다.

## 4. DataCollection 과 Submission처리 방법

* 현재 날짜/시간를 가져오는 방법: `$p.getCurrentServerDate(pattern)`.
* `SelectBox`의 옵션 설정 시 'Choose Option'칸에 아무것도 입력하지 않으면 디폴트르 `-선택-`으로 나온다. 실제로 빈칸으로 보여질려면 '$blank'를 입력해야된다.
* `SelectBox`, `Radio`, `Checkbox`에게 데이터를 설정하는 방식:
    - 컴포넌트를 더블클릭하고 팝업창 상단에서 하드코딩 방식으로 옵션을 설정하는 방식
    - API로 설정하는 방식: `ui_gender.addItem('M', '남성', 1) -> (value, label, index)`
* `DataCollection`을 API로 값 세팅:
```js
const jsonData = [{ "code": "01", "name": "PM" }
        , { "code": "02", "name": "DEV" }
        , { "code": "03", "name": "PL" }
        , { "code": "04", "name": "DES" }
    ];
dc_code.setJSON(jsonData, true);
```
* API를 통한 Bind
```js
// setNodeSet(nodeset, label, value)
// DataSet앞에 data:를 꼭 붙여야한다
chkRole.setNodeSet("data:dc_code", "name", "code");
```

* `Submission`만들고 사용하는 절차
    - `Outline`창의 `Head`탭에서 Submission추가
    - `URL Action`에서 URL입력 후 `Submission Test`클릭
    - `Send`버튼 클릭하고 Response데이터 확인 후 `Create DataCollection`버튼 클릭 후 DataCollection생성
    - `Target`의 `DataCollection`버튼을 클릭하고 생성된 `DataCollection`을 지정하고 추가한다
    - `Submit`에서 Script를 지정하여 Submission호출 전치리할 수 있다
* `Submission`생성 후 조회 버튼에서 클릭 이벤트를 추가한다
```
scwin.btn_select_onclick = function(e) {
    $p.executeSubmission('sbm_search');
};
```
* `DataCollection`에 있는 데이터 항목을 화면에 데이터를 보여줄 컴포넘트와 바인팅시킨다.

* `DataCollection`만들 때 텍스트로 복사 가능하다.