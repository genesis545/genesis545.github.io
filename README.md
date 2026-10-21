# genesis545.github.io
<!doctype html><html><head><meta charset=utf8><meta name=viewport content="width=device-width,initial-scale=1,viewport-fit=cover"><style>:root{color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}html{scroll-padding-top:env(safe-area-inset-top,0px)}body{margin:0;padding:0;font:14px -apple-system,BlinkMacSystemFont,sans-serif;background:#fff;color:#000}img{max-width:100%}[hidden]:not([hidden=until-found i]){display:none!important}</style></head><body>
<title>Para Camila</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Jost:wght@400;500&family=Pinyon+Script&display=swap" rel="stylesheet">
<style>
/* Layout: one full-bleed WebGL scene (Saturn + orbiting gardenias) with a single
   message at a time centered low over a soft dark veil. Tap anywhere to advance. */
:root {
  --bg: #04050b;
  --turquoise: #3fe0d0;
  --red: #ff2d46;
  --cream: #fff3e3;
  --muted: #8d92a8;
  --font-display: 'Pinyon Script', 'Snell Roundhand', 'Apple Chancery', cursive;
  --font-ui: 'Jost', system-ui, -apple-system, 'Segoe UI', sans-serif;
  --tint: #3fe0d0;
  color-scheme: dark;
}
html, body { height: 100%; }
body {
  margin: 0;
  background-color: var(--bg);
  background-image:
    radial-gradient(60% 50% at 72% 28%, rgba(34, 74, 102, 0.38), transparent 70%),
    radial-gradient(50% 45% at 18% 82%, rgba(82, 26, 58, 0.28), transparent 70%);
  color: var(--cream);
  font-family: var(--font-ui);
  overflow: hidden;
  padding-left: max(16px, env(safe-area-inset-left, 0px));
  padding-right: max(16px, env(safe-area-inset-right, 0px));
  -webkit-tap-highlight-color: transparent;
  user-select: none;
  -webkit-user-select: none;
}
.stage {
  position: fixed;
  inset: 0;
  cursor: pointer;
  touch-action: manipulation;
  outline: none;
}
.stage:focus-visible { box-shadow: inset 0 0 0 2px var(--tint); }
#gl {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  display: block;
  opacity: 0;
  transition: opacity 2.2s ease;
}
#gl.ready { opacity: 1; }
.veil {
  position: absolute;
  left: 0; right: 0; bottom: 0;
  height: 52%;
  background: linear-gradient(to top, rgba(4, 5, 11, 0.82), rgba(4, 5, 11, 0));
  pointer-events: none;
}
.dots {
  position: absolute;
  top: calc(env(safe-area-inset-top, 0px) + 18px);
  left: 0; right: 0;
  display: flex;
  justify-content: center;
  gap: 10px;
  pointer-events: none;
}
.dot {
  width: 8px; height: 8px;
  border-radius: 50%;
  border: 1px solid rgba(255, 255, 255, 0.38);
  transition: background-color .6s ease, border-color .6s ease, transform .6s ease;
}
.dot.on { background: var(--tint); border-color: var(--tint); transform: scale(1.25); }
.msgs {
  position: absolute;
  left: max(16px, env(safe-area-inset-left, 0px));
  right: max(16px, env(safe-area-inset-right, 0px));
  bottom: calc(env(safe-area-inset-bottom, 0px) + 76px);
  height: 0;
  pointer-events: none;
}
.msg {
  position: absolute;
  left: 0; right: 0; bottom: 0;
  margin: 0 auto;
  text-align: center;
  font-family: var(--font-display);
  font-weight: 400;
  line-height: 1.12;
  text-wrap: balance;
  opacity: 0;
  transform: translateY(20px) scale(0.985);
  filter: blur(8px);
  transition: opacity .8s ease, transform 1s cubic-bezier(.2,.7,.2,1), filter .8s ease;
}
.msg.on {
  opacity: 1;
  transform: none;
  filter: none;
  transition-delay: .3s;
}
.msg.name {
  font-size: clamp(4.6rem, 17vw, 10.5rem);
  color: var(--turquoise);
  text-shadow: 0 0 28px rgba(63, 224, 208, 0.55), 0 0 70px rgba(63, 224, 208, 0.25), 0 2px 14px rgba(0, 0, 0, 0.7);
}
.msg.ask, .msg.end {
  font-size: clamp(2.5rem, 7.4vw, 5.2rem);
  max-width: 15em;
}
.msg.ask {
  color: var(--red);
  text-shadow: 0 0 26px rgba(255, 45, 70, 0.5), 0 0 64px rgba(255, 45, 70, 0.22), 0 2px 14px rgba(0, 0, 0, 0.7);
}
.msg.end {
  color: var(--cream);
  text-shadow: 0 0 26px rgba(255, 236, 214, 0.4), 0 2px 14px rgba(0, 0, 0, 0.7);
}
.hint {
  position: absolute;
  left: 0; right: 0;
  bottom: calc(env(safe-area-inset-bottom, 0px) + 22px);
  text-align: center;
  font-size: 0.72rem;
  font-weight: 500;
  letter-spacing: 0.26em;
  text-transform: uppercase;
  color: var(--muted);
  pointer-events: none;
  animation: breathe 3.2s ease-in-out infinite;
}
@keyframes breathe { 0%, 100% { opacity: .35; } 50% { opacity: .85; } }
.nogl-note {
  position: absolute;
  top: 40%;
  left: 0; right: 0;
  text-align: center;
  color: var(--muted);
  font-size: 0.9rem;
  padding: 0 24px;
}
@media (prefers-reduced-motion: reduce) {
  .msg, .msg.on { transform: none; filter: none; transition: opacity .6s ease; }
  .hint { animation: none; opacity: .6; }
  #gl { transition: none; }
}
</style>

