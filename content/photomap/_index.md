---
title: "Photography Spectrum"
outputs:
  - HTML
  - RSS
draft: false
---

<div class="photomap-page">

  <h2 style="text-align: center; font-family: 'Cinzel', serif; letter-spacing: 5px; margin-top: 30px;">
    FOOTPRINTS
  </h2>

  <div
    id="photo-map"
    style="
      height: 500px;
      width: 100%;
      border-radius: 12px;
      background: #001529;
      border: 1px solid #333;
      margin: 20px 0;
    ">
  </div>

  <hr style="margin: 50px 0; border: 0; border-top: 1px solid #eee;">

  <h3 style="text-align: center; font-family: 'Cinzel', serif; letter-spacing: 3px; margin-bottom: 30px;">
    EXPLORE GALLERIES
  </h3>

  <div class="gallery-grid" id="gallery-grid"></div>

</div>

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
   🔥 FixIt 布局修复（核心）
   =============================== */

.photomap-page {
  display: block !important;
  width: 100%;
}

/* 强制所有子元素纵向排列 */
.photomap-page > * {
  width: 100%;
}

/* 防止 FixIt 用 flex / grid 并排内容 */
.page-content,
.post-content,
.fi-container main {
  display: block !important;
}

/* ===============================
   地图标记样式
   =============================== */

.photo-marker {
  display: inline-flex !important;
  flex-direction: column;
  align-items: center;
}

.photo-marker .img-wrap {
  height: 66px;
  padding: 4px;
  background: white;
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
  background: rgba(0, 0, 0, 0.6);
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
  transition: all 0.4s ease;
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
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
}

.city-card:hover .card-overlay {
  opacity: 1;
}

.card-info {
  padding: 20px 0;
  text-align: center;
}

.city-desc {
  font-size: 0.8rem;
  color: #888;
}

/* 暗色模式适配 */
@media (prefers-color-scheme: dark) {
  .city-desc {
    color: #aaa;
  }
}
</style>
