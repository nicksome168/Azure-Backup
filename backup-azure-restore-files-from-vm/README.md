## 從 Azure 虛擬機器備份復原檔案
本篇教學將著重如何從 Azure VM 備份來復原檔案和資料夾
**注意⚠️不支援從加密的 VM 備份進行檔案復原**

在這個實作您將會學到：
- [x] 將保存庫還原點連線到地端Windows伺服器
- [x] 從保存庫還原點還原檔案至地端Windows伺服器

## 環境
- Azure 入口網站
- Windows 10 on Lenova

## 使用情境
![image](https://user-images.githubusercontent.com/29058993/138418738-7fd45944-c621-48bb-8d0c-42b6668ca2ee.png)

## 步驟
1. [建立Azure VM備份還原點（略）](link)
2. 下載執行腳本與指令碼
3. 執行腳本並設定磁碟區域，還原檔案
5. 卸載磁碟

## 下載執行腳本與指令碼
a. 登入 Azure 入口網站 ，並搜尋"虛擬機器"
<img width="775" alt="Screen Shot 2021-10-22 at 4 15 15 PM" src="https://user-images.githubusercontent.com/29058993/138418930-7e1bd915-acb8-490c-9a3a-a6074a6d2c27.png">

b. 點擊已備份的虛擬機器，並點選「作業」下的「備份」
<img width="1666" alt="Screen Shot 2021-10-22 at 4 15 30 PM" src="https://user-images.githubusercontent.com/29058993/138418943-3451db3a-6c28-4335-ab8d-9485aec80e1d.png">

c. 點選「檔案復原」

d. 下拉選單「選取復原點」，並點擊「下載可執行檔」
![image](https://user-images.githubusercontent.com/29058993/138419331-f8ef2226-404d-4144-859f-9dd1ce4e23ac.png)

e. 等待約1分鐘初始化，會出現下載按鈕，點選按鈕
![image](https://user-images.githubusercontent.com/29058993/138419559-fa2727e5-75e9-48a8-a172-3321cd20b980.png)


## 執行腳本並設定磁碟區域，還原檔案
a. 下載完成後，「以管理員身分執行」
![image](https://user-images.githubusercontent.com/29058993/138419472-3c3e6fe1-c75f-4bc1-8a7e-659fe5dbd94b.png)

b. 複製Azure網頁上的「執行指令碼的密碼」，在貼入彈跳出來的執行檔視窗
![image](https://user-images.githubusercontent.com/29058993/138419566-809db8ba-3ee0-4311-82a7-2870cff9c52e.png)

c. 之後會彈跳出PS視窗，輸入*Y*確認
![image](https://user-images.githubusercontent.com/29058993/138419576-0284f857-4d80-422f-827b-7c745a4d44df.png)

d. 可以看到還原點有成功掛載到地端的D槽（您的掛載磁碟名稱可能會不同）
![image](https://user-images.githubusercontent.com/29058993/138419587-c728747b-dd10-4faa-bc6d-4ce38498998f.png)

e. 在檔案總管中也可以看到D槽的選項，這時候就可以進行檔案的還原
![image](https://user-images.githubusercontent.com/29058993/138419659-1e4024be-c067-4795-8882-06daeb0945bb.png)

f. 如果在PS沒有顯示還原點掛載的磁碟區，也沒有在檔案總管中看到新的磁碟區。到檔案總管在本機點選右鍵按「管理」
![image](https://user-images.githubusercontent.com/29058993/138419669-0826ee6b-1106-4688-b209-6604952085e4.png)

g. 進入「Disk Management」，檢查是否有成功掛載，若有則在掛載磁碟按右鍵，點選「Change Drive Letter and Paths」
![image](https://user-images.githubusercontent.com/29058993/138419679-87897dd3-132b-4d40-9491-3f5b62448136.png)

h. 點選「Add」新增磁碟區代號，下拉選擇磁碟區代號，完成後就可以在檔案總管中看到掛載的還原點了
![image](https://user-images.githubusercontent.com/29058993/138419698-48341a93-53cf-4331-8107-d43362384b1d.png)

## 卸載磁碟
a. 回到PS畫面，輸入「Q」退出
![image](https://user-images.githubusercontent.com/29058993/138419710-7faacb34-e6a0-4567-9d32-1555d15de2e7.png)

b. 回到Azure頁面，點選「卸載磁碟」
![image](https://user-images.githubusercontent.com/29058993/138419720-e551b6aa-1615-4467-86fc-a9751ddb2594.png)