<div class="stage" id="stage" role="button" tabindex="0" aria-label="Toca para ir a la siguiente pantalla">
  <canvas id="gl" aria-hidden="true"></canvas>
  <div class="veil"></div>
  <div class="dots" aria-hidden="true"><span class="dot"></span><span class="dot"></span><span class="dot"></span></div>
  <div class="msgs" aria-live="polite">
    <p class="msg name" id="m0">Camila</p>
    <p class="msg ask" id="m1">¿Sabías que te amo demasiado?</p>
    <p class="msg end" id="m2">Por eso hice esto para ti</p>
  </div>
  <div class="hint" id="hint">toca la pantalla</div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
(function () {
  'use strict';

  /* ---------- Pantallas ---------- */
  var tints = ['#3fe0d0', '#ff2d46', '#fff3e3'];
  var msgs = [].slice.call(document.querySelectorAll('.msg'));
  var dots = [].slice.call(document.querySelectorAll('.dot'));
  var hint = document.getElementById('hint');
  var stage = document.getElementById('stage');
  var idx = 0;

  function show(i) {
    idx = i;
    msgs.forEach(function (m, k) {
      m.classList.toggle('on', k === i);
      m.setAttribute('aria-hidden', k === i ? 'false' : 'true');
    });
    dots.forEach(function (d, k) { d.classList.toggle('on', k === i); });
    document.documentElement.style.setProperty('--tint', tints[i]);
    hint.textContent = i === 2 ? 'toca para volver a empezar' : 'toca la pantalla';
  }
  function next() { show((idx + 1) % 3); }

  stage.addEventListener('click', next);
  document.addEventListener('keydown', function (e) {
    if (e.key === 'Enter' || e.key === ' ' || e.key === 'ArrowRight') { e.preventDefault(); next(); }
    else if (e.key === 'ArrowLeft') { e.preventDefault(); show((idx + 2) % 3); }
  });
  show(0);

  /* ---------- Escena ---------- */
  try { initScene(); } catch (err) {
    var n = document.createElement('div');
    n.className = 'nogl-note';
    n.textContent = 'Tu navegador no pudo mostrar el planeta, pero el mensaje sigue siendo para ti.';
    stage.appendChild(n);
    if (window.console) console.error(err);
  }

  function initScene() {
    var THREE = window.THREE;
    var reduceMotion = window.matchMedia && matchMedia('(prefers-reduced-motion: reduce)').matches;

    /* ruido */
    function hash2(x, y) {
      var h = (Math.imul(x | 0, 374761393) + Math.imul(y | 0, 668265263)) | 0;
      h = Math.imul(h ^ (h >>> 13), 1274126177);
      return (((h ^ (h >>> 16)) >>> 0) / 4294967296);
    }
    function sm(t) { return t * t * (3 - 2 * t); }
    function vnoise(x, y, px) {
      var xi = Math.floor(x), yi = Math.floor(y), xf = x - xi, yf = y - yi;
      var x0 = ((xi % px) + px) % px, x1 = (x0 + 1) % px;
      var a = hash2(x0, yi), b = hash2(x1, yi), c = hash2(x0, yi + 1), d = hash2(x1, yi + 1);
      var u = sm(xf), v = sm(yf);
      return a + (b - a) * u + (c - a) * v + (a - b - c + d) * u * v;
    }
    function noise1(x) {
      var xi = Math.floor(x), f = x - xi;
      var a = hash2(xi, 77), b = hash2(xi + 1, 77);
      return a + (b - a) * sm(f);
    }
    function rng(seed) {
      return function () {
        seed = (seed + 0x6D2B79F5) | 0;
        var t = Math.imul(seed ^ (seed >>> 15), 1 | seed);
        t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
        return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
      };
    }
    function ss(a, b, x) { var t = Math.min(1, Math.max(0, (x - a) / (b - a))); return t * t * (3 - 2 * t); }

    /* renderer */
    var canvas = document.getElementById('gl');
    var renderer = new THREE.WebGLRenderer({ canvas: canvas, antialias: true, alpha: true, powerPreference: 'high-performance' });
    renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 2));
    renderer.outputEncoding = THREE.sRGBEncoding;
    renderer.toneMapping = THREE.ACESFilmicToneMapping;
    renderer.toneMappingExposure = 1.05;
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    renderer.setClearColor(0x000000, 0);
    var maxAniso = renderer.capabilities.getMaxAnisotropy();

    var scene = new THREE.Scene();
    var camera = new THREE.PerspectiveCamera(38, 1, 0.1, 400);

    function tex(canvasEl, srgb) {
      var t = new THREE.CanvasTexture(canvasEl);
      if (srgb !== false) t.encoding = THREE.sRGBEncoding;
      t.anisotropy = maxAniso;
      return t;
    }

    /* ---------- Luces ---------- */
    var sun = new THREE.DirectionalLight(0xfff1dc, 2.7);
    sun.position.set(7, 4, 6);
    sun.castShadow = true;
    sun.shadow.mapSize.set(2048, 2048);
    sun.shadow.camera.left = -4; sun.shadow.camera.right = 4;
    sun.shadow.camera.top = 4; sun.shadow.camera.bottom = -4;
    sun.shadow.camera.near = 1; sun.shadow.camera.far = 24;
    sun.shadow.bias = -0.0006;
    sun.shadow.normalBias = 0.02;
    scene.add(sun);
    scene.add(new THREE.HemisphereLight(0xbcd0ff, 0x3a2a1c, 0.55));
    var rim = new THREE.DirectionalLight(0x6fa8ff, 0.7);
    rim.position.set(-6, 2, -5);
    scene.add(rim);
    scene.add(new THREE.AmbientLight(0xffffff, 0.12));

    /* ---------- Estrellas ---------- */
    function starSprite() {
      var c = document.createElement('canvas'); c.width = c.height = 64;
      var g = c.getContext('2d');
      var gr = g.createRadialGradient(32, 32, 0, 32, 32, 32);
      gr.addColorStop(0, 'rgba(255,255,255,1)');
      gr.addColorStop(0.25, 'rgba(255,255,255,0.55)');
      gr.addColorStop(1, 'rgba(255,255,255,0)');
      g.fillStyle = gr; g.fillRect(0, 0, 64, 64);
      return tex(c, false);
    }
    var sprite = starSprite();
    var starsGroup = new THREE.Group();
    function makeStars(count, size, opacity, seed) {
      var R = rng(seed), pos = new Float32Array(count * 3), col = new Float32Array(count * 3);
      for (var i = 0; i < count; i++) {
        var u = R() * 2 - 1, th = R() * Math.PI * 2, rad = 70 + R() * 40, s = Math.sqrt(1 - u * u);
        pos[i * 3] = rad * s * Math.cos(th); pos[i * 3 + 1] = rad * u; pos[i * 3 + 2] = rad * s * Math.sin(th);
        var w = R(), b = 0.6 + R() * 0.4;
        col[i * 3] = b * (0.8 + 0.2 * w); col[i * 3 + 1] = b * (0.85 + 0.1 * w); col[i * 3 + 2] = b * (1 - 0.15 * w);
      }
      var g = new THREE.BufferGeometry();
      g.setAttribute('position', new THREE.BufferAttribute(pos, 3));
      g.setAttribute('color', new THREE.BufferAttribute(col, 3));
      var m = new THREE.PointsMaterial({ size: size, map: sprite, vertexColors: true, transparent: true, opacity: opacity, depthWrite: false, sizeAttenuation: false, blending: THREE.AdditiveBlending });
      starsGroup.add(new THREE.Points(g, m));
    }
    makeStars(2200, 2.4, 0.8, 11);
    makeStars(260, 4.6, 0.95, 23);
    scene.add(starsGroup);

    /* ---------- Saturno ---------- */
    function planetTexture() {
      var W = 1024, H = 512, c = document.createElement('canvas'); c.width = W; c.height = H;
      var g = c.getContext('2d'), im = g.createImageData(W, H), d = im.data;
      var S = [[238, 222, 180], [220, 192, 132], [200, 164, 104], [242, 229, 196], [188, 154, 100], [230, 206, 152], [208, 178, 120], [236, 216, 170]];
      for (var y = 0; y < H; y++) {
        var lat = y / H, pol = Math.pow(Math.abs(lat - 0.5) * 2, 3);
        for (var x = 0; x < W; x++) {
          var u = x / W;
          var warp = (vnoise(u * 6, lat * 16, 6) - 0.5) * 0.03 + (vnoise(u * 18, lat * 60, 18) - 0.5) * 0.008;
          var l = lat + warp;
          var b = 0.5 + 0.5 * Math.sin(l * 34.0 + 1.4 * Math.sin(l * 13.0));
          b = 0.5 * b + 0.5 * noise1(l * 30);
          b = Math.min(0.999, Math.max(0, b));
          var f = b * (S.length - 1), i = Math.floor(f), t = f - i, A = S[i], B = S[i + 1];
          var fine = vnoise(u * 80, lat * 260, 80);
          var k = 0.93 + fine * 0.14;
          var r = (A[0] + (B[0] - A[0]) * t) * k;
          var gg = (A[1] + (B[1] - A[1]) * t) * k;
          var bl = (A[2] + (B[2] - A[2]) * t) * k;
          r = r * (1 - pol * 0.3) + 118 * pol * 0.3;
          gg = gg * (1 - pol * 0.3) + 128 * pol * 0.3;
          bl = bl * (1 - pol * 0.3) + 140 * pol * 0.3;
          var o = (y * W + x) * 4;
          d[o] = r; d[o + 1] = gg; d[o + 2] = bl; d[o + 3] = 255;
        }
      }
      g.putImageData(im, 0, 0);
      return tex(c);
    }
    function ringTexture() {
      var W = 2048, H = 8, c = document.createElement('canvas'); c.width = W; c.height = H;
      var g = c.getContext('2d'), im = g.createImageData(W, H), d = im.data;
      for (var x = 0; x < W; x++) {
        var r = x / (W - 1);
        var den = 0.30 * ss(0.0, 0.03, r) * (1 - ss(0.25, 0.27, r))
                + 0.93 * ss(0.25, 0.29, r) * (1 - ss(0.63, 0.645, r))
                + 0.76 * ss(0.705, 0.72, r) * (1 - ss(0.925, 0.935, r));
        den *= 1 - 0.92 * Math.exp(-Math.pow((r - 0.876) / 0.005, 2));
        den += 0.5 * Math.exp(-Math.pow((r - 0.982) / 0.004, 2));
        var n1 = noise1(r * 160), n2 = noise1(r * 520 + 9);
        den *= 0.72 + 0.2 * n1 + 0.16 * n2;
        den = Math.min(1, Math.max(0, den));
        var base = r < 0.27 ? [168, 148, 120] : (r < 0.67 ? [238, 220, 182] : [218, 200, 164]);
        var k = 0.84 + 0.22 * n2 + 0.1 * n1;
        if (r > 0.27 && r < 0.42) k *= 0.82 + 0.18 * ss(0.27, 0.42, r);
        for (var y = 0; y < H; y++) {
          var o = (y * W + x) * 4;
          d[o] = base[0] * k; d[o + 1] = base[1] * k; d[o + 2] = base[2] * k; d[o + 3] = den * 255;
        }
      }
      g.putImageData(im, 0, 0);
      return tex(c);
    }

    var sys = new THREE.Group();
    sys.rotation.set(0.38, 0, -0.42);
    scene.add(sys);

    var planet = new THREE.Mesh(
      new THREE.SphereGeometry(1, 96, 64),
      new THREE.MeshStandardMaterial({ map: planetTexture(), roughness: 0.96, metalness: 0 })
    );
    planet.scale.set(1, 0.9, 1);
    planet.castShadow = true;
    sys.add(planet);

    var atmo = new THREE.Mesh(
      new THREE.SphereGeometry(1, 64, 48),
      new THREE.ShaderMaterial({
        vertexShader: 'varying vec3 vN; void main(){ vN = normalize(normalMatrix * normal); gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0); }',
        fragmentShader: 'varying vec3 vN; void main(){ float i = pow(max(0.0, 0.66 - dot(vN, vec3(0.0, 0.0, 1.0))), 3.0); gl_FragColor = vec4(1.0, 0.86, 0.58, 1.0) * i * 1.5; }',
        side: THREE.BackSide, blending: THREE.AdditiveBlending, transparent: true, depthWrite: false
      })
    );
    atmo.scale.set(1.07, 0.963, 1.07);
    sys.add(atmo);

    var ringGeo = new THREE.RingGeometry(1.24, 2.35, 256, 1);
    var rp = ringGeo.attributes.position, ru = ringGeo.attributes.uv;
    for (var i = 0; i < rp.count; i++) {
      var rr = Math.hypot(rp.getX(i), rp.getY(i));
      ru.setXY(i, (rr - 1.24) / (2.35 - 1.24), 0.5);
    }
    ringGeo.rotateX(-Math.PI / 2);
    var ring = new THREE.Mesh(ringGeo, new THREE.MeshStandardMaterial({
      map: ringTexture(), transparent: true, side: THREE.DoubleSide, roughness: 0.9, metalness: 0, depthWrite: false
    }));
    ring.receiveShadow = true;
    sys.add(ring);

    /* ---------- Gardenias ---------- */
    function petalTexture() {
      var c = document.createElement('canvas'); c.width = c.height = 256;
      var g = c.getContext('2d');
      var gr = g.createLinearGradient(0, 256, 0, 0);
      gr.addColorStop(0, '#ecdca8'); gr.addColorStop(0.22, '#f7efd6'); gr.addColorStop(0.5, '#fcf8ee'); gr.addColorStop(1, '#fffdf8');
      g.fillStyle = gr; g.fillRect(0, 0, 256, 256);
      var R = rng(5), i;
      for (i = 0; i < 260; i++) {
        g.fillStyle = 'rgba(205,190,140,' + (0.02 + R() * 0.04) + ')';
        g.beginPath(); g.arc(R() * 256, R() * 256, 1 + R() * 5, 0, 7); g.fill();
      }
      g.lineCap = 'round';
      for (i = 0; i < 13; i++) {
        var a = (i / 12 - 0.5) * 1.5, tx = 128 + Math.sin(a) * 120, ty = 8 + (1 - Math.cos(a)) * 40;
        g.strokeStyle = 'rgba(176,160,108,' + (i % 2 ? 0.1 : 0.17) + ')';
        g.lineWidth = i % 2 ? 0.8 : 1.5;
        g.beginPath(); g.moveTo(128, 262);
        g.bezierCurveTo(128 + Math.sin(a) * 30, 190, 128 + Math.sin(a) * 90, 110, tx, ty);
        g.stroke();
      }
      var ed = g.createRadialGradient(128, 150, 70, 128, 150, 150);
      ed.addColorStop(0, 'rgba(230,210,160,0)'); ed.addColorStop(1, 'rgba(226,204,150,0.22)');
      g.fillStyle = ed; g.fillRect(0, 0, 256, 256);
      return tex(c);
    }
    function leafTexture() {
      var c = document.createElement('canvas'); c.width = 128; c.height = 256;
      var g = c.getContext('2d');
      var gr = g.createLinearGradient(0, 256, 0, 0);
      gr.addColorStop(0, '#1d4a2a'); gr.addColorStop(1, '#2f6e3b');
      g.fillStyle = gr; g.fillRect(0, 0, 128, 256);
      g.lineCap = 'round';
      g.strokeStyle = 'rgba(160,205,130,0.55)'; g.lineWidth = 3;
      g.beginPath(); g.moveTo(64, 256); g.lineTo(64, 0); g.stroke();
      g.strokeStyle = 'rgba(150,195,120,0.28)'; g.lineWidth = 1.2;
      for (var i = 0; i < 9; i++) {
        var y = 240 - i * 25;
        g.beginPath(); g.moveTo(64, y); g.quadraticCurveTo(90, y - 18, 124, y - 46); g.stroke();
        g.beginPath(); g.moveTo(64, y); g.quadraticCurveTo(38, y - 18, 4, y - 46); g.stroke();
      }
      return tex(c);
    }

    var petalMat = new THREE.MeshStandardMaterial({
      map: petalTexture(), vertexColors: true, roughness: 0.62, metalness: 0,
      side: THREE.DoubleSide, emissive: 0x3a352a, emissiveIntensity: 0.38
    });
    var leafMat = new THREE.MeshStandardMaterial({ map: leafTexture(), roughness: 0.32, metalness: 0, side: THREE.DoubleSide, emissive: 0x06160a, emissiveIntensity: 0.5 });
    var sepalMat = new THREE.MeshStandardMaterial({ color: 0x78a24e, roughness: 0.55, side: THREE.DoubleSide });
    var stemMat = new THREE.MeshStandardMaterial({ color: 0x4f7b3a, roughness: 0.6 });

    function petalGeometry(p) {
      var SU = 12, ST = 14;
      var geo = new THREE.PlaneGeometry(1, 1, SU, ST);
      var uv = geo.attributes.uv, pos = geo.attributes.position, n = pos.count;
      var col = new Float32Array(n * 3);
      var cl = [], r = 0, y = 0, k, dt = 1 / ST;
      for (k = 0; k <= ST; k++) {
        var tt = k / ST, phi = Math.max(-0.25, p.tilt + p.bend * tt);
        cl.push({ r: r, y: y, phi: phi });
        r += Math.sin(phi) * p.L * dt; y += Math.cos(phi) * p.L * dt;
      }
      for (var i = 0; i < n; i++) {
        var u = uv.getX(i) * 2 - 1, t = uv.getY(i);
        var c = cl[Math.round(t * ST)];
        var a = t < 0.62 ? (t - 0.62) / 0.62 : (t - 0.62) / 0.38;
        var prof = Math.max(0.08, Math.pow(Math.max(0, 1 - a * a), 0.55));
        var w = u * p.W * prof;
        var off = p.cup * p.L * u * u * prof + 0.035 * p.L * Math.sin(u * 5.5 + t * 7 + p.seed * 6.283) * Math.abs(u) * t;
        pos.setXYZ(i, c.r - Math.cos(c.phi) * off, c.y + p.y0 + Math.sin(c.phi) * off, w);
        var ao = (0.55 + 0.45 * ss(0, 0.55, t)) * p.shade;
        col[i * 3] = ao; col[i * 3 + 1] = ao * 0.985; col[i * 3 + 2] = ao * 0.94;
      }
      geo.setAttribute('color', new THREE.BufferAttribute(col, 3));
      geo.computeVertexNormals();
      geo.rotateY(p.az);
      return geo;
    }
    function leafGeometry(hw) {
      var g = new THREE.PlaneGeometry(1, 1, 6, 14), uv = g.attributes.uv, p = g.attributes.position;
      for (var i = 0; i < p.count; i++) {
        var u = uv.getX(i) * 2 - 1, t = uv.getY(i);
        var prof = Math.pow(Math.max(0, Math.sin(Math.PI * Math.pow(t, 0.75))), 0.85);
        p.setXYZ(i, 0.22 * t * t - 0.35 * Math.abs(u) * hw * prof, t, u * hw * prof);
      }
      g.computeVertexNormals();
      return g;
    }
    function merge(geos) {
      var pc = 0, ic = 0, g;
      geos.forEach(function (q) { pc += q.attributes.position.count; ic += q.index.count; });
      var P = new Float32Array(pc * 3), N = new Float32Array(pc * 3), U = new Float32Array(pc * 2), C = new Float32Array(pc * 3);
      var I = pc < 65535 ? new Uint16Array(ic) : new Uint32Array(ic), po = 0, io = 0;
      geos.forEach(function (q) {
        var cnt = q.attributes.position.count;
        P.set(q.attributes.position.array, po * 3); N.set(q.attributes.normal.array, po * 3);
        U.set(q.attributes.uv.array, po * 2); C.set(q.attributes.color.array, po * 3);
        for (var j = 0; j < q.index.count; j++) I[io + j] = q.index.array[j] + po;
        po += cnt; io += q.index.count;
      });
      g = new THREE.BufferGeometry();
      g.setAttribute('position', new THREE.BufferAttribute(P, 3));
      g.setAttribute('normal', new THREE.BufferAttribute(N, 3));
      g.setAttribute('uv', new THREE.BufferAttribute(U, 2));
      g.setAttribute('color', new THREE.BufferAttribute(C, 3));
      g.setIndex(new THREE.BufferAttribute(I, 1));
      return g;
    }

    var leafGeo = leafGeometry(0.26), sepalGeo = leafGeometry(0.09);
    var recGeo = new THREE.CylinderGeometry(0.11, 0.05, 0.34, 14);

    function buildGardenia(seed) {
      var R = rng(seed), geos = [], N = 30, GA = 2.399963, i;
      for (i = 0; i < N; i++) {
        var f = i / (N - 1), fp = Math.pow(f, 0.85);
        var p = {
          L: (1 - 0.58 * fp) * (0.94 + R() * 0.12),
          tilt: 1.32 - 1.22 * fp + (R() - 0.5) * 0.12,
          bend: 0.3 - 0.6 * f + (R() - 0.5) * 0.1,
          cup: 0.16 + 0.2 * f,
          az: i * GA + (R() - 0.5) * 0.1,
          y0: 0.02 + f * 0.2,
          shade: 1 - 0.2 * f,
          seed: R()
        };
        p.W = p.L * (0.6 - 0.12 * f);
        geos.push(petalGeometry(p));
      }
      var group = new THREE.Group();
      group.add(new THREE.Mesh(merge(geos), petalMat));
      var rec = new THREE.Mesh(recGeo, stemMat); rec.position.y = -0.15; group.add(rec);
      var s, h, m;
      for (s = 0; s < 6; s++) {
        h = new THREE.Group(); h.rotation.y = s / 6 * Math.PI * 2 + 0.3;
        m = new THREE.Mesh(sepalGeo, sepalMat); m.rotation.z = -1.25; m.position.set(0.05, -0.04, 0); m.scale.setScalar(0.6);
        h.add(m); group.add(h);
      }
      for (s = 0; s < 5; s++) {
        h = new THREE.Group(); h.rotation.y = s / 5 * Math.PI * 2 + 0.5 + R() * 0.3;
        m = new THREE.Mesh(leafGeo, leafMat); m.rotation.z = -(Math.PI - 1.35); m.position.set(0.05, -0.22, 0); m.scale.setScalar(1.0 + R() * 0.12);
        h.add(m); group.add(h);
      }
      return group;
    }
    var variants = [buildGardenia(101), buildGardenia(202), buildGardenia(303)];

    var orbit = [], COUNT = 13, RO = rng(77);
    for (var fi = 0; fi < COUNT; fi++) {
      var rad = 3.0 + (fi % 4) * 0.35 + RO() * 0.2;
      var pivot = new THREE.Group();
      pivot.rotation.x = (RO() - 0.5) * 0.22; pivot.rotation.z = (RO() - 0.5) * 0.22;
      sys.add(pivot);
      var holder = new THREE.Group();
      holder.position.set(rad, 0, 0);
      holder.rotation.set((RO() - 0.5) * 0.7, 0, -Math.PI / 2 + 0.45 + (RO() - 0.5) * 0.35);
      holder.scale.setScalar(0.21 + RO() * 0.08);
      var inner = variants[fi % 3].clone();
      inner.rotation.y = RO() * 6.28;
      holder.add(inner); pivot.add(holder);
      orbit.push({
        pivot: pivot, holder: holder, inner: inner, a0: fi / COUNT * Math.PI * 2 + RO() * 0.3,
        w: 0.22 * Math.pow(3.4 / rad, 1.5), spin: (RO() > 0.5 ? 1 : -1) * (0.15 + RO() * 0.25), ph: RO() * 6.28, baseY: 0
      });
    }

    /* ---------- Cámara por pantalla ---------- */
    var views = [
      { pos: new THREE.Vector3(0, 1.6, 10.5), look: new THREE.Vector3(0, 0, 0), speed: 1.0 },
      { pos: new THREE.Vector3(-3.0, 1.0, 7.2), look: new THREE.Vector3(0.7, 0, 0), speed: 0.8 },
      { pos: new THREE.Vector3(4.7, 0.9, 5.3), look: new THREE.Vector3(1.0, 0, 1.6), speed: 1.2 }
    ];
    var zf = 1, mx = 0, my = 0, smx = 0, smy = 0;
    function resize() {
      var w = window.innerWidth, h = window.innerHeight;
      renderer.setSize(w, h, false);
      camera.aspect = w / h; camera.updateProjectionMatrix();
      zf = Math.max(1, Math.min(2.1, 0.85 / camera.aspect));
    }
    window.addEventListener('resize', resize);
    window.addEventListener('pointermove', function (e) {
      mx = (e.clientX / window.innerWidth) * 2 - 1; my = (e.clientY / window.innerHeight) * 2 - 1;
    });
    resize();

    camera.position.copy(views[0].pos).multiplyScalar(zf * 1.8);
    var look = views[0].look.clone();
    var speedMul = 1, clock = new THREE.Clock(), time = 0, first = true;
    var tmpPos = new THREE.Vector3();

    function frame() {
      var dt = Math.min(clock.getDelta(), 0.05);
      time += dt;
      var v = views[idx];
      speedMul += (v.speed - speedMul) * (1 - Math.exp(-dt * 1.5));
      var damp = 1 - Math.exp(-dt * (reduceMotion ? 6 : 1.5));
      smx += (mx - smx) * (1 - Math.exp(-dt * 2)); smy += (my - smy) * (1 - Math.exp(-dt * 2));

      tmpPos.copy(v.pos).multiplyScalar(zf);
      if (!reduceMotion) {
        tmpPos.x += smx * 0.7 + Math.sin(time * 0.17) * 0.35;
        tmpPos.y += -smy * 0.45 + Math.sin(time * 0.23) * 0.15;
      }
      camera.position.lerp(tmpPos, damp);
      look.lerp(v.look, damp);
      camera.lookAt(look);

      if (!reduceMotion) {
        planet.rotation.y += dt * 0.03;
        starsGroup.rotation.y += dt * 0.004;
        for (var i = 0; i < orbit.length; i++) {
          var o = orbit[i];
          o.pivot.rotation.y = o.a0 + time * o.w * speedMul;
          o.inner.rotation.y += dt * o.spin;
          o.holder.position.y = Math.sin(time * 0.8 + o.ph) * 0.08;
        }
      } else {
        for (var j = 0; j < orbit.length; j++) orbit[j].pivot.rotation.y = orbit[j].a0;
      }

      renderer.render(scene, camera);
      if (first) { first = false; canvas.classList.add('ready'); }
      requestAnimationFrame(frame);
    }
    requestAnimationFrame(frame);
  }
})();
</script>

</body></html>
