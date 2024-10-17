# 策略與實踐

前端測試種類 :

- Unit test
- Integration test
- E2E test

### **Unit test (單元測試)**

單元測試指的是對【**最小單位的測試**】或【**獨立測試特定的程式碼片段**】，主要驗證 function、method 或 class instance 的輸入與輸出是否符合預期。

優點:

- **提高代碼質量**：
    
    單元測試幫助開發者確認每個模塊、函數或組件在不同情境下是否正確運作。這有助於早期發現潛在的 bug，從而提高整體代碼質量。
    
- **減少回歸問題**：
    
    當代碼有變動或重構時，單元測試可以快速確認新代碼是否影響了現有功能，有效減少回歸問題的出現。
    
- **提高開發信心**：
    
    當所有測試通過時，開發者對於功能是否正確運作會有更多的信心，特別是在面對複雜系統時。
    
- **促進代碼設計**：
    
    單元測試推動開發者思考模塊化設計與解耦，這不僅促進了可測試性，還提升了代碼的可維護性和可重用性。
    
- **加快開發過程**：
    
    雖然撰寫測試可能耗時，但長期來看，當測試覆蓋充分時，測試能快速驗證代碼，避免進行大量的手動測試，從而加快開發進度。
    

缺點:

- **無法捕捉整合問題**：
    
    單元測試專注於測試單一模塊或函數，但它無法測試模塊之間的整合與交互。這意味著一些跨模塊的問題或邊界情況可能會被漏掉。
    

---

### **Integration test (整合測試)**

整合測試或稱為**功能測試 (functional testing)** 是針對【合併的程式碼片段】進行測試，包含整合元件、套件、library 及從 API 取得資料後的呈現。

優點

1. **檢驗模塊間的交互**：
    
    整合測試能測試模塊之間的交互是否如預期運行。這對於檢查 API 調用、數據流、事件觸發等非常重要，特別是在前端與後端、第三方服務之間的交互中。
    
2. **捕捉邊界情況**：
    
    單個模塊在單元測試中可能表現正常，但整合測試能幫助揭露邊界情況下模塊之間不兼容或數據格式錯誤等問題。
    
3. **真實場景模擬**：
    
    整合測試能更接近真實的使用場景，測試模塊之間如何配合處理複雜的用戶行為。這能幫助捕捉一些單元測試無法發現的問題，特別是在前端應用中，常涉及多個組件的互動。
    
4. **提升系統穩定性**：
    
    通過整合測試，可以及早發現潛在的系統整合問題，這有助於提高整體系統的穩定性，減少未來的修復成本。
    
5. **幫助發現外部依賴問題**：
    
    在整合測試中，外部依賴如數據庫、第三方 API、服務器之間的問題能被及時捕捉，這是單元測試無法涵蓋的部分。
    

缺點

1. **運行速度較慢**：
    
    整合測試通常需要較多的資源，運行速度比單元測試慢得多，特別是在涉及外部服務、網絡請求或數據庫交互時，這可能會拖慢測試週期。
    
2. **難以診斷問題來源**：
    
    當整合測試失敗時，確定具體是哪個模塊導致錯誤可能比較困難。調試整合測試中的問題通常比單元測試更耗時，因為它涉及多個模塊或系統的聯合作用。
    
3. **維護成本高**：
    
    由於模塊間依賴較多，整合測試的維護成本通常較高。每當某個模塊或外部依賴改變時，整合測試可能需要隨之更新，並重新配置測試環境。
    
4. **易受環境影響**：
    
    整合測試通常需要依賴某些外部資源或環境，因此很容易受限於環境配置問題。例如，如果測試環境中某個外部服務不可用，整合測試可能會失敗，這不一定意味著代碼有錯。
    

---

### E2E test (端對端測試)

E2E 是測試系統從頭到尾的完整流程，從**用戶交互的角度模擬實際操作**，確保整個系統各個部分都能協同工作

