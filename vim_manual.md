# Vim 操作手冊：
<a href="#first">第一級</a>

<a href="#second">第二級</a>

<a href="#third">第三級</a>

<a href="#forth">第四級</a> 

<a href="#reference">參考資料</a>


## <a id="first" href="#first">第一級</a>

- i → Insert 模式，按 ESC 回到 Normal 模式.
- x → 刪當前光標所在的一個字符。
- :wq → 存盤 + 退出 (:w 存盤, :q 退出)   （陳皓註：:w 後可以跟文件名）##
- dd → 刪除當前行，並把刪除的行存到剪貼板裏##
- p → 粘貼剪貼板

**推薦:**

- hjkl ＝ ←↓↑→
- :help <command> → 顯示相關命令的幫助。妳也可以就輸入 :help 而不跟命令。（退出幫助需要輸入:q）

**各種插入模式**
- a → 在光標後插入
- o → 在當前行後插入一個新行
- O → 在當前行前插入一個新行
- cw → 替換從光標所在位置後到一個單詞結尾的字符

---

## <a id="second" href="#second">第二級</a>

**簡單的移動光標**

- 0 → 數字零，到行頭
- Shift + 6 → 到本行第一個不是blank字符的位置（所謂blank字符就是空格，tab，換行，回車等）
- Shift + 4 → 到本行行尾
- g_ → 到本行最後一個不是blank字符的位置。
- /pattern → 搜索 pattern 的字符串（陳皓註：如果搜索出多個匹配，可按n鍵到下一個）

**拷貝/粘貼 （陳皓註：p/P都可以，p是表示在當前位置之後，P表示在當前位置之前）**

- P → 粘貼
- yy → 拷貝當前行當行於 ddP

**Undo/Redo**

- u → undo
- Ctrl+r → redo

**打開/保存/退出/改變文件(Buffer)**

- :e <path/to/file> → 打開一個文件
- :w → 存盤
- :saveas <path/to/file> → 另存為 <path/to/file>
- :x， ZZ 或 :wq → 保存並退出 (:x 表示僅在需要時保存，ZZ不需要輸入冒號並回車)
- :q! → 退出不保存 
- :qa! → 強行退出所有的正在編輯的文件，就算別的文件有更改。
- :bn 和 :bp → 妳可以同時打開很多文件，使用這兩個命令來切換下一個或上一個文件。

---

## <a id="third" href="#third">第三級</a>
 
- 2dd → 刪除2行
- 3p → 粘貼文本3次
- 100idesu [ESC] → 會寫下 “desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu “
- . → 重復上一個命令 —— 100 “desu “
-  3. → 重復 3 次 “desu”



- NG → 到第 N 行 （陳皓註：註意命令中的G是大寫的，另我一般使用 : N 到第N行，如 :137 到第137行）
- gg → 到第一行。（陳皓註：相當於1G，或 :1）
- G → 到最後一行。
按單詞移動：
- w → 到下一個單詞的開頭。
- e → 到下一個單詞的結尾。
> 如果妳認為單詞是由默認方式，那麽就用小寫的e和w。默認上來說，一個單詞由字母，數字和下劃線組成（陳皓註：程序變量）

> 如果妳認為單詞是由blank字符分隔符，那麽妳需要使用大寫的E和W。（陳皓註：程序語句）

