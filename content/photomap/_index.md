---
title: "Photography Spectrum"
outputs:
  - HTML
  - RSS
draft: false
---

<div class="photomap-page">

  <h2 class="section-title">
    FOOTPRINTS
  </h2>

  <!-- ✅ 可展开/收回 -->
  <div class="map-toolbar">
    <button id="toggle-satellite" class="sat-btn" type="button" aria-expanded="false">
      SHOW SATELLITE MAP
    </button>
    <!-- <span class="map-hint">点击展开 / 收回卫星地图</span> -->
  </div>

  <!-- 地图（初始隐藏，不占位） -->
  <div id="photo-map" class="map-collapsed" aria-hidden="true"></div>

  <hr class="section-divider">

  <h3 class="section-subtitle">
    EXPLORE GALLERIES
  </h3>

  <!-- 相册网格 -->
  <div class="gallery-grid" id="gallery-grid"></div>

</div>

<!-- Leaflet -->
<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
window.addEventListener('load', function () {

  var cities = [
    {
      name: "Sydney",
      country: "Australia",
      coords: [-33.8688, 151.2093],
      thumb: "/photomap/sydney/arc over the harbour.jpg",
      link: "/photomap/sydney/",
      desc: "The harbor city with golden sunshine."
    },
    {
      name: "Gold Coast",
      country: "Australia",
      coords: [-28.0167, 153.4],
      thumb: "/photomap/goldcoast/Sinking Fire.jpg",
      link: "/photomap/goldcoast/",
      desc: "Endless coastline and the rhythm of the Pacific."
    }
  ];

  var gridContainer = document.getElementById('gallery-grid');

  // ✅ 先渲染下方卡片（懒加载主要在这里生效）
  cities.forEach(function (city) {
    var card = document.createElement('div');
    card.className = 'city-card';
    card.innerHTML =
      '<a href="' + city.link + '">' +
        '<div class="card-img-box">' +
          '<img src="' + city.thumb + '" alt="' + city.name + '" loading="lazy" decoding="async">' +
          '<div class="card-overlay"><span>ENTER GALLERY</span></div>' +
        '</div>' +
        '<div class="card-info">' +
          '<h4>' + city.name + ', ' + city.country + '</h4>' +
          (city.desc ? '<p class="city-desc">' + city.desc + '</p>' : '') +
        '</div>' +
      '</a>';
    gridContainer.appendChild(card);
  });

  // ===============================
  // ✅ 地图：第一次展开才初始化；之后可收回再展开
  // ===============================
  var map = null;
  var mapInited = false;
  var isOpen = false;

  function initMapOnce() {
    if (mapInited) return;

    map = L.map('photo-map', {
      attributionControl: false,
      scrollWheelZoom: true
    }).setView([-25, 140], 4);

    // ✅ 只用你坚持的卫星底图，不引入其他地图
    L.tileLayer(
      'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
      { maxZoom: 18 }
    ).addTo(map);

    cities.forEach(function (city) {
      var photoIcon = L.divIcon({
        className: 'photo-marker',
        html:
          '<div class="img-wrap"><img src="' + city.thumb + '" alt="' + city.name + '" loading="lazy" decoding="async"></div>' +
          '<div class="label">' + city.name + '</div>',
        iconAnchor: [33, 50]
      });

      L.marker(city.coords, { icon: photoIcon })
        .addTo(map)
        .on('click', function () {
          window.location.href = city.link;
        });
    });

    mapInited = true;
  }

  var btn = document.getElementById('toggle-satellite');
  var mapEl = document.getElementById('photo-map');

  function openMap() {
    // 1) 展开容器（否则 Leaflet 会算错尺寸）
    mapEl.classList.remove('map-collapsed');
    mapEl.classList.add('map-expanded');
    mapEl.setAttribute('aria-hidden', 'false');

    // 2) 初始化地图（只做一次）
    initMapOnce();

    // 3) 让 Leaflet 重新计算尺寸（关键）
    setTimeout(function () {
      map.invalidateSize();
      map.scrollWheelZoom.enable();
    }, 50);

    // 4) 按钮状态
    isOpen = true;
    btn.textContent = 'HIDE SATELLITE MAP';
    btn.setAttribute('aria-expanded', 'true');
  }

  function closeMap() {
    // ✅ 收回时禁用滚轮缩放，避免它截获页面滚动
    if (map) map.scrollWheelZoom.disable();

    mapEl.classList.remove('map-expanded');
    mapEl.classList.add('map-collapsed');
    mapEl.setAttribute('aria-hidden', 'true');

    isOpen = false;
    btn.textContent = 'SHOW SATELLITE MAP';
    btn.setAttribute('aria-expanded', 'false');
  }

  // 初始收起
  closeMap();

  btn.addEventListener('click', function () {
    if (isOpen) closeMap();
    else openMap();
  });

});
</script>

