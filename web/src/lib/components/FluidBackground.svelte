<script lang="js">
  import { browser } from '$app/environment';
  import { onMount, onDestroy } from 'svelte';

  let {PRESSURE_SPREAD, VISCOSITY, PEN_RADIUS } = $props()

  let canvas;
  let ctx;

  let width = 0;
  let height = 0;

  // =========================
  // 🔧 TWEAK THESE SETTINGS
  // =========================
      // 0.94 → 0.999 (higher = smoother/slower)
   // 0.05 → 0.6 (higher = more explosive)        
   // // size of interaction
  let MAX_SPEED = 15;           // intensity of color response
  let CELL_SIZE = 10;           // grid resolution (smaller = more detail, heavier)
	const FLOW_COLOR = { r: 255, g: 255, b: 255 }; // <-- change this { r: 41, g: 62, b: 92 }
	
  // =========================

  const BG = { r: 27, g: 38, b: 54 };
  const FADE_CELLS = 4;

  let COLS, ROWS;
  let cells = [];

  const mouse = { x: -999, y: -999, px: -999, py: -999 };

  function resize() {
    width = canvas.clientWidth;
    height = canvas.clientHeight;

    canvas.width = width;
    canvas.height = height;

    COLS = Math.ceil(width / CELL_SIZE);
    ROWS = Math.ceil(height / CELL_SIZE);

    buildGrid();
    initImageData();
  }

  function buildGrid() {
    cells = [];
    for (let i = 0; i < COLS * ROWS; i++) {
      cells.push({ xv: 0, yv: 0, pressure: 0 });
    }

    for (let row = 0; row < ROWS; row++) {
      for (let col = 0; col < COLS; col++) {
        const c = cell(col, row);
        c.up = row > 0 ? cell(col, row - 1) : null;
        c.down = row < ROWS - 1 ? cell(col, row + 1) : null;
        c.left = col > 0 ? cell(col - 1, row) : null;
        c.right = col < COLS - 1 ? cell(col + 1, row) : null;
        c.upLeft = row > 0 && col > 0 ? cell(col - 1, row - 1) : null;
        c.upRight = row > 0 && col < COLS - 1 ? cell(col + 1, row - 1) : null;
        c.downLeft = row < ROWS - 1 && col > 0 ? cell(col - 1, row + 1) : null;
        c.downRight = row < ROWS - 1 && col < COLS - 1 ? cell(col + 1, row + 1) : null;
      }
    }
  }

  function cell(col, row) {
    return cells[col + row * COLS];
  }

  function injectVelocity(px, py, vx, vy, radius) {
    const c0 = Math.floor(px / CELL_SIZE);
    const r0 = Math.floor(py / CELL_SIZE);
    const cr = Math.ceil(radius / CELL_SIZE) + 1;

    for (let dr = -cr; dr <= cr; dr++) {
      for (let dc = -cr; dc <= cr; dc++) {
        const col = c0 + dc;
        const row = r0 + dr;
        if (col < 0 || col >= COLS || row < 0 || row >= ROWS) continue;

        const dx = (col + 0.5) * CELL_SIZE - px;
        const dy = (row + 0.5) * CELL_SIZE - py;
        const dist = Math.sqrt(dx * dx + dy * dy);

        if (dist < radius) {
          const power = (radius - dist) / radius;
          const c = cell(col, row);
          c.xv += vx * power;
          c.yv += vy * power;
        }
      }
    }
  }

  const gxv = c => (c ? c.xv : 0);
  const gyv = c => (c ? c.yv : 0);
  const gpr = c => (c ? c.pressure : 0);

  function updatePressure(c) {
    const px =
      gxv(c.upLeft) * 0.5 +
      gxv(c.left) +
      gxv(c.downLeft) * 0.5 -
      gxv(c.upRight) * 0.5 -
      gxv(c.right) -
      gxv(c.downRight) * 0.5;

    const py =
      gyv(c.upLeft) * 0.5 +
      gyv(c.up) +
      gyv(c.upRight) * 0.5 -
      gyv(c.downLeft) * 0.5 -
      gyv(c.down) -
      gyv(c.downRight) * 0.5;

    c.pressure = (px + py) * 0.25;
  }

  function updateVelocity(c, col, row) {
    c.xv +=
      (gpr(c.left) - gpr(c.right)) * PRESSURE_SPREAD;

    c.yv +=
      (gpr(c.up) - gpr(c.down)) * PRESSURE_SPREAD;

    c.xv *= VISCOSITY;
    c.yv *= VISCOSITY;

    
  }

  let imgData;
  let buf32;

  function initImageData() {
    imgData = ctx.createImageData(width, height);
    buf32 = new Uint32Array(imgData.data.buffer);
  }

  function rgba(r, g, b, a) {
    return (a << 24) | (b << 16) | (g << 8) | r;
  }

  const BG32 = rgba(BG.r, BG.g, BG.b, 255);

  let animationFrame;

  function draw() {
    const mvx = mouse.x - mouse.px;
    const mvy = mouse.y - mouse.py;

    if (Math.sqrt(mvx * mvx + mvy * mvy) > 0.5) {
      injectVelocity(mouse.x, mouse.y, mvx * 0.9, mvy * 0.9, PEN_RADIUS);
    }

    mouse.px = mouse.x;
    mouse.py = mouse.y;

    for (let i = 0; i < cells.length; i++) updatePressure(cells[i]);

    buf32.fill(BG32);

    for (let row = 0; row < ROWS; row++) {
      for (let col = 0; col < COLS; col++) {
        const c = cell(col, row);
        const speed = Math.sqrt(c.xv * c.xv + c.yv * c.yv);
        if (speed < 0.05) continue;

        const t = Math.sqrt(Math.min(speed, MAX_SPEED) / MAX_SPEED);

        const r = Math.round(BG.r + (FLOW_COLOR.r - BG.r) * t);
				const g = Math.round(BG.g + (FLOW_COLOR.g - BG.g) * t);
				const b = Math.round(BG.b + (FLOW_COLOR.b - BG.b) * t);

        const px = rgba(r, g, b, 255);

        const x0 = col * CELL_SIZE;
        const y0 = row * CELL_SIZE;

for (let dy = 1; dy < CELL_SIZE; dy++) {
  const py = y0 + dy;
  if (py < 0 || py >= height) continue;

  const rowBase = py * width;

  for (let dx = 1; dx < CELL_SIZE; dx++) {
    const pxPos = x0 + dx;
    if (pxPos < 0 || pxPos >= width) continue;

    buf32[rowBase + pxPos] = px;
  }
}
      }
    }

    ctx.putImageData(imgData, 0, 0);

    for (let row = 0; row < ROWS; row++) {
      for (let col = 0; col < COLS; col++) {
        updateVelocity(cell(col, row), col, row);
      }
    }

    animationFrame = requestAnimationFrame(draw);
  }

function handleMouseMove(e) {
  const rect = canvas.getBoundingClientRect();

  mouse.px = mouse.x;
  mouse.py = mouse.y;

  mouse.x = e.clientX - rect.left;
  mouse.y = e.clientY - rect.top;
}

  onMount(() => {
    if (!browser) return;
    ctx = canvas.getContext('2d');

    resize();
    window.addEventListener('resize', resize);
    window.addEventListener('mousemove', handleMouseMove);

    draw();
  });

  onDestroy(() => {
    if (!browser) return;
    cancelAnimationFrame(animationFrame);
    window.removeEventListener('resize', resize);
    window.removeEventListener('mousemove', handleMouseMove);
  });
</script>

<style>
  canvas {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    display: block;
  }

  .wrapper {
    position: absolute;
    inset: 0;
    overflow: hidden;
    z-index: -1; /* makes it a background */
  }
</style>

<div class="wrapper">
  <canvas bind:this={canvas}></canvas>
</div>