- % : 匹配括號移動，包括 (, {, [. （陳皓註：妳需要把光標先移到括號上）
- * 和 #:  匹配光標當前所在的單詞，移動光標到下一個（或上一個）匹配單詞（*是下一個，#是上一個）

妳一定要記住光標的移動，因為很多命令都可以和這些移動光標的命令連動。很多命令都可以如下來幹：

**< start position >< command >< end position >**

例如 0y$ 命令意味著：

1. 0 → 先到行頭
2. y → 從這裏開始拷貝
3. $ → 拷貝到本行最後一個字符
妳可可以輸入 ye，從當前位置拷貝到本單詞的最後一個字符。

妳也可以輸入 y2/foo 來拷貝2個 “foo” 之間的字符串。

還有很多時間並不一定妳就一定要按y才會拷貝，下面的命令也會被拷貝：

**d (刪除 )**

**v (可視化的選擇)**

**gU (變大寫)**

**gu (變小寫)等等**

（陳皓註：可視化選擇是一個很有意思的命令，妳可以先按v，然後移動光標，妳就會看到文本被選擇，然後，妳可能d，也可y，也可以變大寫等）

---

## <a id="forth" href="#forth">第四級</a>

妳只需要掌握前面的命令，妳就可以很舒服的使用VIM了。但是，現在，我們向妳介紹的是VIM殺手級的功能。下面這些功能是我只用vim的原因。

在當前行上移動光標: 0 ^ $ f F t T , ;
- 0 → 到行頭 
- ^ → 到本行的第一個非blank字符
- $ → 到行尾
- g_ → 到本行最後一個不是blank字符的位置。
- fa → 到下一個為a的字符處，妳也可以fs到下一個為s的字符。
- t, → 到逗號前的第一個字符。逗號可以變成其它字符。
- 3fa → 在當前行查找第三個出現的a。
- F 和 T → 和 f 和 t 一樣，只不過是相反方向。
-  dt" → 刪除所有的內容，直到遇到雙引號—— "

**區域選擇 < action >a< object > 或 < action >i< objec t>**
在visual 模式下，這些命令很強大，其命令格式為

**< action >a< object >** 和 **< action >i< objec t>**

action可以是任何的命令，如 d (刪除), y (拷貝), v (可以視模式選擇)。
object 可能是： w 一個單詞， W 一個以空格為分隔的單詞， s 一個句字， p 一個段落。也可以是一個特別的字符："、 '、 )、 }、 ]。
假設妳有一個字符串 (map (+) ("foo")).而光標鍵在第一個 o 的位置。

- vi" → 會選擇 foo.
- va" → 會選擇 "foo".
- vi) → 會選擇 "foo".
- va) → 會選擇("foo").
- v2i) → 會選擇 map (+) ("foo")
- v2a) → 會選擇 (map (+) ("foo"))

**塊操作: < Ctrl + v >**
塊操作，典型的操作： 0 < Ctrl + v > < Ctrl + d > I-- [ESC]

- ^ → 到行頭
- < Ctrl + v > → 開始塊操作
- < Ctrl + d > → 向下移動 (妳也可以使用hjkl來移動光標，或是使用%，或是別的)
- I-- [ESC] → I是插入，插入“--”，按ESC鍵來為每一行生效。

自動提示： < Ctrl + n > 和 < Ctrl + p >
在 Insert 模式下，妳可以輸入一個詞的開頭，然後按 < Ctrl + p >或是< Ctrl + n >，自動補齊功能出現

**宏錄制： qa 操作序列 q, @a, @@**
qa 把妳的操作記錄在寄存器 a。
於是 @a 會replay被錄制的宏。
@@ 是一個快捷鍵用來replay最新錄制的宏。

```
示例:

在一個只有一行且這一行只有“1”的文本中，鍵入如下命令：

1. qaYp< Ctrl + a >q→
2. qa 開始錄制
3. Yp 復制行.
4. < Ctrl + a > 增加1.
5. q 停止錄制.
6. @a → 在1下面寫下 2
7. @@ → 在2 正面寫下3
8. 現在做 100@@ 會創建新的100行，並把數據增加到 103.

```


**可視化選擇： v,V,< Ctrl + v >**

前面，我們看到了 <Ctrl + v > 的示例 ，我們可以使用 v 和 V。一但被選好了，妳可以做下面的事：

- J → 把所有的行連接起來（變成一行）
- < 或 > → 左右縮進
- = → 自動給縮進 （陳皓註：這個功能相當強大，我太喜歡了）

在所有被選擇的行後加上點東西：

< Ctrl + v >
- 選中相關的行 (可使用 j 或 <C-d> 或是 /pattern 或是 % 等……)
- $ 到行最後
- A, 輸入字符串，按 ESC。

**分屏: :split 和 vsplit.**

下面是主要的命令，妳可以使用VIM的幫助 :help split. 妳可以參考本站以前的一篇文章VIM分屏。

- :split → 創建分屏 (:vsplit創建垂直分屏)
- <Ctrl + w > < dir > : dir就是方向，可以是 hjkl 或是 ←↓↑→ 中的一個，其用來切換分屏。
- < Ctrl + w >_ (或 < Ctrl + w >|) : 最大化尺寸 (<Ctrl + w>| 垂直分屏)
- < Ctrl + w >+ (或 <Ctrl + w >-) : 增加尺寸


## <a id="reference" href="#reference">[參考資料]</a>

[Vim簡明教程【CoolShell】](https://blog.csdn.net/niushuai666/article/details/7275406)

