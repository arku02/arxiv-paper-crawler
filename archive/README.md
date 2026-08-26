# archive — 開發過程的舊版本

這裡放的是期中專案開發過程中的中間版本，**不是最終成果**。
最終版本在上層目錄，見主 README。

依時間順序（2026/05）：

| 檔案 | 日期 | 這一版新增了什麼 |
|---|---|---|
| `arxiv.ipynb` | 05/13 | 第一次試打 arXiv API，確認回傳格式 |
| `arxiv_crawler_detailed.ipynb` | 05/13 | 逐步解析 XML、整理成 DataFrame、存 CSV |
| `arxiv_crawler_bs4.ipynb` | 05/16 | 改用 BeautifulSoup 解析 |
| `arxiv_crawler_oop.ipynb` | 05/16 | 開始封裝成 class |
| `arxiv_crawler_oop_CSV.ipynb` | 05/17 | class 加上 CSV 匯出 |
| `arxiv_crawler_oop_and _MySQL.ipynb` | 05/20 | 接上 MySQL，建立 papers / authors / search_logs 三張表 |
| `arxiv_crawler_oop_step5and6_mysql.ipynb` | 05/20 | 補上查詢與紀錄表的步驟五、六 |
| `arxiv_crawler_oop_step5and6_mysqlandwith.ipynb` | 05/20 | 改用 `with` 語法管理 cursor（與上一版 95% 相同） |
| `arxiv_crawler_oop_step5and6_mysqlandwith_475with.ipynb` | 05/20 | 階段四點七五，連線部分也改用 with |
| `arxiv_crawler_oopandsql0.ipynb` | 05/20 | 帳密改從 `config.ini` 讀取（與 oopandsql1 91% 相同） |

註：這些檔案的執行結果多半已清空，直接開啟看不到輸出。
