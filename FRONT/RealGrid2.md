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

- edit 옵션 설정
- 수정 가능/불가능 컬럼 섞여있는 경우 전체 옵션은 true, 각 각의 컬럼에 false값 주어야 함
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

### Footer 설정
```javascript
// Footer 설정
gridView.setFooters({visible: true});

// 1. 컬럼 설정시 합계 식 지정
gridView.setColumns(
  [{
        header: {text: '상품수량'}, 
        name: 'productQty', 
        fieldName: 'productQty',
        type: 'data',
        width: '70',
        footer: {
          text: "합계",
          expression: "sum",
        },        
    }]);

// 2. javascript에서 합계 처리 후 컬럼속성 설정
var strFooter = "합계: "+sumVal;
gridView.setColumnProperty(
  columnNm, 
  "footer", 
  {
    text:strFooter, 
    styleName:"ud-text-bold"
  }
);
```

### 기타 옵션 설정
```javascript
// 체크박스 표시
gridView.setCheckBar({visible: true});

// 체크박스 전체선택 표시
gridView.setCheckBar({showAll: true});

// 체크박스 단일 선택
gridView.checkBar.exclusive = true;
```


[출처:리얼그리드2 공식문서](https://docs.realgrid.com/start/overview)


### 참고자료
```javascript
// 셀클릭 이벤트
gridView.onCellClicked = function (grid, clickData) {
    var objRow = dataProvider.getJsonRow(clickData.dataRow);    // clickData.itemIndex;
    var add_inv_create_gbn = "N";
    
    if(clickData.column == "ADD_INVOICE_VIEW" && objRow.ADD_INVOICE_VIEW != "") {
        if (objRow["DELI_PRGS_STAT_CD"] == "1050" || objRow["DELI_PRGS_STAT_CD"] == "1060") {
            add_inv_create_gbn = "Y"
        }
        fnAddInvoiceViewPop({deli_no : objRow.DELI_NO, deli_seq : objRow.DELI_SEQ, add_inv_create_gbn : add_inv_create_gbn});                
    }
}

// 로우 스타일 콜백
gridView.setRowStyleCallback(function(grid, item, fixed) {
    var row = gridView.getValues(item.index);
    var rtn = "";
    if (row["REAL_QTY"] <= "0") {
        rtn = "ud-btnRedColor";
    }
    return rtn
});

// 셀 스타일 콜백(row, cell의 값 비교해서 스타일 지정하는 방법)
gridView.setCellStyleCallback(function(grid, dataCell) {
    var rtn = {};
    var invoiceChk = dataCell.index.column.fieldName == "INVOICE_CHK";
    var invoiceNo = dataCell.index.column.fieldName == "INVOICE_NO";
    var invoiceValue = dataProvider.getValue(dataCell.index.dataRow, "INVOICE_NO");
    
    if(invoiceNo && dataCell.value == $("#invoice_no").val()){
        rtn.styleName = 'ud-greenColor';
    }else if(invoiceChk){
        if(invoiceValue == $("#invoice_no").val()){
            rtn.styleName = 'ud-greenColor';
        }
    }
    
    return rtn;
});	

// 체크박스 클릭 이벤트
gridView.onItemChecked = function (grid, itemIndex, checked) {
    var startRow = 0;
    var endRow = -1;
    var selectRow = dataProvider.getJsonRow(itemIndex);
    var gridRowData = dataProvider.getJsonRows(startRow, endRow);

    $.each(gridRowData, function(idx, row) {
        // 선택한 행과 같은 DELI_NO가진 행 체크
        if (row["DELI_NO"] == selectRow["DELI_NO"]) {
            gridView.checkRow(idx, checked);
        }
    });
};

// 체크하기
// 한 행씩 체크할 수 있는 API: checkItem(), checkRow()
// 여러 행을 체크할 수 있는 API: checkItems(), checkRows()
// 전체 행을 체크할 수 있는 API: checkAll()
gridView.checkItem(0, true);
gridView.checkRow(0, true);
gridView.checkItems([1, 2], true);
gridView.checkRows([1, 2], true);
gridView.checkAll();

// 행 상태 변경
var curr = gridView.getCurrent();
dataProvider.setRowState(curr.dataRow, "updated", true);

var dataRows = gridView.getCheckedRows();
dataProvider.setRowStates(dataRows, "updated", true);

// 행 이동(현재index, 이동할index)
dataProvider.moveRow(2, 5);


```
