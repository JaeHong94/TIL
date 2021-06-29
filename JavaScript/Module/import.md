# import

import는 다른 모듈에서 내보낸 바인딩을 가져올 때 사용합니다.

## 구문

```js
import defaultExport from "module-name";
import * as name from "module-name";
import { export1 } from "module-name";
```

defaultExport: 모듈에서 가져온 기본 내보내기를 가리킬 이름
module-name: 가져올 대상 모듈, 보통, 모듈을 담은 .js 파일의 절대 또는 상대 경로를 작성
name: 가져온 대상에 접근할 때 일종의 이름공간으로 사용할, 모듈 객체의 이름.
exortN: 내보낸 대상 중 가져올 것의 이름.
aliasN: 가져온 유명 내보내기를 가리킬 이름.
