# 聚星路現場板｜人流與交通實況

天水圍**聚星路**互動 Web App：精美訊號燈 UI ＋ CocoPi 實時人臉／人數圖表 ＋ 運輸署公開資料幹道地圖。

**Live:** https://danielshtong.github.io/tin-shui-wai-traffic/

## 人流訊號（CocoPi）
| 等級 | 條件 |
|------|------|
| 🟢 綠（安全／正常） | ≤ 5 人 |
| 🔴 紅（人數偏高・請注意） | ＞ 5 人 |

- API（HTTPS）：`https://api.cocorobo.hk/iot/data/eventAPIKey/efee43b4132b83b082d15dd9cd3c7f83`
- 主欄位：**`no_of_face`**（後備：`no_of_people`／`count`／`people`）
- **每 60 秒**輪詢；每次成功讀取都會在圖表畫一點（含門檻線 y=5）
- 頁面顯示最近一次 IoT JSON；離線可用**課堂演示滑桿**

## 交通地圖（Open Data）
- 地圖中心：約 `22.459, 114.006`（聚星路一帶）
- 優先探測器：朗天路 `TDSLTR*`、天影路 `TDSTYR*`
- 紅：佔用 ≥60% 或 速度 ≤20 km/h；黃：佔用 ≥40% 或 速度 ≤40；其餘綠
- 資料：`https://resource.data.one.gov.hk/td/traffic-detectors/rawSpeedVol-all.xml`（每 60 秒）

## 技術
單一 `index.html`（Leaflet + Chart.js），GitHub Pages 由 `main` 發佈。
