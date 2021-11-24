linebot
===
+ line  
建議開兩個機器人，一個服務用一個本地開發用，做完再辦第二個也沒關係。
+ 終端機
1. 初始化 node.js :   
   + npm init --yes  
   + 資料夾名稱與 "name" 不能為 linebot，
   因為接下來的套件名稱跟 linebot 相同會產生衝突，
   且資料夾名稱不能為中文。
   + 調整語法規範: package.json 新增 "type": "module",  
   + 初始化 Git，方便上 github
2. 安裝 linebot 套件 :  
   + npm i linebot  
   + i 為安裝，un 為解安裝
   + 新增 node_modules  
   + package.json 會新增 linebot 版本說明  
   + 拷貝檔案時 node_modules 可以刪除再使用指令 npm i 即可，因為 package-lock.json 會記錄相關套件。
3. .gitignore :  
   + 忽略 node_modules
4. 使用 linebot 套件 :
   + npmjs.com -> 搜尋 linebot
   + npm i dotenv -> 安裝 dotenv : 將本機開發的機器人與 server 遠端服務的機器人，兩者的環境分開來，如此一來若本地的機器人正在 debug 也不會影響到正在使用雲端機器人的使用者，也就是為何建議我們申辦兩個機器人，分開環境讓一個自己測試用一個給別人用的。
   + .env 格式 :  
   變數 = 值  
   (換行不須逗號、需使用'等於')  
   例如 : CHANNEL_ID=""  
   + 建立 .env.sample 並忽略 .env
   + 如果開啟機器人後突然要更改 .env 檔，記得要將機器人退出 ctrl + c，存檔後再開一次機器人，因為 nodemon 不會幫你自動更新 .env 的更動。
5. 安裝 axios :  
   + npm i axios  
   + axios 是 AJAX 技術，參考 Ch.18
6. 強制語法風格一致化 :  
   + eslint 是 npm 的一個套件。
   + vs code 的 eslint 是幫助我們使用 eslint 此套件的擴充功能。
   + 兩種安裝語法相同：  
   npm i eslint --save-dev  
   npm i eslint -D 
   + --save-dev 或 -D : 開發用套件，代表只有這個機器人的開發環境才會用到。
   + package.json 會新增一項開發環境套件 devDependencies  
   + 之後寫 node.js 都會安裝，所以之後寫 API、Vue 都會用到，之後工作也會有類似的語法風格要遵循。
   + 安裝後操作：  
   F1(create eslint configuration) -> style -> esm(JS modules) -> none -> No -> Node(用空白鍵選擇) -> guide -> standard -> JSON -> Yes
   + 終端機留一個就好其他刪掉。
   + .eslintrc.json -> "ecmaVersion":13 改成 12
7. 安裝 nodemon :  
   + 原本有改動就要 ctrl+C 把 node.js 關掉重新跑一次，有了 nodemon 就能自動偵測檔案變更，自動幫你重新載入 node.js
8. 安裝 ngrok :  
   + 因為我們 localhost 沒有 https，而 line 有規定機器人的服務網址必須要有 https (s 代表加密)，所以直接跟 line 請求會被擋掉；而 ngrok 有 https，所以我們可以透過 ngrok 做轉發，可以獲得一個免費的暫時 https 網址，有效時間大概 30分鐘，每次重開網址ID會不同。不過 ngrok 偶爾會掉資料，但沒辦法只能這樣測。
   + npm i -D nodemon ngrok
   + 之後"正式環境"用不到 nodemon 和 ngrok 的套件，所以這邊才用 -D (開發用：代表只有開發環境才會用到)，因為之後上 server 不會有 vs code 給你編輯檔案所以用不到 nodemon，之後的 server 也會有 https 所以也用不到 ngrok。
9. 定義指令：
   + package.json -> scripts -> test
   + 將 "test": ... 此行改為：  
   "dev":"nodemon index.js",  
   "start":"node index.js"
   + dev：開發使用，本機的指令。
   + start：正式環境使用，之後上雲端 server 時，預設是會執行 start 的指令，所以這邊的指令一定要叫做 start  
   + npm run dev  
     每次變更都會重載，讓開發方便不用一直 ctrl+c 重開
   + npm run start
   + 可以在 index.js 寫 console 看看兩者差異
10. import 'dotenv/config'  
    import linebot from 'linebot'
    ...

11. npm run dev (機器人啟動) -> 新增終端機 (開啟機器人的終端機和 ngrok 的終端機要同時開著，我們要去 ngrok 做轉送) -> ngrok http 3000 (若找不到需 npm un ngrok -> npm i -g ngrok，還是找不到就 npm un -g ngrok，直接去 ngrok 官網下載) -> 複製 ngrok 終端機最底下的 https/.../io 網址，注意每次重開 ngrok 網址會不一樣，複製時 ctrl + c 小心不要跳出 -> 貼到 Webhook URL -> 認證成功就+好友
12. 這時候傳什麼機器人就會回傳什麼，現在來抓資料看看
13. 找到開放資料後，import axios from 'axios'
14. 如果在群組內傳訊息，機器人會以為你在跟它要資料，這時候可以用不同的開頭來寫判斷式，例如傳給機器人: !name 微熱山丘 -> 新北市、!cost 屋馬 -> $1800
15. 