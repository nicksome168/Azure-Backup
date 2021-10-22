## 使用 Azure 入口網站備份多部虛擬機器並還原（包含建立新的 VM、還原磁碟）
本篇教學將著重使用Azure Backup服務備份Azure VM，並演示使用2種方法進行還原。

在這個實作您將會學到：
- [x] 建立復原服務保存庫與保存原則
- [x] 備份Azure VM
- [x] 使用備份建立新的 VM
- [x] 使用備份還原磁碟，並從範本自訂和建立 VM

## 環境
- Azure 入口網站

## 使用情境
現今許多企業採用公有雲服務，而工作負載備份是必要的作業，但於傳統地端備份不同，您毋需搭建維護備份伺服器，使用Azure Backup統一管理的服務，您可以直接透過幾個按鈕完成備份，並替您管理好備份伺服器的維護拓展。

## 步驟
1. [新建Windows VM（略）](https://github.com/nicksome168/Azure-VM-to-VM-communication#create-windows-vm-in-vnet2)
2. 建立復原服務保存庫
3. 設定儲存體複寫類型
4. 設定備份原則與備份虛擬機器
5. 觸發受保護虛擬機器的隨選備份作業
6. 使用備份建立新的 VM
7. 使用備份還原磁碟，並從範本自訂和建立 VM
8. 刪除資源

## 建立復原服務保存庫
a. 登入Azure入口網站，並搜尋"Recovery Service保存庫"
<img width="771" alt="Screen Shot 2021-10-22 at 10 17 26 AM" src="https://user-images.githubusercontent.com/29058993/138416453-a22e322d-2543-4d57-ab58-35b57ee8b628.png">

b. 點選*建立*，選擇「資源群組」，輸入「保存庫名稱」，並選擇「區域」。**⚠️注意“區域”必須“與您所建立的VM所在的區域相同**
<img width="1030" alt="Screen Shot 2021-10-22 at 10 18 12 AM" src="https://user-images.githubusercontent.com/29058993/138416473-9f762d0b-f866-4ffb-a85c-4e69c8eda10c.png">

## 設定儲存體複寫類型
**注意⚠️設定保存項目後，就無法再作修改儲存體複寫類型**

a. 進入保存庫，點選左邊欄位「設定」的屬性。點選「備份設定」的設定
<img width="846" alt="Screen Shot 2021-10-22 at 10 58 17 AM" src="https://user-images.githubusercontent.com/29058993/138416533-f3db4df7-e935-4570-a7e0-267f11ddd43f.png">

b. 預設是異地備援（GRS），可以設定跨區域還原（CRR），詳細設定差異可以參考[Azure 儲存體備援官方文件](https://docs.microsoft.com/zh-tw/azure/storage/common/storage-redundancy)

## 設定備份原則與備份虛擬機器
a. 資源建立後，點入保存庫，點選*備份*
<img width="1239" alt="Screen Shot 2021-10-22 at 10 28 08 AM" src="https://user-images.githubusercontent.com/29058993/138416550-8bf5ce3e-55b9-4068-8e24-c602af653111.png">

b. 設定備份目標，這邊選擇Azure與虛擬機器
<img width="671" alt="Screen Shot 2021-10-22 at 10 28 40 AM" src="https://user-images.githubusercontent.com/29058993/138416564-fab288a1-6777-49b5-8fd7-c691d288967f.png">

c. 設定備份原則與選擇需要備份的虛擬機器
<img width="777" alt="Screen Shot 2021-10-22 at 10 30 13 AM" src="https://user-images.githubusercontent.com/29058993/138416623-a178e7c1-215b-469a-96fd-d148d373af58.png">

d. 在備份原則中，可以設定「備份排程」、「立即還原」、「保留範圍」
- 備份排程：設定備份頻率
- 立即還原：設定快照集的保存天數，最多5天
- 保留範圍：設定長期保留復原點（LTR point），此為全備份
<img width="761" alt="Screen Shot 2021-10-22 at 10 49 55 AM" src="https://user-images.githubusercontent.com/29058993/138416639-ea3041a9-7d7a-4eee-bb5a-85610d399457.png">

e. 在虛擬機器點選*新增*，並在新視窗選擇需要備份的虛擬機器，這邊我們選擇剛剛建立的VM。注意⚠️只會顯示與保存庫**相同區域**的虛擬機器

f. 完成就按下*認證*，並按*建立備份*

g. 設定備援

## 觸發受保護虛擬機器的隨選備份作業
由於系統預設不會在建立時執行第一次備份，因此使用者需透過「隨選備份」進行第一次備份

a. 進入保存庫，點選左邊欄位「受保護的項目」的備份項目，點選 「Azure Virtual Machine」
<img width="1169" alt="Screen Shot 2021-10-22 at 11 03 55 AM" src="https://user-images.githubusercontent.com/29058993/138416720-23057f23-c1c7-4a0b-86ab-3834f362d4f5.png">

b. 可以看到「上次備份狀態」來問顯示*警告*，表示初始備份尚未完成，點選剛剛建立的備份項目
<img width="1642" alt="Screen Shot 2021-10-22 at 11 04 32 AM" src="https://user-images.githubusercontent.com/29058993/138416771-6c47b5bd-6e58-4798-a1c4-bdd58fe2faea.png">

c. 點選左上角「立即備份」
<img width="1170" alt="Screen Shot 2021-10-22 at 11 05 55 AM" src="https://user-images.githubusercontent.com/29058993/138416783-cafa138d-e98c-4422-95f7-28c5ae5a1081.png">

d. 設定此次備份「保留備份到」，預設是30天。完成按下確定，第一次備份時間較長，需等待約50分鐘
<img width="670" alt="Screen Shot 2021-10-22 at 11 06 17 AM" src="https://user-images.githubusercontent.com/29058993/138416792-65e8b128-8019-4990-9323-1a7681771a2f.png">

e. 完成備份後會在備份項目下方看到一個備份是「應用程式保持一致」且類型為「快照集」，參考[快照集一致性官方文件](https://docs.microsoft.com/zh-tw/azure/backup/backup-azure-vms-introduction#snapshot-consistency)
<img width="1190" alt="Screen Shot 2021-10-22 at 12 00 32 PM" src="https://user-images.githubusercontent.com/29058993/138416827-d4bbc22e-6685-4e54-8011-537e6b12fd0f.png">


## 使用備份建立新的 VM
a. 重覆步驟6移動至備份項目畫面，點選「還原VM」
<img width="1173" alt="Screen Shot 2021-10-22 at 12 03 21 PM" src="https://user-images.githubusercontent.com/29058993/138416842-f0ebb028-10d0-405b-962b-ed0f9a4a941f.png">

b. 選擇還原點，選擇剛剛的備份後，按下完成
<img width="1662" alt="Screen Shot 2021-10-22 at 12 04 31 PM" src="https://user-images.githubusercontent.com/29058993/138416857-829fdd75-4059-46e0-a531-b95d9c40d1e7.png">

c. 接著進入還原虛擬機器畫面，設定「還原設定」為*新建*，還原類型選擇「建立新的虛擬機器」。輸入「虛擬機器名稱」、「虛擬網路」、「子網路」、「預備位置」。完成後按*還原*
注意⚠️預備位置為您的儲存體帳戶，因為在還原時備份會暫存在儲存體帳戶，所以需要建立一儲存體帳戶
<img width="741" alt="Screen Shot 2021-10-22 at 12 05 12 PM" src="https://user-images.githubusercontent.com/29058993/138416870-92c92c9c-f8a2-47d9-b783-16717adea871.png">

d. 等待約數分鐘，在虛擬機器主控台可以看到新建立的VM *myVM1022Backup*
<img width="1656" alt="Screen Shot 2021-10-22-2at 1 31 45 PM" src="https://user-images.githubusercontent.com/29058993/138417122-6be5f726-774d-4631-9bbb-7cb1ecbb1707.png">


## 使用備份還原磁碟，並從範本自訂和建立 VM
a. 重覆上述步驟，移動至備份項目畫面，點選「還原VM」
<img width="1173" alt="Screen Shot 2021-10-22 at 12 03 21 PM" src="https://user-images.githubusercontent.com/29058993/138418386-632b47e4-c8f0-43b7-9c6b-839d1a5512fe.png">

b. 選擇還原點，選擇剛剛的備份後，按下完成
<img width="1662" alt="Screen Shot 2021-10-22 at 12 04 31 PM" src="https://user-images.githubusercontent.com/29058993/138418404-f37386ec-06e7-40cc-aaa8-37df1915cf55.png">

c. 接著進入還原虛擬機器畫面，設定「還原設定」為*新建*，還原類型選擇「還原磁碟」。輸入「預備位置」。完成後按*還原*
注意⚠️預備位置為您的儲存體帳戶，因為在還原時備份會暫存在儲存體帳戶，所以需要建立一儲存體帳戶
<img width="748" alt="Screen Shot 2021-10-22 at 1 20 58 PM" src="https://user-images.githubusercontent.com/29058993/138418424-eeeaac74-c25e-4fa1-91ac-56649f1af2cc.png">

d. 點選「通知欄」，點擊「正在觸發xxxx的還原」通知
<img width="316" alt="Screen Shot 2021-10-22 at 1 21 35 PM" src="https://user-images.githubusercontent.com/29058993/138418436-4a9cf465-e89d-4124-a52e-bfa2cb8ce5c6.png">

e. 進入Backup Jobs畫面，等待約1分鐘的時間，備份及完成。點擊列表中的備份資源最右邊的*View details*
<img width="1674" alt="Screen Shot 2021-10-22 at 1 23 07 PM" src="https://user-images.githubusercontent.com/29058993/138418448-6c7e943c-1c68-492a-8097-3dcdd9eceafa.png">

f. 進入Restore畫面，點選*Deploy Template*
<img width="1258" alt="Screen Shot 2021-10-22 at 1 24 09 PM" src="https://user-images.githubusercontent.com/29058993/138418467-4fe39b53-ab7b-48a5-9745-e251fb221d95.png">

g. 進入自訂範本的畫面。可以「使用編輯範本」透過json檔設定編輯，或透過下方設定操作。設定「資源群組」、「Virtual Machine Name」、「Virtual Network」、「Virtual Network Resource Group」、「Subnet」、「Os Disk Name
」、「Network Interface Prefix Name」、「Public Ip Address Name」。**注意⚠️由於此處設定為手動輸入非下拉式選單，因此您必須確認是否有填寫正確相關資源名稱，確保建立成功**。完成後滑動至下方點選「我同意上方所述的條款及條件」
<img width="725" alt="Screen Shot 2021-10-22 at 1 25 20 PM" src="https://user-images.githubusercontent.com/29058993/138418471-3d369862-d876-44e8-befe-dd53bc3570c1.png">

d. 等待約數分鐘，在虛擬機器主控台可以看到新建立的VM *myVM1022FromDisk*


## 刪除資源
### 刪除備份項目
a. 重覆步驟6移動至備份項目畫面，點選上方點選「停止備份」
<img width="1179" alt="Screen Shot 2021-10-22 at 3 39 09 PM" src="https://user-images.githubusercontent.com/29058993/138418512-be9b19f8-ee0f-4872-9034-e42468a87198.png">

b. 下拉選單選「刪除備份資料」、輸入備份項目的名稱、下拉選單選刪除原因。按停止備份
<img width="610" alt="Screen Shot 2021-10-22 at 3 39 36 PM" src="https://user-images.githubusercontent.com/29058993/138418527-eda2de7a-aa9e-49af-93e6-8e6ff9eef081.png">

c. 由於保存庫有設定「虛刪除」功能，所以等待14天後，備份真正被刪除，才能刪除保存庫

### 刪除VM
a. 到資源群組，勾選虛擬機相關資源進行刪除
