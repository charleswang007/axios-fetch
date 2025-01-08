# axios-fetch

### Axios 和 Fetch 的不同之處（繁體中文）

在前端開發中，**Axios** 和 **Fetch** 是兩個常見的工具，用於執行 HTTP 請求。以下是它們的主要差異，整理成繁體中文，方便理解。

---

### **1. 內建功能**
#### **Axios**
- **JSON 自動處理**：自動將請求的數據轉換為 JSON 格式，並解析伺服器回應。
- **超時設置**：提供內建的超時功能，輕鬆設置請求時限。
- **取消請求**：內建支援取消請求的功能。
- **進度監控**：支援上傳與下載進度的追蹤。
- **錯誤處理更直觀**：所有的錯誤（包括 HTTP 狀態碼錯誤）都會直接進入 `catch` 區塊。

#### **Fetch**
- **內建於瀏覽器**：不需要額外安裝，直接可用於現代瀏覽器。
- **JSON 處理需手動**：必須手動解析伺服器回應中的 JSON（`response.json()`）。
- **缺少內建功能**：
  - 無內建超時設置。
  - 錯誤僅針對網絡問題，HTTP 狀態錯誤（如 404）需手動檢查。
  - 取消請求需要結合 `AbortController`，相對繁瑣。

---

### **2. API 設計與用法**
#### **Axios**
- **簡化的語法**：
  - 支援快捷方法，如 `axios.get()` 和 `axios.post()`。
  - 支援直接在配置中設置參數、標頭等。
- **自動序列化參數**：例如，`params` 會自動轉換為查詢字符串。

範例：
```javascript
axios.get('/api/data', {
  params: { id: 123 },
  headers: { Authorization: 'Bearer token' }
}).then(response => console.log(response.data));
```

#### **Fetch**
- **語法較基礎**：需要手動設置請求配置。
- **查詢參數需手動拼接**：GET 請求的參數需要手動加到 URL。

範例：
```javascript
fetch('/api/data?id=123', {
  method: 'GET',
  headers: { Authorization: 'Bearer token' }
}).then(response => response.json())
  .then(data => console.log(data));
```

---

### **3. 錯誤處理**
#### **Axios**
- **捕捉所有錯誤**：包括 HTTP 錯誤，所有錯誤均進入 `catch`。
```javascript
axios.get('/api/data')
  .then(response => console.log(response.data))
  .catch(error => console.error('Error:', error));
```

#### **Fetch**
- **需手動檢查 HTTP 錯誤**：Fetch 只會對網絡層面的錯誤拋出異常，HTTP 狀態錯誤需要額外處理。
```javascript
fetch('/api/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('HTTP error ' + response.status);
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

---

### **4. 取消請求**
#### **Axios**
- **內建支持取消請求**：通過 `CancelToken` 直接實現。
```javascript
const source = axios.CancelToken.source();
axios.get('/api/data', { cancelToken: source.token });
source.cancel('Request canceled');
```

#### **Fetch**
- **需要 `AbortController`**：相對較繁瑣。
```javascript
const controller = new AbortController();
fetch('/api/data', { signal: controller.signal });
controller.abort();
```

---

### **5. 瀏覽器支援與兼容性**
#### **Axios**
- **廣泛兼容性**：
  - 支援瀏覽器與 Node.js 環境。
  - 兼容老版本瀏覽器（如 IE11，需 polyfill）。

#### **Fetch**
- **現代化設計**：
  - 原生支援於現代瀏覽器，但不支援 IE（需 polyfill）。
  - 在 Node.js 中需安裝 `node-fetch`。

---

### **何時使用**
- **Axios**：適合需要更多功能（如超時、取消請求、進度監控）或頻繁進行複雜請求的場景。
- **Fetch**：適合簡單、輕量的請求，尤其是在現代瀏覽器中。

---

### **參考資料**
- [Axios 官方文件](https://axios-http.com/)
- [MDN Fetch API 文檔](https://developer.mozilla.org/zh-TW/docs/Web/API/Fetch_API)