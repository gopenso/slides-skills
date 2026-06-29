# Swiss Map Component

用於地理、歷史、城市、人文路線、門店/校區/事件點位等內容。它不是新的 Swiss 正文版式，而是 **S08 Duo Compare 的右側插槽擴充**：左側仍是解釋卡片，右側取代為地圖元件。

## 何時使用

- 文件裡出現地點、街區、路線、人物住所、機構分佈、城市漫遊。
- 使用者明確希望有地圖、點位、關係線或地理元件。
- 內容需要解釋「空間關係」，而不只是羅列人物或地點。

## 硬規則

- `<section>` 仍寫 `data-layout="S08"`；不要新增 `P23/P24` 或自定義正文頁。
- 頁面結構必須是：頂部標題 + 左側說明卡片 + 右側地圖卡片。
- 地圖示記由 HTML 元件組成：點 `.pin-dot` + 連線 `.pin-line` + 卡片 `.pin-card`。
- SVG 只畫 fallback 關係線，不要在 SVG 裡寫文字。
- MapLibre 地圖預設關閉滾輪縮放和拖動，避免觸發 PPT 翻頁。
- 右上角必須有 `+` / `-` / `DRAG` 控制。使用者點選 `DRAG` 後才允許拖動地圖。
- 必須有靜態 fallback：CDN 或地圖瓦片失敗時，仍能看到點位、關係線和卡片。

## 資料契約

寫頁面前先定義點位和關係。`x/y` 用於靜態 fallback 百分比座標，`coord` 用於 MapLibre 經緯度。

```js
const MAP_POINTS = [
  { id: 'gu', name: '顧維鈞', meta: '外交', coord: [117.2048, 39.1060], x: 62, y: 68, accent: true },
  { id: 'cao', name: '曹錕', meta: '北洋', coord: [117.1988, 39.1080], x: 34, y: 48 },
  { id: 'sun', name: '孫殿英', meta: '軍閥', coord: [117.2028, 39.1090], x: 52, y: 54 },
  { id: 'zhang', name: '張自忠', meta: '抗戰', coord: [117.1966, 39.1120], x: 58, y: 28, accent: true },
  { id: 'jin', name: '金氏宅邸', meta: '交通站', coord: [117.2012, 39.1114], x: 66, y: 35, side: 'left' },
];

const MAP_RELATIONS = [
  ['gu', 'cao'],
  ['cao', 'sun'],
  ['zhang', 'jin'],
];
```

## 必要 CSS

放到產生頁 `<head>` 的額外 `<style>` 中，不要改 `template-swiss.html` 的全域性基座類。

