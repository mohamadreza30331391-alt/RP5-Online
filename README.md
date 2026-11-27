# RP5-Online
<!doctype html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" />
  <title>RP5 | جهان باز سه‌بعدی</title>
  <style>
/* ===== RP5 — Minimalist UI ===== */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  -webkit-user-select: none;
  user-select: none;
  -webkit-touch-callout: none;
}
html, body { overscroll-behavior: none; touch-action: none; }


:root {
  --bg: #0a0e14;
  --surface: rgba(16, 22, 32, 0.82);
  --surface-solid: #101620;
  --border: rgba(126, 231, 255, 0.18);
  --accent: #7ee7ff;
  --accent-dim: #2a9bc0;
  --text: #e8edf2;
  --text-dim: #8b9bad;
  --green: #5cf08c;
  --yellow: #ffd84d;
  --red: #ff5c5c;
  --radius: 12px;
  --radius-sm: 8px;
  --font: 'Segoe UI', Tahoma, Arial, sans-serif;
}

html, body {
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: var(--bg);
  color: var(--text);
  font-family: var(--font);
}

#game {
  position: fixed;
  inset: 0;
}

canvas {
  display: block;
  width: 100%;
  height: 100%;
  touch-action: none;
}

/* ===== پنل مشترک ===== */
.hud-panel {
  position: fixed;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
  box-shadow: 0 2px 16px rgba(0, 0, 0, 0.35);
  z-index: 3;
}

/* ===== برند ===== */
#brand {
  top: 12px;
  left: 12px;
  padding: 8px 14px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.brand-title {
  font-weight: 800;
  font-size: 18px;
  color: var(--accent);
  letter-spacing: 1px;
}

.brand-tag {
  font-size: 9px;
  font-weight: 700;
  color: var(--bg);
  background: var(--accent);
  padding: 3px 8px;
  border-radius: 4px;
  letter-spacing: 0.5px;
}

/* ===== مینی‌مپ ===== */
#minimap-box {
  top: 62px;
  left: 12px;
  padding: 5px;
  border-radius: 50%;
  width: 112px;
  height: 112px;
  display: flex;
  align-items: center;
  justify-content: center;
}

#minimap {
  width: 102px;
  height: 102px;
  border-radius: 50%;
  background: #0d1a26;
}

/* ===== آمار ===== */
#stats {
  top: 12px;
  right: 12px;
  padding: 10px 14px;
  min-width: 170px;
  font-size: 12.5px;
}

.stat-row {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 3px 0;
  line-height: 1.4;
}

.stat-icon {
  font-size: 14px;
  width: 18px;
  text-align: center;
}

.stat-value {
  margin-right: auto;
  text-align: left;
  font-weight: 700;
  font-variant-numeric: tabular-nums;
}

.stat-value.money { color: var(--green); }
.stat-value.region { color: var(--accent); font-size: 11px; }

/* ===== جوی‌استیک ===== */
#stick {
  position: fixed;
  left: 22px;
  bottom: 26px;
  width: 130px;
  height: 130px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.06);
  border: 2px solid rgba(126, 231, 255, 0.25);
  z-index: 5;
  touch-action: none;
}

#nub {
  position: absolute;
  width: 52px;
  height: 52px;
  border-radius: 50%;
  background: var(--accent);
  opacity: 0.55;
  left: 39px;
  top: 39px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.4);
  transition: opacity 0.15s;
}

#stick:active #nub { opacity: 0.8; }

/* ===== دکمه‌های اکشن ===== */
#actions {
  position: fixed;
  right: 12px;
  bottom: 16px;
  display: grid;
  grid-template-columns: repeat(2, 72px);
  gap: 8px;
  z-index: 5;
}
.action-proxy, #action-proxies { display: none !important; }
#actions .primary-action {
  min-height: 58px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  font-size: 19px;
  border-radius: 14px;
  background: linear-gradient(145deg, rgba(28,48,67,.96), rgba(10,18,29,.94));
  border-color: rgba(126,231,255,.26);
  box-shadow: 0 5px 16px rgba(0,0,0,.28);
}
.primary-action span { font-size: 10px; color: var(--text); }

.action-btn {
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: var(--radius-sm);
  background: rgba(18, 28, 42, 0.88);
  color: var(--text);
  padding: 11px 6px;
  font-weight: 700;
  font-size: 11.5px;
  font-family: var(--font);
  min-height: 44px;
  cursor: pointer;
  transition: transform 0.08s, background 0.15s, border-color 0.15s;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
}

.action-btn:active {
  transform: scale(0.93);
  background: var(--accent-dim);
  border-color: var(--accent);
}

.action-btn:hover {
  border-color: rgba(126, 231, 255, 0.4);
}

/* ===== توست ===== */
#toast {
  position: fixed;
  left: 50%;
  top: 20%;
  transform: translate(-50%, -50%);
  padding: 12px 24px;
  background: var(--surface-solid);
  border: 1px solid var(--accent);
  border-radius: var(--radius);
  z-index: 8;
  display: none;
  text-align: center;
  max-width: 88%;
  font-size: 13px;
  line-height: 1.6;
  box-shadow: 0 6px 28px rgba(0, 0, 0, 0.55);
  animation: toastIn 0.2s ease-out;
}

@keyframes toastIn {
  from { opacity: 0; transform: translate(-50%, -55%); }
  to   { opacity: 1; transform: translate(-50%, -50%); }
}

/* ===== مودال منو ===== */
#menu-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.6);
  display: none;
  align-items: center;
  justify-content: center;
  z-index: 9;
  backdrop-filter: blur(6px);
  -webkit-backdrop-filter: blur(6px);
}

#menu-box {
  width: min(380px, 92vw);
  max-height: 86vh;
  overflow-y: auto;
  background: var(--surface-solid);
  border: 1px solid rgba(126, 231, 255, 0.3);
  border-radius: 16px;
  padding: 22px;
  line-height: 1.8;
  font-size: 13px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
}

#menu-title {
  margin: 0 0 14px;
  color: var(--accent);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  padding-bottom: 10px;
  font-size: 16px;
}

#menu-body p {
  margin: 6px 0;
}

#menu-body button {
  width: 100%;
  margin-top: 8px;
}

.btn-close {
  width: 100%;
  margin-top: 14px;
  padding: 10px;
  border: 1px solid rgba(255, 92, 92, 0.4);
  border-radius: var(--radius-sm);
  background: rgba(92, 35, 35, 0.7);
  color: var(--text);
  font-weight: 700;
  font-size: 12px;
  font-family: var(--font);
  cursor: pointer;
  transition: background 0.15s;
}

.btn-close:hover { background: rgba(120, 40, 40, 0.8); }
.btn-close:active { transform: scale(0.97); }

/* دکمه‌های آپشن داخل منو */
.opt-btn {
  background: rgba(27, 59, 84, 0.7) !important;
  text-align: right;
  padding: 11px 13px !important;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: var(--radius-sm);
  color: var(--text);
  font-weight: 600;
  font-size: 12.5px;
  cursor: pointer;
  transition: border-color 0.15s, background 0.15s;
  font-family: var(--font);
}

.opt-btn:hover {
  border-color: var(--accent);
  background: rgba(27, 59, 84, 0.9) !important;
}

.opt-btn:active { transform: scale(0.98); }

.opt-btn span:last-child {
  font-size: 11px;
  color: var(--text-dim);
}

/* ===== فوتر ===== */
#footer {
  position: fixed;
  bottom: 5px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 10px;
  color: var(--text-dim);
  opacity: 0.7;
  z-index: 2;
  white-space: nowrap;
}

/* ===== رسپانسیو ===== */
@media (max-width: 500px) {
  #actions { grid-template-columns: repeat(2, 64px); gap: 6px; right: 8px; bottom: 10px; }
  .action-btn { padding: 7px 4px; min-height: 48px; }
  #actions .primary-action { min-height: 52px; font-size: 18px; }
  #minimap-box { width: 98px; height: 98px; top: 58px; left: 8px; }
  #minimap { width: 88px; height: 88px; }
  #brand { padding: 6px 11px; }
  .brand-title { font-size: 15px; }
  #stick { width: 110px; height: 110px; left: 12px; bottom: 14px; }
  #actions { grid-template-columns: repeat(2, 64px); gap: 5px; right: 8px; bottom: 10px; }
  .action-btn { min-height: 48px; }
  #actions .primary-action { min-height: 50px; }
  #footer { display:none; }
  #nub { width: 44px; height: 44px; left: 33px; top: 33px; }
  #stats { min-width: 132px; padding: 6px 8px; font-size: 10.5px; }
  .stat-row { padding: 1px 0; }
}


