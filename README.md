# tcssh-timetable-crawler
您可以透過此專案爬取臺中二中的上課課表。

## 前置套件

```bash
pip install requests        # Requests
pip install beautifulsoup4  # BeautifulSoup4
```

## 使用方式

```bash
python3 classList.py
```

## 重點程式碼

```python
# 藉由 requests 套件獲取使用者輸入的班級的課表網頁資料 (line 28)
r = requests.get("https://acdm3.tcssh.tc.edu.tw/csv3_web/SF4.ASP?CLA_NO=" + semesterId + groupId + classId)

# 將獲取到的網頁資料轉為 Big5 編碼 (line 29)
r.encoding = 'big5-hkscs'

# 班級課表網頁中含有意義的文字（老師名、課程名等）都含有 <font> 標籤，因此使用 bs4 套件將其整理為一個陣列 (line 32)
texts = soup.find_all("font")

# 從獲取到含有 <font> 標籤的資料中發現，星期與對應的第一堂課在陣列中的距離為 7 、一堂課與下一堂課的距離為 12 、 當堂課程與授課老師的距離為 5 ， 因而整理為下方迴圈 (line 36)
for i in range(1, 6):
    num = i + 7
    print(texts[i].string)
    while num < i + 8 + 12 * (classNum - 1):
        if len(texts[num].string) > 1:
            print(texts[num].string + ' - ' + texts[num + 5].string)
        num = num + 12
    print(' ')
```
