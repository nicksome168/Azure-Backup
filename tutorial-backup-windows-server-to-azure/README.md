## 將 Windows Server 備份到 Azure （MARS）與將檔案從 Azure 復原至 Windows Server
本篇教學將著重使用Azure Backup服務備份地端Windows伺服器，並演示進行還原。

在這個實作您將會學到：
- [x] 使用復原服務保存庫備份地端Windows伺服器（MARS）
- [x] 使用代理程式將檔案從 Azure 復原至 Windows Server

## 環境
- Azure 入口網站
- Windows 10 on Lenova

## 使用情境
現今許多企業採用混合雲架構，為了更好整合與管理地端與雲端備份資源，Azure Backup統一管理的服務，您可以直接透過幾個按鈕完成地端工作負載備份並存放在Azure上，同時替您管理好備份伺服器的維護拓展。

## 步驟
1. [建立復原服務保存庫（略）](https://github.com/nicksome168/Azure-Backup/blob/main/tutorial-backup-vm-at-scale/README.md#%E5%BB%BA%E7%AB%8B%E5%BE%A9%E5%8E%9F%E6%9C%8D%E5%8B%99%E4%BF%9D%E5%AD%98%E5%BA%AB)
2. [設定儲存體複寫類型（略）](https://github.com/nicksome168/Azure-Backup/blob/main/tutorial-backup-vm-at-scale/README.md#%E8%A8%AD%E5%AE%9A%E5%84%B2%E5%AD%98%E9%AB%94%E8%A4%87%E5%AF%AB%E9%A1%9E%E5%9E%8B)
3. 設定備份項目，下載代理器程式MARS
6. 設定備份排程與需備份的檔案、資料夾
7. 使用MARS備份及還原
8. 刪除資源

## 設定備份項目，下載代理器程式MARS
a. 登入Azure入口網站，並搜尋"Recovery Service保存庫"
<img width="771" alt="Screen Shot 2021-10-22 at 10 17 26 AM" src="https://user-images.githubusercontent.com/29058993/138420297-cfd7c1e5-9526-471a-82eb-ed6361239370.png">

b. 資源建立後，點入保存庫，點選*備份*
<img width="931" alt="Screen Shot 2021-10-22 at 10 56 11 AM" src="https://user-images.githubusercontent.com/29058993/138420328-7aa7df98-862f-4834-adaf-5bb2cc0fd1fc.png">

c. 設定備份目標，這邊選擇「內部部署」與「檔案與資料夾」、「系統狀態」。完成點選「準備基礎架構」
![image](https://user-images.githubusercontent.com/29058993/138420428-5f785257-1b29-4986-9a53-9f2f2ea14a14.png)

d. 點選下載「Windows Server 或 Windows Client 的代理程式」
![image](https://user-images.githubusercontent.com/29058993/138420460-2f1108db-8b32-41e3-9c88-d3b4cb4a0aec.png)

e. 安裝MARS代理程式
![image](https://user-images.githubusercontent.com/29058993/138420479-59103f6f-420c-47b6-9596-bc27ecf0d07d.png)

f. 到Azure頁面，下載連線保存庫的認證，在MARS代理程式選擇認證路徑。**注意⚠️認證期限僅有10天，期限到您需要到Azure頁面重新下載認證**<br>
![image](https://user-images.githubusercontent.com/29058993/138420570-5f08bb3e-8c95-4b58-b197-828c9bd81e61.png)

g. 在MARS代理程式設定加密密碼，為demo目的這邊存放在地端，不建議應用在實際工作負載中

## 設定備份排程與需備份的檔案、資料夾

a. 打開MARS代理程式，點擊右方欄位的「Schedule Backup」設定
![image](https://user-images.githubusercontent.com/29058993/138420656-44446d7d-aca3-4590-ad6e-29cb40f6ba94.png)

b. 選擇備份檔案或資料夾，為demo目的，這邊選擇一PDF檔
![image](https://user-images.githubusercontent.com/29058993/138420592-0db1e3ac-4c8b-43f1-bb78-34668824ac20.png)

c. 接著設定備份排程，每日最多可以選擇3個時間點備份，也可以選擇每週的特定拜幾進行備份
![image](https://user-images.githubusercontent.com/29058993/138420607-f409ad33-8e1d-4db0-befc-6732c13a017e.png)

d. 下一步設定長期保留復原點（LTR point）的保存期，一樣分週、月、年還原點
![image](https://user-images.githubusercontent.com/29058993/138420619-32ec705f-7560-431a-ac25-e227a833874d.png)

e. 接著選擇備份傳輸目標，這邊選擇「線上」
![image](https://user-images.githubusercontent.com/29058993/138420630-e7e778fd-774b-4387-bc2d-52e9f0fa3494.png)

f. 確認頁面，沒問題就按Finish
![image](https://user-images.githubusercontent.com/29058993/138420664-74445f75-69f8-47f9-ae8f-0fb4a68b3955.png)

## 使用MARS備份及還原
由於系統預設不會在建立時執行第一次備份，因此使用者需透過「隨選備份」進行第一次備份

a. 建立完備份排程後，回到MARS代理程式主畫面，點選右方欄位的「Back Up Now」
![image](https://user-images.githubusercontent.com/29058993/138420703-a7787227-c5e0-495e-adcc-01b349d7ea8f.png)

b. 點選今天日期
![image](https://user-images.githubusercontent.com/29058993/138420712-de820bf7-d91c-4ab8-8273-7eadcee393fa.png)

c. 確定備份的檔案，按確定。初次備份需要比較久的時間
![image](https://user-images.githubusercontent.com/29058993/138420880-8b1bd1d8-b593-49a6-9a27-281300edb4ca.png)

d. 完成備份後，回到MARS代理程式主畫面，點選右方欄位的「Restore Data」
![image](https://user-images.githubusercontent.com/29058993/138420860-bf8a1b68-ce83-4944-8cbf-f6e8d2dada18.png)

e. 選擇要還原至地端本伺服器
![image](https://user-images.githubusercontent.com/29058993/138420898-d82e4620-0453-4ff5-8220-4507b0401eac.png)

f. 接著選擇要還原的資料類型，這邊選擇「檔案/資料解」
![image](https://user-images.githubusercontent.com/29058993/138420909-79e95f55-f43d-487d-8334-cfcc52922990.png)

g. 接著選擇當初備份的磁碟代號，以及還原點。確定完就按「掛載」
![image](https://user-images.githubusercontent.com/29058993/138420918-d8939298-e923-4da8-8ce9-b7d7c71f0681.png)

h. 成功掛載後，會彈跳出檔案總管視窗，掛載成功的還原點磁碟會出現，其會保留備份當時的路徑
![image](https://user-images.githubusercontent.com/29058993/138420932-4f72c34a-3419-4fe9-9b0e-b3cd7077d1ff.png)

i. 完成檔案還原後，回到MARS代理程式，點選卸載
![image](https://user-images.githubusercontent.com/29058993/138420956-799bbca2-6ed0-4551-9ba2-ce23b3c27b97.png)

## 刪除資源
a. 進入保存庫，點選左邊欄位「受保護的項目」的備份項目，點選 「Azure Backup Agent」
<img width="1641" alt="Screen Shot 2021-10-22 at 3 45 38 PM" src="https://user-images.githubusercontent.com/29058993/138421025-98c5369f-bcfd-4795-9d4e-a79ed9df5362.png">

b. 點選備份目標

c. 點選電腦名稱
<img width="910" alt="Screen Shot 2021-10-22 at 3 49 40 PM" src="https://user-images.githubusercontent.com/29058993/138421046-9bdb02c7-d07f-4f34-90cb-ddf238882a4c.png">

d. 點選刪除
<img width="934" alt="Screen Shot 2021-10-22 at 3 49 45 PM" src="https://user-images.githubusercontent.com/29058993/138421086-2e020e0a-fe74-41bb-a499-06b49a73f9e8.png">

e. 輸入伺服器名稱、刪除原因、註解刪除原因、勾選
<img width="1180" alt="Screen Shot 2021-10-22 at 3 50 34 PM" src="https://user-images.githubusercontent.com/29058993/138421103-a7dc8d7a-234c-43d8-8678-3a00a385593e.png">