/* ===== Multiplayer HUD ===== */
#rp5-online { position:fixed; top:12px; left:138px; z-index:8; padding:7px 10px; border-radius:10px; color:#e8edf2; background:rgba(10,18,26,.82); border:1px solid rgba(126,231,255,.25); font:700 11px Tahoma,sans-serif; direction:rtl; pointer-events:none; }
#rp5-online .dot { display:inline-block; width:7px; height:7px; border-radius:50%; background:#ff5c5c; margin-left:5px; }
#rp5-online.connected .dot { background:#5cf08c; box-shadow:0 0 8px #5cf08c; }
#rp5-online-count { color:#7ee7ff; }
@media(max-width:600px){ #rp5-online{top:128px;left:12px;font-size:10px;padding:6px 8px;} }

</style>
</head>
<body>
  <div id="game"></div>
  <div id="rp5-online"><span class="dot"></span><span id="rp5-online-label">آفلاین</span> · آنلاین: <span id="rp5-online-count">0</span></div>

  <!-- برند -->
  <div id="brand" class="hud-panel">
    <span class="brand-title">RP5</span>
    <span class="brand-tag">PRO 3D</span>
  </div>

  <!-- مینی‌مپ -->
  <div id="minimap-box" class="hud-panel">
    <canvas id="minimap" width="120" height="120"></canvas>
  </div>

  <!-- آمار -->
  <div id="stats" class="hud-panel">
    <div class="stat-row"><span class="stat-icon">💵</span><span>پول</span><b id="money" class="stat-value money">1500</b></div>
    <div class="stat-row"><span class="stat-icon">🥔</span><span>محصول</span><b id="potatoes" class="stat-value">0</b></div>
    <div class="stat-row"><span class="stat-icon">📍</span><span>استان</span><b id="region" class="stat-value region">پایتخت</b></div>
    <div class="stat-row"><span class="stat-icon">🚘</span><span>وضعیت</span><b id="mode" class="stat-value">پیاده</b></div>
  </div>

  <!-- جوی‌استیک -->
  <div id="stick"><div id="nub"></div></div>

  <!-- نوار کنترل خلوت و مناسب موبایل -->
  <div id="actions">
    <button id="btn-interact" class="action-btn primary-action">🤝<span>تعامل</span></button>
    <button id="btn-vehicle" class="action-btn primary-action">🚗<span>سوار</span></button>
    <button id="btn-jump" class="action-btn primary-action">⬆️<span>پرش</span></button>
    <button id="btn-more" class="action-btn primary-action">☰<span>بیشتر</span></button>
  </div>
  <div id="action-proxies" aria-hidden="true">
    <button id="btn-garage" class="action-proxy">نمایشگاه</button>
    <button id="btn-sell" class="action-proxy">فروش بار</button>
    <button id="btn-home" class="action-proxy">املاک</button>
    <button id="btn-bag" class="action-proxy">کیف و لباس</button>
    <button id="btn-map" class="action-proxy">نقشه کشور</button>
    <button id="btn-jobs" class="action-proxy">مشاغل</button>
    <button id="btn-settings" class="action-proxy">تنظیمات</button>
  </div>

  <!-- پیام کوتاه -->
  <div id="toast"></div>

  <!-- پنجره مودال -->
  <div id="menu-overlay">
    <div id="menu-box">
      <h3 id="menu-title">RP5</h3>
      <div id="menu-body"></div>
      <button id="menu-close" class="btn-close">بستن</button>
    </div>
  </div>

  <!-- فوتر -->
  <div id="footer">حرکت: جوی‌استیک • چرخش دوربین: کشیدن روی محیط • زوم: دو انگشت</div>
<script type="importmap">
{ "imports": {
  "three": "https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js",
  "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/"
} }
</script>
<script src="https://cdn.socket.io/4.8.1/socket.io.min.js"></script>
<script type="module">
import * as THREE from "three";

// Show useful diagnostics instead of leaving a silent black screen.
window.addEventListener('error', (event) => {
  let panel = document.getElementById('rp5-error-panel');
  if (!panel) {
    panel = document.createElement('div');
    panel.id = 'rp5-error-panel';
    panel.style.cssText = 'position:fixed;z-index:99999;top:10px;left:10px;right:10px;max-height:35vh;overflow:auto;background:#260d12;color:#fff;padding:12px;border:1px solid #ff5c5c;border-radius:10px;font:13px/1.6 sans-serif;direction:ltr;white-space:pre-wrap';
    document.body.appendChild(panel);
  }
  panel.textContent = 'RP5 Error: ' + (event.message || 'Unknown error') + (event.filename ? '\n' + event.filename + ':' + event.lineno : '');
});
window.addEventListener('unhandledrejection', (event) => {
  const message = event.reason?.message || String(event.reason || 'Unknown promise error');
  let panel = document.getElementById('rp5-error-panel');
  if (!panel) {
    panel = document.createElement('div'); panel.id = 'rp5-error-panel';
    panel.style.cssText = 'position:fixed;z-index:99999;top:10px;left:10px;right:10px;max-height:35vh;overflow:auto;background:#260d12;color:#fff;padding:12px;border:1px solid #ff5c5c;border-radius:10px;font:13px/1.6 sans-serif;direction:ltr;white-space:pre-wrap';
    document.body.appendChild(panel);
  }
  panel.textContent = 'RP5 Error: ' + message;
});
// ===== constants.js =====
// ===== پالت رنگی مینی‌مال =====
const PALETTE = {
  sky: 0x1a2838,
  fog: 0x1a2838,
  grass: 0x3d6b3a,
  road: 0x2a2e34,
  highway: 0x1e2228,
  line: 0xe8d05a,
  wall: 0xc4b8a0,
  roof: 0x8c4638,
  glass: 0x5fb8d8,
  trunk: 0x5c3d28,
  leaf: 0x2d6b3b,
  sand: 0xd4c090,
  desert: 0xc9a468,
  water: 0x1868a8,
  rock: 0x5e6469,
  snow: 0xeef4f8,
  crop: 0x4c962b,
  soil: 0x634424,
  skin: 0xf0bd91,
  black: 0x181c20,
  runway: 0x353a40,
  containerA: 0xc43b33,
  containerB: 0x2467a8,
  containerC: 0xd99226,
  accent: 0x7ee7ff,
};

// ===== متریال‌های فلت‌شدید (flat-shaded minimalist) =====
const mat = (color, roughness = 0.9, metalness = 0.0, flat = true) =>
  new THREE.MeshStandardMaterial({
    color,
    roughness,
    metalness,
    flatShading: flat,
  });

const MATERIALS = {
  grass:   mat(PALETTE.grass),
  road:    mat(PALETTE.road),
  highway: mat(PALETTE.highway),
  line:    mat(PALETTE.line, 0.8),
  wall:    mat(PALETTE.wall),
  roof:    mat(PALETTE.roof),
  glass:   mat(PALETTE.glass, 0.25, 0.5),
  trunk:   mat(PALETTE.trunk),
  leaf:    mat(PALETTE.leaf),
  sand:    mat(PALETTE.sand),
  desert:  mat(PALETTE.desert),
  water:   mat(PALETTE.water, 0.2, 0.3),
  rock:    mat(PALETTE.rock),
  snow:    mat(PALETTE.snow, 0.7),
  crop:    mat(PALETTE.crop),
  soil:    mat(PALETTE.soil),
  skin:    mat(PALETTE.skin),
  black:   mat(PALETTE.black),
  runway:  mat(PALETTE.runway),
  containerA: mat(PALETTE.containerA),
  containerB: mat(PALETTE.containerB),
  containerC: mat(PALETTE.containerC),
};

// In the original modules, M is the shared material palette.
const M = MATERIALS;

// ===== کاتالوگ خودروها =====
const CAR_CATALOG = [
  { name: 'پراید ۱۳۱',           brand: 'ایرانی',  color: 0xf0f2f5, maxSpeed: 24, accel: 18, price: 0,    type: 'sedan' },
  { name: 'پژو پارس ELX',        brand: 'ایرانی',  color: 0x222629, maxSpeed: 33, accel: 24, price: 1800, type: 'sedan' },
  { name: 'دنا پلاس توربو',      brand: 'ایرانی',  color: 0xb82525, maxSpeed: 36, accel: 27, price: 2600, type: 'sedan' },
  { name: 'تویوتا لندکروزر',     brand: 'ژاپنی',   color: 0xd9cbb8, maxSpeed: 38, accel: 26, price: 4200, type: 'suv'   },
  { name: 'لامبورگینی اونتادور', brand: 'ایتالیایی', color: 0xf2b705, maxSpeed: 52, accel: 38, price: 7500, type: 'sport' },
];

// ===== کاتالوگ خانه‌ها =====
const HOUSE_CATALOG = [
  { id: 0, name: 'ویلای ساحلی خزر',     x: 235,  z: -45,  price: 3500, owned: false, color: 0xf5efe6 },
  { id: 1, name: 'خانه حومه پایتخت',    x: -45,  z: 135,  price: 1200, owned: true,  color: 0xd8ccb8 },
  { id: 2, name: 'کلبه جنگلی شمال',     x: -165, z: -165, price: 2200, owned: false, color: 0x8c5a32 },
  { id: 3, name: 'خانه محله شمال‌غرب', x: -34, z: -42, price: 1600, owned: false, color: 0xd9c8ae },
  { id: 4, name: 'خانه محله شمال‌شرق', x: 34, z: -42, price: 1750, owned: false, color: 0xe7dfd1 },
  { id: 5, name: 'خانه محله جنوب‌غرب', x: -34, z: 72, price: 1550, owned: false, color: 0xc9d8d8 },
  { id: 6, name: 'خانه محله جنوب‌شرق', x: 34, z: 72, price: 1900, owned: false, color: 0xe2c9b9 },
];

// ===== رنگ‌های لباس =====
const OUTFITS = [
  { name: 'اسپرت آبی',       hex: 0x267bc4 },
  { name: 'کت‌وشلوار مشکی',  hex: 0x1f2429 },
  { name: 'لباس ریسینگ قرمز', hex: 0xc0392b },
  { name: 'لباس دریایی سفید', hex: 0xeef4f8 },
  { name: 'لباس جنگلی سبز',  hex: 0x2d6b3b },
];

// ===== NPCها =====
const NPC_NAMES = ['علی_تهران', 'سارا_۹۹', 'رضا_شوتی', 'امیر_RP', 'مهدی_کینگ', 'نازنین', 'کیان_پرو', 'حسین_راننده'];
const NPC_COLORS = [0xe74c3c, 0x2ecc71, 0x9b59b6, 0xf39c12, 0x1abc9c, 0xe67e22, 0x3498db, 0xd35400];

// ===== نقاط تعامل =====
const POI = {
  farm:    { x: -220, z: 45,   radius: 48, label: 'مزرعه' },
  port:    { x: 315,  z: -180, radius: 55, label: 'بندر' },
  airport: { x: 140,  z: 210,  radius: 50, label: 'فرودگاه' },
};

// ===== حد حرکت =====
const BOUNDS = { minX: -900, maxX: 900, minZ: -900, maxZ: 1050 };

// ===== تنظیمات بازی =====
const CONFIG = {
  initialMoney: 1500,
  walkSpeed: 9.5,
  jumpVelocity: 8.5,
  gravity: 22,
  cameraLerp: 6,
  carDamping: 0.982,
  carSteer: 2.1,
  potatoPrice: 25,
  portWage: 150,
  houseBonus: 100,
};


// ===== world.js =====
// ===== توابع کمکی =====
function box(x, y, z, sx, sy, sz, material, parent, shadow = false) {
  const m = new THREE.Mesh(new THREE.BoxGeometry(sx, sy, sz), material);
  m.position.set(x, y, z);
  if (shadow) { m.castShadow = true; m.receiveShadow = true; }
  parent.add(m);
  return m;
}

function cyl(x, y, z, r1, r2, h, material, parent, seg = 10) {
  const m = new THREE.Mesh(new THREE.CylinderGeometry(r1, r2, h, seg), material);
  m.position.set(x, y, z);
  m.castShadow = false;
  m.receiveShadow = true;
  parent.add(m);
  return m;
}

function cone(x, y, z, r, h, material, seg = 8, parent) {
  const m = new THREE.Mesh(new THREE.ConeGeometry(r, h, seg), material);
  m.position.set(x, y, z);
  m.castShadow = false;
  parent.add(m);
  return m;
}

function sphere(x, y, z, r, material, segW = 7, segH = 6) {
  const m = new THREE.Mesh(new THREE.SphereGeometry(r, segW, segH), material);
  m.position.set(x, y, z);
  m.castShadow = true;
  return m;
}

// ===== ساخت جهان بازی =====
function buildWorld(scene) {
  // --- زمین اصلی ---
  const ground = new THREE.Mesh(new THREE.PlaneGeometry(2200, 2200), M.grass);
  ground.rotation.x = -Math.PI / 2;
  ground.receiveShadow = true;
  scene.add(ground);

  // --- ساحل و دریا (شرق) ---
  box(310, 0.03, 0, 180, 0.06, 1400, M.sand, scene, false);
  box(530, -0.2, 0, 320, 0.4, 1400, M.water, scene, false);
  // Thin translucent wave crests on the sea surface.
  for (let i=0;i<22;i++) {
    const waveMat = new THREE.MeshStandardMaterial({color:0x9bd9ef, transparent:true, opacity:0.22, roughness:0.25, metalness:0.12});
    const wave = box(410 + (i%5)*48, 0.025, -650 + i*58, 12 + (i%4)*5, 0.018, 0.22, waveMat, scene, false);
    wave.userData.speed = 1.8 + (i%4)*0.45;
    wave.userData.phase = i * 0.7;
    waterWaves.push(wave);
  }

  // --- بندر تجاری و کانتینرها ---
  box(330, 0.6, -180, 95, 1.2, 65, mat(0x7a828a), scene);
  for (let i = 0; i < 6; i++) {
    const cMat = [M.containerA, M.containerB, M.containerC][i % 3];
    box(300 + (i % 3) * 14, 2.6, -195 + Math.floor(i / 3) * 28, 10, 4, 4.5, cMat, scene);
    box(300 + (i % 3) * 14, 6.6, -195 + Math.floor(i / 3) * 28, 10, 4, 4.5, [M.containerB, M.containerC, M.containerA][i % 3], scene);
  }

  // --- کشتی باری ---
  const ship = new THREE.Group();
  ship.position.set(395, 1.5, -180);
  box(0, 2, 0, 18, 6, 54, mat(0x232d3b), ship);
  box(0, 7, -18, 14, 7, 10, M.wall, ship);
  box(0, 6.5, 6, 12, 4, 28, M.containerA, ship);
  scene.add(ship);

  // --- کویر (جنوب) ---
  box(-80, 0.03, 440, 620, 0.06, 440, M.desert, scene, false);
  for (let i = 0; i < 14; i++) {
    cone(-280 + i * 45, 4, 340 + (i % 3) * 65, 28 + (i % 4) * 10, 10 + (i % 3) * 5, M.desert, 8, scene);
  }

  // --- رشته‌کوه برفی (شمال) ---
  for (let i = 0; i < 18; i++) {
    const h = 55 + (i % 5) * 18;
    const r = 38 + (i % 4) * 12;
    const mx = -420 + i * 52;
    const mz = -360 - (i % 3) * 45;
    cone(mx, h / 2, mz, r, h, M.rock, 8, scene);
    cone(mx, h * 0.79, mz, r * 0.42, h * 0.42, M.snow, 8, scene);
  }

  // --- جاده‌ها ---
  buildRoad(scene, 10, 0, 560, 18, true);
  buildRoad(scene, 0, 30, 18, 720, true);
  buildRoad(scene, 0, -75, 220, 12);
  buildRoad(scene, 0, 75, 220, 12);
  buildRoad(scene, -75, 0, 12, 220);
  buildRoad(scene, 75, 0, 12, 220);

  // --- پایتخت (برج‌ها) ---
  buildCapital(scene);

  // --- فرودگاه ---
  buildAirport(scene);

  // --- مزرعه و روستا ---
  buildFarm(scene);

  // --- جنگل و درختان ---
  buildForest(scene);

  // --- خانه‌های قابل خرید ---
  HOUSE_CATALOG.forEach(h => buildHouse(scene, h));

  // --- فضای سبز اطراف تمام خانه‌های محله ---
  HOUSE_CATALOG.slice(3).forEach((h, i) => {
    makeTree(scene, h.x - 11, h.z + 9, i % 3, 0.82);
    makeTree(scene, h.x + 11, h.z + 9, (i + 1) % 3, 0.78);
  });

  // --- خانه‌های روستایی (دکوراتیو) ---
  [[-210, -45], [-245, -45], [-210, -85], [-245, -85]].forEach(([vx, vz]) => {
    box(vx, 2.5, vz, 11, 5, 10, M.wall, scene);
    const r = new THREE.Mesh(new THREE.ConeGeometry(8.5, 4.2, 4), M.roof);
    r.position.set(vx, 7.1, vz);
    r.rotation.y = Math.PI / 4;
    scene.add(r);
  });

  // --- حومه‌های جدید و جاده کمربندی برای بزرگ‌تر شدن نقشه ---
  buildRoad(scene, 0, -650, 1500, 22, true);
  buildRoad(scene, 0, 760, 1500, 24, true);
  buildRoad(scene, -720, 30, 22, 1450, true);
  buildRoad(scene, 720, 30, 22, 1450, true);
  // شهرک غربی
  for (let i=0;i<9;i++) {
    for (let j=0;j<7;j++) {
      const x = -650 + i*42, z = -430 + j*42;
      const h = 10 + ((i*7+j*3)%5)*4;
      box(x, h/2, z, 22, h, 22, mat([0x9aabb7,0xb7a58d,0x71899a][(i+j)%3],0.7,0.12), scene);
      for(let f=0;f<3;f++) box(x, 2+f*3.1, z+11.15, 3.2, 1.6, 0.08, M.glass, scene, false);
    }
  }
  // دریاچه و پارک شرقی-شمالی
  box(430, 0.02, -500, 220, 0.05, 150, M.water, scene, false);
  for(let i=0;i<34;i++){
    const x=330+(i%9)*27, z=-590+Math.floor(i/9)*34;
    cyl(x,1.3,z,0.32,0.48,2.6,M.trunk,scene,8);
    const crown=new THREE.Mesh(new THREE.SphereGeometry(2.2,10,8),M.leaf);
    crown.position.set(x,3.8,z); scene.add(crown);
  }
  // مبلمان شهری و وسایل خیابانی ساخته‌شده داخل بازی.
  for(let i=0;i<8;i++){
    const x=-130+i*36;
    // Street lamps.
    cyl(x,3.2,105,0.11,0.15,6.4,mat(0x303943),scene,8);
    box(x+0.75,6.25,105,1.55,0.12,0.12,mat(0x303943),scene);
    scene.add(sphere(x+1.45,6.1,105,0.28,mat(0xffe4a1),10,8));
    // Benches on the sidewalk.
    box(x,0.65,112,3.0,0.18,0.65,mat(0x815b3d),scene);
    box(x,1.05,112.28,3.0,0.85,0.16,mat(0x815b3d),scene);
    for(const dx of [-1.15,1.15]) box(x+dx,0.34,112,0.12,0.65,0.12,mat(0x353b40),scene);
  }
  // پمپ‌بنزین در جاده کمربندی
  box(610,0.2,350,55,0.4,42,mat(0x555d64),scene);
  box(610,4,350,28,7,18,mat(0xe4e8ec),scene);
  for(let k=0;k<3;k++){
    box(592+k*18,1.4,374,4,2.8,3,mat(0xd52c32),scene);
  }
  // تپه‌های سنگی و مسیر خاکی در شمال‌غرب
  for(let i=0;i<18;i++){
    const x=-820+(i%6)*48, z=-760+Math.floor(i/6)*55;
    const rock=new THREE.Mesh(new THREE.DodecahedronGeometry(10+(i%3)*3,1),M.rock);
    rock.position.set(x,7,z); rock.scale.y=0.7; scene.add(rock);
  }

}

function buildRoad(scene, x, z, w, l, isHighway = false) {
  box(x, 0.04, z, w, 0.08, l, isHighway ? M.highway : M.road, scene, false);
  if (w > l) box(x, 0.09, z, w - 4, 0.03, 0.35, M.line, scene, false);
  else box(x, 0.09, z, 0.35, 0.03, l - 4, M.line, scene, false);
}

function buildCapital(scene) {
  const towerColors = [0x6e8599, 0x4b6a88, 0x8ea4b8, 0x5c738a, 0xa8b5c2];
  for (let x = -110; x <= 110; x += 36) {
    for (let z = -110; z <= 110; z += 36) {
      if (Math.abs(x) < 22 || Math.abs(z) < 22) continue;
      if (Math.abs(Math.abs(x) - 75) < 14 || Math.abs(Math.abs(z) - 75) < 14) continue;
      const isSky = (Math.abs(x) + Math.abs(z)) < 90;
      const w = isSky ? 16 : 13;
      const dp = isSky ? 16 : 13;
      const h = isSky ? 38 + (Math.abs(x * z) % 35) : 14 + (Math.abs(x + z) % 16);
      const bMat = isSky ? mat(towerColors[Math.abs(x + z) % towerColors.length], 0.35, 0.4) : M.wall;
      box(x, h / 2, z, w, h, dp, bMat, scene);
      box(x, h + 0.4, z, w + 0.6, 0.8, dp + 0.6, isSky ? M.glass : M.roof, scene);
      if (isSky) cyl(x, h + 5, z, 0.2, 0.4, 9, mat(0xd93838), scene);
    }
  }
}

function buildAirport(scene) {
  box(140, 0.05, 210, 32, 0.1, 170, M.runway, scene, false);
  for (let z = 135; z <= 285; z += 18) box(140, 0.11, z, 1.2, 0.03, 9, mat(0xffffff), scene, false);
  box(105, 6, 210, 22, 12, 48, M.glass, scene);
  cyl(105, 16, 180, 2.5, 2, 14, M.wall, scene);

  // هواپیمای مسافربری
  const plane = new THREE.Group();
  plane.position.set(140, 2.2, 165);
  cyl(0, 0, 0, 1.6, 1.6, 18, mat(0xffffff), plane, 12).rotation.x = Math.PI / 2;
  box(0, 0, 1, 20, 0.3, 3.5, mat(0xdbe6f0), plane);
  box(0, 2.2, -7.5, 0.3, 3.5, 2.5, mat(0x1b6ca8), plane);
  scene.add(plane);
}

function buildFarm(scene) {
  box(-220, 0.08, 45, 65, 0.16, 65, M.soil, scene);
  for (let r = 0; r < 9; r++) {
    for (let c = 0; c < 9; c++) {
      const cx = -245 + c * 6;
      const cz = 20 + r * 6;
      cyl(cx, 0.7, cz, 0.2, 0.2, 1.2, M.crop, scene);
      const leaf = sphere(cx, 1.1, cz, 0.85, M.crop);
      scene.add(leaf);
    }
  }
}

const windTrees = [];
const windClouds = [];
const animatedLamps = [];
const waterWaves = [];

function makeTree(scene, x, z, variant=0, scale=1) {
  const tree = new THREE.Group(); tree.position.set(x,0,z); tree.scale.setScalar(scale); scene.add(tree);
  tree.userData.windPhase = (Math.abs(x * 0.17 + z * 0.11) % 6.28);
  tree.userData.windStrength = 0.018 + (Math.abs(x + z) % 5) * 0.003;
  tree.userData.baseRotation = tree.rotation.z;
  windTrees.push(tree);
  const trunkMat = mat(variant===2 ? 0x765034 : 0x62412b, 0.95);
  const leafMats = [mat(0x2e713c),mat(0x3e8748),mat(0x548f42),mat(0x24623c)];
  cyl(0,1.8,0,0.38,0.55,3.6,trunkMat,tree,9);
  if (variant===1) {
    for(let k=0;k<3;k++){
      const crown=cone(0,3.2+k*1.65,0,2.35-k*0.42,3.5,leafMats[(k+1)%leafMats.length],9,tree);
    }
  } else if (variant===2) {
    // Palm-style tree with curved fronds.
    cyl(0,3.7,0,0.22,0.36,4.0,trunkMat,tree,8);
    for(let k=0;k<7;k++){
      const a=k*Math.PI*2/7;
      const frond=box(Math.cos(a)*1.25,5.3,Math.sin(a)*1.25,2.8,0.12,0.38,leafMats[1],tree);
      frond.rotation.y=-a; frond.rotation.z=-0.22;
    }
    tree.add(sphere(0,5.3,0,0.75,leafMats[2],10,8));
  } else {
    for(const [ox,oy,oz,r] of [[0,4.3,0,2.2],[-1.2,3.8,0,1.45],[1.15,3.9,0.3,1.5],[0,3.9,1.1,1.5],[0.2,5.1,-0.2,1.45]]) {
      const crown=sphere(ox,oy,oz,r,leafMats[Math.floor(Math.abs(x+z+ox)*3)%leafMats.length],12,9);
      crown.scale.set(1,0.9,1); tree.add(crown);
    }
  }
  return tree;
}
function buildForest(scene) {
  // Dense but varied forest outside the central streets.
  for (let i=0;i<170;i++) {
    const tx=-330+(i*73%540), tz=-270+(i*47%500);
    if (Math.abs(tx)<135 && Math.abs(tz)<135) continue;
    if (Math.abs(tx)<18 || Math.abs(tz)<18) continue;
    if (tx>-265 && tx<-175 && tz>5 && tz<90) continue;
    makeTree(scene,tx,tz,i%3,0.82+(i%5)*0.11);
  }
  // Small park near the lake: paths, benches and lamps.
  for(let i=0;i<5;i++){
    const x=350+i*18;
    box(x,0.07,-500,3.8,0.14,34,mat(0xb6aa91),scene,false);
    box(x,0.65,-493,2.3,0.18,0.65,mat(0x805536),scene);
    for(const dx of [-0.9,0.9]) box(x+dx,0.35,-493,0.12,0.6,0.12,mat(0x343b42),scene);
    cyl(x+5,3,-505,0.09,0.12,6,mat(0x303840),scene,8);
    scene.add(sphere(x+5,6.1,-505,0.38,mat(0xffe5a0),10,8));
    makeTree(scene,x+9,-515,i%3,0.72);
  }
  // Soft moving clouds add depth to the sky without heavy textures.
  for (let i=0;i<9;i++) {
    const cloud = new THREE.Group();
    cloud.position.set(-360 + i*92, 82 + (i%3)*12, -300 + (i%4)*170);
    const cloudMat = new THREE.MeshStandardMaterial({color:0xe7edf2, transparent:true, opacity:0.76, roughness:1});
    for (const [ox,oy,oz,r] of [[0,0,0,5],[4,1,0,4],[-4,0.5,1,3.6],[1,1.8,-1,3.2]]) {
      const puff = new THREE.Mesh(new THREE.SphereGeometry(r,12,8),cloudMat);
      puff.position.set(ox,oy,oz); cloud.add(puff);
    }
    cloud.userData.speed = 0.8 + (i%3)*0.22;
    scene.add(cloud); windClouds.push(cloud);
  }
}

function buildHouse(scene, h) {
  const visual = new THREE.Group();
  visual.position.set(h.x, 0, h.z);
  scene.add(visual);
  h.visualGroup = visual;
  const fallback = new THREE.Group();
  fallback.name = 'rp5-detailed-house';
  visual.add(fallback);
  h.fallbackGroup = fallback;

  const wall = mat(h.color || 0xe9dfcf, 0.82);
  const trim = mat(0xf4eee3, 0.72);
  const roof = mat(0x653d35, 0.88);
  const wood = mat(0x70472e, 0.8);
  const glass = new THREE.MeshPhysicalMaterial({color:0x79c6df, roughness:0.18, metalness:0.12, transparent:true, opacity:0.78});
  const sofa = mat(0x32728b, 0.88);
  const cushion = mat(0xd8c6a8, 0.9);
  const dark = mat(0x252b31, 0.48, 0.25);
  // House-local metallic material (the vehicle builder has its own chrome material).
  const chrome = mat(0xb9c2ca, 0.3, 0.85);
  const green = mat(0x397444, 0.95);
  const flower = mat(0xd85b78, 0.8);

  // Foundation and floor.
  box(0,0.22,0,15.2,0.44,13.2,mat(0x77736b),fallback);
  box(0,0.48,0,14.4,0.12,12.4,mat(0xb69b78),fallback);
  // Walls: open front section makes the furnished room visible in-game.
  box(-7,3.35,0,0.55,5.8,12.2,wall,fallback);
  box(7,3.35,0,0.55,5.8,12.2,wall,fallback);
  box(0,3.35,-5.95,14.5,5.8,0.55,wall,fallback);
  box(-4.9,3.35,5.95,4.2,5.8,0.55,wall,fallback);
  box(4.9,3.35,5.95,4.2,5.8,0.55,wall,fallback);
  box(0,5.75,5.95,5.8,1.0,0.55,wall,fallback);
  // Roof slabs and trim.
  const roofL = box(-3.65,7.15,0,7.7,0.38,14.5,roof,fallback); roofL.rotation.z=-0.34;
  const roofR = box(3.65,7.15,0,7.7,0.38,14.5,roof,fallback); roofR.rotation.z=0.34;
  box(0,8.35,0,0.45,0.45,14.4,trim,fallback);
  // Chimney.
  box(-5.1,8.2,-3.6,1.1,3.2,1.1,mat(0x8c5847),fallback);
  box(-5.1,9.9,-3.6,1.4,0.28,1.4,mat(0x51433d),fallback);
  // Entrance, porch and a hinged animated door.
  const doorPivot = new THREE.Group();
  doorPivot.position.set(-0.75, 0, 6.18);
  fallback.add(doorPivot);
  box(0.82,1.9,0,1.65,3.45,0.16,wood,doorPivot);
  box(1.34,2.0,0.11,0.08,0.08,0.05,mat(0xd8b45b),doorPivot);
  h.doorPivot = doorPivot;
  h.doorTarget = 0;
  box(0,0.3,7.1,4.8,0.3,2.4,mat(0xc9c0b1),fallback);
  for (const x of [-2.0,2.0]) box(x,1.65,6.9,0.28,3.0,0.28,trim,fallback);
  box(0,3.2,6.9,4.5,0.25,2.5,roof,fallback);
  // Front windows and frames.
  for (const x of [-4.7,4.7]) {
    box(x,3.6,6.25,2.65,2.2,0.12,glass,fallback);
    box(x,3.6,6.34,2.85,0.13,0.12,trim,fallback);
    box(x,3.6,6.34,0.13,2.35,0.12,trim,fallback);
    box(x,3.6,6.4,0.08,2.1,0.08,trim,fallback);
    box(x,3.6,6.4,2.6,0.08,0.08,trim,fallback);
  }
  // Back windows.
  for (const x of [-4.6,4.6]) {
    box(x,3.6,-6.25,2.4,1.9,0.12,glass,fallback);
    box(x,3.6,-6.34,2.55,0.12,0.12,trim,fallback);
  }

  // Door panels and handle are attached to the hinge so they move with the door.
  for (const y of [1.0, 1.85, 2.65]) box(0.82, y, 0.095, 1.38, 0.055, 0.035, mat(0x4d3023), doorPivot);
  const knob = new THREE.Mesh(new THREE.SphereGeometry(0.075, 12, 8), mat(0xd9b45f, 0.3, 0.8));
  knob.position.set(1.32, 1.9, 0.13); doorPivot.add(knob);
  for (const x of [-4.7,4.7]) {
    box(x,3.6,6.43,0.07,2.0,0.055,trim,fallback);
    box(x,3.6,6.44,2.45,0.07,0.055,trim,fallback);
    box(x,3.6,6.46,0.055,2.0,0.055,trim,fallback);
    box(x-1.55,3.6,6.28,0.28,2.55,0.22,wood,fallback);
    box(x+1.55,3.6,6.28,0.28,2.55,0.22,wood,fallback);
  }
  // Mailbox and house number beside the entrance.
  box(2.2,1.15,7.15,0.55,0.72,0.48,mat(0x263e56),fallback);
  box(2.2,1.55,7.15,0.62,0.08,0.52,chrome,fallback);
  box(2.2,0.62,7.15,0.09,0.72,0.09,dark,fallback);
  // Small exterior wall lamps.
  for (const x of [-2.4,2.4]) {
    box(x,3.15,6.35,0.28,0.42,0.16,dark,fallback);
    const bulb = new THREE.Mesh(new THREE.SphereGeometry(0.13,10,8), new THREE.MeshStandardMaterial({color:0xffe6a8,emissive:0xffc66d,emissiveIntensity:0.35}));
    bulb.position.set(x,3.12,6.46); fallback.add(bulb);
  }

  // Living room: sofa, cushions, coffee table, rug and TV cabinet.
  box(-3.8,0.62,-0.8,4.2,0.18,3.2,cushion,fallback);
  box(-3.8,1.12,-2.05,4.2,1.05,0.42,sofa,fallback);
  box(-5.65,0.98,-0.8,0.45,0.9,2.7,sofa,fallback);
  box(-1.95,0.98,-0.8,0.45,0.9,2.7,sofa,fallback);
  for (const x of [-4.8,-3.8,-2.8]) box(x,1.0,-0.75,0.9,0.18,0.85,cushion,fallback);
  box(-3.6,0.72,2.25,2.5,0.18,1.3,wood,fallback);
  for (const x of [-4.55,-2.65]) for (const z of [1.8,2.7]) box(x,0.35,z,0.12,0.65,0.12,dark,fallback);
  box(0,0.58,-5.35,3.0,0.8,0.55,wood,fallback);
  box(0,1.5,-5.0,2.1,1.25,0.16,dark,fallback);
  // TV screen glow.
  box(0,1.52,-4.88,1.9,1.05,0.04,mat(0x172b3b,0.22,0.3),fallback);
  // Dining set.
  box(3.5,1.05,0.4,2.7,0.18,1.7,wood,fallback);
  for (const x of [2.4,4.6]) for (const z of [-0.15,0.95]) box(x,0.52,z,0.13,1.0,0.13,dark,fallback);
  for (const [x,z] of [[2.1,-0.1],[4.9,-0.1],[2.1,1.0],[4.9,1.0]]) {
    box(x,0.7,z,0.75,0.16,0.72,sofa,fallback); box(x,1.15,z-0.28,0.75,0.9,0.16,sofa,fallback);
  }
  // Bedroom at rear right: bed, pillows and wardrobe.
  box(4.3,0.65,-3.35,3.0,0.38,3.3,wood,fallback);
  box(4.3,0.91,-3.35,2.9,0.2,3.1,mat(0xe8e5dd),fallback);
  box(4.3,1.08,-4.55,2.9,0.25,0.85,mat(0x6d93b1),fallback);
  box(3.5,1.2,-4.8,0.85,0.2,0.55,trim,fallback);
  box(5.1,1.2,-4.8,0.85,0.2,0.55,trim,fallback);
  box(6.0,1.45,-1.7,1.2,2.8,1.0,wood,fallback);
  box(6.0,1.45,-1.15,0.05,2.45,0.06,trim,fallback);
  // Kitchen counter and cabinets.
  box(-3.9,1.25,-4.8,3.4,1.3,1.0,mat(0xd9d3c8),fallback);
  box(-3.9,1.95,-4.8,3.5,0.14,1.1,mat(0x343b42),fallback);
  box(-4.7,2.55,-5.35,1.2,1.0,0.35,trim,fallback);
  box(-3.1,2.55,-5.35,1.2,1.0,0.35,trim,fallback);
  // Indoor plant and ceiling lamp.
  cyl(-6.0,0.85,2.0,0.22,0.28,0.9,wood,fallback,10);
  const plant = new THREE.Group(); plant.position.set(-6,1.3,2); fallback.add(plant);
  for(let i=0;i<5;i++){ const leaf=new THREE.Mesh(new THREE.ConeGeometry(0.22,1.0,6),green); leaf.position.set(Math.cos(i*1.25)*0.18,0.35,Math.sin(i*1.25)*0.18); leaf.rotation.z=(i-2)*0.22; plant.add(leaf); }
  cyl(0,5.5,0,0.12,0.12,0.7,dark,fallback,10);
  fallback.add(sphere(0,5.05,0,0.32,mat(0xffe6a1),12,8));

  // Garden, shrubs, flower boxes and path.
  box(0,0.08,10.0,3.0,0.16,5.0,mat(0xb9a98e),fallback);
  for (const x of [-6.0,-4.8,4.8,6.0]) {
    const bush=sphere(x,0.9,8.2,0.95,green,10,8); bush.scale.set(1.25,0.75,0.9); fallback.add(bush);
  }
  for (const x of [-4.7,4.7]) {
    box(x,1.0,7.25,1.8,0.22,0.55,wood,fallback);
    for(let i=0;i<3;i++) fallback.add(sphere(x-0.55+i*0.55,1.35,7.25,0.18,flower,8,6));
  }
  for(let i=0;i<4;i++) box(0,0.1,8.5+i*1.4,1.4,0.12,1.0,mat(0xc7c0b2),fallback);

  const marker = new THREE.Mesh(new THREE.OctahedronGeometry(0.75), mat(PALETTE.accent, 0.6, 0.1));
  marker.position.set(h.x, 11.5, h.z); scene.add(marker);
  h.marker = marker;
  return marker;
}

function getRegion(x, z) {
  if (x > 220 && z < -110) return 'بندر تجاری شرق';
  if (x > 220) return 'ساحل و دریای شرق';
  if (x < -165 && z > -20) return 'استان کشاورزی';
  if (x < -165) return 'روستای سرسبز';
  if (z < -220) return 'رشته‌کوه برفی شمال';
  if (z > 230) return 'استان کویری جنوب';
  if (x > 85 && z > 120) return 'فرودگاه بین‌المللی';
  return 'پایتخت (مرکز)';
}


// ===== player.js =====

function createPlayer(scene) {
  const group = new THREE.Group();
  group.position.set(0, 0, 14);
  scene.add(group);

  const skin = new THREE.MeshStandardMaterial({ color: 0xd99b70, roughness: 0.78 });
  const shirt = new THREE.MeshStandardMaterial({ color: 0x267bc4, roughness: 0.72 });
  const pants = new THREE.MeshStandardMaterial({ color: 0x202a36, roughness: 0.9 });
  const shoes = new THREE.MeshStandardMaterial({ color: 0x111820, roughness: 0.8 });
  const hair = new THREE.MeshStandardMaterial({ color: 0x211a18, roughness: 0.95 });
  const eyes = new THREE.MeshStandardMaterial({ color: 0x171717, roughness: 0.4 });
  const white = new THREE.MeshStandardMaterial({ color: 0xf4eee6, roughness: 0.45 });

  const torso = new THREE.Mesh(new THREE.CapsuleGeometry(0.39, 0.72, 5, 10), shirt);
  torso.position.y = 1.27;
  torso.castShadow = true;
  group.add(torso);

  const neck = new THREE.Mesh(new THREE.CylinderGeometry(0.13, 0.15, 0.22, 12), skin);
  neck.position.y = 1.83;
  group.add(neck);

  const head = new THREE.Group();
  head.position.y = 2.12;
  group.add(head);
  const skull = new THREE.Mesh(new THREE.SphereGeometry(0.34, 24, 18), skin);
  skull.scale.set(0.94, 1.08, 0.92);
  skull.castShadow = true;
  head.add(skull);

  const hairCap = new THREE.Mesh(new THREE.SphereGeometry(0.345, 20, 12, 0, Math.PI * 2, 0, Math.PI * 0.48), hair);
  hairCap.position.y = 0.08;
  hairCap.scale.set(1.02, 0.9, 1.02);
  head.add(hairCap);

  // Face points toward +Z: eyes, brows, nose and mouth.
  for (const x of [-0.125, 0.125]) {
    const eye = new THREE.Mesh(new THREE.SphereGeometry(0.043, 10, 8), white);
    eye.position.set(x, 0.035, 0.292);
    head.add(eye);
    const pupil = new THREE.Mesh(new THREE.SphereGeometry(0.021, 8, 6), eyes);
    pupil.position.set(x, 0.03, 0.327);
    head.add(pupil);
    const brow = new THREE.Mesh(new THREE.BoxGeometry(0.105, 0.025, 0.025), hair);
    brow.position.set(x, 0.105, 0.292);
    head.add(brow);
  }
  const nose = new THREE.Mesh(new THREE.SphereGeometry(0.045, 10, 8), skin);
  nose.position.set(0, -0.035, 0.33);
  nose.scale.set(0.75, 1, 0.8);
  head.add(nose);
  const mouth = new THREE.Mesh(new THREE.BoxGeometry(0.105, 0.018, 0.018), new THREE.MeshStandardMaterial({color:0x713c36}));
  mouth.position.set(0, -0.14, 0.302);
  head.add(mouth);
  for (const x of [-0.335, 0.335]) {
    const ear = new THREE.Mesh(new THREE.SphereGeometry(0.075, 10, 8), skin);
    ear.position.set(x, -0.005, 0);
    ear.scale.set(0.55, 1, 0.75);
    head.add(ear);
  }

  const armPivots = [];
  for (const side of [-1, 1]) {
    const pivot = new THREE.Group();
    pivot.position.set(side * 0.47, 1.67, 0);
    group.add(pivot);
    const upper = new THREE.Mesh(new THREE.CapsuleGeometry(0.115, 0.36, 4, 8), shirt);
    upper.position.y = -0.27;
    upper.castShadow = true;
    pivot.add(upper);
    const elbow = new THREE.Mesh(new THREE.SphereGeometry(0.115, 10, 8), skin);
    elbow.position.y = -0.53;
    pivot.add(elbow);
    const forearm = new THREE.Mesh(new THREE.CapsuleGeometry(0.095, 0.30, 4, 8), skin);
    forearm.position.y = -0.72;
    forearm.castShadow = true;
    pivot.add(forearm);
    const hand = new THREE.Mesh(new THREE.SphereGeometry(0.105, 10, 8), skin);
    hand.position.y = -0.94;
    pivot.add(hand);
    armPivots.push(pivot);
  }

  const legPivots = [];
  for (const side of [-1, 1]) {
    const pivot = new THREE.Group();
    pivot.position.set(side * 0.19, 0.91, 0);
    group.add(pivot);
    const leg = new THREE.Mesh(new THREE.CapsuleGeometry(0.145, 0.43, 4, 8), pants);
    leg.position.y = -0.31;
    leg.castShadow = true;
    pivot.add(leg);
    const shoe = new THREE.Mesh(new THREE.BoxGeometry(0.27, 0.16, 0.42), shoes);
    shoe.position.set(0, -0.62, 0.09);
    shoe.castShadow = true;
    pivot.add(shoe);
    legPivots.push(pivot);
  }

  const player = {
    group, body: torso, head, armPivots, legPivots,
    jumpV: 0, walkPhase: 0, camera: null, inCar: false
  };
  return player;
}

function updatePlayer(player, input, dt) {
  if (player.inCar) return;
  // حرکت نسبت به جهت دوربین: بالا = جلو، پایین = عقب، چپ/راست مطابق صفحه
  const yaw = cameraOrbitYaw || 0;
  const right = new THREE.Vector3(-Math.cos(yaw), 0, -Math.sin(yaw));
  const forward = new THREE.Vector3(-Math.sin(yaw), 0, Math.cos(yaw));
  const move = right.multiplyScalar(input.x).add(forward.multiplyScalar(input.y));
  if (move.lengthSq() > 1) move.normalize();

  const moving = move.lengthSq() > 0.01;
  player.group.position.x += move.x * CONFIG.walkSpeed * dt;
  player.group.position.z += move.z * CONFIG.walkSpeed * dt;
  player.group.position.x = THREE.MathUtils.clamp(player.group.position.x, BOUNDS.minX, BOUNDS.maxX);
  player.group.position.z = THREE.MathUtils.clamp(player.group.position.z, BOUNDS.minZ, BOUNDS.maxZ);

  if (moving) {
    player.group.rotation.y = Math.atan2(move.x, move.z);
    player.walkPhase += dt * 10.5;
  }
  if (player.modelMixer) { player.modelMixer.update(dt); setRigAction(player, moving ? 'walk' : 'idle'); }
  const swing = moving ? Math.sin(player.walkPhase) * 0.62 : 0;
  const idleBreath = moving ? 0 : Math.sin(performance.now() * 0.0018) * 0.018;
  player.legPivots.forEach((p, i) => p.rotation.x = i === 0 ? swing : -swing);
  player.armPivots.forEach((p, i) => p.rotation.x = i === 0 ? -swing * 0.8 : swing * 0.8);
  player.body.position.y = 1.27 + idleBreath + (moving ? Math.abs(Math.sin(player.walkPhase * 2)) * 0.035 : 0);
  player.head.rotation.z = moving ? Math.sin(player.walkPhase * 0.5) * 0.025 : Math.sin(performance.now() * 0.0012) * 0.012;

  player.jumpV -= CONFIG.gravity * dt;
  player.group.position.y = Math.max(0, player.group.position.y + player.jumpV * dt);
  if (player.group.position.y === 0) player.jumpV = 0;

  const target = player.group.position.clone();
  const dist = cameraDistance || 10.5;
  const cp = Math.cos(cameraPitch || 0.28);
  const offset = new THREE.Vector3(
    Math.sin(cameraOrbitYaw || 0) * cp * dist,
    2.2 + Math.sin(cameraPitch || 0.28) * dist,
    -Math.cos(cameraOrbitYaw || 0) * cp * dist
  );
  player.camera.position.lerp(target.clone().add(offset), 1 - Math.exp(-CONFIG.cameraLerp * dt));
  player.camera.lookAt(target.x, target.y + 1.35, target.z);
}
function jumpPlayer(player) {
  if (!player.inCar && player.group.position.y < 0.1) {
    player.jumpV = CONFIG.jumpVelocity;
  }
}


function setOutfit(player, hex) {
  player.body.material.color.setHex(hex);
  player.armPivots.forEach(p => {
    const upper = p.children[0];
    if (upper && upper.material) upper.material.color.setHex(hex);
  });
}


// ===== vehicle.js =====
function createCar(scene) {
  const car = new THREE.Group();
  car.position.set(4, 0, 14);
  scene.add(car);
  return car;
}


function buildCarModel(car, spec) {
  if (car.userData.modelRoot) {
    car.remove(car.userData.modelRoot);
    car.userData.modelRoot = null;
  }
  car.userData.proceduralChildren = null;
  while (car.children.length) {
    const child = car.children[0];
    car.remove(child);
    child.traverse?.(o => { if (o.geometry) o.geometry.dispose(); });
  }

  const paint = new THREE.MeshStandardMaterial({ color: spec.color, roughness: 0.28, metalness: 0.58 });
  const darkPaint = new THREE.MeshStandardMaterial({ color: 0x171c22, roughness: 0.42, metalness: 0.35 });
  const glass = new THREE.MeshPhysicalMaterial({ color: 0x182c3a, roughness: 0.12, metalness: 0.25, transparent: true, opacity: 0.88, clearcoat: 1 });
  const chrome = new THREE.MeshStandardMaterial({ color: 0xb9c2ca, metalness: 0.88, roughness: 0.22 });
  const rubber = new THREE.MeshStandardMaterial({ color: 0x101215, roughness: 0.95 });
  const lamp = new THREE.MeshStandardMaterial({ color: 0xfff0c2, emissive: 0xffc45a, emissiveIntensity: 0.45 });
  const tail = new THREE.MeshStandardMaterial({ color: 0xb51f27, emissive: 0x6d080c, emissiveIntensity: 0.35 });

  const add = (geo, material, x, y, z, parent = car) => {
    const m = new THREE.Mesh(geo, material);
    m.position.set(x, y, z);
    m.castShadow = true; m.receiveShadow = true;
    parent.add(m); return m;
  };
  const boxPart = (x,y,z,sx,sy,sz,material) => add(new THREE.BoxGeometry(sx,sy,sz), material,x,y,z);
  const spherePart = (x,y,z,sx,sy,sz,material) => {
    const m = add(new THREE.SphereGeometry(1,20,12),material,x,y,z);
    m.scale.set(sx,sy,sz); return m;
  };

  const suv = spec.type === 'suv';
  const sport = spec.type === 'sport';
  const length = suv ? 4.9 : sport ? 4.55 : 4.35;
  const width = suv ? 2.42 : 2.12;
  const baseY = suv ? 0.62 : 0.53;

  // Lower chassis, sculpted bumpers, hood and boot.
  boxPart(0, baseY, 0, width, 0.55, length, paint);
  boxPart(0, baseY + 0.16, length * 0.36, width * 0.96, 0.28, length * 0.25, paint);
  boxPart(0, baseY + 0.13, -length * 0.37, width * 0.96, 0.25, length * 0.22, paint);
  boxPart(0, 0.39, length * 0.51, width * 0.92, 0.22, 0.18, darkPaint);
  boxPart(0, 0.39, -length * 0.51, width * 0.92, 0.22, 0.18, darkPaint);

  // Roof and glasshouse.
  const cabin = new THREE.Group();
  cabin.position.set(0, 0, -0.1);
  car.add(cabin);
  const cabinH = suv ? 1.0 : sport ? 0.62 : 0.78;
  const cabinW = width * (sport ? 0.78 : 0.82);
  const cabinL = sport ? 2.0 : 2.18;
  boxPart(0, 1.28 + cabinH * 0.28, -0.08, cabinW, cabinH * 0.42, cabinL, paint);
  boxPart(0, 1.36 + cabinH * 0.28, 0.02, cabinW * 0.94, cabinH * 0.54, cabinL * 0.92, glass);

  // Windshields and side windows as dark glass panels.
  const windshield = boxPart(0, 1.27, 0.96, cabinW * 0.88, 0.56, 0.07, glass);
  windshield.rotation.x = -0.22;
  const rearGlass = boxPart(0, 1.26, -1.12, cabinW * 0.86, 0.48, 0.07, glass);
  rearGlass.rotation.x = 0.24;
  for (const side of [-1,1]) {
    const sideWindow = boxPart(side * (cabinW/2 + 0.012), 1.34, -0.05, 0.055, 0.48, 1.55, glass);
    sideWindow.rotation.x = 0.02;
    // Mirrors and door handles.
    const mirror = boxPart(side * (width/2 + 0.12), 1.15, 0.72, 0.24, 0.13, 0.22, paint);
    mirror.rotation.y = side * 0.12;
    boxPart(side * (width/2 + 0.018), 0.78, 0.12, 0.035, 0.045, 0.48, chrome);
    boxPart(side * (width/2 + 0.018), 0.78, -0.72, 0.035, 0.045, 0.48, chrome);
  }

  // Front grille, headlights, fog lamps and tail lights.
  boxPart(0, 0.58, length/2 + 0.015, width * 0.34, 0.2, 0.035, darkPaint);
  boxPart(0, 0.57, length/2 + 0.04, width * 0.2, 0.09, 0.025, chrome);
  for (const side of [-1,1]) {
    boxPart(side * width*0.34, 0.78, length/2 + 0.035, 0.42, 0.18, 0.055, lamp);
    boxPart(side * width*0.36, 0.73, -length/2 - 0.035, 0.34, 0.18, 0.055, tail);
    boxPart(side * width*0.38, 0.43, length/2 + 0.03, 0.22, 0.09, 0.04, darkPaint);
  }

  // Four detailed wheels with alloy rims and brake hubs.
  const wheels = [];
  for (const side of [-1,1]) for (const z of [-length*0.32, length*0.32]) {
    const wheelPivot = new THREE.Group();
    wheelPivot.position.set(side * (width/2 - 0.03), 0.43, z);
    car.add(wheelPivot);
    const tire = add(new THREE.CylinderGeometry(0.43,0.43,0.24,24), rubber,0,0,0,wheelPivot);
    tire.rotation.z = Math.PI/2;
    const rim = add(new THREE.CylinderGeometry(0.245,0.245,0.255,20), chrome,0,0,0,wheelPivot);
    rim.rotation.z = Math.PI/2;
    const hub = add(new THREE.CylinderGeometry(0.09,0.09,0.27,16), darkPaint,0,0,0,wheelPivot);
    hub.rotation.z = Math.PI/2;
    for(let k=0;k<8;k++){
      const spoke = boxPart(0,0,0,0.035,0.24,0.035,chrome);
      spoke.position.set(0, Math.cos(k*Math.PI/4)*0.14, Math.sin(k*Math.PI/4)*0.14);
      wheelPivot.add(spoke);
    }
    wheels.push(wheelPivot);
  }

  // Sport/suv-specific details.
  if (sport) {
    boxPart(0, 1.05, -2.05, 2.15, 0.08, 0.18, darkPaint);
    boxPart(0, 0.38, 1.95, 1.5, 0.12, 0.22, darkPaint);
  } else if (suv) {
    boxPart(0, 1.95, -0.1, 1.55, 0.07, 2.0, darkPaint);
    for (const side of [-1,1]) boxPart(side*1.12, 1.72, -0.1, 0.07, 0.12, 2.8, chrome);
  }

  car.userData.wheels = wheels;
  car.userData.paint = paint;
}

// نمایشگاه قابل مشاهده در شروع بازی؛ هر پنج خودرو مدل سه‌بعدی داخلی دارند.
function buildCarShowroom(scene) {
  const floorMat = mat(0x555d64, 0.82, 0.12);
  const lineMat = mat(0xe9edf0, 0.65);
  box(0, 0.08, 38, 70, 0.16, 25, floorMat, scene, false);
  for (let i = 0; i < CAR_CATALOG.length; i++) {
    const x = -25 + i * 12.5;
    box(x, 0.18, 38, 0.12, 0.035, 10.5, lineMat, scene, false);
    const displayCar = new THREE.Group();
    displayCar.position.set(x, 0.18, 38);
    displayCar.rotation.y = Math.PI;
    scene.add(displayCar);
    buildCarModel(displayCar, CAR_CATALOG[i]);
    displayCar.userData.showroomCar = true;
  }
  // تابلو و نورهای محوطه نمایشگاه
  // Signboard behind the display row so it cannot hide the cars from the spawn camera.
  box(0, 3.2, 55.2, 30, 5.8, 0.45, mat(0x1c2a38), scene);
  box(0, 3.3, 54.9, 26, 3.8, 0.08, mat(0x67d9f2, 0.35, 0.12), scene, false);
  for (const x of [-31, 31]) {
    cyl(x, 3.4, 22, 0.12, 0.16, 6.8, mat(0x303943), scene, 10);
    const lampGlow = sphere(x, 6.9, 22, 0.32, mat(0xffe6a1), 12, 8);
    animatedLamps.push(lampGlow);
  }
}

function addBox(x, y, z, sx, sy, sz, material, parent) {
  const m = new THREE.Mesh(new THREE.BoxGeometry(sx, sy, sz), material);
  m.position.set(x, y, z);
  m.castShadow = true;
  parent.add(m);
  return m;
}

function updateCar(car, input, dt, carState) {
  const spec = CAR_CATALOG[carState.currentIdx];

  carState.speed += input.y * spec.accel * dt;
  carState.speed *= CONFIG.carDamping;
  carState.speed = THREE.MathUtils.clamp(carState.speed, -14, spec.maxSpeed);

  if (Math.abs(carState.speed) > 0.4) {
    const dir = carState.speed >= 0 ? 1 : -1;
    // با جهت دوربین یکسان: لمس راست، خودرو را به سمت راست صفحه می‌چرخاند.
    car.rotation.y -= input.x * CONFIG.carSteer * dt * dir * Math.min(1, Math.abs(carState.speed) / 7);
  }

  car.position.x += Math.sin(car.rotation.y) * carState.speed * dt;
  car.position.z += Math.cos(car.rotation.y) * carState.speed * dt;

  car.position.x = THREE.MathUtils.clamp(car.position.x, BOUNDS.minX, BOUNDS.maxX);
  car.position.z = THREE.MathUtils.clamp(car.position.z, BOUNDS.minZ, BOUNDS.maxZ);
  if (car.userData.wheels) {
    car.userData.wheels.forEach((w, i) => {
      w.rotation.x += carState.speed * dt / 0.43;
      if (i < 2) w.rotation.y = -input.x * 0.22;
    });
  }
  return carState.speed;
}

function updateCarCamera(car, camera, dt) {
  const yaw = car.rotation.y + (cameraOrbitYaw || 0);
  const dist = cameraDistance ? cameraDistance * 1.25 : 13.2;
  const pitch = cameraPitch || 0.28;
  const cp = Math.cos(pitch);
  const offset = new THREE.Vector3(Math.sin(yaw) * cp * dist, 2.6 + Math.sin(pitch) * dist, -Math.cos(yaw) * cp * dist);
  const target = car.position.clone().add(offset);
  camera.position.lerp(target, 1 - Math.exp(-CONFIG.cameraLerp * dt));
  camera.lookAt(car.position.x, car.position.y + 1.1, car.position.z);
}

function tryEnterCar(player, car) {
  if (player.group.position.distanceTo(car.position) < 9) {
    player.inCar = true;
    player.group.visible = false;
    return true;
  }
  return false;
}

function exitCar(player, car) {
  player.inCar = false;
  player.group.visible = true;
  const offset = new THREE.Vector3(2.8, 0, 0)
    .applyAxisAngle(new THREE.Vector3(0, 1, 0), car.rotation.y);
  player.group.position.copy(car.position).add(offset);
  player.group.position.y = 0;
}


// ===== npc.js =====

function createNPCs(scene) {
  const npcs = [];
  for (let i = 0; i < NPC_NAMES.length; i++) {
    const p = createPlayer(scene);
    p.group.position.set(-60 + i * 18, 0, -40 + (i % 3) * 35);
    p.body.material.color.setHex(NPC_COLORS[i]);
    npcs.push({
      mesh: p.group, rig: p,
      angle: Math.random() * Math.PI * 2,
      speed: 1.2 + Math.random() * 1.1,
      timer: 2 + Math.random() * 4,
      phase: Math.random() * 6
    });
  }
  return npcs;
}

function updateNPCs(npcs, dt) {
  for (const n of npcs) {
    n.timer -= dt;
    if (n.timer <= 0) {
      n.angle = Math.random() * Math.PI * 2;
      n.timer = 2 + Math.random() * 4;
    }
    n.mesh.rotation.y = n.angle;
    n.phase += dt * 5;
    if (n.rig.modelMixer) { n.rig.modelMixer.update(dt); setRigAction(n.rig, 'walk'); }
    n.rig.legPivots.forEach((p, i) => p.rotation.x = Math.sin(n.phase) * 0.38 * (i === 0 ? 1 : -1));
    n.rig.armPivots.forEach((p, i) => p.rotation.x = Math.sin(n.phase) * 0.25 * (i === 0 ? -1 : 1));
    n.mesh.position.x += Math.sin(n.angle) * n.speed * dt;
    n.mesh.position.z += Math.cos(n.angle) * n.speed * dt;

    // محدودیت حرکت NPC
    n.mesh.position.x = THREE.MathUtils.clamp(n.mesh.position.x, -200, 200);
    n.mesh.position.z = THREE.MathUtils.clamp(n.mesh.position.z, -200, 200);
  }
}


// ===== minimap.js =====
function createMinimap() {
  const canvas = document.getElementById('minimap');
  const ctx = canvas.getContext('2d');
  const W = 120, H = 120;

  function draw(px, pz) {
    ctx.clearRect(0, 0, W, H);

    // پس‌زمینه (سبزهزار)
    ctx.fillStyle = '#1d3b2a';
    ctx.fillRect(0, 0, W, H);

    // دریا (شرق)
    ctx.fillStyle = '#1868a8';
    ctx.fillRect(92, 0, 28, H);

    // ساحل (نوار کم‌رنگ)
    ctx.fillStyle = '#3a5048';
    ctx.fillRect(88, 0, 4, H);

    // کویر (جنوب)
    ctx.fillStyle = '#c9a468';
    ctx.fillRect(0, 92, 92, 28);

    // جاده‌ها
    ctx.fillStyle = '#3a3e44';
    ctx.fillRect(58, 5, 4, 110);
    ctx.fillRect(5, 62, 90, 4);

    // فرودگاه (نقطه روشن)
    ctx.fillStyle = '#4a4f55';
    ctx.fillRect(70, 80, 8, 14);

    // پایتخت (نقطه خاکستری)
    ctx.fillStyle = '#4a5560';
    ctx.fillRect(44, 44, 16, 16);

    // نشانگر بازیکن
    const mx = THREE_Utils.clamp(60 + (px / 650) * 52, 4, 116);
    const mz = THREE_Utils.clamp(60 + (pz / 650) * 52, 4, 116);

    // هاله
    ctx.fillStyle = 'rgba(255,59,59,0.25)';
    ctx.beginPath();
    ctx.arc(mx, mz, 7, 0, Math.PI * 2);
    ctx.fill();

    // نقطه
    ctx.fillStyle = '#ff3b3b';
    ctx.beginPath();
    ctx.arc(mx, mz, 3.5, 0, Math.PI * 2);
    ctx.fill();
  }

  return { draw };
}

// ابزار کمکی clamp بدون وابستگی به three
const THREE_Utils = {
  clamp: (v, min, max) => Math.max(min, Math.min(max, v)),
};


// ===== ui.js =====
// ===== توابع کمکی DOM =====
const $ = (id) => document.getElementById(id);

// ===== به‌روزرسانی آمار =====
function updateStats(state) {
  $('money').textContent = state.money === Infinity ? '∞' : state.money.toLocaleString('fa-IR');
  $('potatoes').textContent = state.potatoes;
  $('mode').textContent = state.inCar
    ? CAR_CATALOG[state.currentCarIdx].name.split(' ')[0]
    : 'پیاده';
  $('region').textContent = state.region;
}

// ===== توست =====
let toastTimer;
function toast(text) {
  const el = $('toast');
  el.textContent = text;
  el.style.display = 'block';
  el.style.animation = 'none';
  void el.offsetWidth;
  el.style.animation = '';
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => { el.style.display = 'none'; }, 2800);
}

