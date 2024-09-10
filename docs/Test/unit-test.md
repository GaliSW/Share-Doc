# 單元測試

> *Unit tests are typically automated tests written and run by software developers to ensure that a section of an application(known as the “unit”) **meets its design and behaves as intended**.*
> 
> 
> 單元測試是由軟體開發人員開發和執行的自動化測試，目的是確保一段特定程式碼（稱為「單元」）**達到預期的設計目標並表現出正確的行為。**
> 

> *A unit test is an automated piece of code that invokes the unit of work being tested, and then checks some assumptions about a single end result of that unit. A unit test is almost always **written using a unit testing framework**. It can be written easily and **runs quickly**. It’s **trust-worthy, readable, and maintainable**. It’s **consistent in its results** as long as production code hasn’t changed.*
> 
> 
> 單元測試是一段能自動執行的程式碼，這段程式碼會去呼叫一個工作單元並確認該工作單元最終的單一結果符合一些假設。單元測試幾乎總是依循單元測試框架所撰寫出來。它能夠被輕易地撰寫並且快速執行。它值得信任、可讀而且容易維護。此外只要程式碼沒有更改，單元測試的運行結果必定總是保持一至。
> 



### 基本範例

```jsx
const addNumbers = (a,b) => a + b;
```

Case: 輸入1,2 預期得到3的結果。

```jsx
//import addNumber.js

describe('addNumbers'()=>{
	it('should be return 3 when 1 + 2',() => {
		expect(addNumber(1,2)).toBe(3);
	})
})
```

## **單元測試的基本結構**

一個單元測試通常由三個主要部分組成，這種模式稱為 **AAA**：

- **Arrange（準備）：** 設置測試環境，準備所需的輸入、數據或模擬對象。
- **Act（執行）：** 執行待測單元的操作（如調用函數或渲染元件）。
- **Assert（斷言）：** 驗證輸出是否符合預期的結果。

## **單元測試的常用工具**

在 JavaScript/TypeScript 中進行單元測試時，常見的測試框架和工具包括：

- **Jest：** 最流行的 JavaScript 測試框架之一，具有內建的測試運行器和斷言庫，特別適合 React 應用。
- **React Testing Library（RTL）：** 與 Jest 搭配使用的庫，用來測試 React 元件。它側重於模擬用戶行為，而不是檢查實際的實現細節。
- **Vitest：** 和 Jest 類似的測試框架，針對 Vite 項目進行了優化，提供更快的測試速度。

## **常用測試技術與概念**

- **斷言（Assertion）：** 斷言是測試的一部分，用來確認待測單元的行為是否符合預期。通常使用如 `expect` 或 `assert` 函數。
    - 例如：`expect(sum(1, 2)).toBe(3);`
- **測試替身/替代（Test Doubles）：**
    - **模擬（Mock）：** 模擬某些行為的對象，常用於 API 請求或外部依賴。
    - **間諜（Spy）：** 用來監控函數的調用次數、參數等細節。
    - **假對象（Stub）：** 是對象的預設實現，能夠返回特定結果。
- **快照測試（Snapshot Testing）：** 通常用來驗證 UI 元件是否保持穩定，當元件渲染時會保存一個“快照”，以後每次測試都將當前的渲染結果與快照比較。
- **隔離測試（Isolation）：** 單元測試應盡量避免與外部依賴耦合（如數據庫、網絡請求），測試應專注於待測單元的內部邏輯。

## 單元測試案例 (React & Jest)

### 1.渲染 ⇒

測試確保 `MyComponent` 元件渲染了正確的內容。

```jsx
//MyComponent.jsx
import React from 'react';

const MyComponent = () => {
  return <div>Hello, World!</div>;
};

export default MyComponent;
```

```jsx
//MyComponent.test.jsx
import { render, screen } from '@testing-library/react';
import MyComponent from './MyComponent';

it('renders the correct text', () => {
  render(<MyComponent />);
  const textElement = screen.getByText(/Hello, World!/i);
  expect(textElement).toBeInTheDocument();
});
```

### 2.按鈕點擊⇒

測試確保點擊按鈕後，計數器會正常更新。

```jsx
//ButtonComponent.jsx
import React, { useState } from 'react';

const ButtonComponent = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <p>You clicked {count} times</p>
    </div>
  );
};

export default ButtonComponent;
```

