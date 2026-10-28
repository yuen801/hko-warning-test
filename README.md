# HKO 天氣警告圖標 Dashboard

## 用途
喺 SenseCraft HMI dashboard 上面，即時顯示天文台現時生效嘅天氣警告。
**多個警告可以同時出現**，畫面會自動由左至右並排排列；警告越多，每個圖標自動縮細，唔會重疊或裁切。無任何警告時畫面留空。

## 資料來源（官方開放數據，CORS 開放）
```
https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType=warnsum&lang=tc
```
- 無警告時回傳 `{}`，頁面保持空白（屬正常，唔係壞）。
- 已自動過濾 `actionCode = CANCEL`（已取消嘅警告唔會顯示）。

## 檔案說明
- `warnings.html` — **正式使用版本**，圖標已 base64 內嵌，單一檔案。
- `warnings_test.html` — 獨立測試頁，寫死 4 個模擬警告，用嚟喺無警告時驗證部署同版面。

## 點樣喺「無警告」時驗證 link 係咪 work

### 方法一：正式版加測試參數（推薦）
正式 `warnings.html` 支援測試模式，link 後面加 `?test=1` 就會顯示模擬警告（三號風球＋黃雨＋雷暴＋寒冷），唔影響正式數據：
```
https://yuen801.github.io/hko-warning/warnings.html?test=1
```
見到 4 個圖標 = 證明 GitHub Pages 有正常出頁、內嵌圖標有載入、版面渲染正常。
確認無誤後，SenseCraft 改用唔帶參數嘅正式 link：
```
https://yuen801.github.io/hko-warning/warnings.html
```

### 方法二：獨立測試頁
```
https://yuen801.github.io/hko-warning/warnings_test.html
```
打開見到 4 個模擬圖標即代表部署成功。

### 點樣分「正常空白」定「link 壞咗」
- **正常**：純白色、無裂圖圖示、無 GitHub 404 文字。
- **壞咗**：見到 GitHub 404 頁、README 文字、或者裂圖（broken image）。

## 警告代號 → 圖標對照表（涵蓋全部 21 個官方圖標）

| API code | 警告 | 圖標檔 |
| --- | --- | --- |
| TC1 | 一號戒備信號 | tc1.png |
| TC3 | 三號強風信號 | tc3.png |
| TC8NE | 八號東北烈風或暴風信號 | tc8ne.png |
| TC8SE | 八號東南烈風或暴風信號 | tc8b.png |
| TC8SW | 八號西南烈風或暴風信號 | tc8c.png |
| TC8NW | 八號西北烈風或暴風信號 | tc8d.png |
| TC9 | 九號烈風或暴風風力增強信號 | tc9.png |
| TC10 | 十號颶風信號 | tc10.png |
| WRAINA | 黃色暴雨警告信號 | raina.png |
| WRAINR | 紅色暴雨警告信號 | rainr.png |
| WRAINB | 黑色暴雨警告信號 | rainb.png |
| WTS | 雷暴警告 | ts.png |
| WCOLD | 寒冷天氣警告 | cold.png |
| WHOT | 酷熱天氣警告 | vhot.png |
| WFIREY | 黃色火災危險警告 | firey.png |
| WFIRER | 紅色火災危險警告 | firer.png |
| WMSGNL | 強烈季候風信號 | sms.png |
| WL | 山泥傾瀉警告 | landslip.png |
| WFNTSA | 新界北部水浸特別報告 | ntfl.png |
| WFROST | 霜凍警告 | frost.png |
| WTMW | 海嘯警告 | tsunami-warn.png |

### 顯示排序（由左至右）
熱帶氣旋 → 暴雨 → 雷暴 → 寒冷 → 酷熱 → 火災 → 季候風 → 山泥傾瀉 → 新界北部水浸 → 霜凍 → 海嘯。

## 上傳步驟
1. 登入 `github.com/yuen801/hko-warning` → **Add file → Upload files**。
2. 拖入 `warnings.html`（覆蓋舊版）同 `warnings_test.html`（新增）→ **Commit changes**。
3. 等 1–2 分鐘，先用上面嘅測試 link 驗證，再喺 SenseCraft 加 HTML widget 貼正式 link。

## SenseCraft widget 尺寸建議
- 建議拉成一條**橫條**（例如 600 × 120 或更闊），單行由左至右排列最順眼。
- 圖標會跟隨 widget 高度自動縮放；橫條越闊，圖標越大。

## 注意事項
- 天文台官方警告圖標同 PSR 雨傘一樣，**原檔只有 50×50**；本檔已用官方原圖做 256px 高質素放大（LANCZOS＋銳化、白底）。
- 部分官方圖標本身**內嵌中英文字**（例如「冷 COLD」「SE 東南」「Amber 黃」），屬於天文台原圖設計，無法移除。
