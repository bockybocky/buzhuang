# 不裝（buzhuang）

> 一隻讓 AI 助理不敢裝懂、不敢裝做完、被指出錯誤時先重算再解釋的 Claude Code skill。
> 每一條規則都附當初付出的代價。

---

## 這是要解決什麼

你花了一整晚跟 AI 說「這張圖的數據有問題」，它一直回你「沒問題」。
你發火了，它才承認：「對不起，有一個站把平均拉低了。」

**問題不在它不夠聰明，在於沒有任何東西逼它去重算。**
它可以一路解釋到底，而解釋比重算便宜太多了。

這裡的每一條規則都在做同一件事：
**把「我覺得沒問題」變成一個它答不出來的問題。**

---

## 五秒鐘版本

不想讀完的話，把這三句貼進你的系統提示詞：

```
1. 我說數據有問題時，你的第一個動作是去重算，不是解釋為什麼沒問題。算完再講結論。
2. 凡是平均值、比率、統計量，先講分母是什麼、有沒有東西被排除或合併，再講數值。
3. 不確定就說不確定。我寧可你說「我沒查」，也不要你說「沒問題」。
```

第 2 句最划算 —— 開頭那個「有一個站拉低平均」的案子，
如果一開始就被要求先報分母，它第一輪就會自己講出來。

更完整的版本在 [`prompts/system-prompt.md`](prompts/system-prompt.md)。

---

## 內容

| 檔案 | 治的是 | 幾條 |
|---|---|---|
| [SKILL.md](SKILL.md) | 三個攔截點與收據規格 | — |
| [references/01-no-masquerading.md](references/01-no-masquerading.md) | 它說「做完了」 | 6 |
| [references/02-disconfirming.md](references/02-disconfirming.md) | 你說「你錯了」 | 6 |
| [references/03-receipts.md](references/03-receipts.md) | 你想查核 | 5 |
| [prompts/system-prompt.md](prompts/system-prompt.md) | 可直接貼的提示詞（三種長度） | — |

---

## 安裝

Claude Code：

```bash
git clone https://github.com/bockybocky/buzhuang.git ~/.claude/skills/buzhuang
```

然後在對話裡說「用不裝檢查一下」，或直接 `/buzhuang`。

**其他工具**：這份東西的主體是 Markdown 文字，不綁 Claude Code。
把 `prompts/system-prompt.md` 貼進 ChatGPT 的自訂指令、Cursor 的規則檔，
或任何「每次對話都會載入」的地方，一樣有效。

---

## ⚠️ 這隻 skill 的天花板（先講清楚）

**skill 要被觸發才會載入，而觸發是機率性的。**

「不准裝做完」需要的是每次都生效，但 skill 只在模型認為該用它的時候才被叫用 ——
而**一個正在裝做完的模型，最不可能想到要叫用一隻攔它的 skill**。

所以正確的用法是兩層：

| 層 | 放哪 | 什麼時候生效 | 放什麼 |
|---|---|---|---|
| 提示詞層 | 全域規則檔、系統提示詞 | **每次** | 三句最短的紀律 |
| skill 層 | skill 目錄 | 被叫用時 | 完整檢查清單與病理 |

只裝 skill 不設提示詞，等於把煞車裝在一個「要你先想到踩它」的位置上。

---

## 這些規則的來歷

規則不是設計出來的，是**每一條都對應一次真實的損失**。
所以每條都寫了當初付了什麼代價 —— 沒有代價的規則會被當成裝飾品略過。

裡面包括一次相當難堪的紀錄：
[編造了一整段從未執行的工具輸出](references/01-no-masquerading.md#16-絕不編造工具輸出)，
並拿那份憑空的資料改了真實的檔案與網站。留著它，是因為它比任何論證都有說服力。

其中「十二條假裝完成清單」移植自 [AlexZio00/sovereign-skills](https://github.com/AlexZio00/sovereign-skills)
的 `goal-lock`（Success Masquerading Blocklist）。我們移植的是當時的十二條版本，
該專案目前已增至十三條，建議一併參考原作。其餘各條為自有實證。

原始版本用在一個中文的個人 AI 工作系統上。
公開版抽掉了所有專案細節，只留病理、規則與做法。

MIT 授權。
