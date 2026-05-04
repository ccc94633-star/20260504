# 猜拳遊戲 JavaScript 筆記

---

## 1. 電腦如何亂數出拳

把三種拳放進**陣列**，用 `Math.random()` 隨機取一個。

```js
function getComputerChoice() {
    const choices = ['石頭', '剪刀', '布'];
    const index = Math.floor(Math.random() * choices.length);
    return choices[index];
}
```

| 步驟 | 說明 |
|------|------|
| `Math.random()` | 產生 0.0 ~ 0.999 的隨機數 |
| `* choices.length` | 乘以 3，變成 0.0 ~ 2.999 |
| `Math.floor()` | 取整數，得到 0、1 或 2 |
| `choices[index]` | 對應石頭、剪刀、布 |

---

## 2. 如何判斷輸贏

列出玩家獲勝的三種情況，其餘就是電腦贏。

```js
function judge(player, computer) {
    if (player === computer) return '平手';

    if (
        (player === '石頭' && computer === '剪刀') ||
        (player === '剪刀' && computer === '布')  ||
        (player === '布'   && computer === '石頭')
    ) return '玩家贏';

    return '電腦贏';
}
```

**勝負關係：**
- 石頭 > 剪刀
- 剪刀 > 布
- 布 > 石頭

---

## 3. 用 Radio Button 選拳

### HTML
```html
<label><input type="radio" name="player-choice" value="石頭"> 石頭</label>
<label><input type="radio" name="player-choice" value="剪刀"> 剪刀</label>
<label><input type="radio" name="player-choice" value="布"> 布</label>
```

- `name` 要一樣 → 確保三個只能選一個
- `value` 設成對應的拳名，方便直接使用

### JS 讀取選了哪個
```js
const selected = document.querySelector('input[name="player-choice"]:checked');

if (selected === null) {
    alert('請先選擇出拳！');
    return;
}

playGame(selected.value); // "石頭" / "剪刀" / "布"
```

- `:checked` 找到目前被勾選的那個
- 沒有選時會是 `null`，要先檢查再用

---

## 4. 字串如何累加並換行

### `<div>` → 用 `innerHTML` + `<br>`
```js
const div = document.querySelector('.tatal');
div.innerHTML += '第一局結果<br>';
div.innerHTML += '第二局結果<br>';
```

### `<textarea>` → 用 `.value` + `\n`
```js
const textarea = document.querySelector('#total');
textarea.value += '第一局結果\n';
textarea.value += '第二局結果\n';
```

> ⚠️ `<textarea>` **不會解析 HTML**，所以 `<br>` 會直接顯示成文字，換行要用 `\n`。

| 元素 | 換行符號 | 讀寫屬性 |
|------|---------|---------|
| `<div>` | `<br>` 或 `<p>` | `.innerHTML` |
| `<textarea>` | `\n` | `.value` |

---

## 5. 用模板字串嵌入變數

用反引號 `` ` `` 包起來，變數用 `${}` 嵌入，比用 `+` 拼接更清楚。

```js
// 舊寫法（容易出錯）
total.value += '玩家: ' + player + ' 電腦: ' + computer + '\n';

// 模板字串（推薦）
total.value += `玩家: ${player} 電腦: ${computer} 結果: ${result}\n`;
```

---

## 6. 完整流程

```
玩家點擊出拳按鈕
    ↓
取得玩家選的拳（radio :checked）
    ↓
playGame(player)
    ↓
getComputerChoice() → 電腦亂數出拳
    ↓
judge(player, computer) → 判斷輸贏
    ↓
顯示結果 + 累計分數 + 寫入紀錄
```

---

## 7. 常見錯誤提醒

### `if / else if / else` 要寫完整
```js
// ❌ 錯誤：result 一定會被最後一行蓋掉
if (player === computer) result = '平手';
if (...) result = '玩家贏';
result = '電腦贏'; // 這行沒有 if，永遠執行！

// ✅ 正確
if (player === computer) result = '平手';
else if (...) result = '玩家贏';
else result = '電腦贏';
```

### 沒選拳就出拳會噴錯
```js
// 若沒選，.value 會噴錯，要先判斷是否為 null
const selected = document.querySelector('input[name="player-choice"]:checked');
if (selected === null) { alert('請先選！'); return; }
```
