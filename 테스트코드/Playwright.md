## E2E 테스트

E2E는 End to End 말 그대로 애플리케이션 흐름이 예상대로 동작하는 지 확인하기 위해 처음부터 끝까지 테스트 하는 기술이다. 사용자 시나리오를 시뮬레이션하여 최종 사용작 경험에서 테스트 하는 것이다.

### Playwright

크로미움 기반의 브라우저, 파이어폭스, 사파리 같은 WebKit 기반 브라우저를 지원한다. 모바일도 마찬가지.

Playwright는 동시에 수행할 수 있는 테스트가 병렬로 실행된다. 따라서 테스트파일(spec파일)의 갯수가 증가해도 다른 테스트 도구보다 빠르다.

```jsx
import { test, expect } from '@playwright/test';

const url = 'https://www.test.com/';

test.beforeEach(async ({ page}) => {
    await page.goto(url);
});

test('test단계', async({ page }) ={
    await page.click('.header');
    await page.keyboard.type('단계1');
    // ...

    const list = page.locator('');
    // await...
});
```

await을 사용하여 비동기를 제어해 실행순서가 보장된다. 또한 hover와 drag 이벤트를 공신 API로 제공해 쉽게 테스트도 가능하다.

-> 문서 https://playwright.dev/

-> 설치방법:

```bash
npm init playwright@latest
```

설치하면서 TS, JS 선택 및 깃허브 Action workflow도 추가 가능하다.

설치 완료 되면, example.spec.ts가 추가 된다. tests 디렉토릴 아래에 있는 것만 실행되므로 주의..

- 디렉토리에 있는 모든 테스트 실행 :

```bash
  npx playwright test
```

- 단일 테스트 파일 실행:

```bash
npx playwright test file-name
```

VSCode 플로그인으로 테스트도 가능!
-> Playwright Test for VSCode

문법은 이어서...
