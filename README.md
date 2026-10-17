# 天水圍實時交通狀況

用地圖顯示天水圍（及周邊幹道）運輸署交通探測器的實時車速、流量與佔用率。

## 即時網頁

https://danielshtong.github.io/tin-shui-wai-traffic/

## 資料來源

- [主要幹道及道路交通數據](https://data.gov.hk/tc-data/dataset/hk-td-sm_4-traffic-data-strategic-major-roads)（運輸署）
  - 實時 XML：`https://resource.data.one.gov.hk/td/traffic-detectors/rawSpeedVol-all.xml`
  - 探測器位置 CSV（已內嵌天水圍相關站點，避免 CSV 無 CORS）
- （可選）[特別交通消息](https://www.td.gov.hk/tc/special_news/trafficnews.xml)

本示範**不使用**智慧燈柱數據（天水圍無覆蓋）。

## 顏色說明

| 顏色 | 條件 |
|------|------|
| 綠（暢通） | 平均車速 ≥ 45 km/h 且 佔用率 &lt; 18% |
| 黃（一般） | 車速 &lt; 45 或 佔用率 ≥ 18% |
| 紅（擠塞） | 車速 &lt; 25 或 佔用率 ≥ 35% |
| 灰 | 該時段無有效車道數據 |

高流量但高車速仍顯示綠色（暢通）。

## 技術

- 單頁 `index.html`（Leaflet + OpenStreetMap）
- 實時 XML 支援 CORS（`Access-Control-Allow-Origin: *`），瀏覽器直接抓取
- 約每 90 秒自動刷新

## 本機預覽

```bash
cd tin-shui-wai-traffic
python3 -m http.server 8080
# 開啟 http://localhost:8080
```
