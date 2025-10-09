<h1>Timeline Demo</h1>

<div class="tl-root">
  <svg class="tl-wires" xmlns="http://www.w3.org/2000/svg"></svg>
  <div class="tl-scroll">
    <div class="tl-col">
      <div class="tl-event" id="ev-a1"><h4>Arrival</h4><p>Lands at Duskport.</p></div>
      <div class="tl-event" id="ev-a2"><h4>Customs</h4><p>Sealed crate found.</p></div>
    </div>
    <div class="tl-col">
      <div class="tl-event" id="ev-b1"><h4>Briefing</h4><p>Route options.</p></div>
      <div class="tl-event" id="ev-b2" data-after="#ev-c1"><h4>Marsh incident</h4><p>Mile 12.</p></div>
    </div>
    <div class="tl-col">
      <div class="tl-event" id="ev-c1"><h4>Council hearing</h4><p>Permit requested.</p></div>
    </div>
    <div class="tl-col">
      <div class="tl-event" id="ev-d1"><h4>Warehouse lease</h4><p>3‑month lease.</p></div>
    </div>
  </div>
</div>

<script type="application/json" id="tl-links">
[
  { "from": "#ev-a1", "to": "#ev-b1" },
  { "from": "#ev-a2", "to": "#ev-c1" },
  { "from": "#ev-b1", "to": "#ev-d1" }
]
</script>

<style>
:root { --tl-col-width:260px; --tl-col-gap:24px; --tl-padding:16px; --tl-wire-width:2px; }
.tl-root{position:relative;width:100%;}
.tl-scroll{display:grid;grid-auto-flow:column;grid-auto-columns:var(--tl-col-width);gap:var(--tl-col-gap);padding:var(--tl-padding);overflow-x:auto;}
.tl-event{border:2px solid black;background:var(--background-primary);color:var(--text-normal);padding:10px;margin:12px;border-radius:6px;}
.tl-wires{position:absolute;top:0;left:0;width:100%;height:100%;pointer-events:none;}
.tl-wire-path{fill:none;stroke:black;stroke-width:var(--tl-wire-width);}
</style>

<script>
function rect(el,root){const r=el.getBoundingClientRect(),rr=root.getBoundingClientRect();return{x:r.left-rr.left,y:r.top-rr.top,w:r.width,h:r.height};}
function drawWires(root){const svg=root.querySelector('.tl-wires'),scroll=root.querySelector('.tl-scroll');svg.innerHTML='';const links=JSON.parse(document.getElementById('tl-links').textContent);for(const link of links){const f=document.querySelector(link.from),t=document.querySelector(link.to);if(!f||!t)continue;const rf=rect(f,scroll),rt=rect(t,scroll);const x1=rf.x+rf.w/2,y1=rf.y+rf.h,x2=rt.x+rt.w/2,y2=rt.y,dx=Math.abs(x2-x1),dy=Math.abs(y2-y1),cx=dx*0.3;const d=`M ${x1},${y1} C ${x1+cx},${y1+dy/2} ${x2-cx},${y2-dy/2} ${x2},${y2}`;const p=document.createElementNS('http://www.w3.org/2000/svg','path');p.setAttribute('d',d);p.setAttribute('class','tl-wire-path');svg.appendChild(p);}const sr=scroll.getBoundingClientRect();svg.setAttribute('viewBox',`0 0 ${sr.width} ${sr.height}`);svg.setAttribute('width',sr.width);svg.setAttribute('height',sr.height);}
function applyConstraints(root){const scroll=root.querySelector('.tl-scroll');scroll.querySelectorAll('.tl-event').forEach(ev=>{const after=ev.getAttribute('data-after');if(after){const other=document.querySelector(after);if(other&&ev.offsetTop<=other.offsetTop){const diff=(other.offsetTop+other.offsetHeight+20)-ev.offsetTop;ev.style.marginTop=diff+'px';}}});}
function render(){const root=document.querySelector('.tl-root');if(!root)return;applyConstraints(root);drawWires(root);}
document.addEventListener('DOMContentLoaded',()=>{render();window.addEventListener('resize',render);document.querySelector('.tl-scroll').addEventListener('scroll',render);});
</script>