```jsx
//ButtonComponent.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import ButtonComponent from './ButtonComponent';

it('button click updates the count', () => {
  render(<ButtonComponent />);
  const button = screen.getByText(/Click me/i);
  
  //fireEvent 是 trigger 事件的方法
  fireEvent.click(button);
  fireEvent.click(button);
  
  const countText = screen.getByText(/You clicked 2 times/i);
  expect(countText).toBeInTheDocument();
});
```

### 3.具有 props 的元件⇒

確保傳遞正確的 `props` 時，元件渲染了預期的輸出。

```jsx
//Greeting.js
import React from 'react';

const Greeting = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};

export default Greeting;
```

```jsx
//Greeting.test.js
import { render, screen } from '@testing-library/react';
import Greeting from './Greeting';

it('renders with correct name prop', () => {
  render(<Greeting name="John" />);
  const greetingText = screen.getByText(/Hello, John!/i);
  expect(greetingText).toBeInTheDocument();
});
```

### 4.Mock API 呼叫⇒

模擬了 API 呼叫並確保資料被正確渲染。

```jsx
//FetchData.jsx
import React, { useEffect, useState } from 'react';

const FetchData = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/todos/1')
      .then((response) => response.json())
      .then((json) => setData(json));
  }, []);

  return <div>{data ? data.title : 'Loading...'}</div>;
};

export default FetchData;
```

```jsx
//FetchData.test.jsx
import { render, screen, waitFor } from '@testing-library/react';
import FetchData from './FetchData';

// 模擬全局 fetch 方法
global.fetch = jest.fn(() =>
  Promise.resolve({
    json: () => Promise.resolve({ title: 'Test Todo' }),
  })
);

it('fetches and displays data', async () => {
  render(<FetchData />);
  
  // 等待資料被加載並更新畫面
  const dataText = await waitFor(() => screen.getByText(/Test Todo/i));
  expect(dataText).toBeInTheDocument();
});
```

### 5.表單輸入的處理⇒

測試確保表單輸入的值被正確更新並顯示在頁面上。

```jsx
//FormComponent.jsx
import React, { useState } from 'react';

const FormComponent = () => {
  const [value, setValue] = useState('');

  return (
    <form>
      <label htmlFor="input">Enter text:</label>
      <input
        id="input"
        type="text"
        value={value}
        onChange={(e) => setValue(e.target.value)}
      />
      <p>You entered: {value}</p>
    </form>
  );
};

export default FormComponent;
```

```jsx
//FormComponent.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import FormComponent from './FormComponent';

it('updates input field value correctly', () => {
  render(<FormComponent />);
  
  const input = screen.getByLabelText(/enter text/i);
  fireEvent.change(input, { target: { value: 'Hello' } });
  
  const displayedValue = screen.getByText(/You entered: Hello/i);
  expect(displayedValue).toBeInTheDocument();
});
```

### 6.條件渲染⇒

確保當按鈕被點擊時，條件文本的顯示與隱藏能夠正常運作。

```jsx
//ConditionalComponent.jsx
import React, { useState } from 'react';

const ConditionalComponent = () => {
  const [showText, setShowText] = useState(false);

  return (
    <div>
      <button onClick={() => setShowText(!showText)}>
        Toggle Text
      </button>
      {showText && <p>This is conditional text</p>}
    </div>
  );
};

export default ConditionalComponent;
```

```jsx
//ConditionalComponent.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import ConditionalComponent from './ConditionalComponent';

it('toggles text on button click', () => {
  render(<ConditionalComponent />);
  
  const button = screen.getByText(/Toggle Text/i);
  fireEvent.click(button);

  const text = screen.getByText(/This is conditional text/i);
  expect(text).toBeInTheDocument();
  
  fireEvent.click(button);
  expect(screen.queryByText(/This is conditional text/i)).toBeNull();
});
```

### 7.異常處理與錯誤邊界⇒

確保當觸發錯誤時，錯誤訊息會正確顯示。

```jsx
// ErrorComponent.jsx
import React, { useState } from 'react';

const ErrorComponent = () => {
  const [hasError, setHasError] = useState(false);

  const throwError = () => {
    try {
      throw new Error('An unexpected error occurred!');
    } catch (error) {
      setHasError(true);
    }
  };

  return (
    <div>
      <button onClick={throwError}>Trigger Error</button>
      {hasError && <p>Error: Something went wrong.</p>}
    </div>
  );
};

export default ErrorComponent;
```

