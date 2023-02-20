## MSW (Mock Service Worker) 라이브러리
- **기능**
   - 백엔드 API 모킹
   - 테스트용 API 서버

---
- **설치순서**
1. yarn add msw --dev
2. npx msw init public/ --save
---
3. mkdir src/mocks
4. mkdir src/mocks/api
5. mkdir src/mocks/api/data
6. touch src/mocks/workers.ts
7. touch src/mocks/api/data/testData.ts (이름자유)
4. touch src/mocks/handlers.ts

<img src='uploads/a2046a27be30ece528734379e0fab30f/image.png' width='200px' />

- **여기부터 무조건 적어줘야 하는 코드 셋팅**<br>
**/src/mocks/worker.ts**
```
import { setupWorker } from "msw";
import handlers from "./api/handlers";

export const worker = setupWorker(...handlers);
```

**/src/index.ts**
```
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";

import App from "./App";
import { worker } from "./mocks/worker";
if (process.env.NODE_ENV === "development") {
  worker.start();
}

const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```
---
- **사용**

**/src/mocks/api/data/testData.ts**
```
export const testData = {
  email: 'test@gmail.com'
}
```

**/src/mocks/handlers.ts**
```
import { rest } from 'msw';
import { testData } from './data/testData';

const handlers = [
  rest.get('/post/api/users', (req, res, ctx) => {
    return res(ctx.status(200), ctx.json(testData));
  }),
  rest.post('post/api/register', (req, res, ctx) => {
    return res(ctx.status(200), ctx.json(res));
  }),
];

export default handlers;
```
---
- **에러**<br>
mockServiceWorker.js 에러시
<img src='uploads/4d2255212a3d84430cd57652846d1ca7/image.png' width='200px'/>

**.eslintrc.json** 코드추가하세요

<img src='uploads/7fd240531af0197890802404069c8f1f/image.png' width='400px' />
<br />

## 더 자세히
- https://www.daleseo.com/mock-service-worker/
- https://fe-developers.kakaoent.com/2022/220825-msw-integration-testing/