```html
<link href="https://unpkg.com/maplibre-gl@5.14.0/dist/maplibre-gl.css" rel="stylesheet">
<script src="https://unpkg.com/maplibre-gl@5.14.0/dist/maplibre-gl.js"></script>
<style>
.history-map-grid{display:grid;grid-template-columns:4.2fr 7.8fr;gap:2vw;flex:1;min-height:0;margin-top:2vh;align-items:stretch}
.history-side{display:grid;grid-template-rows:1.08fr repeat(4,1fr);gap:1vh;min-height:0;height:100%}
.history-side-head{background:var(--accent);color:var(--accent-on);padding:2.2vh 1.4vw 1.8vh;border-radius:3px}
.history-side-head .big{font-family:var(--sans),var(--sans-zh);font-size:max(22px,2.2vw);font-weight:300;line-height:1.08;letter-spacing:-.02em}
.history-side-head .small{font-family:var(--sans),var(--sans-zh);font-size:max(11px,.82vw);font-weight:300;line-height:1.55;color:rgba(255,255,255,.82);margin-top:1.2vh}
.relation-card{background:var(--grey-1);padding:1.45vh 1.1vw;border-radius:3px;display:grid;grid-template-columns:auto 1fr;gap:.8vw;align-items:start;min-height:0}
.relation-card .nb{font-family:var(--mono);font-size:max(10px,.75vw);letter-spacing:.16em;color:var(--accent)}
.relation-card .ttl{font-family:var(--sans),var(--sans-zh);font-size:max(14px,1.05vw);font-weight:500;line-height:1.25}
.relation-card .desc{font-family:var(--sans),var(--sans-zh);font-size:max(11px,.78vw);line-height:1.5;color:var(--text-secondary);margin-top:.55vh}
.map-panel{position:relative;background:var(--grey-1);border-radius:3px;overflow:hidden;min-height:0;height:100%}
.map-panel .map-title{position:absolute;top:1.4vh;left:1.2vw;z-index:3;background:rgba(250,250,248,.92);padding:1.2vh 1vw;border-radius:3px;max-width:28vw}
.map-panel .map-title .k{font-family:var(--mono);font-size:max(10px,.72vw);letter-spacing:.18em;color:var(--text-helper)}
.map-panel .map-title .t{font-family:var(--sans),var(--sans-zh);font-size:max(18px,1.5vw);font-weight:400;letter-spacing:-.015em;margin-top:.4vh}
.map-controls{position:absolute;top:1.4vh;right:1.2vw;z-index:4;display:flex;gap:6px;background:rgba(250,250,248,.9);padding:6px;border-radius:3px}
.map-ctrl{min-width:32px;height:32px;border:1px solid var(--ink);background:transparent;color:var(--ink);font-family:var(--mono);font-size:12px;letter-spacing:.08em;text-transform:uppercase;border-radius:0;cursor:pointer}
.map-ctrl.drag{min-width:58px}
.map-ctrl.active{background:var(--accent);border-color:var(--accent);color:var(--accent-on)}
.wudadao-map,.swiss-map{position:absolute;inset:0;background:#f4f4f0}
.wudadao-map.map-live .map-static,.swiss-map.map-live .map-static{display:none}
.map-static{position:absolute;inset:0;display:block;background:linear-gradient(18deg,transparent 0 44%,rgba(25,25,25,.11) 44% 44.2%,transparent 44.2%),linear-gradient(-8deg,transparent 0 54%,rgba(25,25,25,.09) 54% 54.16%,transparent 54.16%),linear-gradient(0deg,transparent 0 61%,rgba(25,25,25,.08) 61% 61.15%,transparent 61.15%),#f4f4f0}
.static-relations{position:absolute;inset:0;width:100%;height:100%;pointer-events:none}
.static-relations line{stroke:var(--accent);stroke-width:.24;stroke-dasharray:1.4 1.2;opacity:.68}
.static-marker{position:absolute;transform:translate(-50%,-50%);width:0;height:0}
.static-marker .pin-dot,.person-marker .pin-dot{position:absolute;left:-6px;top:-6px;width:12px;height:12px;border-radius:50%;background:var(--ink);border:2px solid #fff;box-shadow:0 0 0 1px rgba(0,0,0,.22)}
.static-marker.accent .pin-dot,.person-marker.accent .pin-dot{background:var(--accent)}
.static-marker .pin-line,.person-marker .pin-line{position:absolute;left:7px;top:0;width:24px;height:1px;background:var(--ink);opacity:.45}
.static-marker.accent .pin-line,.person-marker.accent .pin-line{background:var(--accent);opacity:.75}
.static-marker .pin-card,.person-marker .pin-card{position:absolute;left:31px;top:-18px;min-width:72px;background:rgba(250,250,248,.9);box-shadow:0 0 0 1px rgba(0,0,0,.06);border-radius:2px;padding:6px 7px;font-family:var(--sans),var(--sans-zh);white-space:nowrap}
.static-marker .pin-name,.person-marker .pin-name{font-size:12px;line-height:1.05;color:var(--ink)}
.static-marker .pin-meta,.person-marker .pin-meta{font-family:var(--mono);font-size:9px;line-height:1;letter-spacing:.12em;color:var(--text-helper);margin-top:4px;text-transform:uppercase}
.static-marker.accent .pin-name,.person-marker.accent .pin-name{color:var(--accent)}
.static-marker.left .pin-line,.person-marker.left .pin-line{left:auto;right:7px}
.static-marker.left .pin-card,.person-marker.left .pin-card{left:auto;right:31px}
.person-marker{position:relative;width:0;height:0;pointer-events:auto}
.maplibregl-ctrl-bottom-left,.maplibregl-ctrl-bottom-right{display:none!important}
</style>
```