常見的 E2E 測試工具包括：

- **Cypress**：簡單易用，界面友好，適合前端測試。
- **Playwright**：多瀏覽器支持且性能卓越，適合需要跨瀏覽器測試的項目。
- **Puppeteer**：基於 Chrome/Chromium 瀏覽器，對 Google 環境友好。

優點

1. **覆蓋整體流程**：
    
    E2E 測試覆蓋系統的所有層次，從用戶界面到後端數據庫，確保每個部分都正常工作。這類測試能確保系統在用戶視角下的整體功能，如網頁是否加載正常、按鈕點擊是否有效、數據是否正確保存等。
    
2. **模擬真實用戶行為**：
    
    E2E 測試能有效模擬用戶的實際操作行為，測試整個系統是否能處理複雜的使用場景，讓開發者對最終用戶的體驗有更多信心。
    
3. **減少手動測試**：
    
    通過自動化 E2E 測試，可以減少手動測試的需求，特別是針對跨頁面或跨模塊的流程測試。這可以加快測試的迭代，尤其是在頻繁部署的情況下。
    
4. **檢測跨模塊問題**：
    
    E2E 測試能捕捉到跨模塊或系統邊界的問題，這些是單元測試或整合測試無法覆蓋的部分。例如，前端與後端之間的接口調用是否正確，數據是否被正確傳遞和顯示。
    
5. **提高用戶體驗可靠性**：
    
    通過 E2E 測試來檢驗完整的用戶流程，能有效防止因為某些小問題而導致用戶體驗下降，從而提高整個產品的可靠性和質量。
    

缺點

1. **編寫與維護成本高**：
    
    E2E 測試的編寫和維護成本通常很高，特別是針對複雜的系統。每當系統或用戶界面有變動時，E2E 測試可能需要重新調整。這使得在需求經常變化的情況下，維護 E2E 測試變得非常耗時。
    
2. **運行速度慢**：
    
    由於 E2E 測試涵蓋了整個系統的流程，並涉及模擬用戶操作、數據處理和界面顯示，測試的運行速度通常很慢。當 E2E 測試集變大時，測試週期可能變得更長，影響開發和部署的效率。
    
3. **容易受環境影響**：
    
    由於 E2E 測試通常在接近真實的環境中運行，它容易受限於測試環境的不穩定性。例如，測試依賴的後端服務如果不可用或延遲較大，會導致測試失敗，即使這些問題並非由前端或業務邏輯引起。
    
4. **調試困難**：
    
    當 E2E 測試失敗時，確定問題的來源可能較為困難。由於 E2E 測試覆蓋的範圍廣，錯誤可能來自前端、後端、網絡請求或其他系統交互的某個部分，這使得調試問題變得更加複雜和耗時。
    
5. **容易導致過度測試**：
    
    E2E 測試通常比單元測試或整合測試更難編寫並且更耗資源，如果測試範圍過大，可能導致開發團隊過度依賴 E2E 測試來捕捉所有錯誤，而忽略了其他層次的測試，這反而可能降低測試的效率。
    

---

### 測試策略

1. **金字塔測試模型**：
    
    ![image.png](@site/src/image/test/test-pyramid.png)
    
    測試策略通常可以按照「測試金字塔」模型來設計：
    
    - **單元測試**處於基礎層，數量最多，運行速度最快。
    - **整合測試**位於中間，數量次之，主要測試模塊之間的交互。
    - **E2E 測試**在頂層，數量最少，涵蓋用戶的整體流程，確保核心功能正常。
    
    這種策略建議大部分測試集中在單元測試，因為它們輕量、高效，並且能及早捕捉錯誤。整合測試用來檢查模塊之間的交互，而 E2E 測試則保證關鍵的業務流程不被破壞。
    
