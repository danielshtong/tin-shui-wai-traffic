# 天水圍｜人流＋交通雙條件示範

用地圖顯示天水圍（及周邊幹道）運輸署交通探測器的實時車速、流量與佔用率，並加入 **CocoPi 人數（條件1）** 與 **交通黃／紅（條件2）** 雙條件升級邏輯。

## 即時網頁

https://danielshtong.github.io/tin-shui-wai-traffic/

## 雙條件規則

| 條件 | 觸發方式 |
|------|----------|
| **條件1（人數）** | CocoPi／演示：`level` 為 yellow 或 red，**或** `surge`＝true，**或** `count` ≥ 可調門檻 N（預設 4；演示 yellow≥4／red≥7） |
| **條件2（交通）** | 附近探測器（見下）任一為黃或紅（與地圖顏色相同） |

- **兩樣齊** → 橫幅顯示「**建議通知執法**」（附人數＋交通原因）
- **只得條件1** →「人數達門檻，交通仍暢通」
- **只得條件2** →「交通擠塞，人數未達門檻」

### 「附近」探測器定義

地圖上所有 **天影路 `TDSTYR*`** 與 **朗天路 `TDSLTR*`** 探測器（優先核心站）。不按半徑，按路段前綴，方便課堂對應 open data 清單。

### 條件1 資料欄位（建議）

```json
{
  "device_id": "cocopi-tsw-01",
  "zone": "tin-shui-wai",
  "count": 6,
  "level": "yellow",
  "surge": false,
  "ts": "2026-10-17T20:07:00+08:00"
}
```

- 觸發：`level` 為 yellow／red，**或** `surge`＝true，**或** `count` ≥ N（預設 **4**）
- 演示滑桿會自動對齊約 **yellow≥4／red≥7**
- 可選欄位 `device_id`、`zone` 會顯示在條件1面板
- 左側有演示控制＋可選 **IoT JSON URL**（GET）；填入後「載入 IoT」並約每 15 秒輪詢

## 資料來源（條件2）

- [主要幹道及道路交通數據](https://data.gov.hk/tc-data/dataset/hk-td-sm_4-traffic-data-strategic-major-roads)（運輸署）
  - 實時 XML：`https://resource.data.one.gov.hk/td/traffic-detectors/rawSpeedVol-all.xml`
  - 探測器位置已內嵌（避免 CSV 無 CORS）
- （可選）[特別交通消息](https://www.td.gov.hk/tc/special_news/trafficnews.xml)

本示範**不使用**智慧燈柱數據（天水圍無覆蓋）。

## 交通顏色說明（條件2，房間議定）

| 顏色 | 條件 |
|------|------|
| 紅（擠塞） | 佔用率 ≥ 60% **或** 車速 ≤ 20 km/h |
| 黃（一般） | 佔用率 ≥ 40% **或** 車速 ≤ 40 km/h |
| 綠（暢通） | 其餘 |
| 灰 | 該時段無有效車道數據 |

地圖標記色與雙條件「附近探測器黃／紅」判斷用同一套規則。交通約每 90 秒自動刷新。

## 技術

- 單頁 `index.html`（Leaflet + OpenStreetMap）
- 實時 XML 支援 CORS，瀏覽器直接抓取
- 條件1 可用演示控制或外部 IoT JSON（需該端點允許 CORS）

## 本機預覽

```bash
cd tin-shui-wai-traffic
python3 -m http.server 8080
# 開啟 http://localhost:8080
```