// ===== منو =====
function openMenu(title, html) {
  $('menu-title').textContent = title;
  $('menu-body').innerHTML = html;
  $('menu-overlay').style.display = 'flex';
}

function closeMenu() {
  $('menu-overlay').style.display = 'none';
}

// ===== جوی‌استیک =====
function setupJoystick() {
  let joy = { x: 0, y: 0 };
  let active = false;
  const stick = $('stick');
  const nub = $('nub');

  function setJoy(e) {
    const r = stick.getBoundingClientRect();
    const cx = r.left + r.width / 2;
    const cy = r.top + r.height / 2;
    let dx = e.clientX - cx;
    let dy = e.clientY - cy;
    const len = Math.hypot(dx, dy);
    const max = 42;
    if (len > max) { dx = (dx / len) * max; dy = (dy / len) * max; }
    joy.x = dx / max;
    joy.y = -dy / max;
    nub.style.left = (39 + dx) + 'px';
    nub.style.top = (39 + dy) + 'px';
  }

  stick.addEventListener('pointerdown', (e) => {
    active = true;
    stick.setPointerCapture(e.pointerId);
    setJoy(e);
  });
  stick.addEventListener('pointermove', (e) => { if (active) setJoy(e); });

  function release() {
    active = false;
    joy = { x: 0, y: 0 };
    nub.style.left = '39px';
    nub.style.top = '39px';
  }
  stick.addEventListener('pointerup', release);
  stick.addEventListener('pointercancel', release);

  return () => joy;
}

