<iframe
  src="/static/export-obsidian-canvas/index.html?file=/DDC-Canon/Events/Full_Timeline.canvas"
  width="100%"
  height="600"
  style="border:1px solid #ccc; border-radius:4px;"
></iframe>
<script>
  window.addEventListener('DOMContentLoaded', () => {
    const iframe = document.createElement('iframe');
    iframe.src = '/viewer.html?file=/canvas/myfile.canvas';
    iframe.width = '100%';
    iframe.height = '600';
    iframe.style.border = 'none';
    document.getElementById('viewer-container').appendChild(iframe);
  });
</script>