## 頁面骨架

```html
<section class="slide" data-layout="S08" data-animate="duo-mirror">
  <div class="canvas-card">
    <header class="chrome-min"><div class="l">06 / NN · MAP COMPONENT</div><div class="r">MAPLIBRE / STATIC FALLBACK</div></header>
    <h2 class="h-xl-zh">把人物住所放回街區裡</h2>
    <div class="history-map-grid">
      <aside class="history-side">
        <div class="history-side-head">
          <div class="big">住所不是點位，<br/>而是關係入口。</div>
          <div class="small">這頁用地圖承載空間關係，用左側卡片解釋人物之間的牽連。</div>
        </div>
        <div class="relation-card"><div class="nb">01</div><div><div class="ttl">顧維鈞 ↔ 曹錕</div><div class="desc">說明兩者為什麼有關係，至少寫成完整一句。</div></div></div>
        <div class="relation-card"><div class="nb">02</div><div><div class="ttl">曹錕 ↔ 孫殿英</div><div class="desc">不要只寫標籤，寫清歷史關係或空間關係。</div></div></div>
        <div class="relation-card"><div class="nb">03</div><div><div class="ttl">張自忠 ↔ 金氏宅邸</div><div class="desc">每張卡控制在 2-3 行，形成資訊密度。</div></div></div>
        <div class="relation-card"><div class="nb">04</div><div><div class="ttl">張自忠 ↔ 利德爾</div><div class="desc">可以用跨身份對照補充人文厚度。</div></div></div>
      </aside>
      <div class="map-panel">
        <div class="map-title"><div class="k">RELATION MAP</div><div class="t">地點 / 人物 / 事件</div></div>
        <div class="map-controls" aria-label="地圖控制">
          <button class="map-ctrl" type="button" data-map-ctrl="zoom-in" aria-label="放大地圖">+</button>
          <button class="map-ctrl" type="button" data-map-ctrl="zoom-out" aria-label="縮小地圖">-</button>
          <button class="map-ctrl drag" type="button" data-map-ctrl="drag" aria-label="拖動地圖" aria-pressed="false">DRAG</button>
        </div>
        <div id="swiss-map" class="swiss-map" data-points='[填入 JSON]' data-relations='[填入 JSON]'>
          <div class="map-static" aria-hidden="true">
            <svg class="static-relations" viewBox="0 0 100 100" preserveAspectRatio="none">[靜態連線]</svg>
            [靜態 marker 卡片]
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
```

## 必要 JS

放到 `</body>` 前。產生多張地圖頁時，把 id 從 `swiss-map` 改成唯一 id，並讓初始化函式接收 selector。

