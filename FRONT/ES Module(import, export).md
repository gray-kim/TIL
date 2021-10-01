# ES Module (import, export)

## named exports
- 많은 것들을 import, export 하고 싶을 때 사용

```js
// math.js
// function 들을 각각 export
export const plus = (a, b) => a+b;
export const minus = (a, b) => a-b;
export const divide = (a, b) => a/b;

// 증괄호 안에 사용할 모듈을 선언해야 함. export 시의 이름과 동일해야 함
import {plus,minus,divide} from "./math";

// as 를 붙여 이름 변경 가능
import {plus as add} from "./math";
```


## default export
- 각 파일마다 단 한개의 default export만 존재할 수 있음
- 한개의 파일에서 모든걸 export하고 모든걸 import 하면 됨

```js
// math.js
// function 들을 export default로 한 번에 export
const plus = (a, b) => a+b;
const minus = (a, b) => a-b;
const divide = (a, b) => a/b;
export default {plus, minus, divide}


// 해당 파일로부터 전부 import
import math from "./math";

// default export는 원하는 이름으로 import 가능
import myMath from "./math";
myMath.plus(2,2);
```

```js
// db.js
// 하나의 function만 export 하고 싶은 경우 해당 function을 export default 하면 됨
const connectToDB = () => {/*magic*/};
export const getUrl = () => {/*magic*/};
export default connectToDB; 

// 그럼 다른 이름을 써서 import 가능
import connect from "./db"

// named와 default 혼용해서도 사용 가능
import connect, {getUrl} from "./db"
```

## * import
- 모든 exported 된 내용을 import 하고 싶을 때 사용

```js
// math.js
// function 들을 각각 export
export const plus = (a, b) => a+b;
export const minus = (a, b) => a-b;
export const divide = (a, b) => a/b;

// * as 로 import 해서 사용
import * as myMath from "./math";
myMath.plus(2,2);
```

## dynamic import
- 사용자가 사용할 module만 import 하는 방식

```js
function doMath(} {
    import("./math")
    .then(math => math.plus(2,2));
)

// async, await 적용
async function doMath(} {
    const math = await import("./math");
    math.plus(2,2);
)

btn.addEventListener("click", doMath);
```