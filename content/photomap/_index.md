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

  <!-- 地图 -->
  <div id="photo-map"></div>

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
      thumb: "/photomap/sydney/Sydney Harbor Bridge.jpg",
      link: "/photomap/sydney/",
      desc: "The harbor city with golden sunshine."
    },
    {
      name: "Gold Coast",
      country: "Australia",
      coords: [-28.0167, 153.4],
      thumb: "/photomap/goldcoast/sunset sea.jpg",
      link: "/photomap/goldcoast/",
      desc: "Endless coastline and the rhythm of the Pacific."
    }
  ];

  /* ===============================
     地图初始化
     =============================== */
  var map = L.map('photo-map', {
    attributionControl: false,
    scrollWheelZoom: true
  }).setView([-25, 140], 4);

  L.tileLayer(
    'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
    { maxZoom: 18 }
  ).addTo(map);

  var gridContainer = document.getElementById('gallery-grid');

  cities.forEach(function (city) {

    /* 地图 Marker */
    var photoIcon = L.divIcon({
      className: 'photo-marker',
      html:
        '<div class="img-wrap"><img src="' +
        city.thumb +
        '"></div><div class="label">' +
        city.name +
        '</div>',
      iconAnchor: [33, 50]
    });

    L.marker(city.coords, { icon: photoIcon })
      .addTo(map)
      .on('click', function () {
        window.location.href = city.link;
      });

    /* 下方卡片 */
    var card = document.createElement('div');
    card.className = 'city-card';
    card.innerHTML =
      '<a href="' + city.link + '">' +
        '<div class="card-img-box">' +
          '<img src="' + city.thumb + '" alt="' + city.name + '">' +
          '<div class="card-overlay"><span>ENTER GALLERY</span></div>' +
        '</div>' +
        '<div class="card-info">' +
          '<h4>' + city.name + ', ' + city.country + '</h4>' +
          (city.desc ? '<p class="city-desc">' + city.desc + '</p>' : '') +
        '</div>' +
      '</a>';

    gridContainer.appendChild(card);
  });
});
</script>

<style>
/* ===============================
   页面整体结构
   =============================== */

.photomap-page {
  width: 100%;
  display: block;
}

/* 防止 FixIt 并排布局 */
.page-content,
.post-content,
.fi-container main {
  display: block !important;
}

/* ===============================
   标题
   =============================== */

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

/* ===============================
   地图（高度在这里控制）
   👉 改 620px / 65vh 都行
   =============================== */

#photo-map {
  height: 660px;
  width: 100%;
  border-radius: 12px;
  background: #001529;
  border: 1px solid #333;
  margin: 20px 0;
}

@media (max-width: 768px) {
  #photo-map {
    height: 420px;
  }
}

/* ===============================
   地图 Marker
   =============================== */

.photo-marker {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
}

.photo-marker .img-wrap {
  height: 66px;
  padding: 4px;
  /* background: white; */
  border: 1px solid #000;
  border-radius: 4px;
}

.photo-marker img {
  height: 100%;
  display: block;
}

.photo-marker .label {
  margin-top: 8px;
  font-size: 11px;
  font-weight: bold;
  color: #fff;
  background: rgba(0,0,0,0.6);
  padding: 2px 8px;
  border-radius: 4px;
}

/* ===============================
   相册网格
   =============================== */

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 40px;
}

.city-card {
  transition: transform 0.35s ease;
}

.city-card:hover {
  transform: translateY(-4px);
}

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

.city-card:hover .card-overlay {
  opacity: 1;
}

.card-overlay span {
  color: white;
  border: 1px solid white;
  padding: 10px 20px;
  font-size: 10px;
  letter-spacing: 3px;
  font-family: "Cinzel", serif;
}

/* ===============================
   文字排版（核心精修）
   =============================== */

.card-info {
  padding: 14px 0 0;
  text-align: center;
}

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

@media (prefers-color-scheme: dark) {
  .city-desc {
    color: #aaa;
  }
}

.city-card a {
  text-decoration: none;
  color: inherit;
}

/* === 地图缩略图 hover 互动效果 === */
.photo-marker {
    cursor: pointer;
    transition: transform 0.35s cubic-bezier(0.22, 1, 0.36, 1);
}

/* 核心：向右轻晃 + 微缩放 */
.photo-marker:hover {
    transform: translateX(8px) scale(1.05);
}

/* 图片本体更细腻一点 */
.photo-marker .img-wrap {
    transition: box-shadow 0.35s ease, transform 0.35s ease;
}

.photo-marker:hover .img-wrap {
    box-shadow: 0 8px 24px rgba(0,0,0,0.45);
}

/* 标签轻微跟随 */
.photo-marker .label {
    transition: transform 0.35s ease, opacity 0.35s ease;
}

.photo-marker:hover .label {
    transform: translateX(4px);
    opacity: 1;
}

</style>
