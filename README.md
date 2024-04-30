# Neutec Interview
demo url: https://neutec.zeabur.app/

## 額外需求

### 主頁面九宮格動畫

2. 四顆球同時朝同一個座標點移動：使用CSS Transform translate() 方法

### 側邊選單記憶功能

2. 請實作記憶功能，關閉分頁後再開啟，可以顯示上次選取的項目：使用localStorage儲存用戶上一次的選擇
3. 請在效能考量下，設計可收合、展開最多一百層的選單：當用戶展開選單時，再動態加載子選單的內容；或是實作Virtualize Long List，
   只渲染選單的一部分內容，減少DOM的數量
