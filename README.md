# arXiv 論文爬蟲

AIPE 期中專案。抓取 [arXiv](https://arxiv.org/) 上的論文資料，整理後存成 CSV 與 MySQL 資料庫，並提供關鍵字查詢。

## 功能

- 透過 arXiv 官方 API 依分類或關鍵字搜尋論文，可指定筆數與排序
- 用 BeautifulSoup 解析回傳的 Atom XML，取出標題、摘要、發表日期、作者群、論文網址
- 整理成 pandas DataFrame，可匯出 CSV（`utf-8-sig`，Excel 開啟不亂碼）
- 寫入 MySQL：論文、作者、搜尋紀錄分成三張表，重複論文自動略過
- 從資料庫查詢：全部論文、關鍵字篩選、查某篇論文的所有作者、查看歷次爬取紀錄
- 全部封裝成 `Arxiv` 類別，資料庫帳密從 `config.ini` 讀取，不寫死在程式裡

## 環境需求

Python 3.12 以上（專案的 `.venv` 是 3.13.7）、MySQL 8.0 以上。

```bash
pip install requests beautifulsoup4 lxml pandas pymysql
```

## 設定

資料庫帳密放在 `config.ini`，這個檔案**不進版控**（已在 `.gitignore` 裡）。
複製範本後填入自己的設定：

```bash
cp config.ini.example config.ini
```

```ini
[DB]
host = localhost
user = root
password = 你的密碼
port = 3306
database = arxiv_db
```

## 資料庫結構

執行 `connect_db()` 時會自動建立資料庫與下列三張表：

| 資料表 | 用途 | 主要欄位 |
|---|---|---|
| `papers` | 論文主表 | `title`, `summary`, `published`, `link`（UNIQUE，用來擋重複） |
| `authors` | 作者表 | `paper_id`（FK → papers.id）, `name` |
| `search_logs` | 爬取紀錄 | `query`, `result_count`, `searched_at` |

一篇論文通常有多位作者，所以 `papers` 與 `authors` 拆成一對多；`link` 設為 UNIQUE，重複爬到同一篇時會觸發 `IntegrityError` 並被略過，因此可以反覆搜尋不同關鍵字累積資料。

## 使用方式

打開 `arxiv_oop_final.ipynb`，由上往下執行。核心用法：

```python
crawler = Arxiv(max_results=10)
crawler.search("cat:cs.CV")      # 搜尋電腦視覺分類的最新論文
crawler.paper_dataFrame()        # 看成表格
crawler.show_paper(0)            # 看第一篇的完整內容
crawler.save_csv("cv_papers.csv")

crawler.connect_db()             # 連線並建表
crawler.save_db()                # 存進資料庫

crawler.search("cat:cs.CL")      # 換關鍵字再搜，資料繼續累積
crawler.save_db()

crawler.query_papers("recognition")   # 從資料庫用關鍵字查
crawler.query_authors(1)              # 查第 1 篇論文的所有作者
crawler.show_logs()                   # 歷次爬取紀錄
crawler.close_db()
```

搜尋語法沿用 arXiv API，例如 `cat:cs.CV`（分類）、`all:transformer`（全欄位關鍵字）、`au:Hinton`（作者）。

## 檔案說明

| 檔案 | 說明 |
|---|---|
| `arxiv_oop_final.ipynb` | **最終成果**。完整的 `Arxiv` 類別與使用示範 |
| `arxiv_crawler_oopandsql1.ipynb` | 分步驟講解版，從發送請求一路推導到 OOP 封裝，附文字說明 |
| `arxiv1.ipynb` | 開發草稿，保留當時的執行結果 |
| `config.ini.example` | 資料庫設定範本 |
| `cv_papers_results=2.csv` | 早期測試輸出的範例資料 |
| `archive/` | 開發過程的中間版本，另有 README 說明各版差異 |

## 備註

`arxiv_crawler_oopandsql1.ipynb` 的執行結果已清空，需要看輸出的話請自行重新執行。