// ===== کیبورد =====
function setupKeyboard() {
  const keys = {};

  addEventListener('keydown', (e) => {
    const k = e.key.toLowerCase();
    keys[k] = true;
    if (k === ' ') $('btn-jump').click();
    if (k === 'f') $('btn-vehicle').click();
    if (k === 'e') $('btn-interact').click();
  });

  addEventListener('keyup', (e) => {
    keys[e.key.toLowerCase()] = false;
  });

  return () => {
    let x = 0, y = 0;
    if (keys['a'] || keys['arrowleft']) x = -1;
    if (keys['d'] || keys['arrowright']) x = 1;
    if (keys['w'] || keys['arrowup']) y = 1;
    if (keys['s'] || keys['arrowdown']) y = -1;
    return { x, y };
  };
}

// ===== ترکیب ورودی جوی‌استیک + کیبورد =====
function combineInput(joyFn, keyFn) {
  return () => {
    const j = joyFn();
    const k = keyFn();
    return {
      x: Math.abs(j.x) > Math.abs(k.x) ? j.x : (k.x || j.x),
      y: Math.abs(j.y) > Math.abs(k.y) ? j.y : (k.y || j.y),
    };
  };
}

// ===== محتوای منوها =====
function garageMenuHTML(state, callbacks) {
  let html = '<p>خودروهای قابل خرید و احضار:</p>';
  CAR_CATALOG.forEach((c, i) => {
    const owned = state.ownedCars.includes(i);
    const label = owned
      ? (state.currentCarIdx === i ? '✅ فعال' : '🔑 انتخاب')
      : `💵 ${c.price} سکه`;
    html += `<button class="opt-btn" data-action="select-car" data-idx="${i}"><span>${c.name} <small style="color:#8b9bad">(${c.brand})</small></span><span>${label}</span></button>`;
  });
  return html;
}

