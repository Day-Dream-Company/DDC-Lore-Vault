<div id="timeline"></div>
<link href="https://unpkg.com/vis-timeline/styles/vis-timeline-graph2d.min.css" rel="stylesheet">
<script src="https://unpkg.com/vis-timeline/standalone/umd/vis-timeline-graph2d.min.js"></script>
<script>
  const container = document.getElementById('timeline');
  const items = new vis.DataSet([
    {id: 1, content: 'Event A', start: '1900-01-01'},
    {id: 2, content: 'Event B', start: '1910-01-01'},
    {id: 3, content: 'Event C', start: '1920-01-01'}
  ]);
  const options = {
    editable: true,
    margin: {item: 20},
    stack: false,
    orientation: 'top',
    zoomable: true,
    moveable: true
  };
  new vis.Timeline(container, items, options);
</script>
