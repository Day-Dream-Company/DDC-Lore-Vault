<div id="timeline"></div>
<div id="timeline" style="height:600px;"></div>

<!-- vis-timeline CSS/JS -->
<link href="https://unpkg.com/vis-timeline/styles/vis-timeline-graph2d.min.css" rel="stylesheet">
<script src="https://unpkg.com/vis-timeline/standalone/umd/vis-timeline-graph2d.min.js"></script>

<script>
  const container = document.getElementById('timeline');

  // Define groups (parallel tracks)
  const groups = new vis.DataSet([
    {id: 1, content: 'Track A'},
    {id: 2, content: 'Track B'},
    {id: 3, content: 'Track C'},
    {id: 4, content: 'Track D'},
    {id: 5, content: 'Track E'},
    {id: 6, content: 'Track F'}
  ]);

  // Define events, assigned to groups
  const items = new vis.DataSet([
    {id: 1, group: 1, content: 'A1', start: '1900-01-01'},
    {id: 2, group: 1, content: 'A2', start: '1910-01-01'},
    {id: 3, group: 2, content: 'B1', start: '1905-01-01'},
    {id: 4, group: 2, content: 'B2', start: '1920-01-01'},
    {id: 5, group: 3, content: 'C1', start: '1915-01-01'},
    {id: 6, group: 3, content: 'C2', start: '1930-01-01'},
    {id: 7, group: 4, content: 'D1', start: '1902-01-01'},
    {id: 8, group: 4, content: 'D2', start: '1940-01-01'},
    {id: 9, group: 5, content: 'E1', start: '1925-01-01'},
    {id: 10, group: 5, content: 'E2', start: '1950-01-01'},
    {id: 11, group: 6, content: 'F1', start: '1935-01-01'},
    {id: 12, group: 6, content: 'F2', start: '1960-01-01'}
  ]);

  // Timeline options
  const options = {
    editable: true,
    stack: false,          // keeps each group separate
    margin: {item: 20},
    orientation: 'top',
    zoomable: true,
    moveable: true
  };

  new vis.Timeline(container, items, groups, options);
</script>
