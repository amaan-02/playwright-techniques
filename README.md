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
---
---
---
---
---
---
---





# Playwright Locator Combinations



## **1️⃣ CSS and Text-Based Combination**
Ensures precise selection by combining **CSS selectors** with **text content**.

```ts
await page.locator('button:has-text("Login")').click();
```

✅ Selects a button **only if it has "Login" text**.

---

## **2️⃣ Combining XPath with Attributes**
Improves precision by targeting both structure and attributes.

```ts
await page.locator('//button[contains(@class, "btn") and text()="Submit"]').click();
```

✅ Selects a `<button>` that contains `class="btn"` **AND** text `"Submit"`.

---

## **3️⃣ `has()` for Parent-Child Relationship**
Selects a **parent element** based on its **child content**.

```ts
await page.locator('div:has(span.icon-success)').click();
```

✅ Selects a `<div>` **only if** it has a child `<span class="icon-success">`.

---

## **4️⃣ `has-text()` with Parent-Child Locator**
```ts
await page.locator('div:has-text("Welcome")').click();
```

✅ Selects any `<div>` containing the text `"Welcome"`.

---

## **5️⃣ `nth()` for Multiple Elements**
Selects a **specific occurrence** of multiple elements.

```ts
await page.locator('button').nth(1).click();
```

✅ Selects the **second button** on the page.

---

## **6️⃣ `:not()` for Filtering Elements**
Excludes elements that match a given selector.

```ts
await page.locator('input:not([disabled])').fill('Test User');
```

✅ Fills only **enabled** input fields.

---

## **7️⃣ `or` Condition in XPath**
Selects multiple elements using `|`.

```ts
await page.locator('//button[text()="Save"] | //button[text()="Submit"]').click();
```

✅ Clicks either a `"Save"` **or** `"Submit"` button.

---

## **8️⃣ Chained Locators for Specific Elements**
Instead of writing long XPath queries, **chaining** locators makes it more readable.

```ts
const container = page.locator('.menu');
await container.locator('button:has-text("Start")').click();
```

✅ Targets **only buttons inside `.menu`**.

---

## **🔥 Best Practices for Reliable Locators**
✅ **Use `data-testid` or `data-qa` attributes** for stable locators:

```ts
await page.locator('[data-testid="login-button"]').click();
```

✅ **Avoid absolute XPath**, which may break on UI changes.

✅ **Use Playwright’s built-in `getByRole()`** for accessibility-friendly locators:

```ts
await page.getByRole('button', { name: 'Login' }).click();
```

---

🎯 **Following these techniques will make your Playwright tests more reliable, efficient, and maintainable!** 🚀