function homeMenuHTML(state) {
  let html = '<p>املاک قابل خرید و ورود:</p>';
  HOUSE_CATALOG.forEach((h, i) => {
    const status = h.owned ? '🚪 ورود به خانه' : `🏠 خرید (${h.price} سکه)`;
    html += `<button class="opt-btn" data-action="house" data-idx="${i}"><span>${h.name}</span><span>${status}</span></button>`;
  });
  return html;
}

function houseInteriorHTML(h) {
  return `
    <p>🛋️ داخل خانه: مبل، میز جلو مبلی، تلویزیون، میز غذاخوری، تخت، کمد و کابینت آشپزخانه.</p>
    <p>🌿 حیاط خانه هم باغچه، گلدان و مسیر ورودی دارد.</p>
    <button class="opt-btn" data-action="house-bonus"><span>🎁 دریافت پاداش روزانه خانه</span><span>+${CONFIG.houseBonus} سکه</span></button>
  `;
}

function bagMenuHTML(state) {
  let html = `
    <p>🥔 محصولات کشاورزی: <b>${state.potatoes}</b> جعبه</p>
    <p>👥 بازیکنان آنلاین در سرور: <b>${NPC_NAMES.length + 1}</b> نفر</p>
    <p>🧍 انتخاب اسکین سه‌بعدی:</p>
  `;
  if (typeof PEOPLE_MODELS !== 'undefined') PEOPLE_MODELS.forEach((p, i) => {
    html += `<button class="opt-btn" data-action="skin" data-idx="${i}"><span>${p.name}</span><span>انتخاب</span></button>`;
  });
  return html;
}

