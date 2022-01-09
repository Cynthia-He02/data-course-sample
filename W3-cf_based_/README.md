# Week3推薦系統-Collaborative filtering recommendation<br>
### 目的<br>
本周課程實作Collaborative filtering推薦系統，基於使用者及商品間的關係，以推薦使用者所在的群組中熱門的項目(User-based)或推薦被同一群使用者共同喜歡的商品(Item-based)。<br>
<br>
### 實作發現-Collaborative filtering系統優缺點<br>
在實作過程中發現，以Collaborative filtering為出發點，優點是增加推薦多樣性，不限於使用者喜歡類型的商品<br>
缺點則是當重複購買的使用者不多時，效果不顯著。<br>
<br>
### 使用資料集&欄位<br>
--資料集--<br>
http://deepyeti.ucsd.edu/jianmo/amazon/categoryFilesSmall/All_Beauty.csv<br>
http://deepyeti.ucsd.edu/jianmo/amazon/metaFiles2/meta_All_Beauty.json.gz<br>
<br>
--使用欄位--<br>
Rating: asin(商品ID)、reviewerID(評論者ID)、overall(商品評分)、unixReviewTime(評分時間)<br>
metadata: title(商品名稱)、brand(品牌)、rank(商品排名)、price(商品價格)、asin(商品ID)<br>
<br>
--資料集時間--<br>
訓練資料：2000-01-10 - 2018-09-01<br>
測試資料：2018-09-01 - 2018-09-30<br>
<br>
<br>
### 推薦邏輯<br>
step1: 計算不同使用者/商品間的相似度，去推薦同樣喜歡<br>
step2: 偕同過濾無法規件的商品，以規則推薦補足<br>
<br>
### 實作過程<br>
#### 資料整理及EDA<br>
---Ratings---<br>
1.調整日期欄位、去除重複資料<br>
2.切分資料集<br>
3.觀察使用者評分狀態<br>
4.觀察評分(overall)欄位狀況<br>

#### 推薦方式-比較不同偕同過濾搭配規則推薦方式<br>
◆ part1: cf-user-based (recommender1)<br>
結合user-based偕同過濾及規則推薦◆<br>
延續前兩周發現之結果，規則推薦採近期高分熱銷品推薦<br>
<br>
◆ part2:cf-item-based (recommender2) <br>
結合item-based偕同過濾及規則推薦<br>
延續前兩周發現之結果，規則推薦採近期高分熱銷品推薦<br>
<br>
◆ part3:using_surprise (recommender3) <br>
結合surprise模型偕同過濾及規則推薦<br>
延續前兩周發現之結果，規則推薦採近期高分熱銷品推薦<br>
<br>
#### 推薦結果評分<br>
part1: 比較不同商品特徵值結果<br>
| method   |  Description      | score |
|----------|:-----------------:|------:|
|     1    |    cf_user+rule   |0.15762|
|     2    |    cf_item+rule   |0.15593|
|     3    |  cf_surprise+rule |0.15762|
<br>
### 結果發現<br>
三種偕同過濾的結果分數相差不大<br>
因為資料集舊客較少，除了Collaborative filtering推薦，若搭配rule-based能提高準確率<br>

