# Week2推薦系統-Content-based recommendation<br>
### 目的<br>
本周課程實作Content-based推薦系統，基於使用者的喜好，根據使用者購買過的商品推薦與商品屬性相似的商品組合，<br>
<br>
### 實作發現-Content-based系統優缺點<br>
在實作過程中發現，以Content-based為出發點，優點是能夠使用者購買過的商品去推薦其他類似商品、推薦方式很直覺，<br>
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
step1: 先以不同特徵值計算相似度做推薦，找出最適特徵值(分數最高者)<br>
step2: 以第一步驟找出的最適特徵值相似度做舊客推薦，輔以高評分、高購買率新品做新客推薦<br>
<br>
### 實作過程<br>
#### 資料整理及切分<br>
---metadata---<br>
1.將時間戳記欄位轉換成年月日，較易理解及計算<br>
2.去除重複、取代空值、置換空list、選取所需欄位<br>
3.整理欄位rank、price，及合併所需欄位(data_all、data_tbc、data_tc)<br>
<br>
---Ratings---<br>
1.切分資料集<br>
2.將評價數取平均、計算商品銷售數量<br>
<br>
#### EDA<br>
觀察metadata欄位空值、畫出/統計各類別中資料分布狀況<br>
發現title及rank拆出的品項分類(rank_category)欄位資料較完整，適合用做商品特徵<br>
brand、price欄位空值多，有嘗試做商品特徵，但效果不佳<br>
description欄位與title欄位較類似，本次未使用<br>
<br>
#### 推薦方式<br>
part1: <br>
先以不同特徵值計算相似度做推薦，找出最適特徵值，最後取rank_category相似度做為舊客推薦方式<br>
(即以消費者曾經購買過的商品之相關品項類別，相似度最高者為推薦)<br>
<br>
part2: <br>
沿用part1的舊客推薦方式，輔以近期熱銷商品推薦給新客<br>
<br>
#### 推薦結果評分<br>
part1: 比較不同商品特徵值結果<br>
| method   |  Description      | score |
|----------|:-----------------:|------:|
|     1    |    by title       |   0   |
|     2    |    by brand       |   0   |
|     3    |      by price     |   0   |
|     4    | by rank_category  | 0.0256|
|     5    | by title&category |   0   |
|     6    |    by t,c,b       |   0   |
|     7    |    by all         |   0   |
<br>
part2: 比較不同時間段結果<br>
| method   |  Description        |  score |
|----------|:-------------------:|------ :|
|     1    |2020-01-01~2018-08-31|0.081356|
|     2    |2018-01-01~2018-08-31|0.096610|
|     3    |2018-03-01~2018-08-31|0.093220|
|     4    |2018-06-01~2018-08-31|0.132203|
|     5    |2018-08-01~2018-08-31|0.154237|
<br>
最終推薦準確率為15.42%<br>
<br>
### 結果發現
在各種品項特徵值中，品項類別的表現結果最好，推測可能是因為資料較完整、且資料集中的消費者購買時較容易購買到同類別的商品<br>
因為資料集終舊客較少的關係，除了content_based推薦，若搭配rule-based能提高準確率<br>

