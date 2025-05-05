<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>UK RAM Recyclability Assessment Tool (2025)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f4f8;
      padding: 20px;
      color: #333;
    }
    h1, h2 {
      background-color: #003366;
      color: white;
      padding: 10px;
      border-radius: 4px;
    }
    select, button {
      padding: 8px;
      margin-top: 10px;
      width: 300px;
    }
    .section {
      display: none;
      background: #fff;
      padding: 20px;
      margin-top: 20px;
      border-left: 5px solid #0074D9;
      box-shadow: 0 0 10px rgba(0,0,0,0.05);
    }
    .result {
      margin-top: 10px;
      font-weight: bold;
    }
    .green { color: green; }
    .amber { color: orange; }
    .red { color: red; }
  </style>
</head>
<body>

<h1>UK RAM Recyclability Assessment Tool (2025)</h1>

<h2>Select Packaging Material Type</h2>
<select id="materialSelector" onchange="toggleMaterialSections()">
  <option value="">-- Select Material Type --</option>
  <option value="paper">Paper and Board</option>
  <option value="fibre">Fibre-based Composite Materials</option>
  <option value="plastic">Plastic (Flexibles & Rigids)</option>
  <option value="steel">Steel</option>
  <option value="aluminium">Aluminium</option>
  <option value="glass">Glass</option>
  <option value="wood">Wood</option>
  <option value="other">Other Materials</option>
</select>

<!-- Insert all material sections here, each wrapped in <div id="section_material"> -->
<div id="section_paper" class="section">
  <h2>Paper and Board</h2>
  <p>[Insert paper/board-specific questionnaire HTML here]</p>
</div>

<div id="section_fibre" class="section">
  <h2>Fibre-Based Composite</h2>
  <p>[Insert fibre-based composite questionnaire HTML here]</p>
</div>

<div id="section_plastic" class="section">
  <h2>Plastic (Flexibles & Rigids)</h2>
  <p>[Insert plastic questionnaire HTML here]</p>
</div>

<div id="section_steel" class="section">
  <h2>Steel</h2>
  <p>[Insert steel questionnaire HTML here]</p>
</div>

<div id="section_aluminium" class="section">
  <h2>Aluminium</h2>
  <p>[Insert aluminium questionnaire HTML here]</p>
</div>

<div id="section_glass" class="section">
  <h2>Glass</h2>
  <p>[Insert glass questionnaire HTML here]</p>
</div>

<div id="section_wood" class="section">
  <h2>Wood</h2>
  <p>[Insert wood questionnaire HTML here]</p>
</div>

<div id="section_other" class="section">
  <h2>Other Materials</h2>
  <p>[Insert other materials questionnaire HTML here]</p>
</div>

<script>
  function toggleMaterialSections() {
    const selected = document.getElementById("materialSelector").value;
    const allSections = ["paper", "fibre", "plastic", "steel", "aluminium", "glass", "wood", "other"];
    allSections.forEach(id => {
      document.getElementById("section_" + id).style.display = (id === selected) ? "block" : "none";
    });
  }
</script>

</body>
</html>
