<div class="tl-root">
  <svg class="tl-wires" xmlns="http://www.w3.org/2000/svg"></svg>
  <div class="tl-scroll">
    <div class="tl-col">
      <div class="tl-event" id="ev-a1" data-color="blue">
        <h4>Arrival</h4>
        <p>The expedition lands at Duskport.</p>
      </div>
      <div class="tl-event" id="ev-a2" data-color="green">
        <h4>Customs inspection</h4>
        <p>Routine check uncovers a sealed crate.</p>
      </div>
    </div>
    <div class="tl-col">
      <div class="tl-event" id="ev-b1" data-color="red">
        <h4>Scout briefing</h4>
        <p>Route options: ridge path vs. marsh road.</p>
      </div>
    </div>
  </div>
</div>

<script type="application/json" id="tl-links">
[
  { "from": "#ev-a1", "to": "#ev-b1", "color": "#7f8c8d" },
  { "from": "#ev-a2", "to": "#ev-b1", "color": "#c0392b" }
]
</script>

<style>
:root {
  --tl-col-width: 320px;
  --tl-col-gap: 24px;
  --tl-padding: 16px;
  --tl-wire-width: 2.5px;
  --tl-wire-alpha: 0.85;
  --tl-wire-curvature: 0.18;
}
.tl-root {
  position: relative;
  width: 100%;
}
.tl-scroll {
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: var(--tl-col-width);
  gap: var(--tl-col-gap);
  padding: var(--tl-padding);
  overflow-x: auto;
}
.tl-event {
  border-radius: 8px;
  border: 1px solid var(--background-modifier-border);
  background: var(--background-primary);
  color: var(--text-normal);
  padding: 12px 14px;
  margin-block: 12px;
  box-shadow: 0 1px 2px var(--background-modifier-box-shadow);
}
.tl-event h4 { margin: 0 0 8px 0; }
.tl-event[data-color="blue"]   { border-left: 4px solid #3498db; }
.tl-event[data-color="green"]  { border-left: 4px solid #2ecc71; }
.tl-event[data-color="red"]    { border-left: 4px solid #e74c3c; }
.tl-wires {
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 100%;
  pointer-events: none;
}
.tl-wire-path {
  fill: none;
  stroke: var(--text-muted);
  stroke-width: var(--tl-wire-width);
  opacity: var(--tl-wire-alpha);
}
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
    if (link.color) path.style.stroke = link.color;
    svg.appendChild(path);
  }
  // Resize SVG to cover scroll area
  const sr = scroll.getBoundingClientRect();
  svg.setAttribute('viewBox', `0 0 ${sr.width} ${sr.height}`);
  svg.setAttribute('width', sr.width);
  svg.setAttribute('height', sr.height);
}
function render() {
  const root = document.querySelector('.tl-root');
  if (!root) return;
  drawWires(root);
}
document.addEventListener('DOMContentLoaded', () => {
  render();
  window.addEventListener('resize', render);
  document.querySelector('.tl-scroll').addEventListener('scroll', render);
});
</script>
