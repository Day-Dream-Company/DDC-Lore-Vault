<!-- Timeline container: paste into your Obsidian note (as HTML block) -->
<div class="tl-root">
  <!-- SVG layer for arrows -->
  <svg class="tl-wires" xmlns="http://www.w3.org/2000/svg"></svg>

  <!-- Scrolling columns -->
  <div class="tl-scroll">
    <!-- Column: fixed width; add/remove more as needed -->
    <div class="tl-col" data-col="alpha">
      <div class="tl-event" id="ev-a1" data-color="blue">
        <h4>Arrival</h4>
        <p>The expedition lands at Duskport.</p>
        <p><a href="#notes/duskport">Notes</a></p>
      </div>

      <div class="tl-event" id="ev-a2" data-color="gray">
        <h4>Customs inspection</h4>
        <p>Routine check uncovers a sealed crate.</p>
      </div>

      <div class="tl-event" id="ev-a3" data-color="green"
           data-after="#ev-b2">
        <h4>Warehouse lease</h4>
        <p>Secured a 3-month lease near the east quay.</p>
      </div>
    </div>

    <div class="tl-col" data-col="beta">
      <div class="tl-event" id="ev-b1" data-color="purple">
        <h4>Scout briefing</h4>
        <p>Route options: ridge path vs. marsh road.</p>
      </div>

      <div class="tl-event" id="ev-b2" data-color="red"
           data-before="#ev-a3">
        <h4>Marsh incident</h4>
        <p>Encounter near mile marker 12.</p>
      </div>

      <div class="tl-event" id="ev-b3" data-color="yellow">
        <h4>Supply delay</h4>
        <p>Cargo barge postponed due to low tide.</p>
      </div>
    </div>

    <div class="tl-col" data-col="gamma">
      <div class="tl-event" id="ev-c1" data-color="teal">
        <h4>Town council hearing</h4>
        <p>Permit for night operations requested.</p>
      </div>

      <div class="tl-event" id="ev-c2" data-color="orange"
           data-after="#ev-a2">
        <h4>Crate inspection</h4>
        <p>Seal broken, contents logged.</p>
      </div>
    </div>
  </div>
</div>

<!-- Wire definitions: arrows between events (source -> target) -->
<!-- Place inside the same note; JS will read and render these -->
<script type="application/json" id="tl-links">
[
  { "from": "#ev-a1", "to": "#ev-b1", "color": "#7f8c8d" },
  { "from": "#ev-a2", "to": "#ev-c2", "color": "#c0392b" },
  { "from": "#ev-b2", "to": "#ev-a3", "color": "#e67e22" },
  { "from": "#ev-b1", "to": "#ev-b3", "color": "#9b59b6" }
]
</script>

<!-- Include the CSS and JS: inline for Obsidian, or link external files in Quartz -->
<style>
/* timeline.css */
:root {
  --tl-col-width: 320px;       /* Change to define column width */
  --tl-col-gap: 24px;          /* Space between columns */
  --tl-padding: 16px;
  --tl-wire-width: 2.5px;
  --tl-wire-alpha: 0.85;
  --tl-wire-curvature: 0.18;   /* 0..1, increases bend */
}

.tl-root {
  position: relative;
  block-size: auto;
  inline-size: 100%;
  overflow: hidden;
}

.tl-scroll {
  position: relative;
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: var(--tl-col-width);
  gap: var(--tl-col-gap);
  padding-inline: var(--tl-padding);
  padding-block: var(--tl-padding);
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x proximity; /* optional */
}

.tl-col {
  scroll-snap-align: start;
}

.tl-event {
  border-radius: 8px;
  border: 1px solid rgba(0,0,0,0.1);
  background: #fff;
  padding: 12px 14px;
  margin-block: 12px;
  box-shadow: 0 1px 2px rgba(0,0,0,0.06);
  max-inline-size: calc(var(--tl-col-width) - 16px);
}

.tl-event h4 {
  margin: 0 0 8px 0;
  font-size: 1rem;
}

