<div id="cy" style="width: 100%; height: 600px; background: #1e1e1e;"></div>

<!-- Cytoscape core -->
<script src="https://unpkg.com/cytoscape/dist/cytoscape.min.js"></script>

<script>
  var cy = cytoscape({
    container: document.getElementById('cy'),

    elements: [
      // Nodes (commits/events)
      { data: { id: 'A', label: 'Root A', branch: 'blue' } },
      { data: { id: 'B', label: 'Commit B', branch: 'blue' } },
      { data: { id: 'C', label: 'Root C', branch: 'orange' } },
      { data: { id: 'D', label: 'Commit D', branch: 'orange' } },
      { data: { id: 'E', label: 'Merge', branch: 'blue' } },

      // Edges (branch lines)
      { data: { source: 'A', target: 'B', branch: 'blue' } },
      { data: { source: 'C', target: 'D', branch: 'orange' } },
      { data: { source: 'B', target: 'E', branch: 'blue' } },
      { data: { source: 'D', target: 'E', branch: 'orange' } }
    ],

    style: [
      {
        selector: 'node',
        style: {
          'background-color': 'data(branch)',
          'label': 'data(label)',
          'color': '#fff',
          'text-valign': 'center',
          'text-halign': 'center',
          'font-size': 10,
          'width': 20,
          'height': 20,
          'shape': 'ellipse',
          'border-width': 2,
          'border-color': '#111'
        }
      },
      {
        selector: 'edge',
        style: {
          'width': 3,
          'line-color': 'data(branch)',
          'curve-style': 'bezier',
          'target-arrow-shape': 'none'
        }
      }
    ],

    layout: {
      name: 'breadthfirst',
      directed: true,
      padding: 10,
      spacingFactor: 1.5
    }
  });
  cy.nodes().forEach(n => n.lock());
</script>
