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

## 5. DataCollection 과 gridView연동

* gridView과 td에게 동일한 속성에게 다른 값을 줄 때 td의 우선순위가 높다.
* gridView의 열 너비를 조정하는 방법
    - 화면에서 바로 드래그
    - gridView를 더블 클릭하고 다이어로그창에서 width값을 조절하면된다
* gridView의 열 너비를 자동으로 전체 너비를 채울려면 `autoFit`속성값을 `allColumn`으로 설정한다
* `autoFixMinWidth`를 설정하면 자동 너비 조절의 최소 너비를 지정할 수 있다
* `fixedColumn`속성의 값을 설정하여 틀고정할 컴럼을 지정한다
* `columnMove`속성의 값을 `true`로 설정하여 컬럼을 이동할 수 있게 한다
* `sortable`속성의 값을 `true`로 설정하여 정렬할 수 있게 한다
* `sortEvent`속성의 값은 2가지가 있다 (onclick, ondbclick)
* gridView의 헤더에게 `useFilter`의 값을 `true`로 설정하여 해당 컬럼에게 필터링 기능을 적용한다
* gridView에게 `useFilterList`의 값을 `true`로 설정하여 더욱 편리하게 체크박스를 선택해서 필터링하는 기능이 적용된다
* gridView의 `rowNumVisible`속성의 값을 `true`로 설정하면 순번이 자동으로 표시된다 
* 순번 Label를 표시할려면 직접적으로 설정이 안되고 `rowNumHeaderValue`속성으로 설정해야한다
* Row의 데이터가 변경될 때 표시할려면 `rowStatusVisible`의 값을 `true`로 설정한다.  
해당 설정 후, 상태를 표시하는 열이 하나 추가된다. 마찬가지로 헤더Label은 `rowStatusHeaderValue`속성으로 설정한다
* 셀 편집할려면 디폴트로 마우스 더블클릭해야하는데, 이 설정을 바꿀려면 `editModeEvent`속설을 설정한다.
* 탭 키로 커서 이동하면서 바로 편집할려면 `keyMoveEditMode`속성의 값을 `true`로 설정한다
* 탭 이동을 자동으로 아래 행으로 가게할려면 `focusFlow`의 값을 `linear`로 설정한다
* 셀 안에 있는 내용은 여러 타입으로 설정 가능하다. `inputType`으로 변경할 수 있다. 예: `select`, `calendar`
* 셀 내용의 타입이 `select`라는건 현재는 편집모드여야 선택할 수 있다는걸 알 수 있다.  
 편집 상태가 아니어도 선택할 수 있다는걸 알 수 있을려면 `셀`의 `viewType`속성을 `icon`으로 설정해야 한다.
* 셀 내용을 형식을 지정할려면, 간단한 방식은 `displayFormat`속성에서 `###-###`식으로 지정할 수 있다
* 약간 복잡하고 로직이 필요한 형식을 지정할려면, `displayFormatter`속성에서 함수를 지정하고, `Script`탭에서 해당 함수를 구현하면된다.

## 6. gridView의 Event 및 API
* gridVew의 속성은 gridView전체, 헤더, 셀 별로 설정할 수 있지만, `Event`는 gridView전체에게만 적용가능하다.
* gridView관련 이벤트 처리 시 컴럼 인덴스를 사용하기 보다 컬럼ID를 사용하는 것을 권장한다.
* 하지만 컬럼ID를 바로 사용할 수 있는 이벤트는 `oncellclick`과 `oncelldbclick`밖에 없다.
* 그중 어떤 이빈테는 `info`파라미터가 있는데, 여기서 컴럼ID를 찾아볼 수 있다.
* 혹은 gridView에서 제공한 API`getColumnId(col)`를 사용할 수도 있다. 
```js
scwin.ui_memberList_oncelldblclick = function (row, col, colId) {
    // 사번일 경우에만
    if (colId === 'EMP_CD') {
        alert('사번');
    }
};

scwin.ui_memberList_onafteredit = function (row, col, value) {
    const _colId = this.getColumnID(col);
    // 이름일 경우에만
    if (_colId === 'EMP_NM') {
        this.setCellColor(row, col, 'red');
    }
};
```

* gridView뿐만 아니라 그와 연결된 `dataList`에도 이벤트가 있다.  
주로 gridView안에 있는 데이터와 관련된 이벤트다.
```js
scwin.dc_userInfoList_ondataload = function () {
    // 건수 구하기
    span1.setValue(dc_userInfoList.getRowCount());
};

scwin.dc_userInfoList_oninsertrow = function (info) {
    // 건수 구하기
    span1.setValue(dc_userInfoList.getRowCount());
};

scwin.btnInsert_onclick = function (e) {
    // 첫째 행에 추가
    // dc_userInfoList.insertRow(0);
    const rowIdx = dc_userInfoList.insertRow();
};

scwin.btnDelete_onclick = function (e) {
    const focusIdx = ui_memberList.getFocusedRowIndex();
    dc_userInfoList.deleteRow(focusIdx);
};

scwin.btnRemove_onclick = function (e) {
    const focusIdx = ui_memberList.getFocusedRowIndex();
    const obj = dc_userInfoList.removeRow(focusIdx);
};

scwin.btnDeleteRows_onclick = function (e) {
    const chkidxArr = ui_memberList.getCheckedIndex('CHK');
    dc_userInfoList.deleteRows(chkidxArr);
};

scwin.btnRemoveRows_onclick = function (e) {
    const chkidxArr = ui_memberList.getCheckedIndex('CHK');
    const obj = dc_userInfoList.removeRows(chkidxArr);
};

scwin.btnInit_onclick = function (e) {
    //const obj = dc_userInfoList.removeAll();
    dc_userInfoList.setData([]);
};

scwin.btnExcelDown_onclick = function (e) {
    ui_memberList.advancedExcelDownload([]);
};

scwin.btnExcelUpload_onclick = function (e) {
    var options = {};
    options.headerExist = "1";
    options.type = "1";
    ui_memberList.advancedExcelUpload(options);
};

```

* `customFormatter`속성으로 필요한 서식 로직을 지정할 수 있다.
```js
scwin.cus = function (data, formattedData, rowIdx, colIdx) {
    if (data === "F") {
        ui_memberList.setCellColor(rowIdx, "EMP_CD", "orange");
    }
    return formattedData;
}
```