.tl-event[data-color="blue"]   { border-color: #3498db33; background: #3498db10; }
.tl-event[data-color="gray"]   { border-color: #7f8c8d33; background: #7f8c8d10; }
.tl-event[data-color="green"]  { border-color: #2ecc7133; background: #2ecc7110; }
.tl-event[data-color="red"]    { border-color: #e74c3c33; background: #e74c3c10; }
.tl-event[data-color="yellow"] { border-color: #f1c40f33; background: #f1c40f10; }
.tl-event[data-color="purple"] { border-color: #9b59b633; background: #9b59b610; }
.tl-event[data-color="teal"]   { border-color: #1abc9c33; background: #1abc9c10; }
.tl-event[data-color="orange"] { border-color: #e67e2233; background: #e67e2210; }

.tl-wires {
  position: absolute;
  inset: 0;
  pointer-events: none; /* let clicks go to events/links */
  overflow: visible;
}

.tl-wire-path {
  fill: none;
  stroke: #34495e;
  stroke-width: var(--tl-wire-width);
  opacity: var(--tl-wire-alpha);
}

.tl-wire-head {
  fill: currentColor;
  opacity: var(--tl-wire-alpha);
}

/* Spacer injected to satisfy cross-column constraints */
.tl-spacer {
  height: var(--tl-spacer-size, 0px);
  margin: 0;
  padding: 0;
  border: 0;
}
</style>

<script>
/* timeline.js */
// Utilities
const q = (sel, root = document) => root.querySelector(sel);
const qa = (sel, root = document) => Array.from(root.querySelectorAll(sel));

// Compute bounding rect relative to root
function rect(el, root) {
  const r = el.getBoundingClientRect();
  const rr = root.getBoundingClientRect();
  return {
    x: r.left - rr.left,
    y: r.top - rr.top,
    w: r.width,
    h: r.height
  };
}

// Parse links JSON
function parseLinks() {
  const raw = q('#tl-links');
  if (!raw) return [];
  try { return JSON.parse(raw.textContent.trim()); }
  catch { return []; }
}

// Draw wires as cubic Beziers with arrowheads
function drawWires(root) {
  const svg = q('.tl-wires', root);
  const scroll = q('.tl-scroll', root);
  if (!svg || !scroll) return;

  // Clear existing
  while (svg.firstChild) svg.removeChild(svg.firstChild);

  const links = parseLinks();

  const pad = 8; // extra bend padding
  const curvature = parseFloat(getComputedStyle(document.documentElement)
                      .getPropertyValue('--tl-wire-curvature')) || 0.18;

  for (const link of links) {
    const fromEl = q(link.from);
    const toEl   = q(link.to);
    if (!fromEl || !toEl) continue;

    const rf = rect(fromEl, scroll);
    const rt = rect(toEl, scroll);

    // Start at bottom center of source
    const x1 = rf.x + rf.w / 2;
    const y1 = rf.y + rf.h;
    // End at top center of target
    const x2 = rt.x + rt.w / 2;
    const y2 = rt.y;

    // Control points for smooth spline
    const dx = Math.abs(x2 - x1);
    const dy = Math.max(24, Math.abs(y2 - y1));
    const cx = Math.max(24, dx) * curvature + pad;

    const c1x = x1 + (x2 >= x1 ? cx : -cx);
    const c1y = y1 + dy * 0.25;
    const c2x = x2 - (x2 >= x1 ? cx : -cx);
    const c2y = y2 - dy * 0.25;

    // Path
    const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
    path.setAttribute('class', 'tl-wire-path');
    if (link.color) path.style.stroke = link.color;

    path.setAttribute('d',
      `M ${x1},${y1} C ${c1x},${c1y} ${c2x},${c2y} ${x2},${y2}`
    );
    svg.appendChild(path);

    // Arrowhead: small triangle oriented upward
    const head = document.createElementNS('http://www.w3.org/2000/svg', 'path');
    head.setAttribute('class', 'tl-wire-head');
    head.style.color = link.color || '#34495e';
    const size = 8;
    head.setAttribute('d',
      `M ${x2},${y2} l ${-size/2},${-size} l ${size},0 z`
    );
    svg.appendChild(head);
  }

  // Resize SVG canvas to scroll area
  const sr = scroll.getBoundingClientRect();
  svg.setAttribute('viewBox', `0 0 ${sr.width} ${sr.height}`);
  svg.setAttribute('width', sr.width);
  svg.setAttribute('height', sr.height);
}

// Ensure cross-column ordering constraints by injecting spacers
// Supported data attributes on .tl-event:
// - data-before="#selector"  => this event must be above target
// - data-after="#selector"   => this event must be below target
function applyConstraints(root, maxPasses = 8) {
  const scroll = q('.tl-scroll', root);
  if (!scroll) return;

  const events = qa('.tl-event', scroll);

  // Build constraint list
  const constraints = [];
  for (const ev of events) {
    const beforeSel = ev.getAttribute('data-before');
    const afterSel  = ev.getAttribute('data-after');
    if (beforeSel) constraints.push({ type: 'before', ev, other: q(beforeSel) });
    if (afterSel)  constraints.push({ type: 'after',  ev, other: q(afterSel)  });
  }

  // Helper: add spacer before a node in its column to push it down/up
  function addSpacerBefore(node, px) {
    const spacer = document.createElement('div');
    spacer.className = 'tl-spacer';
    spacer.style.setProperty('--tl-spacer-size', `${px}px`);
    node.parentNode.insertBefore(spacer, node);
  }

  // Iteratively satisfy constraints (basic, stable approach)
  for (let pass = 0; pass < maxPasses; pass++) {
    let changed = false;

    for (const c of constraints) {
      if (!c.other) continue;

      const rEv = rect(c.ev, scroll);
      const rOther = rect(c.other, scroll);

      if (c.type === 'before') {
        // ev must be above other: if not, push other down
        if (rEv.y >= rOther.y) {
          const diff = (rEv.y - rOther.y) + 20; // +20px buffer
          addSpacerBefore(c.other, diff);
          changed = true;
        }
      } else if (c.type === 'after') {
        // ev must be below other: if not, push ev down
        if (rEv.y <= rOther.y) {
          const diff = (rOther.y - rEv.y) + 20;
          addSpacerBefore(c.ev, diff);
          changed = true;
        }
      }
    }

    if (!changed) break;
    // Reflow happens automatically; proceed to next pass
  }
}

// Prevent overlaps within each column after constraints
function resolveColumnOverlaps(root) {
  const cols = qa('.tl-col', root);
  for (const col of cols) {
    const items = qa('.tl-event, .tl-spacer', col);
    let cursor = 0;
    for (const item of items) {
      const style = getComputedStyle(item);
      const mt = parseFloat(style.marginTop) || 0;
      const mb = parseFloat(style.marginBottom) || 0;
      if (item.classList.contains('tl-spacer')) {
        const h = parseFloat(style.getPropertyValue('--tl-spacer-size')) || item.offsetHeight;
        cursor += h + mt + mb;
        continue;
      }
      // Position naturally; if top < cursor, add extra margin to push down
      const top = item.offsetTop;
      if (top < cursor) {
        const extra = cursor - top;
        item.style.marginTop = `${mt + extra}px`;
      }
      cursor = (item.offsetTop + item.offsetHeight + mb);
    }
  }
}

// Pipeline: first draw naturally, then enforce constraints, fix overlaps, draw wires
function renderTimeline() {
  const root = q('.tl-root');
  if (!root) return;

  // Clear any previous dynamic margins/spacers
  qa('.tl-event', root).forEach(ev => ev.style.marginTop = '');
  qa('.tl-spacer', root).forEach(s => s.remove());

  // Apply constraints and overlaps
  applyConstraints(root);
  resolveColumnOverlaps(root);

  // Draw wires last (positions stabilized)
  drawWires(root);
}

// Debounce utility
function debounce(fn, ms = 100) {
  let t;
  return (...args) => {
    clearTimeout(t);
    t = setTimeout(() => fn(...args), ms);
  };
}

// Initialize
document.addEventListener('DOMContentLoaded', () => {
  renderTimeline();

  // Re-render on resize and horizontal scroll to keep wires aligned
  window.addEventListener('resize', debounce(renderTimeline, 100));
  const scroll = q('.tl-scroll');
  if (scroll) scroll.addEventListener('scroll', debounce(renderTimeline, 50));

  // Observe content changes (e.g., edits) and re-render
  const mo = new MutationObserver(debounce(renderTimeline, 50));
  mo.observe(q('.tl-root'), { subtree: true, childList: true, attributes: true, characterData: true });
});
</script>
