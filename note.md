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
5. npm i axios