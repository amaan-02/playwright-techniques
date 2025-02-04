# Playwright Locator Techniques

## 1️⃣ Using `*` in Locators
The `*` symbol is a wildcard used in both **CSS Selectors** and **XPath**.

### CSS Selectors (`*` for "contains" in attributes)
- `*=` → Selects elements where the attribute **contains** a value.
  ```ts
  await page.locator('[class*="button"]').click();
  ```
  - Matches:
    ```html
    <div class="primary-button"></div>
    <div class="button-secondary"></div>
    ```

- `^=` → Selects elements where the attribute **starts with** a value.
  ```ts
  await page.locator('[id^="btn_"]').click();
  ```
  - Matches:
    ```html
    <button id="btn_login"></button>
    <button id="btn_signup"></button>
    ```

- `$=` → Selects elements where the attribute **ends with** a value.
  ```ts
  await page.locator('[id$="_submit"]').click();
  ```
  - Matches:
    ```html
    <button id="form_submit"></button>
    <button id="user_submit"></button>
    ```

### XPath Selectors (`*` as a wildcard)
- `//tagname[*]` → Selects any element that has child elements.
  ```ts
  await page.locator('//div[*]').click();
  ```
  - Matches:
    ```html
    <div>
      <span>Hello</span>
    </div>
    ```

- `//*[@id="container"]//*` → Selects **all** child elements inside a specific container.
  ```ts
  await page.locator('//*[@id="container"]//*').click();
  ```
  - Matches:
    ```html
    <div id="container">
      <p>Paragraph</p>
      <button>Click me</button>
    </div>
    ```

---

## 2️⃣ Using `+` in Locators
The `+` symbol is used in **CSS selectors** to **select the immediately following sibling element**.

### CSS Adjacent Sibling Selector (`+`)
- `selector1 + selector2` → Selects `selector2` **only if it comes immediately after** `selector1`.

  ```ts
  await page.locator('h1 + p').click();
  ```
  - Matches:
    ```html
    <h1>Title</h1>
    <p>Subtitle</p> <!-- This will be selected -->
    <p>Another paragraph</p>
    ```

- Example: Clicking a button that comes **right after** a div.
  ```ts
  await page.locator('div.success-message + button').click();
  ```
  - Matches:
    ```html
    <div class="success-message">Success</div>
    <button>OK</button> <!-- This will be selected -->
    ```

### XPath (Does NOT use `+`)
XPath does not use `+` for sibling selection. Instead, you use `following-sibling::`.
  ```ts
  await page.locator('//h1/following-sibling::p').click();
  ```
  - Matches the first `<p>` after an `<h1>`.

---

## 💡 Summary Table
| Symbol  | CSS Usage | XPath Usage | Example |
|---------|-----------|-------------|---------|
| `*` | Wildcard for "contains" | Matches any element | `[class*="btn"]`, `//*[@id="container"]//*` |
| `+` | Selects the next sibling | **Not used in XPath** | `h1 + p` selects the `<p>` after `<h1>` |

---

## 

