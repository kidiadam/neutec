# Animation and Dropdown Menu

> A simple demo with vue3 for Neutec.

## Live Demo

[GitHub Page](https://kidiadam.github.io/neutec/dist/)

## 額外需求

### 九宮格動畫

- 原先預設動畫以css animation進行處理，四顆球往同座標移動則是以js進行

### 側邊選單

- 下拉選單及儲存上次開啟之選單請參照demoe頁面。
- 100層選單，若依照現有menu schema結構為多維度陣列，前端的menu的interface會調整為{ key: string, text: string, expanded?: boolean, init?: boolean, children?: menu[]}，其中init作為chidren內容的v-if外層，當選單未被首次點擊前，v-if="init"不成立，故而不向下進行children的渲染，點擊後，init及expanded為true，依此類推，每次點擊僅往下渲染一層。而expanded則做為v-show使用，當選單被開啟後，改點選其他選單時，僅將原本的expanded改為false，不變動init，避免後續開啟時需要進行重新渲染。
