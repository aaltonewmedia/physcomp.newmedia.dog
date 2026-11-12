---
title: Proposal - BANANA CLIMBER GAME
date: 2026-11-12T10:55:00Z
authors:
  - Jin
image: /images/2026/jin/mwdbco9t-h4smt8ww.png
showBgImage: true
showComments: true
heroStyle: background
draft: false
---
[Klifur — Play the physics climbing puzzle online](https://www.klifur.games/)

![1794474233412.png](/images/2026/jin/mwdbmo45-hh8mf0lu.png)



![1794474270745.png](/images/2026/jin/mwdbnl1d-pzjtugdw.png)



[MPR121](https://www.adafruit.com/product/1982)  - 4 bananas 

[MPU6050](https://www.adafruit.com/product/4517) or MSA311 - 

**[Balance Board, Rocker Board：👇](https://www.bidfta.com/es/426816/item-detail/33289118 "Balance Board, Rocker Board Physical Therapy,Balance Boards for Adults ...")**

![image.png](/images/2026/jin/mwdc6j8q-l4qv45ua.png)



```
<!doctype html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>BANANA CLIMBER</title>
    <style>
        * {
            box-sizing: border-box
        }

        body {
            margin: 0;
            background: #eeeae0;
            color: #222;
            font-family: Arial, Helvetica, sans-serif
        }

        main {
            max-width: 850px;
            margin: auto;
            padding: 22px
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px
        }

        h1 {
            font-size: 22px;
            letter-spacing: -.04em;
            margin: 0
        }

        .sub {
            font-family: monospace;
            font-size: 12px
        }

        .stage {
            background: #f8f6ef;
            border: 2px solid #222;
            border-radius: 12px;
            overflow: hidden
        }

        svg {
            width: 100%;
            height: auto;
            display: block
        }

        .controls {
            padding: 18px 0;
            display: grid;
            gap: 15px
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px
        }

        button {
            border: 2px solid #222;
            border-radius: 8px;
            background: transparent;
            padding: 14px 5px;
            font-size: 14px;
            font-weight: 700;
            cursor: pointer
        }

        button.active {
            background: #f7c83d
        }

        button:hover {
            background: #f4df9b
        }

        .sliderrow {
            display: flex;
            align-items: center;
            gap: 15px
        }

        input[type=range] {
            flex: 1;
            accent-color: #222;
            height: 28px
        }

        #deg {
            font-family: monospace;
            font-size: 20px;
            min-width: 66px;
            text-align: right
        }

        .actions {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 10px
        }

        .actions button {
            padding: 10px 18px
        }

        .hint {
            font-family: monospace;
            font-size: 11px;
            color: #666
        }

        @media(max-width:550px) {
            main {
                padding: 12px
            }

            .buttons button {
                font-size: 12px;
                padding: 12px 2px
            }

            .hint {
                font-size: 10px
            }
        }
    </style>
</head>

<body>
    <main>
        <header>
            <h1>BANANA CLIMBER <span style="color:#c89b12">●</span></h1><span class="sub">INTERACTIVE PROTOTYPE /
                01</span>
        </header>
        <div class="stage"><svg id="scene" viewBox="0 0 760 510" xmlns="http://www.w3.org/2000/svg">
                <defs>
                    <pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
                        <path d="M40 0H0V40" fill="none" stroke="#dedbd2" stroke-width="1" />
                    </pattern>
                </defs>
                <rect width="760" height="510" fill="url(#grid)" /><text x="22" y="32" font-family="monospace"
                    font-size="13" fill="#777">ROUTE 01 / HOLD TO SNAP</text><text x="738" y="32" text-anchor="end"
                    font-family="monospace" font-size="13" fill="#777" id="count">0 / 0 CONNECTED</text>
                <g id="holds"></g>
                <g id="figure">
                    <g id="limbs"></g>
                    <rect x="354" y="223" width="52" height="100" rx="20" fill="#262626" />
                    <circle cx="380" cy="200" r="27" fill="#262626" />
                    <path d="M367 198h8m11 0h8" stroke="#f8f6ef" stroke-width="3" stroke-linecap="round" />
                </g><text x="22" y="486" font-family="monospace" font-size="12" fill="#777">SELECT LIMB → ROTATE →
                    SNAP</text>
            </svg></div>
        <section class="controls">
            <div class="buttons"><button data-limb="lh" class="active">🍌 LEFT HAND</button><button data-limb="rh">🍌
                    RIGHT HAND</button><button data-limb="lf">🍌 LEFT FOOT</button><button data-limb="rf">🍌 RIGHT
                    FOOT</button></div>
            <div class="sliderrow"><span class="sub">ROTATION</span><input id="rotation" type="range" min="-180"
                    max="180" value="0" step="1"><span id="deg">0°</span></div>
            <div class="actions"><span class="hint" id="status">LEFT HAND SELECTED</span><button id="reset">RESET
                    ↺</button></div>
        </section>
    </main>
    <script>
        const NS = 'http://www.w3.org/2000/svg'; const holds = [{ x: 247, y: 159, c: '#e98660' }, { x: 485, y: 160, c: '#63a5ba' }, { x: 254, y: 367, c: '#e5b845' }, { x: 502, y: 378, c: '#7da88d' }, { x: 179, y: 237, c: '#7da88d' }, { x: 575, y: 254, c: '#e98660' }, { x: 314, y: 105, c: '#63a5ba' }, { x: 455, y: 100, c: '#e5b845' }]; const defs = { lh: { x: 360, y: 241, len: 126, angle: -155 }, rh: { x: 400, y: 241, len: 126, angle: -25 }, lf: { x: 368, y: 311, len: 116, angle: 135 }, rf: { x: 392, y: 311, len: 116, angle: 45 } }; let selected = 'lh'; let state = {}; const $ = s => document.querySelector(s); const create = (tag, attrs, parent) => { let e = document.createElementNS(NS, tag); for (const [k, v] of Object.entries(attrs)) e.setAttribute(k, v); parent.appendChild(e); return e }; const holdLayer = $('#holds'), limbLayer = $('#limbs'); holds.forEach((h, i) => { const g = create('g', {}, holdLayer); create('path', { d: `M${h.x - 21} ${h.y - 9} Q${h.x - 9} ${h.y - 24} ${h.x + 14} ${h.y - 14} Q${h.x + 29} ${h.y + 1} ${h.x + 13} ${h.y + 16} Q${h.x - 10} ${h.y + 23} ${h.x - 23} ${h.y + 6}Z`, fill: h.c, stroke: '#222', 'stroke-width': 2 }, g); create('circle', { cx: h.x, cy: h.y, r: 3, fill: '#222', opacity: .45 }, g) });
        function init() { state = {}; for (let k in defs) state[k] = { angle: defs[k].angle, snap: null }; selected = 'lh'; sync(); render() }
        function point(k) { const d = defs[k], s = state[k], a = s.angle * Math.PI / 180; return { x: d.x + d.len * Math.cos(a), y: d.y + d.len * Math.sin(a) } }
        function render() { limbLayer.innerHTML = ''; let connected = 0; for (const k of ['lf', 'rf', 'lh', 'rh']) { const d = defs[k], s = state[k]; let p = point(k); if (s.snap !== null) { p = { x: holds[s.snap].x, y: holds[s.snap].y }; connected++ } if (k === selected) { create('circle', { cx: d.x, cy: d.y, r: d.len, fill: 'none', stroke: '#c89b12', 'stroke-dasharray': '5 9', 'stroke-width': 2, opacity: .8 }, limbLayer) } create('line', { x1: d.x, y1: d.y, x2: p.x, y2: p.y, stroke: k === selected ? '#e4ad2f' : '#262626', 'stroke-width': 15, 'stroke-linecap': 'round' }, limbLayer); create('circle', { cx: p.x, cy: p.y, r: 10, fill: s.snap !== null ? '#e4ad2f' : k === selected ? '#e4ad2f' : '#262626', stroke: '#262626', 'stroke-width': 2 }, limbLayer); if (s.snap !== null) create('circle', { cx: p.x, cy: p.y, r: 17, fill: 'none', stroke: '#e4ad2f', 'stroke-width': 3 }, limbLayer) } $('#count').textContent = `${connected} / 4 CONNECTED` }
        function sync() { document.querySelectorAll('[data-limb]').forEach(b => b.classList.toggle('active', b.dataset.limb === selected)); $('#rotation').value = state[selected].angle; $('#deg').textContent = state[selected].angle + '°'; $('#status').textContent = ({ lh: 'LEFT HAND', rh: 'RIGHT HAND', lf: 'LEFT FOOT', rf: 'RIGHT FOOT' }[selected]) + (state[selected].snap !== null ? ' · SNAPPED ✓' : ' · FREE') }
        function rotate(v) { let s = state[selected]; s.angle = Number(v); s.snap = null; let p = point(selected), best = -1, dist = Infinity; holds.forEach((h, i) => { let d = Math.hypot(p.x - h.x, p.y - h.y); if (d < dist) { dist = d; best = i } }); if (dist < 27) s.snap = best; sync(); render() }
        document.querySelectorAll('[data-limb]').forEach(b => b.onclick = () => { selected = b.dataset.limb; sync(); render() }); $('#rotation').addEventListener('input', e => rotate(e.target.value)); $('#reset').onclick = init; init();
    </script>
</body>

</html>
```

