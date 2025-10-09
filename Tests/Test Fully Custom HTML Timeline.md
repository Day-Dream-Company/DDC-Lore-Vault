<div class="tl-root">
  <svg class="tl-wires" xmlns="http://www.w3.org/2000/svg"></svg>
  <div class="tl-scroll">
    <!-- Column Alpha -->
    <div class="tl-col">
      <div class="tl-event" id="ev-a1">
        <h4>Arrival</h4>
        <p>The expedition lands at Duskport.</p>
      </div>
      <div class="tl-event" id="ev-a2">
        <h4>Customs inspection</h4>
        <p>Routine check uncovers a sealed crate.</p>
      </div>
    </div>

    <!-- Column Beta -->
    <div class="tl-col">
      <div class="tl-event" id="ev-b1">
        <h4>Scout briefing</h4>
        <p>Route options: ridge path vs. marsh road.</p>
      </div>
      <div class="tl-event" id="ev-b2" data-after="#ev-c1">
        <h4>Marsh incident</h4>
        <p>Encounter near mile marker 12.</p>
      </div>
    </div>

    <!-- Column Gamma -->
    <div class="tl-col">
      <div class="tl-event" id="ev-c1">
        <h4>Town council hearing</h4>
        <p>Permit for night operations requested.</p>
      </div>
    </div>

    <!-- Column Delta (new) -->
    <div class="tl-col">
      <div class="tl-event" id="ev-d1">
        <h4>Warehouse lease</h4>
        <p>Secured a 3‑month lease near the east quay.</p>
      </div>
    </div>
  </div>
</div>

<!-- Declare connections -->
<script type="application/json" id="tl-links">
[
  { "from": "#ev-a1", "to": "#ev-b1" },
  { "from": "#ev-a2", "to": "#ev-c1" },
  { "from": "#ev-b1", "to": "#ev-d1" }
]
</script>

<style>
:root {
  --tl-col-width: 280px;
  --tl-col-gap: 24px;
  --tl-padding: 16px;
  --tl-wire-width: 2px;
  --tl-wire-alpha: 0.9;
}
.tl-root { position: relative; width: 100%; }
.tl-scroll {
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: var(--tl-col-width);
  gap: var(--tl-col-gap);
  padding: var(--tl-padding);
  overflow-x: auto;
}
.tl-event {
  border: 2px solid black;
  background: var(--background-primary);
  color: var(--text-normal);
  padding: 12px 14px;
  margin-block: 12px;
  border-radius: 6px;
}
.tl-event h4 { margin: 0 0 8px 0; }
.tl-wires {
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 100%;
  pointer-events: none;
}
.tl-wire-path {
  fill: none;
  stroke: black;
  stroke-width: var(--tl-wire-width);
  opacity: var(--tl-wire-alpha);
}
.tl-spacer { height: var(--tl-spacer-size,0px); }
</style>

<script>
function rect(el, root) {
  const r = el.getBoundingClientRect();
  const rr = root.getBoundingClientRect();
  return { x: r.left - rr.left, y: r.top - rr.top, w: r.width, h: r.height };
}
function drawWires(root) {
  const svg = root.querySelector('.tl-wires');
  const scroll = root.querySelector('.tl-scroll');
  svg.innerHTML = '';
  const links = JSON.parse(document.getElementById('tl-links').textContent);
  for (const link of links) {
    const fromEl = document.querySelector(link.from);
    const toEl = document.querySelector(link.to);
    if (!fromEl || !toEl) continue;
    const rf = rect(fromEl, scroll);
    const rt = rect(toEl, scroll);
    const x1 = rf.x + rf.w/2, y1 = rf.y + rf.h;
    const x2 = rt.x + rt.w/2, y2 = rt.y;
    const dx = Math.abs(x2 - x1);
    const dy = Math.abs(y2 - y1);
    const cx = dx * 0.3;
    const d = `M ${x1},${y1} C ${x1+cx},${y1+dy/2} ${x2-cx},${y2-dy/2} ${x2},${y2}`;
    const path = document.createElementNS('http://www.w3.org/2000/svg','path');
    path.setAttribute('d', d);
    path.setAttribute('class','tl-wire-path');
    svg.appendChild(path);
  }
  const sr = scroll.getBoundingClientRect();
  svg.setAttribute('viewBox', `0 0 ${sr.width} ${sr.height}`);
  svg.setAttribute('width', sr.width);
  svg.setAttribute('height', sr.height);
}
// Constraint handling: data-after
function applyConstraints(root) {
  const scroll = root.querySelector('.tl-scroll');
  const events = scroll.querySelectorAll('.tl-event');
  events.forEach(ev => {
    const afterSel = ev.getAttribute('data-after');
    if (afterSel) {
      const other = document.querySelector(afterSel);
      if (other && ev.offsetTop <= other.offsetTop) {
        const diff = (other.offsetTop - ev.offsetTop) + other.offsetHeight + 20;
        ev.style.marginTop = diff + 'px';
      }
    }
  });
}
function render() {
  const root = document.querySelector('.tl-root');
  if (!root) return;
  applyConstraints(root);
  drawWires(root);
}
document.addEventListener('DOMContentLoaded', () => {
  render();
  window.addEventListener('resize', render);
  document.querySelector('.tl-scroll').addEventListener('scroll', render);
});
</script>
