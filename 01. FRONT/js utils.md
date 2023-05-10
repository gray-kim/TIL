# JS Utils


### SNAKE_CASE to camelCase in ES6
다음은 ES6에서 대문자 snake_case를 camelCase로 변환하는 함수입니다.

```javascript
function snakeToCamel(str) {
    return str.toLowerCase().replace(/_([a-z])/g, function(m, w) {
        return w.toUpperCase();
    });
}
```

예를 들어, "FIRST_NAME"을 인자로 넘기면 "firstName"이 반환됩니다. 함수를 사용할 때는 다음과 같이 호출할 수 있습니다.

```javascript
const myVar = "FIRST_NAME";
const camelCaseVar = snakeToCamel(myVar);
console.log(camelCaseVar); // "firstName"
```