<style>
/* 页面整体结构 */
.photomap-page { width: 100%; display: block; }
.page-content, .post-content, .fi-container main { display: block !important; }

/* 标题 */
.section-title {
  text-align: center;
  font-family: "Cinzel", serif;
  letter-spacing: 5px;
  margin: 30px 0 20px;
}
.section-subtitle {
  text-align: center;
  font-family: "Cinzel", serif;
  letter-spacing: 3px;
  margin-bottom: 30px;
}
.section-divider {
  margin: 50px 0;
  border: none;
  border-top: 1px solid #eee;
}

/* ====== 按钮工具条 ====== */
.map-toolbar{
  display:flex;
  align-items:center;
  justify-content:center;
  gap:12px;
  flex-wrap:wrap;
  margin: 10px 0 14px;
}
.sat-btn{
  font-family: "Cinzel", serif;
  letter-spacing: 2px;
  font-size: 11px;
  padding: 10px 16px;
  border-radius: 10px;
  border: 1px solid #333;
  background: rgba(0,0,0,0.35);
  color: #fff;
  cursor: pointer;
  transition: transform .25s ease, opacity .25s ease;
}
.sat-btn:hover{ transform: translateY(-1px); }
.sat-btn:disabled{
  opacity: .6;
  cursor: default;
  transform: none;
}
.map-hint{ font-size: 12px; opacity: .7; }

/* ✅ 地图：初始完全不出现、不占位 */
#photo-map{
  width: 100%;
  border-radius: 12px;
  border: 1px solid #333;
  overflow: hidden;
}

/* 初始隐藏：不占高度、不显示 */
.map-collapsed{
  display: none;
  height: 0;
  margin: 0;
}

/* 展开后才给高度 */
.map-expanded{
  display: block;
  height: 520px;
  margin: 10px 0 20px;
  background: #001529;
}

@media (max-width: 768px) {
  .map-expanded { height: 420px; }
}

/* Marker */
.photo-marker {
  display: inline-flex;
  flex-direction: column;
  align-items: center;

  cursor: pointer;
  transition: transform 0.35s cubic-bezier(0.22, 1, 0.36, 1);
}
.photo-marker:hover { transform: translateX(8px) scale(1.05); }

.photo-marker .img-wrap {
  height: 66px;
  padding: 4px;
  border: 1px solid #000;
  border-radius: 4px;
  transition: box-shadow 0.35s ease, transform 0.35s ease;
}
.photo-marker:hover .img-wrap { box-shadow: 0 8px 24px rgba(0,0,0,0.45); }

.photo-marker img { height: 100%; display: block; }

.photo-marker .label {
  margin-top: 8px;
  font-size: 11px;
  font-weight: bold;
  color: #fff;
  background: rgba(0,0,0,0.6);
  padding: 2px 8px;
  border-radius: 4px;
  transition: transform 0.35s ease, opacity 0.35s ease;
}
.photo-marker:hover .label { transform: translateX(4px); opacity: 1; }

/* 相册网格 */
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 40px;
}

.city-card { transition: transform 0.35s ease; }
.city-card:hover { transform: translateY(-4px); }

.card-img-box {
  position: relative;
  height: 220px;
  border: 1px solid currentColor;
  overflow: hidden;
  background: #000;
}

.card-img-box img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.45);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.city-card:hover .card-overlay { opacity: 1; }

.card-overlay span {
  color: white;
  border: 1px solid white;
  padding: 10px 20px;
  font-size: 10px;
  letter-spacing: 3px;
  font-family: "Cinzel", serif;
}

/* 文字 */
.card-info { padding: 14px 0 0; text-align: center; }
.card-info h4 {
  margin: 0;
  font-family: "Cinzel", serif;
  font-size: 0.95rem;
  letter-spacing: 2px;
  text-transform: uppercase;
}

.city-desc {
  margin-top: 6px;
  font-family: "Cinzel", serif;
  font-style: italic;
  font-size: 0.78rem;
  letter-spacing: 1px;
  line-height: 1.6;
  color: #888;
}

@media (prefers-color-scheme: dark) { .city-desc { color: #aaa; } }

.city-card a { text-decoration: none; color: inherit; }
</style>