function mapMenuHTML() {
  return `
    <p>🏙️ <b>مرکز (۰, ۰):</b> پایتخت، برج‌ها و نمایشگاه</p>
    <p>🌾 <b>غرب (-۲۲۰, ۴۵):</b> استان کشاورزی و روستا</p>
    <p>🏖️ <b>شرق (+۳۱۰, ۰):</b> ساحل، دریا و بندر تجاری</p>
    <p>✈️ <b>جنوب شرق (+۱۴۰, +۲۱۰):</b> فرودگاه بین‌المللی</p>
    <p>🏜️ <b>جنوب (۰, +۴۴۰):</b> استان کویری و تپه‌های شنی</p>
    <p>🏔️ <b>شمال (۰, -۳۶۰):</b> رشته‌کوه‌های برفی</p>
  `;
}


// ===== RP5 local 3D model library (procedural; no external model downloads) =====
const CAR_MODELS = CAR_CATALOG.map(c => ({name:c.name, type:c.type}));
const HOUSE_MODELS = HOUSE_CATALOG.map(h => ({name:h.name}));
const PEOPLE_MODELS = [
  {name:'اسپرت آبی', shirt:0x267bc4, pants:0x202a36, hair:0x211a18, style:'cap'},
  {name:'رسمی مشکی', shirt:0x242a32, pants:0x161b22, hair:0x35251c, style:'tie'},
  {name:'قرمز', shirt:0xb93632, pants:0x252b34, hair:0x211a18, style:'glasses'},
  {name:'سفید', shirt:0xe8e8e4, pants:0x38485a, hair:0x4a3023, style:'scarf'},
  {name:'سبز', shirt:0x367b48, pants:0x26352b, hair:0x241d19, style:'cap'},
  {name:'کارگر', shirt:0xe2a52d, pants:0x343c45, hair:0x2a211d, style:'helmet'},
  {name:'پزشک', shirt:0xe9eee9, pants:0x46647a, hair:0x30231d, style:'stethoscope'}
];
function applyCharacterModel(rig, index=0, isPlayer=false) {
  const item = PEOPLE_MODELS[index % PEOPLE_MODELS.length];
  if (!rig || !rig.group) return;
  if (rig.body?.material?.color) rig.body.material.color.setHex(item.shirt);
  rig.armPivots?.forEach(p => { if(p.children[0]?.material?.color) p.children[0].material.color.setHex(item.shirt); });
  rig.legPivots?.forEach(p => { if(p.children[0]?.material?.color) p.children[0].material.color.setHex(item.pants); });
  if (rig.head?.children?.[1]?.material?.color) rig.head.children[1].material.color.setHex(item.hair);
  if (rig.skinAccessory) { rig.group.remove(rig.skinAccessory); rig.skinAccessory.traverse(o=>{if(o.geometry)o.geometry.dispose();}); }
  const deco = new THREE.Group(); deco.name = 'rp5-skin-accessories'; rig.group.add(deco); rig.skinAccessory = deco;
  const addBox = (x,y,z,sx,sy,sz,color) => { const m=new THREE.Mesh(new THREE.BoxGeometry(sx,sy,sz),new THREE.MeshStandardMaterial({color,roughness:0.65})); m.position.set(x,y,z); m.castShadow=true; deco.add(m); return m; };
  if (item.style === 'cap' || item.style === 'helmet') {
    const helmet = new THREE.Mesh(new THREE.SphereGeometry(item.style==='helmet'?0.36:0.32,18,10,0,Math.PI*2,0,Math.PI/2),new THREE.MeshStandardMaterial({color:item.style==='helmet'?0xf0b323:0x243b59,roughness:0.62}));
    helmet.position.set(0,2.47,0); deco.add(helmet);
    addBox(0,2.42,0.16,0.58,0.07,0.35,item.style==='helmet'?0xe6a51a:0x1c2f48);
  } else if (item.style === 'tie') {
    addBox(0,1.38,0.39,0.13,0.52,0.055,0x9c2730);
    addBox(0,1.67,0.39,0.22,0.12,0.055,0xe5e5e5);
  } else if (item.style === 'glasses') {
    for (const x of [-0.13,0.13]) { const lens=new THREE.Mesh(new THREE.BoxGeometry(0.16,0.085,0.035),new THREE.MeshStandardMaterial({color:0x111820,metalness:0.25,roughness:0.25})); lens.position.set(x,2.16,0.305); deco.add(lens); }
    addBox(0,2.16,0.31,0.12,0.035,0.035,0x252525);
  } else if (item.style === 'scarf') {
    addBox(0,1.82,0.02,0.72,0.16,0.48,0x4f92a8);
  } else if (item.style === 'stethoscope') {
    const tube=new THREE.Mesh(new THREE.TorusGeometry(0.22,0.035,8,18,Math.PI),new THREE.MeshStandardMaterial({color:0x263b4b,metalness:0.25}));
    tube.rotation.z=Math.PI; tube.position.set(0,1.72,0.36); deco.add(tube);
    const bell=new THREE.Mesh(new THREE.CylinderGeometry(0.07,0.07,0.045,12),new THREE.MeshStandardMaterial({color:0xb9c4ca,metalness:0.75})); bell.rotation.x=Math.PI/2; bell.position.set(0.2,1.42,0.42); deco.add(bell);
  }
  if (isPlayer) toast(`اسکین «${item.name}» فعال شد.`);
}
function applyPlayerSkin(player, index=0) { applyCharacterModel(player,index,true); }
function applyNPCSkin(npcRig, index=0) { applyCharacterModel(npcRig,index,false); }
function supportersMenuHTML() {
  return `<p>📦 کتابخانه مدل‌های داخلی RP5</p>
    <p>خودروها، خانه‌ها، شخصیت‌ها، درختان و مبلمان با هندسه سه‌بعدی داخل خود بازی ساخته می‌شوند و برای نمایش این مدل‌ها دانلود جداگانه لازم نیست.</p>
    <p><b>🚗 خودروها:</b> ${CAR_MODELS.map(x=>x.name).join("، ")}</p>
    <p><b>🏠 خانه‌ها:</b> ${HOUSE_MODELS.map(x=>x.name).join("، ")}</p>
    <p><b>🧍 شخصیت‌ها:</b> ${PEOPLE_MODELS.map(x=>x.name).join("، ")}</p>
    <p><b>🌳 محیط:</b> درخت‌های چندمدلی، بوته، نیمکت، چراغ پارک و فضای سبز.</p>`;
}
function assetLibraryHTML() {
  return `<p>📦 کتابخانه سه‌بعدی داخلی RP5</p>
    <p>خودروها، خانه‌ها و شخصیت‌ها با هندسه داخلی Three.js ساخته می‌شوند؛ برای خود مدل‌ها دانلود جداگانه لازم نیست.</p>
    <button class="opt-btn" data-action="open-garage"><span>🚘 خودروهای سه‌بعدی</span><span>انتخاب</span></button>
    <button class="opt-btn" data-action="open-homes"><span>🏠 خانه‌های مبله</span><span>مشاهده</span></button>
    <button class="opt-btn" data-action="open-skins"><span>🧍 شخصیت‌ها و لباس‌ها</span><span>انتخاب</span></button>
    <p><b>محیط بازی:</b> درخت‌های متنوع، بوته‌ها، نیمکت، چراغ پارک، خانه‌های مبله، ساحل، بندر، فرودگاه، مزرعه، کویر و کوهستان.</p>`;
}
function skinMenuHTML() {
  return `<p>یک ظاهر برای شخصیت انتخاب کن:</p>` + PEOPLE_MODELS.map((p,i)=>`<button class="opt-btn" data-action="skin" data-idx="${i}"><span>${p.name}</span><span>انتخاب</span></button>`).join('');
}

// ===== صحنه، دوربین و رندرر =====
const scene = new THREE.Scene();
scene.background = new THREE.Color(PALETTE.sky);
scene.fog = new THREE.FogExp2(PALETTE.fog, 0.0016);

const camera = new THREE.PerspectiveCamera(62, innerWidth / innerHeight, 0.2, 950);
camera.position.set(0, 8.5, -14);

const renderer = new THREE.WebGLRenderer({ antialias: true, powerPreference: 'high-performance' });
renderer.setPixelRatio(Math.min(devicePixelRatio || 1, 1.25));
renderer.setSize(innerWidth, innerHeight);
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
renderer.shadowMap.autoUpdate = true;
document.getElementById('game').appendChild(renderer.domElement);

