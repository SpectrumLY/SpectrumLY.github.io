---
title: "Photography Spectrum"
outputs:
  - HTML
  - RSS
draft: false
---


<h2 style="text-align: center; font-family: 'Cinzel', serif; letter-spacing: 5px; margin-top: 30px;">FOOTPRINTS</h2>

<div id="photo-map" style="height: 500px; width: 100%; border-radius: 12px; background: #001529; border: 1px solid #333; margin: 20px 0;"></div>

<hr style="margin: 50px 0; border: 0; border-top: 1px solid #eee;">

<h3 style="text-align: center; font-family: 'Cinzel', serif; letter-spacing: 3px; margin-bottom: 30px;">EXPLORE GALLERIES</h3>

<div class="gallery-grid" id="gallery-grid"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
window.addEventListener('load', function() {
    var cities = [
        {
            name: "Sydney",
            country: "Australia",
            coords: [-33.8688, 151.2093],
            thumb: "/photomap/sydney/Sydney Harbor Bridge.jpg", 
            link: "/photomap/sydney/",
            desc: "The harbor city with golden sunshine."
        },
    // --- 新增黄金海岸 ---
        {
            name: "Gold Coast",
            country: "Australia",
            coords: [-28.0167, 153.4000],
            thumb: "/photomap/goldcoast/sunset sea.jpg", // 确保这张封面图路径正确
            link: "/photomap/goldcoast/",
            desc: "Endless coastline and the rhythm of the Pacific."
        }
    ];

    var map = L.map('photo-map', {
        attributionControl: false,
        scrollWheelZoom: true 
    }).setView([-25, 140], 4); 

    L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
        maxZoom: 18
    }).addTo(map);

    var gridContainer = document.getElementById('gallery-grid');

    cities.forEach(function(city) {
        var photoIcon = L.divIcon({
            className: 'photo-marker',
            html: `<div class="img-wrap"><img src="${city.thumb}"></div><div class="label">${city.name}</div>`,
            iconSize: null,
            iconAnchor: [33, 50]
        });
        L.marker(city.coords, { icon: photoIcon }).addTo(map)
         .on('click', function() { window.location.href = city.link; });

        var card = document.createElement('div');
        card.className = 'city-card';
        card.innerHTML = `
            <a href="${city.link}">
                <div class="card-img-box">
                    <img src="${city.thumb}" alt="${city.name}">
                    <div class="card-overlay">
                        <span>ENTER GALLERY</span>
                    </div>
                </div>
                <div class="card-info">
                    <h4>${city.name}, ${city.country}</h4>
                    ${city.desc ? `<p class="city-desc">${city.desc}</p>` : ''}
                </div>
            </a>
        `;
        gridContainer.appendChild(card);
    });
});
</script>

<style>
/* 地图标记保持不变 */
.photo-marker { display: inline-flex !important; flex-direction: column; align-items: center; }
.photo-marker .img-wrap { height: 66px; width: auto; padding: 4px; background: white; border: 1px solid #000; border-radius: 4px; line-height: 0; }
.photo-marker img { height: 100%; width: auto; display: block; border-radius: 2px; }
.photo-marker .label { margin-top: 8px; font-size: 11px; font-weight: bold; color: #fff; text-shadow: 0 1px 4px #000; background: rgba(0,0,0,0.6); padding: 2px 8px; border-radius: 4px; }

/* 网格布局 */
.gallery-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 40px;
    padding: 20px 0;
}

.city-card {
    background: transparent;
    overflow: hidden;
    transition: all 0.4s ease;
}

.card-img-box {
    position: relative;
    height: 220px;
    /* 这里的边框颜色设为 currentColor 或固定的深浅适配色 */
    border: 1px solid currentColor; 
    overflow: hidden;
    background: #000;
}

.card-img-box img {
    width: 100%; height: 100%;
    object-fit: cover;
    transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
    opacity: 0.9;
}

.city-card:hover .card-img-box img {
    transform: scale(1.08);
    opacity: 1;
}

.card-overlay {
    position: absolute;
    top: 0; left: 0; width: 100%; height: 100%;
    background: rgba(0,0,0,0.4); /* 暗色背景增强对比 */
    display: flex; align-items: center; justify-content: center;
    opacity: 0; transition: opacity 0.3s;
}

.card-overlay span {
    color: white; border: 1px solid white;
    padding: 10px 20px; font-size: 10px;
    letter-spacing: 3px; font-family: 'Cinzel', serif;
}

.city-card:hover .card-overlay { opacity: 1; }

/* --- 核心修复：文字与适配 --- */
.card-info { padding: 20px 0; text-align: center; }

.card-info h4 { 
    margin: 0; 
    font-family: 'Cinzel', serif; 
    font-size: 1rem; 
    /* 移除固定颜色，让它跟随主题色 */
    color: inherit; 
    letter-spacing: 2px;
    text-transform: uppercase;
}

.city-desc { 
    margin: 12px 0 0; 
    font-size: 0.8rem; 
    /* 使用稍浅的颜色 */
    color: #888; 
    letter-spacing: 1px;
    line-height: 1.5;
    font-style: italic;
    font-family: "Cinzel", serif;
}

/* --- 暗色模式强制适配 --- */
@media (prefers-color-scheme: dark) {
    .card-info h4 { color: #f0f0f0; } /* 暗色模式下标题变亮白 */
    .card-img-box { border-color: rgba(255,255,255,0.3); } /* 黑框变浅色细线 */
    .city-desc { color: #aaa; } /* 描述文字变亮一点 */
}

.city-card a { text-decoration: none; color: inherit; }
</style>