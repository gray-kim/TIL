# RealGrid2 사용법
### 기본 그리드 생성법
- 컬럼, 필드 선언
```javascript
var fields = [
    {
        fieldName: 'productName',
        dataType: 'text',
    },
    {
        fieldName: 'productQty',
        dataType: 'number',
    }    
];

var columns = [
    {
        header: {text: '상품명'}, 
        name: 'productName', 
        fieldName: 'productName',
        type: 'data',
        width: '70'
    },
    {
        header: {text: '상품수량'}, 
        name: 'productQty', 
        fieldName: 'productQty',
        type: 'data',
        width: '70'
    }
];
```
- dataProvider, gridView 선언 및 필드, 컬럼 연결
```javascript
var dataProvider = new RealGrid.LocalDataProvider();
var gridView = new RealGrid.GridView('gridTagId');

gridView.setDataSource(dataProvider);

dataProvider.setFields(fields);
gridView.setColumns(columns);
```

- 그리드에 데이터 채우기(연결하기)
```javascript
dataProvider.setRows(jsonResult);
```

- 기타 옵션 설정
```javascript
gridView.setEditOptions({editable: false});
```

- 그리드에서 데이터 가져오기(https://docs.realgrid.com/guides/data-manager/get-values)
```javascript
// 1. getRows() 사용
var rows = dataProvider.getRows();
// 반환값 : RowValues[][] ([["박영호",71],["조일형",62]])

// 2. getValue(row, field)
var rows = dataProvider.getValue(0, "productName");
// 반환값 : any

// 3. getValues(row)
var rows = dataProvider.getValues(0);
// 반환값 : RowValues any[];

// 4. getJsonRow(row)
var rows = dataProvider.getJsonRow(0);
// 반환값 : RowObject { [key: string]: any; };

// 5. getRows(startRow, endRow)
var rows = dataProvider.getRows(0, -1);
// 반환값 : RowValues[][] ([["박영호",71],["조일형",62]])

// 6. getJsonRows(startRow, endRow)
var rows = dataProvider.getJsonRows(0, -1);
// 반환값 : object[]

// 7. getFieldValues(field, startRow, endRow)
var rows = dataProvider.getFieldValues("productName", 0, -1);
// 반환값 : any[]

```


### 그리드 조작하기
- 행 추가 또는 삽입
```javascript
// 기존의 행과 행 사이에 새로운 행 삽입 옵션
gridView.editOptions.insertable = true;

// 마지막에 새로운 행 추가 옵션
gridView.editOptions.appendable = true;

// 한 번에 옵션 설정
gridView.setEditOptions({
  insertable: true,
  appendable: true
});

gridView.beginInsertRow(itemIndex);
gridView.beginAppendRow();
```
- 행 삭제
```javascript
// 삭제 가능 옵션
gridView.setEditOptions({
  deletable: true
});

dataProvider.setOptions({
  // 즉시 삭제가 아닌 행 상태 삭제로 변경
  softDeleting: true,   
  // RowState.CREATE인 행을 삭제 
  deleteCreated: true,  
  // RowState.DELETED, RowState.CREATE_AND_DELETED
  // 인 행을 그리드에서 제외
  hideDeletedRows: true,    
});

gridView.deleteSelection(true);
dataProvider.removeRow(2);
```

### 그리드 이벤트 및 펑션
```javascript
//체크박스 클릭
gridView.onItemChecked = function (grid, itemIndex, checked) {
}

// 셀 클릭
gridView.onCellClicked = function (grid, clickData) {
}

// 셀 수정
gridView.onCellEdited = function (grid, itemIndex, row, field) {
}
```

[출처:리얼그리드2 공식문서](https://docs.realgrid.com/start/overview)