// کنترل چرخش دوربین با کشیدن انگشت و زوم با دو انگشت
let cameraOrbitYaw = 0;
let cameraPitch = 0.28;
let cameraDistance = 10.5;
let lookPointer = null;
let lastLookX = 0, lastLookY = 0;
const activeTouches = new Map();
let pinchStart = 0, pinchDistanceStart = 0;
renderer.domElement.addEventListener('pointerdown', (e) => {
  activeTouches.set(e.pointerId, {x:e.clientX,y:e.clientY});
  if (activeTouches.size === 2) {
    const pts = [...activeTouches.values()];
    pinchStart = Math.hypot(pts[0].x-pts[1].x, pts[0].y-pts[1].y);
    pinchDistanceStart = cameraDistance;
  }
  lookPointer = e.pointerId; lastLookX = e.clientX; lastLookY = e.clientY;
  try { renderer.domElement.setPointerCapture(e.pointerId); } catch (_) {}
});
renderer.domElement.addEventListener('pointermove', (e) => {
  if (activeTouches.has(e.pointerId)) activeTouches.set(e.pointerId, {x:e.clientX,y:e.clientY});
  if (activeTouches.size === 2 && pinchStart > 0) {
    const pts = [...activeTouches.values()];
    const d = Math.hypot(pts[0].x-pts[1].x, pts[0].y-pts[1].y);
    cameraDistance = THREE.MathUtils.clamp(pinchDistanceStart * pinchStart / Math.max(30,d), 6.5, 18);
    return;
  }
  if (lookPointer !== e.pointerId) return;
  const dx = e.clientX - lastLookX, dy = e.clientY - lastLookY;
  lastLookX = e.clientX; lastLookY = e.clientY;
  cameraOrbitYaw -= dx * 0.006;
  cameraPitch = THREE.MathUtils.clamp(cameraPitch + dy * 0.0035, 0.08, 0.78);
});
const stopLook = (e) => {
  activeTouches.delete(e.pointerId);
  if (activeTouches.size < 2) pinchStart = 0;
  lookPointer = null;
};
renderer.domElement.addEventListener('pointerup', stopLook);
renderer.domElement.addEventListener('pointercancel', stopLook);
window.addEventListener('blur', () => { activeTouches.clear(); lookPointer = null; pinchStart = 0; });
renderer.domElement.addEventListener('wheel', (e) => {
  cameraDistance = THREE.MathUtils.clamp(cameraDistance + Math.sign(e.deltaY) * 0.8, 6.5, 18);
}, { passive: true });

// ===== نورپردازی =====
const hemi = new THREE.HemisphereLight(0xc4d8ea, 0x3a4a3a, 1.3);
scene.add(hemi);

const sun = new THREE.DirectionalLight(0xfff0d8, 2.0);
sun.position.set(-140, 220, 120);
sun.castShadow = true;
sun.shadow.mapSize.set(1024, 1024);
sun.shadow.camera.near = 10;
sun.shadow.camera.far = 650;
const sd = 220;
sun.shadow.camera.left = -sd; sun.shadow.camera.right = sd;
sun.shadow.camera.top = sd; sun.shadow.camera.bottom = -sd;
scene.add(sun);

// ===== ساخت جهان =====
buildWorld(scene);
// All houses are created locally by buildWorld(); no remote model requests.

// ===== بازیکن =====
const player = createPlayer(scene);
player.inCar = false;
player.jumpV = 0;
player.camera = camera;

// ===== ماشین =====
const car = createCar(scene);
buildCarModel(car, CAR_CATALOG[0]);

buildCarShowroom(scene);
applyPlayerSkin(player, 0);

// ===== NPCها =====
const npcs = createNPCs(scene);
npcs.forEach((npc, index) => applyNPCSkin(npc.rig, index % PEOPLE_MODELS.length));

// ===== مینی‌مپ =====
const minimap = createMinimap();

// ===== وضعیت بازی =====
const state = {
  activeJob: null,
  money: Infinity, // پول نامحدود: خریدها موجودی را کم نمی‌کنند
  potatoes: 0,
  inCar: false,
  currentCarIdx: 0,
  ownedCars: [0],
  carSpeed: 0,
  region: 'پایتخت',
};

// ===== ورودی =====
const joyFn = setupJoystick();
const keyFn = () => ({ x: 0, y: 0 }); // نسخه موبایل: ورودی فقط لمسی
const getInput = combineInput(joyFn, keyFn);

// ===== به‌روزرسانی آمار =====
function refreshStats() {
  state.inCar = player.inCar;
  updateStats(state);
}


function jobsMenuHTML(state) {
  const jobs = [
    { id:'farm', title:'کشاورز', place:'مزرعه غرب', pay:320, desc:'برداشت محصول و تحویل بار' },
    { id:'port', title:'کارگر بندر', place:'بندر شرق', pay:520, desc:'تخلیه و تحویل محموله کشتی' },
    { id:'pilot', title:'خدمات فرودگاه', place:'فرودگاه جنوب‌شرق', pay:650, desc:'انجام مأموریت زمینی فرودگاه' }
  ];
  return `<p>یک شغل انتخاب کن، سپس به محل کار برو و دکمه «تعامل» را بزن.</p>` +
    jobs.map(j => `<button class="opt-btn" data-action="job" data-job="${j.id}">
      <span><b>${j.title}</b><br><small>${j.place} · ${j.desc}</small></span>
      <span>${j.pay} سکه</span></button>`).join('') +
    `<p style="margin-top:12px">شغل فعال: <b>${state.activeJob || 'نداری'}</b></p>`;
}

// ===== عملکردهای دکمه‌ها =====

// سوار/پیاده شدن
document.getElementById('btn-vehicle').onclick = () => {
  if (!player.inCar) {
    if (tryEnterCar(player, car)) {
      state.inCar = true;
      toast(`سوار ${CAR_CATALOG[state.currentCarIdx].name} شدی!`);
      refreshStats();
    } else {
      toast('برای سوار شدن به ماشین نزدیک شو (یا از نمایشگاه احضار کن).');
    }
  } else {
    exitCar(player, car);
    state.carSpeed = 0;
    state.inCar = false;
    toast('از ماشین پیاده شدی.');
    refreshStats();
  }
};

// پرش
document.getElementById('btn-jump').onclick = () => {
  jumpPlayer(player);
};

// نمایشگاه ماشین
document.getElementById('btn-garage').onclick = () => {
  openMenu('نمایشگاه و پارکینگ RP5', garageMenuHTML(state));
};

// منوی جمع‌وجور برای امکانات فرعی؛ دکمه‌های اصلی همیشه روی صفحه می‌مانند.
document.getElementById('btn-more').onclick = () => {
  openMenu('منوی بازی RP5', `
    <button class="opt-btn" data-action="quick-garage"><span>🚘 نمایشگاه و پارکینگ</span><span>باز کردن</span></button>
    <button class="opt-btn" data-action="quick-home"><span>🏠 املاک و خانه‌ها</span><span>باز کردن</span></button>
    <button class="opt-btn" data-action="quick-bag"><span>🧍 کیف و اسکین‌ها</span><span>باز کردن</span></button>
    <button class="opt-btn" data-action="quick-jobs"><span>💼 مشاغل و مأموریت‌ها</span><span>باز کردن</span></button>
    <button class="opt-btn" data-action="quick-map"><span>🗺️ نقشه کشور</span><span>باز کردن</span></button>
    <button class="opt-btn" data-action="quick-sell"><span>📦 فروش محصولات</span><span>فروش</span></button>
    <button class="opt-btn" data-action="quick-settings"><span>⚙️ تنظیمات و کتابخانه</span><span>باز کردن</span></button>`);
};

// مشاغل
 document.getElementById('btn-jobs').onclick = () => {
  openMenu('مشاغل و مأموریت‌ها', jobsMenuHTML(state));
};

// فروش بار
document.getElementById('btn-sell').onclick = () => {
  if (state.potatoes > 0) {
    const earned = state.potatoes * CONFIG.potatoPrice;
    state.money += earned;
    toast(`محصولات در بازار فروخته شد! درآمد: ${earned} سکه`);
    state.potatoes = 0;
    refreshStats();
  } else {
    toast('باری برای فروش نداری! به مزرعه در غرب کشور برو.');
  }
};

// املاک
document.getElementById('btn-home').onclick = () => {
  openMenu('بنگاه املاک کشور RP5', homeMenuHTML(state));
};

// کیف و لباس
document.getElementById('btn-bag').onclick = () => {
  openMenu('کیف و کمد لباس', bagMenuHTML(state));
};

// تنظیمات، کتابخانه مدل‌ها و حمایت‌کننده‌ها
document.getElementById('btn-settings').onclick = () => {
  openMenu('تنظیمات RP5', `<p>⚙️ تنظیمات بازی</p>
    <button class="opt-btn" data-action="multiplayer"><span>🌐 اتصال به دنیای آنلاین</span><span>ورود</span></button>
    <button class="opt-btn" data-action="library"><span>📦 کتابخانه مدل‌های سه‌بعدی</span><span>باز کردن</span></button>
    <button class="opt-btn" data-action="supporters"><span>💙 حمایت‌کننده‌ها و سازندگان</span><span>مشاهده</span></button>`);
};

// نقشه کشور
document.getElementById('btn-map').onclick = () => {
  openMenu('نقشه جامع کشور RP5', mapMenuHTML());
};

// تعامل
document.getElementById('btn-interact').onclick = () => {
  const pos = player.inCar ? car.position : player.group.position;

  // شغل کشاورزی
  if (dist2D(pos, POI.farm.x, POI.farm.z) < POI.farm.radius) {
    if (state.activeJob !== 'farm') { toast('برای کار در مزرعه، اول از مرکز استخدام شغل کشاورز را بگیر.'); return; }
    state.potatoes += 4;
    state.money += 320;
    state.activeJob = null;
    toast('🌾 مأموریت کشاورزی انجام شد! ۴ جعبه محصول + ۳۲۰ سکه');
    refreshStats(); return;
  }
  // شغل بندر
  if (dist2D(pos, POI.port.x, POI.port.z) < POI.port.radius) {
    if (state.activeJob !== 'port') { toast('اول شغل کارگر بندر را از مرکز استخدام انتخاب کن.'); return; }
    state.money += 520;
    state.activeJob = null;
    toast('🚢 محموله بندر تحویل شد! دستمزد: ۵۲۰ سکه');
    refreshStats(); return;
  }
  // شغل فرودگاه
  if (dist2D(pos, POI.airport.x, POI.airport.z) < POI.airport.radius) {
    if (state.activeJob !== 'pilot') { toast('اول مأموریت خدمات فرودگاه را از مرکز استخدام بگیر.'); return; }
    state.money += 650;
    state.activeJob = null;
    toast('✈️ مأموریت فرودگاه تکمیل شد! پاداش: ۶۵۰ سکه');
    refreshStats(); return;
  }
  // خانه‌ها
  for (let i = 0; i < HOUSE_CATALOG.length; i++) {
    if (dist2D(pos, HOUSE_CATALOG[i].x, HOUSE_CATALOG[i].z) < 16) {
      handleHouse(i);
      return;
    }
  }
  toast('نزدیک مزرعه، بندر، فرودگاه یا خانه‌ها دکمه تعامل را بزن.');
};

// بستن منو
document.getElementById('menu-close').onclick = closeMenu;

