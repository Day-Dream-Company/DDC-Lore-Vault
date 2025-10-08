<canvas id="gitGraph"></canvas>
<script src="https://cdn.jsdelivr.net/npm/@gitgraph/js"></script>
<script>
  const graphContainer = document.getElementById("gitGraph");
  const gitgraph = GitgraphJS.createGitgraph(graphContainer);

  const master = gitgraph.branch("master");
  master.commit("Initial event");

  const featureA = master.branch("Branch A");
  featureA.commit("Event A1").commit("Event A2");

  const featureB = master.branch("Branch B");
  featureB.commit("Event B1");

  featureA.merge(master).commit("Merge point");
  featureB.merge(master).commit("Unified event");
</script>
