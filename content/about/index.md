---
title: "Spectrum Space"
---

<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&family=Lora:ital,wght@0,400;1,400&display=swap" rel="stylesheet">

<div class="hero-header">
    <div class="hero-content">
        <div class="hero-tag">EST. 2025</div>
        <h1 class="hero-title">YUN SPACE</h1>
        <div class="hero-buttons">
            <a href="/photomap/" class="btn-main">Photography</a>
            <a href="/research/" class="btn-sub">Research</a>
        </div>
    </div>
</div>

<div class="about-me-container">
    <div class="about-me-content">
        <h2 class="section-title">About Me</h2>
        <div class="bio-text">
            <p class="drop-cap">嗨，我是 <strong>YUN</strong>。</p>
            <!-- <p>在这个空间里，我试图平衡感性的视觉表达与理性的思维厚度。这里的每一帧画面和每一行文字，都是我观察世界的独特频谱。</p> -->
        </div>
        <div class="about-dynamic-grid">
            <div class="dynamic-column">
                <h3 class="sub-section-title">The Journey</h3>
                <div class="timeline-container">
                    <div class="timeline-item">
                        <span class="time-year">2025</span>
                        <p class="time-event">YUN Space 数字化空间正式上线。</p>
                    </div>
                    </div>
                </div>
            </div>
            <div class="dynamic-column">
                <h3 class="sub-section-title">Recent Log</h3>
                {{< recent_log >}}
            </div>
        </div>
        <div class="social-icons">
            <!-- <a href="https://github.com/SpectrumLY" target="_blank" class="social-link">GitHub</a>
            <a href="mailto:your-email@example.com" class="social-link">Email</a> -->
        </div>
    </div>
</div>

<style>
/* --- 隐藏主题自带标题 --- */
.page.single .single-title { display: none !important; }

/* --- Hero 封面样式 --- */
.hero-header {
    height: 92vh; width: 100vw;
    margin-left: calc(50% - 50vw);
    margin-top: -120px;
    background-image: linear-gradient(rgba(0,0,0,0.25), rgba(0,0,0,0.45)), url('Mel_point.jpg');
    background-size: cover; background-position: center; background-attachment: fixed;
    display: flex; align-items: center; justify-content: center; color: white !important;
    animation: heroFadeIn 1.8s ease-out;
}

@keyframes heroFadeIn { from { opacity: 0; filter: blur(5px); } to { opacity: 1; filter: blur(0); } }

.hero-content { text-align: center; animation: contentUp 1.2s cubic-bezier(0.2, 0.8, 0.2, 1); }

@keyframes contentUp { from { transform: translateY(30px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }

.hero-tag { font-size: 0.9rem; letter-spacing: 6px; opacity: 0.7; margin-bottom: 25px; text-transform: uppercase; }

.hero-title {
    font-family: 'Cinzel', serif !important; 
    font-size: clamp(2.2rem, 7vw, 4.8rem);
    letter-spacing: 20px; font-weight: 400; margin: 0; padding-left: 18px;
    text-shadow: 0 12px 40px rgba(0,0,0,0.4);
}

/* --- 按钮样式 --- */
.hero-buttons { margin-top: 60px; display: flex; gap: 35px; justify-content: center; }
.btn-main, .btn-sub { padding: 14px 40px; border-radius: 0; text-decoration: none !important; font-size: 0.72rem; letter-spacing: 3px; text-transform: uppercase; transition: 0.4s; }
.btn-main { background: white; color: black !important; }
.btn-sub { border: 1px solid rgba(255,255,255,0.7); color: white !important; }
.btn-main:hover, .btn-sub:hover { transform: translateY(-5px); background: white; color: black !important; box-shadow: 0 10px 20px rgba(0,0,0,0.3); }

/* --- 布局容器 --- */
.about-me-container { max-width: 900px; margin: 120px auto; padding: 0 30px; }
.section-title { font-family: 'Cinzel', serif; font-size: 1.8rem; letter-spacing: 8px; margin-bottom: 60px; text-align: center; color: var(--color-contrast-high); }
.bio-text { margin-bottom: 100px; text-align: left; }
.bio-text p { line-height: 2.2; font-size: 1.05rem; color: var(--color-contrast-medium); margin-bottom: 30px; font-family: 'Lora', serif; }

.drop-cap::first-letter { font-family: 'Cinzel', serif; float: left; font-size: 3.5rem; line-height: 1; padding-right: 12px; color: var(--color-contrast-high); }

/* --- 动态模块网格 --- */
.about-dynamic-grid { display: grid; grid-template-columns: 1.2fr 1fr; gap: 60px; margin-bottom: 80px; text-align: left; }
.sub-section-title { font-family: 'Cinzel', serif; font-size: 1.1rem; letter-spacing: 4px; margin-bottom: 35px; color: var(--color-contrast-high); border-left: 2px solid var(--color-contrast-low); padding-left: 15px; }

/* 时间轴 */
.timeline-container { border-left: 1px solid var(--color-contrast-low); padding-left: 25px; margin-left: 5px; }
.timeline-item { position: relative; margin-bottom: 30px; }
.timeline-item::before { content: ""; position: absolute; left: -30px; top: 6px; width: 8px; height: 8px; background: var(--color-contrast-high); border-radius: 50%; }
.time-year { font-family: 'Cinzel', serif; font-size: 0.85rem; font-weight: 700; display: block; margin-bottom: 5px; color: var(--color-contrast-high); }
.time-event { font-family: 'Lora', serif; font-size: 0.9rem; color: var(--color-contrast-medium); line-height: 1.6; }

/* 自动更新列表样式 (与 Shortcode 配合) */
.update-list { list-style: none; padding: 0; }
.update-list li { display: flex; flex-direction: column; margin-bottom: 20px; padding-bottom: 15px; border-bottom: 1px dashed var(--color-contrast-low); }
.update-date { font-family: 'Cinzel', serif; font-size: 0.65rem; color: var(--color-contrast-low); margin-bottom: 5px; }
.update-content { font-family: 'Lora', serif; font-size: 0.9rem; color: var(--color-contrast-medium); line-height: 1.5; }

/* --- 底部社交 --- */
.social-icons { margin-top: 80px; border-top: 1px solid var(--color-contrast-low); padding-top: 40px; text-align: center; }
.social-link { margin: 0 20px; font-size: 0.75rem; letter-spacing: 4px; text-transform: uppercase; text-decoration: none !important; color: var(--color-contrast-low) !important; transition: 0.3s; }
.social-link:hover { color: var(--color-contrast-high) !important; }

@media (max-width: 850px) { .about-dynamic-grid { grid-template-columns: 1fr; gap: 50px; } }
</style>