```html
<script>
(() => {
  function readJson(el, key, fallback){
    try { return JSON.parse(el.dataset[key] || ''); }
    catch { return fallback; }
  }
  function initSwissMap(){
    const el = document.getElementById('swiss-map');
    if(!el || el.dataset.ready) return;
    el.dataset.ready = '1';
    const points = readJson(el, 'points', []);
    const relations = readJson(el, 'relations', []);
    function coord(id){ return points.find(p => p.id === id).coord; }
    const panel = el.closest('.map-panel');
    panel?.addEventListener('wheel', (event) => event.stopPropagation(), {passive:true});
    ['pointerdown','pointermove','pointerup','click','dblclick','touchstart','touchmove'].forEach((type) => {
      panel?.addEventListener(type, (event) => event.stopPropagation(), {passive:true});
    });
    if(window.__lowPowerMode || !window.maplibregl){
      el.classList.add('fallback-only');
      return;
    }
    const center = points.length
      ? [points.reduce((sum, p) => sum + p.coord[0], 0) / points.length, points.reduce((sum, p) => sum + p.coord[1], 0) / points.length]
      : [0, 0];
    const map = new maplibregl.Map({
      container: el,
      style: {
        version:8,
        sources:{ osm:{ type:'raster', tiles:['https://tile.openstreetmap.org/{z}/{x}/{y}.png'], tileSize:256, attribution:'© OpenStreetMap contributors' } },
        layers:[{ id:'osm', type:'raster', source:'osm', paint:{ 'raster-saturation':-0.88, 'raster-contrast':0.08, 'raster-opacity':0.46 } }]
      },
      center,
      zoom: Number(el.dataset.zoom || 15),
      interactive: true,
      attributionControl: false
    });
    map.scrollZoom.disable();
    map.boxZoom.disable();
    map.doubleClickZoom.disable();
    map.dragPan.disable();
    map.on('load', () => {
      el.classList.add('map-live');
      map.addSource('relations', {
        type:'geojson',
        data:{ type:'FeatureCollection', features:relations.map(([a,b]) => {
          const from = coord(a);
          const to = coord(b);
          return from && to ? { type:'Feature', geometry:{ type:'LineString', coordinates:[from, to] }, properties:{} } : null;
        }).filter(Boolean) }
      });
      map.addLayer({ id:'relations', type:'line', source:'relations', paint:{ 'line-color':'#1936b3', 'line-opacity':.62, 'line-width':2, 'line-dasharray':[2,2] } });
      for(const p of points){
        const marker = document.createElement('div');
        marker.className = 'person-marker' + (p.accent ? ' accent' : '') + (p.side === 'left' ? ' left' : '');
        marker.innerHTML = '<span class="pin-dot"></span><span class="pin-line"></span><span class="pin-card"><span class="pin-name">' + p.name + '</span><span class="pin-meta">' + p.meta + '</span></span>';
        marker.title = p.name;
        new maplibregl.Marker({ element: marker }).setLngLat(p.coord).addTo(map);
      }
      setTimeout(() => map.resize(), 300);
    });
    document.getElementById('deck')?.addEventListener('transitionend', () => map.resize());
    const zoomIn = panel?.querySelector('[data-map-ctrl="zoom-in"]');
    const zoomOut = panel?.querySelector('[data-map-ctrl="zoom-out"]');
    const drag = panel?.querySelector('[data-map-ctrl="drag"]');
    zoomIn?.addEventListener('click', (event) => { event.stopPropagation(); map.zoomIn(); });
    zoomOut?.addEventListener('click', (event) => { event.stopPropagation(); map.zoomOut(); });
    drag?.addEventListener('click', (event) => {
      event.stopPropagation();
      const active = drag.classList.toggle('active');
      drag.setAttribute('aria-pressed', active ? 'true' : 'false');
      drag.textContent = active ? 'DRAG ON' : 'DRAG';
      if(active) map.dragPan.enable(); else map.dragPan.disable();
    });
  }
  window.addEventListener('DOMContentLoaded', () => setTimeout(initSwissMap, 500));
})();
</script>
```

## 視覺檢查

- 左側卡片總高度要和右側地圖卡片對齊，不要上浮一半。
- 地圖示題和控制按鈕不能互相遮擋；點位卡片不能壓到右上角控制區。
- marker 卡片至少顯示地點名，`meta` 只作為短標籤。
- 左側關係卡不要惜字如金，每張卡應有完整一句解釋。
- 若地圖無法載入，靜態 fallback 仍必須可讀。