// ===== هندلر کلیک‌های داخل منو (event delegation) =====
document.getElementById('menu-body').addEventListener('click', (e) => {
  const btn = e.target.closest('[data-action]');
  if (!btn) return;
  const action = btn.dataset.action;
  const idx = parseInt(btn.dataset.idx, 10);

  if (action === 'quick-garage') { $('btn-garage').click();
  } else if (action === 'quick-home') { $('btn-home').click();
  } else if (action === 'quick-bag') { $('btn-bag').click();
  } else if (action === 'quick-jobs') { $('btn-jobs').click();
  } else if (action === 'quick-map') { $('btn-map').click();
  } else if (action === 'quick-sell') { closeMenu(); $('btn-sell').click();
  } else if (action === 'quick-settings') { $('btn-settings').click();
  } else if (action === 'supporters') {
    openMenu('حمایت‌کننده‌ها', supportersMenuHTML());
  } else if (action === 'multiplayer') {
    openMenu('🌐 چندنفره RP5', multiplayerMenuHTML());
  } else if (action === 'connect-multiplayer') {
    const url = document.getElementById('mp-server-url')?.value?.trim();
    const nick = document.getElementById('mp-player-name')?.value?.trim() || 'Player';
    if (!url || !/^https?:\/\//i.test(url)) { toast('آدرس سرور را با https:// وارد کن.'); return; }
    connectMultiplayer(url, nick);
  } else if (action === 'disconnect-multiplayer') {
    disconnectMultiplayer();
    openMenu('🌐 چندنفره RP5', multiplayerMenuHTML());
  } else if (action === 'library') {
    openMenu('کتابخانه مدل‌ها', assetLibraryHTML());
  } else if (action === 'open-garage') {
    openMenu('نمایشگاه خودروها', garageMenuHTML(state));
  } else if (action === 'open-homes') {
    openMenu('املاک و خانه‌ها', homeMenuHTML(state));
  } else if (action === 'open-skins') {
    openMenu('انتخاب اسکین', skinMenuHTML());
  } else if (action === 'skin') {
    applyPlayerSkin(player, idx); closeMenu();
  } else if (action === 'job') {
    const job = btn.dataset.job;
    state.activeJob = job;
    closeMenu();
    const labels = {farm:'کشاورز', port:'کارگر بندر', pilot:'خدمات فرودگاه'};
    toast(`شغل «${labels[job]}» فعال شد. به محل کار برو و تعامل را بزن.`);
  } else if (action === 'select-car') {
    selectCar(idx);
  } else if (action === 'house') {
    handleHouse(idx);
  } else if (action === 'house-bonus') {
    state.money += CONFIG.houseBonus;
    refreshStats();
    toast(`پاداش روزانه خانه دریافت شد: +${CONFIG.houseBonus} سکه`);
    closeMenu();
  } else if (action === 'outfit') {
    const o = OUTFITS[idx];
    setOutfit(player, o.hex);
    toast(`لباس به «${o.name}» تغییر کرد.`);
    closeMenu();
  }
});

// ===== انتخاب / خرید ماشین =====
function parkSelectedCarNearPlayer() {
  if (player.inCar) return;
  const offset = new THREE.Vector3(-4.2, 0, 0).applyAxisAngle(new THREE.Vector3(0,1,0), player.group.rotation.y);
  car.position.copy(player.group.position).add(offset);
  car.position.y = 0;
  car.rotation.y = player.group.rotation.y;
  state.carSpeed = 0;
}

function selectCar(idx) {
  const spec = CAR_CATALOG[idx];
  if (!spec) return;
  if (!state.ownedCars.includes(idx) && state.money >= spec.price) {
    state.money -= spec.price;
    state.ownedCars.push(idx);
  }
  if (state.ownedCars.includes(idx)) {
    state.currentCarIdx = idx;
    buildCarModel(car, spec);
    parkSelectedCarNearPlayer();
    closeMenu();
    toast(`خودرو ${spec.name} کنار تو آماده شد؛ نزدیکش شو و سوار شو.`);
    refreshStats();
  } else {
    toast('موجودی کافی نیست!');
  }
}

// ===== ورود / خرید خانه =====
function handleHouse(idx) {
  const h = HOUSE_CATALOG[idx];
  if (!h) return;
  if (h.owned) {
    h.doorTarget = Math.PI / 2;
    closeMenu();
    openMenu(`داخل ${h.name}`, houseInteriorHTML(h));
  } else if (state.money >= h.price) {
    state.money -= h.price;
    h.owned = true;
    h.doorTarget = Math.PI / 2;
    refreshStats();
    toast(`سند ${h.name} به نام تو ثبت شد! در خانه باز شد.`);
    closeMenu();
  } else {
    toast('پول کافی برای خرید این ملک نداری!');
  }
}

// ===== سیستم چندنفره آنلاین RP5 =====
let rp5Socket = null;
let rp5RemotePlayers = new Map();
let rp5LastSend = 0;
let rp5PlayerName = localStorage.getItem('rp5_player_name') || 'Player';
let rp5ServerUrl = localStorage.getItem('rp5_server_url') || '';

function multiplayerMenuHTML() {
  const connected = !!(rp5Socket && rp5Socket.connected);
  return `<p>برای دیدن بازیکنان دیگر، آدرس سرور عمومی RP5 را وارد کن.</p>
    <label style="display:block;margin:10px 0 5px">نام بازیکن</label>
    <input id="mp-player-name" maxlength="18" value="${escapeHTML(rp5PlayerName)}" placeholder="نام داخل بازی" style="width:100%;padding:11px;border-radius:8px;border:1px solid #456;background:#101620;color:white">
    <label style="display:block;margin:10px 0 5px">آدرس سرور آنلاین</label>
    <input id="mp-server-url" value="${escapeHTML(rp5ServerUrl)}" placeholder="https://your-rp5-server.example" dir="ltr" style="width:100%;padding:11px;border-radius:8px;border:1px solid #456;background:#101620;color:white">
    <p style="margin:10px 0;color:#8b9bad">وضعیت: <b style="color:${connected?'#5cf08c':'#ffb65c'}">${connected?'متصل':'قطع'}</b></p>
    <button class="opt-btn" data-action="connect-multiplayer"><span>🌐 اتصال به سرور</span><span>اتصال</span></button>
    ${connected?'<button class="opt-btn" data-action="disconnect-multiplayer"><span>⛔ قطع اتصال</span><span>خروج</span></button>':''}
    <small style="display:block;margin-top:10px;color:#8b9bad">نکته: این بخش به سرور Node.js عمومی نیاز دارد؛ اجرای فایل HTML به‌تنهایی اتصال آنلاین ایجاد نمی‌کند.</small>`;
}
function escapeHTML(value) {
  return String(value).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}
function makeRemoteNameSprite(name) {
  const canvas = document.createElement('canvas');
  canvas.width = 256; canvas.height = 64;
  const ctx = canvas.getContext('2d');
  ctx.fillStyle = 'rgba(5,12,20,.78)';
  ctx.beginPath(); ctx.roundRect(8, 8, 240, 48, 14); ctx.fill();
  ctx.fillStyle = '#ffffff'; ctx.font = 'bold 25px Tahoma,Arial';
  ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  ctx.fillText(String(name || 'Player').slice(0,18), 128, 33);
  const texture = new THREE.CanvasTexture(canvas);
  const sprite = new THREE.Sprite(new THREE.SpriteMaterial({map:texture, transparent:true, depthWrite:false}));
  sprite.scale.set(2.4, .6, 1); sprite.position.y = 2.25;
  return sprite;
}
function makeRemoteAvatar(data) {
  const group = new THREE.Group();
  const color = new THREE.Color(data.color || 0x36b8ff);
  const bodyMat = new THREE.MeshStandardMaterial({color, roughness:.75});
  const skinMat = new THREE.MeshStandardMaterial({color:0xe1b18d, roughness:.85});
  const torso = new THREE.Mesh(new THREE.CapsuleGeometry(.34,.72,4,8), bodyMat);
  torso.position.y = 1.02; torso.castShadow = true;
  const head = new THREE.Mesh(new THREE.SphereGeometry(.24,14,12), skinMat);
  head.position.y = 1.72; head.castShadow = true;
  const nameTag = makeRemoteNameSprite(data.name);
  group.add(torso, head, nameTag);
  group.position.set(data.x||0,data.y||0,data.z||0);
  group.rotation.y = data.rotation||0;
  scene.add(group);
  return {group, target:new THREE.Vector3(data.x||0,data.y||0,data.z||0), rotation:data.rotation||0, nameTag, driving:!!data.driving};
}
function addRemotePlayer(data) {
  if (!data || !data.id || (rp5Socket && data.id === rp5Socket.id)) return;
  if (!rp5RemotePlayers.has(data.id)) rp5RemotePlayers.set(data.id, makeRemoteAvatar(data));
  updateRemotePlayer(data);
}
function updateRemotePlayer(data) {
  if (!data || !data.id || (rp5Socket && data.id === rp5Socket.id)) return;
  let item = rp5RemotePlayers.get(data.id);
  if (!item) { addRemotePlayer(data); item = rp5RemotePlayers.get(data.id); }
  if (!item) return;
  item.target.set(Number(data.x)||0, Number(data.y)||0, Number(data.z)||0);
  item.rotation = Number(data.rotation)||0;
  item.driving = !!data.driving;
  if (item.nameTag && data.name) {
    const replacement = makeRemoteNameSprite(data.name);
    item.group.remove(item.nameTag);
    item.nameTag.material.map.dispose(); item.nameTag.material.dispose();
    item.nameTag = replacement; item.group.add(replacement);
  }
}
function removeRemotePlayer(id) {
  const item = rp5RemotePlayers.get(id); if (!item) return;
  scene.remove(item.group);
  item.group.traverse(o=>{ if(o.geometry)o.geometry.dispose(); if(o.material){if(Array.isArray(o.material))o.material.forEach(m=>m.dispose());else o.material.dispose();} });
  rp5RemotePlayers.delete(id);
}
function setOnlineStatus(connected, count) {
  const box = document.getElementById('rp5-online');
  const label = document.getElementById('rp5-online-label');
  const counter = document.getElementById('rp5-online-count');
  if (box) box.classList.toggle('connected', !!connected);
  if (label) label.textContent = connected ? 'متصل' : 'آفلاین';
  if (counter && Number.isFinite(count)) counter.textContent = String(count);
}
function connectMultiplayer(url, nick) {
  if (!window.io) { toast('کتابخانه اتصال آنلاین بارگذاری نشده؛ اینترنت را بررسی کن.'); return; }
  if (rp5Socket) { rp5Socket.disconnect(); rp5Socket = null; }
  for (const id of [...rp5RemotePlayers.keys()]) removeRemotePlayer(id);
  rp5ServerUrl = url.replace(/\/$/,'');
  rp5PlayerName = nick.slice(0,18) || 'Player';
  localStorage.setItem('rp5_server_url', rp5ServerUrl);
  localStorage.setItem('rp5_player_name', rp5PlayerName);
  toast('در حال اتصال به سرور RP5...');
  rp5Socket = window.io(rp5ServerUrl, {transports:['websocket','polling'], timeout:10000});
  rp5Socket.on('connect', ()=>{ setOnlineStatus(true, rp5RemotePlayers.size+1); toast('به دنیای آنلاین RP5 وصل شدی!'); closeMenu(); });
  rp5Socket.on('connect_error', err=>{ setOnlineStatus(false,0); toast('اتصال ناموفق؛ آدرس سرور و اینترنت را بررسی کن.'); console.warn('RP5 multiplayer:',err); });
  rp5Socket.on('disconnect', ()=>{ setOnlineStatus(false,0); for (const id of [...rp5RemotePlayers.keys()]) removeRemotePlayer(id); });
  rp5Socket.on('world:init', list=>{ list.forEach(addRemotePlayer); });
  rp5Socket.on('player:joined', addRemotePlayer);
  rp5Socket.on('player:update', updateRemotePlayer);
  rp5Socket.on('player:left', removeRemotePlayer);
  rp5Socket.on('online:count', count=>setOnlineStatus(true,count));
}
function disconnectMultiplayer() {
  if (rp5Socket) { rp5Socket.disconnect(); rp5Socket = null; }
  for (const id of [...rp5RemotePlayers.keys()]) removeRemotePlayer(id);
  setOnlineStatus(false,0); toast('از سرور آنلاین خارج شدی.');
}
function updateMultiplayer(now) {
  for (const item of rp5RemotePlayers.values()) {
    item.group.position.lerp(item.target, .28);
    item.group.rotation.y += (item.rotation-item.group.rotation.y)*.28;
  }
  if (!rp5Socket || !rp5Socket.connected || now-rp5LastSend<50) return;
  rp5LastSend = now;
  const position = player.inCar ? car.position : player.group.position;
  const rotation = player.inCar ? car.rotation.y : player.group.rotation.y;
  rp5Socket.emit('player:update', {
    name:rp5PlayerName, x:position.x, y:position.y, z:position.z,
    rotation, driving:!!player.inCar
  });
}

// ===== حلقه اصلی =====
const clock = new THREE.Clock();
const daySky = new THREE.Color(0x91c7e8);
const nightSky = new THREE.Color(0x101a2b);
const skyNow = new THREE.Color();

function animate() {
  requestAnimationFrame(animate);
  const dt = Math.min(clock.getDelta(), 0.05);
  const input = getInput();

  // NPCها
  updateNPCs(npcs, dt);

  // باد ملایم: تاب خوردن درخت‌ها، حرکت ابرها و نورهای محوطه.
  const t = clock.elapsedTime;
  // چرخه آرام روشنایی روز و شب؛ هر چرخه حدود ۵ دقیقه.
  const dayPhase = (t / 300) * Math.PI * 2;
  const daylight = 0.5 + 0.5 * Math.sin(dayPhase + Math.PI / 2);
  skyNow.lerpColors(nightSky, daySky, 0.22 + daylight * 0.78);
  scene.background.copy(skyNow);
  scene.fog.color.copy(skyNow);
  sun.intensity = 0.65 + daylight * 1.45;
  sun.position.set(-140 + Math.cos(dayPhase) * 95, 55 + daylight * 170, 120 + Math.sin(dayPhase) * 75);
  hemi.intensity = 0.55 + daylight * 0.85;

  for (const h of HOUSE_CATALOG) {
    if (h.doorPivot) {
      const pNow = player.inCar ? car.position : player.group.position;
      if (dist2D(pNow, h.x, h.z) > 22) h.doorTarget = 0;
      h.doorPivot.rotation.y += (h.doorTarget - h.doorPivot.rotation.y) * Math.min(1, dt * 5);
    }
    if (h.marker) {
      h.marker.rotation.y += dt * 0.8;
      h.marker.position.y = 11.5 + Math.sin(t * 1.6 + h.id) * 0.28;
    }
  }

  for (const tree of windTrees) {
    const phase = t * 0.72 + tree.userData.windPhase;
    tree.rotation.z = tree.userData.baseRotation + Math.sin(phase) * tree.userData.windStrength;
    tree.rotation.x = Math.cos(phase * 0.83) * tree.userData.windStrength * 0.45;
  }
  for (const cloud of windClouds) {
    cloud.position.x += cloud.userData.speed * dt;
    if (cloud.position.x > 520) cloud.position.x = -520;
  }
  for (const wave of waterWaves) {
    wave.position.z += wave.userData.speed * dt;
    wave.position.x += Math.sin(t*0.7 + wave.userData.phase) * 0.012;
    if (wave.position.z > 720) wave.position.z = -720;
  }
  for (const lamp of animatedLamps) lamp.material.emissiveIntensity = 0.22 + (Math.sin(t*2.2)+1)*0.12;

  // بازیکن یا ماشین
  if (player.inCar) {
    state.carSpeed = updateCar(car, input, dt, { currentIdx: state.currentCarIdx, speed: state.carSpeed });
    updateCarCamera(car, camera, dt);
  } else {
    updatePlayer(player, input, dt);
  }

  // تشخیص استان
  const p = player.inCar ? car.position : player.group.position;
  state.region = getRegion(p.x, p.z);
  $('region').textContent = state.region;

  // مینی‌مپ
  minimap.draw(p.x, p.z);

  // همگام‌سازی بازیکنان آنلاین
  updateMultiplayer(performance.now());

  renderer.render(scene, camera);
}

// ===== ابزار کمکی =====
function dist2D(pos, x, z) {
  const dx = pos.x - x;
  const dz = pos.z - z;
  return Math.sqrt(dx * dx + dz * dz);
}

// ===== ری‌سایز =====
addEventListener('resize', () => {
  camera.aspect = innerWidth / innerHeight;
  camera.updateProjectionMatrix();
  renderer.setPixelRatio(Math.min(devicePixelRatio || 1, 1.25));
  renderer.setSize(innerWidth, innerHeight);
});

// ===== شروع =====
refreshStats();
setOnlineStatus(false, 0);
toast('RP5 آماده است! امکانات بازی حفظ شده‌اند؛ برای اتصال چندنفره از تنظیمات وارد بخش آنلاین شو.');
animate();

</script>
</body>
</html>