```jsx
//ErrorComponent.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import ErrorComponent from './ErrorComponent';

it('displays error message on error', () => {
  render(<ErrorComponent />);
  
  const button = screen.getByText(/Trigger Error/i);
  fireEvent.click(button);
  
  const errorMessage = screen.getByText(/Error: Something went wrong./i);
  expect(errorMessage).toBeInTheDocument();
});
```

### 8.元件的生命週期⇒

測試元件在掛載或卸載時的行為，特別是當你使用 `useEffect` 等 hook 時。

模擬元件的掛載與卸載過程，並檢查訊息是否按預期顯示。

```jsx
//LifecycleComponent.jsx
import React, { useEffect, useState } from 'react';

const LifecycleComponent = () => {
  const [message, setMessage] = useState('Initializing...');

  useEffect(() => {
    const timer = setTimeout(() => {
      setMessage('Component Mounted');
    }, 1000);

    return () => {
      setMessage('Component Unmounted');
    };
  }, []);

  return <div>{message}</div>;
};

export default LifecycleComponent;
```

```jsx
//LifecycleComponent.test.jsx
import { render, screen, act } from '@testing-library/react';
import LifecycleComponent from './LifecycleComponent';

it('updates message after mount and unmount', () => {
  jest.useFakeTimers();
  
  const { unmount } = render(<LifecycleComponent />);
  
  // 測試掛載後的更新
  expect(screen.getByText(/Initializing.../i)).toBeInTheDocument();
  
  // Fast-forward until all timers have been executed
  act(() => {
    jest.advanceTimersByTime(1000);
  });
  
  expect(screen.getByText(/Component Mounted/i)).toBeInTheDocument();
  
  // 測試卸載時的更新
  unmount();
  expect(screen.queryByText(/Component Unmounted/i)).toBeNull();
});
```

### 9.context 的使用⇒

確保 context 中的使用者名稱能夠正確顯示。

```jsx
//UserContext.jsx
import React, { createContext, useContext } from "react";

const UserContext = createContext();

export const UserProvider = ({ children }) => {
  const user = { name: "John" };
  return <UserContext.Provider value={user}>{children}</UserContext.Provider>;
};

export const useUser = () => {
  return useContext(UserContext);
};
```

```jsx
//UserProfile.jsx
import React from "react";
import { useUser } from "./UserContext";

const UserProfile = () => {
  const user = useUser();

  return <div>User: {user.name}</div>;
};

export default UserProfile;
```

```jsx
//UserProfile.test.jsx
import { render, screen } from "@testing-library/react";
import { UserProvider } from "./UserContext";
import UserProfile from "./UserProfile";

it("displays user name from context", () => {
  render(
    <UserProvider>
      <UserProfile />
    </UserProvider>
  );

  const userName = screen.getByText(/User: John/i);
  expect(userName).toBeInTheDocument();
});
```

### 10.元件的樣式變化⇒

生成一個元件的快照，當元件未來的外觀變更時，測試將檢測是否與快照相匹配。

```jsx
//SnapshotComponent.jsx
import React from "react";

const SnapshotComponent = () => {
  return <button className="btn-primary">Snapshot Button</button>;
};

export default SnapshotComponent;
```

```jsx
//SnapshotComponent.test.jsx
import { render } from "@testing-library/react";
import SnapshotComponent from "./SnapshotComponent";

it("matches snapshot", () => {
  const { asFragment } = render(<SnapshotComponent />);
  expect(asFragment()).toMatchSnapshot();
});
```

## **測試最佳實踐**

- **保持測試簡單明確：** 測試的重點是驗證某個單元的預期行為，因此測試應該簡單且易於理解。每個測試應聚焦於一個特定的功能或場景。
- **獨立性：** 單元測試應該是獨立的，不依賴其他測試或外部系統，這樣測試結果才不會受到其他因素影響。
- **模擬外部依賴：** 測試不應該依賴外部 API、數據庫或其他外部系統，這些依賴應該被模擬或偽造。
- **保持可讀性：** 測試代碼應保持簡潔、易讀，盡量避免過度復雜的邏輯。好的測試代碼也是一種文檔，能幫助開發者理解代碼行為。