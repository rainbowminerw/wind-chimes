# 🎐 風鈴 v0.14 — Wind Chime Simulator

[![Live Demo](https://img.shields.io/badge/🎐-Live%20Demo-blue)](https://rainbowminerw.github.io/wind-chimes/)
[![Download](https://img.shields.io/badge/⬇️-Download-green)](https://github.com/rainbowminerw/wind-chimes/raw/main/wind-chime.html)
[![GitHub](https://img.shields.io/badge/GitHub-Repo-181717?logo=github)](https://github.com/rainbowminerw/wind-chimes)

A beautiful wind chime simulator for your desktop.

一款優美的桌面風鈴模擬器，基於 Web Audio API 打造，完全離線使用。模擬風鈴隨機敲擊的聲音，配合玻璃質感 UI 與動態光球視覺效果，創造永不重複的聽覺體驗。

A beautiful desktop wind chime simulator built with Web Audio API, fully offline. It simulates random wind chime strikes with a glass-textured UI and dynamic glowing particle visuals, creating a never-repeating auditory experience.

> ⚠️ **注意**：本軟體由 AI 編程生成，請在使用前自行評估程式碼安全性，不保證完全缺陷。
>
> ⚠️ **Note**: This software is AI-generated. Please evaluate code security before use. No warranty is provided.

## ✨ 特色 / Features

### 🎼 場景模板系統 / Scene Template System

四種風格一鍵切換 ｜ Four scene styles, one-click switch:

- **🧘 禪風冥想 / Zen Meditation** — 五聲懸浮和弦（C G D A / Am7 / Dm7），中式靜心
  Pentatonic suspended chords (C G D A / Am7 / Dm7), Chinese zen-style tranquility
- **🌻 鄉村花園 / Country Garden** — G 大調 Gmaj7 / Am7 / D7，輕快活潑
  G major Gmaj7 / Am7 / D7, bright and cheerful
- **🎷 Jazz Ambient** — Melodic Minor CmM7 / Fm7 / Dm7b5，慵懶深夜
  Melodic Minor CmM7 / Fm7 / Dm7b5, languid late-night vibe
- **🌌 星空夜語 / Starry Night** — 敘事弧線：神秘(Dm9) → 壯美(Gmaj9) → 空靈(Bm7b5) → 舒眠(Dm7)
  Narrative arc: Mysterious(Dm9) → Grand(Gmaj9) → Ethereal(Bm7b5) → Slumber(Dm7)

### 🌬️ 自然風效果 / Natural Wind Effect

兩層 Perlin Noise 疊加（週期 8s + 3s），S 曲線對比度拉伸，風速平滑變化驅動敲擊密度與力度

Two-layer Perlin Noise (periods 8s + 3s) with S-curve contrast stretching. Smooth wind speed variation drives strike density and intensity.

### 🎵 五種音色 / Five Timbres

玻璃 Glass ｜ 💎水晶 Crystal（真實 Sample） ｜ 陶瓷 Ceramic ｜ 木質 Wood ｜ 敲弦 FMSynth

### 🌊 四個段落 / Four Sections

自動循環，段落間共同音設計（60%~80% 保留），轉換順滑

Auto-looping with shared-note design between sections (60%~80% retention), seamless transitions.

### 🌟 動態光球粒子 / Dynamic Particle Glow

隨音樂波動，視聽一體的沉浸體驗

Particles pulse with the music for an immersive audiovisual experience.

### 📦 離線使用 / Fully Offline

單一 HTML 檔，無需安裝或網路連線

Single HTML file, no installation or network required.

### 🎚️ 即時控制 / Real-time Controls

速度滑桿、音量、音色、殘響，切模板時自動同步

Speed slider, volume, timbre, reverb — auto-sync when switching templates.

## 🚀 使用方式 / How to Use

<p align="left">
  <a href="https://rainbowminerw.github.io/wind-chimes/">
    <img src="https://img.shields.io/badge/🎮-線上演示-2ea44f?style=for-the-badge&logo=google-chrome&logoColor=white" alt="線上演示 Live Demo">
  </a>
  &nbsp;&nbsp;
  <a href="https://github.com/rainbowminerw/wind-chimes/raw/main/wind-chime.html">
    <img src="https://img.shields.io/badge/⬇️-下載檔案-0077b6?style=for-the-badge&logo=files&logoColor=white" alt="下載 Download">
  </a>
</p>

由上面按鈕直接開啟演示；或下載後，直接在瀏覽器打開 `wind-chime.html`，點擊「開始」按鈕即可。

Click the button above to open the live demo, or download `wind-chime.html` and open it in your browser. Click the "Start" button to begin.

技術要求：支援 Web Audio API 的現代瀏覽器（Chrome、Firefox、Edge、Safari）。

Requirement: A modern browser with Web Audio API support (Chrome, Firefox, Edge, Safari).

## ⚙️ 核心機制 / Core Mechanics

### 音符間隔計算鏈（六層疊加） / Note Interval Calculation Chain (6-layer Stack)

```
avgInterval = 1 / (density × 1.8)

  ① 基底間隔    密度決定平均間隔
     Base interval determined by density
  ② 基底搖擺     × [0.5, 1.5]（均勻隨機 ±50%）
     Base swing × [0.5, 1.5] (uniform random ±50%)
  ③ 基速因子     ÷ (bpm / 60)
     Tempo factor ÷ (bpm / 60)
  ④ 隨機修飾     25% burst (×0.5) | 10% 稀疏 (×2)
     Random modifier: 25% burst (×0.5) | 10% sparse (×2)
  ⑤ 風速因子     × 2^p，p = 1.5 - wind × 5.0
                 無風→慢(p=1.5)，強風→快(p=-3.5)
     Wind factor × 2^p, p = 1.5 - wind × 5.0
                 Calm→slow(p=1.5), Windy→fast(p=-3.5)
  ⑥ 安全裁切     clamp [0.1s, 5s]
     Safety clamp [0.1s, 5s]
```

### 風速系統 / Wind System

- **Perlin Noise 1D** 兩層疊加（慢層 8s×0.35 + 快層 3s×0.65）
  Two-layer overlay (slow 8s×0.35 + fast 3s×0.65)
- **S 曲線對比度拉伸**，解決常態分布中間值過多的問題
  S-curve contrast stretching to mitigate normal distribution clustering
- 風速（0~1）映射為 **p 值（1.5 ~ -3.5）**，間隔 × 2^p 調變
  Wind speed (0~1) maps to **p value (1.5 ~ -3.5)**, interval × 2^p modulation

## 🛡️ 安全性與隱私 / Security & Privacy

- **無外部引用**：所有依賴（Tone.js）已內嵌於單一 HTML 檔案中
  **No external references**: All dependencies (Tone.js) are embedded in the single HTML file
- **無資訊蒐集**：不發送任何統計、追蹤或分析資料
  **No data collection**: No analytics, tracking, or telemetry
- **無須聯網**：開啟後可完全離線使用
  **No network required**: Works fully offline after opening
- **不向外傳遞資訊**：程式碼中沒有任何網路請求
  **No outbound communication**: Zero network requests in the code

## 📂 相關檔案 / Related Files

| 檔案 File | 說明 Description |
|-----------|-----------------|
| `wind-chime.html` | 主程式（單一 HTML）／ Main program (single HTML) |
| `00_前期研究.md` | 完整研究記錄、數學原理、實作決策 ／ Research notes, math, implementation decisions |
| `README.md` | 本檔案 — 快速入門與功能概覽 ／ This file — quick start & overview |
| `端點拉伸加成_說明.md` | 端點拉伸（冪次映射）數學原理 ／ Endpoint stretching (power mapping) math |
| `端點拉伸演示.html` | 可互動的端點拉伸演示工具 ／ Interactive endpoint stretching demo |
| `LICENSE` | MIT 授權條款 ／ MIT License |

## 📜 授權 / License

本軟體採用 **MIT License** — 詳見 [LICENSE](LICENSE) 檔案。

This software is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

## 🙏 致謝 / Acknowledgements

本軟體使用了以下開源專案與資源，在此致上誠摯謝意：

This project uses the following open-source projects and resources, to which we express our sincere gratitude:

### Tone.js v14.7.77

音頻引擎核心，提供 Web Audio API 的高階封裝與合成器功能。

Audio engine core, providing high-level Web Audio API abstractions and synthesizer capabilities.

- **作者 / Author**：Yotam Mann 與 Tone.js 社群 / Yotam Mann & the Tone.js community
- **授權 / License**：MIT License
- **官網 / Website**：https://tonejs.github.io/
- **Copyright**：© 2014-2019 Yotam Mann
- **原始碼 / Source**：https://github.com/Tonejs/Tone.js

Tone.js 已內嵌於 `wind-chime.html` 檔案中，確保完全離線使用。

Tone.js is embedded in `wind-chime.html` for fully offline use.

### 水晶音效 Sample / Crystal Sound Sample（Pixabay）

水晶撞擊音效（`samples/crystal_sample.mp3`）使用 **Pixabay License**：

Crystal strike sound effect licensed under **Pixabay License**:

- **來源 Source**：https://pixabay.com/
- **授權 License**：Pixabay License — 免費使用，無需標註來源，可用於商業用途
  Free to use, no attribution required, commercial use allowed
- **檔案 File**：已以 base64 內嵌於 `wind-chime.html`，離線播放無須外部載入
  Embedded as base64 in `wind-chime.html`, no external loading needed

---

## 🗺️ 預計完成事項 / Roadmap

> ⚠️ **開發者註 / Developer Note：** 目前效果還不成熟，聽感更接近即興彈奏而非真實風鈴。
> 風鈴模擬需要更細緻的物理模型與取樣音才能達到一陣一陣連續清脆聲的自然效果。
> 目前仍在實驗階段，請放寬心聆聽 🎐
>
> The current effect is still experimental — more like improvisational playing than real wind chimes.
> True wind chime simulation requires finer physical modeling and sampled sounds.
> Listen with an open heart 🎐

### ✅ 已完成 / Completed

| 版本 Ver | 內容 Content |
|----------|-------------|
| v0.03~v0.04 | 🌬️ 自然風效果（Perlin Noise）、🎚️ 速度滑桿整合、📊 四層速率疊加 |
| | Natural wind (Perlin Noise), speed slider, 4-layer rate stacking |
| v0.05 | 🖼️ 模板切換系統（禪風/Jazz/花園）Template switching (Zen/Jazz/Garden) |
| v0.06~v0.07 | 🎹 和弦思維音階、段落間音階變化（ABAC）、撥弦音色實作 |
| | Chord-based scale, section-to-section variation (ABAC), pluck timbre |
| v0.08~v0.09 | 四個模板全面和弦化、敲弦風鈴（FMSynth）、星空夜語模板 |
| | All 4 templates chordified, struck chime (FMSynth), Starry Night template |
| v0.11 | 🔄 風速因子改為 2^p 映射（p=-3.5~1.5）、隨機修飾移至③、裁切放寬至[0.1,5]、點擊觸發音符 |
| | Wind factor → 2^p mapping, random modifier repositioned, clamp [0.1,5], click-to-play |
| v0.11 | 💎 新增水晶音效（Tone.Player + Sample）、playbackRate 映射水晶 | Crystal timbre (Tone.Player + Sample), playbackRate mapping |
| v0.12 | 🔧 水晶 playbackRange 放寬至 [0.25, 3.5] | Crystal playbackRange widened to [0.25, 3.5] |
| v0.12 | 🧘 禪風模板改為五聲懸浮和弦方案，段落間 4/7 共同音 | Zen → pentatonic suspended chords, 4/7 shared notes |
| v0.12 | 🌌 星空夜語全面重構：神秘→壯美→空靈→舒眠 | Starry Night: 4-section narrative arc |
| v0.13 | 🎨 CSS input width:100% 自適應 | CSS input width:100% responsive fix |
| v0.13 | 🎵 水晶 baseFreq 130.82(C3)、音量重平衡、移除 metal | Crystal C3 baseFreq, volume rebalance, removed metal |
| v0.13 | 🎼 模板各延長~60%、crossfade 4s、預設音量 50% | Templates extended ~60%, crossfade 4s, default vol 50% |
| v0.14 | 💎 水晶 baseFreq 重設為 659.25(E5) | Crystal baseFreq reset to 659.25(E5) |
| v0.14 | 📊 playbackRange 放寬為 [0.15, 4.0] | playbackRange widened to [0.15, 4.0] |
| v0.14 | 🔇 預設音量從 50 降為 40 | Default volume lowered from 50 to 40 |

### 🔜 待規劃 / Planned

- 🎼 **音樂模板擴充機制 / Template Expansion**
  - 模板定義系統：音階、節奏型態、段落結構的模組化設計
    Modular template definition: scales, rhythm patterns, section structures
  - 可載入外部模板檔案（JSON 格式），讓使用者自訂
    External JSON template loading for user customization
  - 預設模板：禪風、自然、夢幻、沉靜等多種風格
    Default templates: Zen, Nature, Dreamy, Silent...

- 🎚️ **音色優化 / Timbre Refinement**
  - 各音色增加微妙的隨機變化（pitch drift、輕微失諧）
    Subtle randomization per timbre (pitch drift, slight detuning)
  - ✅ 取樣音（sample-based）混合 — 水晶音效已實現
    Sample-based mixing — Crystal already implemented

---

*🎐 願風鈴為您帶來片刻寧靜 / May the wind chimes bring you a moment of peace*
