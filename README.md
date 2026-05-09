# 🎐 風鈴 v0.14 — Wind Chime Simulator

[![Live Demo](https://img.shields.io/badge/🎐-Live%20Demo-blue)](https://rainbowminerw.github.io/wind-chimes/)
[![Download](https://img.shields.io/badge/⬇️-Download-green)](https://github.com/rainbowminerw/wind-chimes/raw/main/wind-chime.html)
[![GitHub](https://img.shields.io/badge/GitHub-Repo-181717?logo=github)](https://github.com/rainbowminerw/wind-chimes)

A beautiful wind chime simulator for your desktop.

一款優美的桌面風鈴模擬器，基於 Web Audio API 打造，完全離線使用。模擬風鈴隨機敲擊的聲音，配合玻璃質感 UI 與動態光球視覺效果，創造永不重複的聽覺體驗。

> ⚠️ **注意**：本軟體由 AI 編程生成，請在使用前自行評估程式碼安全性，不保證完全缺陷。

## ✨ 特色

- 🎼 **場景模板系統** — 四種風格一鍵切換：
  - 🧘 禪風冥想 — 五聲懸浮和弦（C G D A / Am7 / Dm7），中式靜心
  - 🌻 鄉村花園 — G 大調 Gmaj7 / Am7 / D7，輕快活潑
  - 🎷 Jazz Ambient — Melodic Minor CmM7 / Fm7 / Dm7b5，慵懶深夜
  - 🌌 **星空夜語** — 敘事弧線：神秘(Dm9) → 壯美(Gmaj9) → 空靈(Bm7b5) → 舒眠(Dm7)
- 🌬️ **自然風效果** — 兩層 Perlin Noise 疊加（週期 8s + 3s），S 曲線對比度拉伸，風速平滑變化驅動敲擊密度與力度
- 🎵 **五種音色**：玻璃、💎水晶音效（真實 Sample）、陶瓷、木質、敲弦（FMSynth）
- 🌊 **四個段落**：自動循環，段落間共同音設計（60%~80% 保留），轉換順滑
- 🌟 **動態光球粒子**：隨音樂波動，視聽一體的沉浸體驗
- 📦 **離線使用**：單一 HTML 檔，無需安裝或網路連線
- 🎚️ **即時控制**：速度滑桿、音量、音色、殘響，切模板時自動同步

## 🚀 使用方式

<p align="center">
  <a href="https://rainbowminerw.github.io/wind-chimes/">
    <img src="https://img.shields.io/badge/🎮-線上演示-2ea44f?style=for-the-badge&logo=google-chrome&logoColor=white" alt="線上演示">
  </a>
  &nbsp;&nbsp;
  <a href="https://github.com/rainbowminerw/wind-chimes/raw/main/wind-chime.html">
    <img src="https://img.shields.io/badge/⬇️-下載檔案-0077b6?style=for-the-badge&logo=files&logoColor=white" alt="下載">
  </a>
</p>

直接在瀏覽器打開 `wind-chime.html`，點擊「開始」按鈕即可。

技術要求：支援 Web Audio API 的現代瀏覽器（Chrome、Firefox、Edge、Safari）。

## ⚙️ 核心機制

### 音符間隔計算鏈（六層疊加）

```
avgInterval = 1 / (density × 1.8)

  ① 基底間隔    密度決定平均間隔
  ② 基底搖擺     × [0.5, 1.5]（均勻隨機 ±50%）
  ③ 基速因子     ÷ (bpm / 60)
  ④ 隨機修飾     25% burst (×0.5) | 10% 稀疏 (×2)
  ⑤ 風速因子     × 2^p，p = 1.5 - wind × 5.0
                 無風→慢(p=1.5)，強風→快(p=-3.5)
  ⑥ 安全裁切     clamp [0.1s, 5s]
```

### 風速系統

- **Perlin Noise 1D** 兩層疊加（慢層 8s×0.35 + 快層 3s×0.65）
- **S 曲線對比度拉伸**，解決常態分布中間值過多的問題
- 風速（0~1）映射為 **p 值（1.5 ~ -3.5）**，間隔 × 2^p 調變

## 🛡️ 安全性與隱私

- **無外部引用**：所有依賴（Tone.js）已內嵌於單一 HTML 檔案中
- **無資訊蒐集**：不發送任何統計、追蹤或分析資料
- **無須聯網**：開啟後可完全離線使用
- **不向外傳遞資訊**：程式碼中沒有任何網路請求

## 📂 相關檔案

| 檔案 | 說明 |
|------|------|
| `wind-chime.html` | 主程式（單一 HTML） |
| `00_前期研究.md` | 完整研究記錄、數學原理、實作決策 |
| `README.md` | 本檔案 — 快速入門與功能概覽 |
| `端點拉伸加成_說明.md` | 端點拉伸（冪次映射）數學原理，適合高中生程度 |
| `端點拉伸演示.html` | 可互動的端點拉伸演示工具（p 範圍 -5 ~ 5） |

## 📜 授權

本軟體採用 **MIT License** — 詳見 [LICENSE](LICENSE) 檔案。

## 🙏 致謝

本軟體使用了以下開源專案與資源，在此致上誠摯謝意：

### Tone.js v14.7.77

音頻引擎核心，提供 Web Audio API 的高階封裝與合成器功能。

- **作者**：Yotam Mann 與 Tone.js 社群
- **授權**：MIT License
- **官網**：https://tonejs.github.io/
- **Copyright**：© 2014-2019 Yotam Mann
- **原始碼**：https://github.com/Tonejs/Tone.js

Tone.js 已內嵌於 `wind-chime.html` 檔案中，確保完全離線使用。

### 水晶音效 Sample（Pixabay）

水晶撞擊音效（`samples/crystal_sample.mp3`）使用 **Pixabay License**：

- **來源**：https://pixabay.com/
- **授權**：Pixabay License — 免費使用，無需標註來源，可用於商業用途
- **檔案**：已以 base64 內嵌於 `wind-chime.html`，離線播放無須外部載入

---

## 🗺️ 預計完成事項

> ⚠️ **開發者註：** 目前效果還不成熟，聽感更接近即興彈奏而非真實風鈴。
> 風鈴模擬需要更細緻的物理模型與取樣音才能達到一陣一陣連續清脆聲的自然效果。
> 目前仍在實驗階段，請放寬心聆聽 🎐

### ✅ 已完成

| 版本 | 內容 |
|------|------|
| v0.03~v0.04 | 🌬️ 自然風效果（Perlin Noise）、🎚️ 速度滑桿整合、📊 四層速率疊加 |
| v0.05 | 🖼️ 模板切換系統（禪風/Jazz/花園） |
| v0.06~v0.07 | 🎹 和弦思維音階、段落間音階變化（ABAC）、撥弦音色實作 |
| v0.08~v0.09 | 四個模板全面和弦化、敲弦風鈴（FMSynth）、星空夜語模板 |
| v0.11 | 🔄 風速因子改為 2^p 映射（p=-3.5~1.5）、隨機修飾移至③、裁切放寬至[0.1,5]、點擊觸發音符 |
| v0.11 | 💎 新增水晶音效（Tone.Player + Sample）、音頻鏈路 Player→Reverb→MasterGain、playbackRate 映射 |
| v0.12 | 🔧 水晶 playbackRange 放寬至 [0.25, 3.5]，解鎖完整七音音高範圍 |
| v0.12 | 🧘 禪風模板改為五聲懸浮和弦方案（C G D A / Am7 / Dm7），段落間 4/7 共同音設計 |
| v0.12 | 🌌 **星空夜語全面重構**：神秘(Dm9)→壯美(Gmaj9)→空靈(Bm7b5)→舒眠(Dm7)，密度 0.30~0.55，BPM 65 |
| v0.13 | 🎨 **CSS 速度/殘響 input width:100% 自適應**：.control-item.half 移除 inline flex:1，.control-row 加 min-width:0，移除多餘 max-width 限制 |
| v0.13 | 🎵 水晶音色 baseFreq 130.82(C3)、playbackRate 0.25~3.5，音量重平衡（glass+40%、ceramic/wood -20%），移除 metal preset |
| v0.13 | 🎼 模板各延長 ~60%（新和弦：禪風/星空/花園/Jazz），crossfade 4s（SECTION_FADE=4, rampTo），預設音量 50% |
| v0.14 | 💎 **水晶 baseFreq 重設為 659.25(E5)**——對應 sample 原始音高，C4=rate 0.40，G6=rate 2.38 |
| v0.14 | 📊 **playbackRange 放寬為 [0.15, 4.0]**——涵蓋更廣音域（C2~G7+） |
| v0.14 | 🔇 **預設音量從 50 降為 40**——更安靜的初始體驗 |

### 🔜 待規劃

- 🎼 **音樂模板擴充機制**
  - 模板定義系統：音階、節奏型態、段落結構的模組化設計
  - 可載入外部模板檔案（JSON 格式），讓使用者自訂
  - 預設模板：禪風、自然、夢幻、沉靜等多種風格

- 🎚️ **音色優化**
  - 各音色增加微妙的隨機變化（pitch drift、輕微失諧）
  - ✅ 取樣音（sample-based）混合 — 水晶音效已實現（Tone.Player + base64 內嵌）

---

*🎐 願風鈴為您帶來片刻寧靜*