2. **確保測試的實用性與覆蓋率**：
    
    不同類型的測試有不同的覆蓋重點。單元測試應當關注**邏輯和邊界情況**的覆蓋，整合測試則應針對**系統邊界和數據流**，而 E2E 測試應聚焦在核心**用戶流程**上。你不必追求 100% 的測試覆蓋率，而是確保測試覆蓋了項目中的關鍵功能和可能出現問題的地方。
    
3. **選擇適合的測試工具**：
    - **單元測試**：使用輕量、快速的測試框架，如 Jest 或 Vitest。
    - **整合測試**：選擇能處理多模塊交互的工具，如 Jest 或 Vitest 或 Mocha + Chai，結合 Sinon 進行模擬。
    - **E2E 測試**：使用專門的 E2E 測試工具，如 Cypress 或 Playwright，這些工具具有友好的 API 並支持瀏覽器自動化。
    

---

### 實踐

1. **TDD（測試驅動開發）實踐**：
    
    在可能的情況下，考慮採用測試驅動開發（Test-Driven Development, TDD），這種方法先撰寫測試，然後再實現功能。這不僅有助於撰寫可測試的代碼，還能促進良好的代碼設計。
    
2. **單元測試優先**：
    
    單元測試應該是你測試策略的基石，因為它們執行快速且容易維護。在每次提交代碼或重構時，單元測試能幫助你快速捕捉錯誤並提供信心。優先編寫對業務邏輯、數據處理、狀態管理等部分的單元測試，並確保涵蓋各種邊界條件。
    
3. **針對關鍵功能的整合測試**：
    
    整合測試應著重於系統的關鍵邊界和模塊之間的數據傳遞，比如前端和 API 的交互、表單提交與數據驗證等。整合測試不需要測試所有的細節，而應關注於模塊之間的協同是否正常。
    
4. **聚焦關鍵流程的 E2E 測試**：
    
    由於 E2E 測試的成本較高，應將其重點放在測試應用中的核心業務流程上，比如用戶登錄、購物車結算、數據提交等。這些流程直接影響用戶體驗，確保它們運行順暢至關重要。此外，避免用 E2E 測試覆蓋過多的細節，專注於用戶操作的主要路徑。
    
5. **持續集成與自動化測試**：
    
    結合持續集成（CI）工具（如 GitHub Actions、GitLab CI 或 Jenkins）進行自動化測試，確保每次代碼提交都能觸發測試套件自動執行。這樣能在代碼合併到主分支前，及時發現問題，保持系統穩定。
    
6. **靈活性與適當的測試顆粒度**：
    
    在編寫測試時，要考慮到未來的變更和擴展，避免過於緊耦合。測試不應過度依賴具體實現細節，而應關注功能需求的實現。這樣可以在重構或功能變更時減少測試的破壞性失敗。
    

---

### 測試命名規則

主流為 **Give-When-Then** 與 **it should** 這兩種模式。

- **Give-When-Then**
    - **Give (給訂)**：描述測試的初始條件或前提。這部分通常設定了測試所需的環境或狀態。例如，設置用戶資料、初始化應用狀態等。
        
        **例子**: `Give a user with a valid email address`
        
    - **When (當)**：描述執行的操作或事件。這部分說明在給定條件下，系統應該執行什麼操作或用戶應該執行什麼動作。
        
        **例子**: `When the user tries to log in`
        
    - **Then (那麼)**：描述預期的結果或系統應該如何響應。這部分定義了操作後的期望結果或系統行為。
        
        **例子**: `Then the user should be redirected to the dashboard`
        
        **範例**：
        
        - **Give**: A user with a valid email address and password.
        - **When**: The user tries to log in with correct credentials.
        - **Then**: The user should be redirected to their dashboard.

- **It should**
    - **Test Scenario**: 用戶嘗試用正確的憑證登錄
    - **Test Description**:
        - `It should redirect the user to the dashboard`

**範例**：

```jsx
describe('User Login', () => {
  it('should redirect the user to the dashboard when provided with valid credentials', () => {
    // assert
  });
});
```