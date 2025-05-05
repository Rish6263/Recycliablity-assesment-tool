<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Paper/Board RAM Classification Tool</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f7fa;
      color: #333;
      padding: 20px;
      line-height: 1.6;
    }
    h2 {
      background: #0074D9;
      color: white;
      padding: 10px;
      border-radius: 4px;
    }
    .question {
      margin-top: 20px;
      padding: 15px;
      background: #fff;
      border-left: 5px solid #0074D9;
      box-shadow: 0 0 10px rgba(0,0,0,0.05);
    }
    label {
      display: block;
      margin-top: 10px;
    }
    select, input[type="number"] {
      padding: 5px;
      width: 100px;
    }
    .result {
      margin-top: 20px;
      font-weight: bold;
      font-size: 18px;
    }
    .green { color: green; }
    .amber { color: orange; }
    .red { color: red; }
    .section {
      border-top: 2px solid #ddd;
      margin-top: 30px;
      padding-top: 20px;
    }
  </style>
</head>
<body>

<h1>Paper/Board Packaging RAM Evaluation Tool</h1>

<h2>1. Classification</h2>
<div class="question">
  <label>Q1. Is the primary material made of paper or board?</label>
  <select id="q1" onchange="evaluateClassification()">
    <option value="">Select</option>
    <option value="yes">Yes</option>
    <option value="no">No</option>
  </select>

  <div id="q1result" class="result"></div>

  <div id="q2Block" style="display:none">
    <label>Q2. Plastic content ≤5% by weight?</label>
    <select id="q2" onchange="evaluateClassification()">
      <option value="">Select</option>
      <option value="yes">Yes</option>
      <option value="no">No</option>
    </select>
    <div id="q2result" class="result"></div>
  </div>

  <div id="q3Block" style="display:none">
    <label>Q3. Are the fibres cellulosic (wood-derived)?</label>
    <select id="q3" onchange="evaluateClassification()">
      <option value="">Select</option>
      <option value="yes">Yes</option>
      <option value="no">No</option>
    </select>
    <div id="q3result" class="result"></div>
  </div>

  <div id="q3bBlock" style="display:none">
    <label>Verified test data proves recyclability?</label>
    <select id="q3b" onchange="evaluateClassification()">
      <option value="">Select</option>
      <option value="yes">Yes</option>
      <option value="no">No</option>
    </select>
    <div id="q3bresult" class="result"></div>
  </div>

  <div id="q4Block" class="section" style="display:none">
    <h3>Q4. Enter component values (gsm):</h3>
    <label>Fibre (wood):</label><select id="fibre"></select>
    <label>Filler (CaCO₃):</label><select id="filler"></select>
    <label>Water (bound):</label><select id="water"></select>
    <label>Additives:</label><select id="additives"></select>
    <label>Colour Coating:</label><select id="coating"></select>
    <label>Total Mass:</label><select id="totalMass"></select>
    <br><br>
    <button onclick="calculatePaperContent()">🧮 Calculate Paper Content</button>
    <div id="contentResult" class="result"></div>
  </div>
</div>

<script>
  const populateDropdown = (id, values) => {
    const dropdown = document.getElementById(id);
    values.forEach(val => {
      const opt = document.createElement("option");
      opt.value = val;
      opt.text = val;
      dropdown.appendChild(opt);
    });
  };

  populateDropdown("fibre", [0,10,20,30,40,50,60,70,80]);
  populateDropdown("filler", [0,5,10,15,20,25,30]);
  populateDropdown("water", [0,2,4,6,8,10,12]);
  populateDropdown("additives", [0,1,2,3,4,5]);
  populateDropdown("coating", [0,5,10,15,20]);
  populateDropdown("totalMass", [50,60,70,80,90,100,110,120,130]);

  function evaluateClassification() {
    const q1 = document.getElementById("q1").value;
    const q2 = document.getElementById("q2").value;
    const q3 = document.getElementById("q3").value;
    const q3b = document.getElementById("q3b").value;

    const q1result = document.getElementById("q1result");
    const q2Block = document.getElementById("q2Block");
    const q2result = document.getElementById("q2result");
    const q3Block = document.getElementById("q3Block");
    const q3result = document.getElementById("q3result");
    const q3bBlock = document.getElementById("q3bBlock");
    const q3bresult = document.getElementById("q3bresult");
    const q4Block = document.getElementById("q4Block");

    if (q1 === "yes") {
      q1result.innerHTML = "→ Go to Q2";
      q1result.className = "result";
      q2Block.style.display = "block";
    } else if (q1 === "no") {
      q1result.innerHTML = "❌ Not classified as paper/board";
      q1result.className = "result red";
      q2Block.style.display = "none";
    }

    if (q2 === "yes") {
      q2result.innerHTML = "✔ Assess under paper/board guidance";
      q2result.className = "result green";
      q3Block.style.display = "block";
    } else if (q2 === "no") {
      q2result.innerHTML = "❌ Assess under Fibre-Based Composite criteria";
      q2result.className = "result red";
      q3Block.style.display = "none";
    }

    if (q3 === "yes") {
      q3result.innerHTML = "→ Go to Q4";
      q3result.className = "result";
      q4Block.style.display = "block";
      q3bBlock.style.display = "none";
    } else if (q3 === "no") {
      q3result.innerHTML = "";
      q3bBlock.style.display = "block";
    }

    if (q3b === "yes") {
      q3bresult.innerHTML = "✔ Acceptable";
      q3bresult.className = "result green";
    } else if (q3b === "no") {
      q3bresult.innerHTML = "❌ Not acceptable";
      q3bresult.className = "result red";
    }
  }

  function calculatePaperContent() {
    const fibre = parseInt(document.getElementById("fibre").value || 0);
    const filler = parseInt(document.getElementById("filler").value || 0);
    const water = parseInt(document.getElementById("water").value || 0);
    const additives = parseInt(document.getElementById("additives").value || 0);
    const coating = parseInt(document.getElementById("coating").value || 0);
    const total = parseInt(document.getElementById("totalMass").value || 0);

    const numerator = fibre + filler + water + additives + coating;
    const paperContent = total ? ((numerator / total) * 100).toFixed(2) : 0;

    const contentResult = document.getElementById("contentResult");
    contentResult.innerHTML = `📐 Paper Content: ${paperContent}%<br>`;
    if (paperContent >= 95) {
      contentResult.innerHTML += "✅ Assess under Paper & Board guidelines";
      contentResult.className = "result green";
    } else {
      contentResult.innerHTML += "⚠ Refer to Fibre-Based Composite guide";
      contentResult.className = "result amber";
    }
  }
</script>

</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Fibre-Based Composite RAM Assessment (UK, 2025)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f7f8;
      padding: 20px;
      color: #333;
    }
    h1, h2 {
      background: #004b8d;
      color: white;
      padding: 10px;
      border-radius: 5px;
    }
    .section {
      margin-top: 30px;
    }
    .question {
      background: #fff;
      padding: 15px;
      margin: 10px 0;
      border-left: 4px solid #004b8d;
      box-shadow: 0 2px 5px rgba(0,0,0,0.05);
    }
    label, select, input {
      display: block;
      margin: 8px 0;
    }
    select {
      padding: 5px;
      width: 200px;
    }
    button {
      margin-top: 10px;
      padding: 8px 15px;
      background-color: #0074D9;
      border: none;
      color: white;
      cursor: pointer;
      border-radius: 3px;
    }
    button:hover {
      background-color: #005fa3;
    }
    .result {
      font-weight: bold;
      margin-top: 10px;
    }
    .green { color: green; }
    .amber { color: orange; }
    .red { color: red; }
  </style>
</head>
<body>

<h1>Fibre-Based Composite RAM Assessment Questionnaire (UK, 2025)</h1>

<h2>1. Classification</h2>
<div class="question">
  <label>Q1. Is the packaging made of a fibre-based composite (paper + plastic or others)?</label>
  <select id="q1" onchange="showQ2()">
    <option value="">Select</option>
    <option value="yes">Yes</option>
    <option value="no">No</option>
  </select>
  <div id="q1Result" class="result"></div>
</div>

<div id="q2Block" class="question" style="display:none;">
  <label>Q2. Plastic content >5% by total mass?</label>
  <select id="q2" onchange="showQ3()">
    <option value="">Select</option>
    <option value="yes">Yes</option>
    <option value="no">No</option>
  </select>
  <div id="q2Result" class="result"></div>
</div>

<div id="q3Block" class="question" style="display:none;">
  <label>Q3. Does the packaging consist of wood-derived cellulosic fibres?</label>
  <select id="q3" onchange="showQ3b()">
    <option value="">Select</option>
    <option value="yes">Yes</option>
    <option value="no">No</option>
  </select>
</div>

<div id="q3bBlock" class="question" style="display:none;">
  <label>Industry-standard test data confirms suitability for reprocessing?</label>
  <select id="q3b">
    <option value="">Select</option>
    <option value="yes">✔ Acceptable</option>
    <option value="no">❌ Not acceptable</option>
  </select>
</div>

<div id="q4Block" class="question" style="display:none;">
  <h3>Q4. Calculate Paper Content Percentage</h3>
  <label>Fibre (wood):</label>
  <select id="fibre"></select>
  <label>Filler (CaCO₃):</label>
  <select id="filler"></select>
  <label>Water (bound):</label>
  <select id="water"></select>
  <label>Additives:</label>
  <select id="additives"></select>
  <label>Colour Coating:</label>
  <select id="coating"></select>
  <label>Total Mass:</label>
  <select id="totalMass"></select>

  <button onclick="calculatePaperContent()">🧮 Calculate</button>
  <div id="contentResult" class="result"></div>
</div>

<script>
  const populateDropdown = (id, values) => {
    const el = document.getElementById(id);
    values.forEach(val => {
      const option = document.createElement("option");
      option.value = val;
      option.text = val;
      el.appendChild(option);
    });
  };

  // Populate component dropdowns
  populateDropdown("fibre", [0,10,20,30,40,50,60,70,80]);
  populateDropdown("filler", [0,5,10,15,20,25,30]);
  populateDropdown("water", [0,2,4,6,8,10,12]);
  populateDropdown("additives", [0,1,2,3,4,5]);
  populateDropdown("coating", [0,5,10,15,20]);
  populateDropdown("totalMass", [50,60,70,80,90,100,110,120,130]);

  function showQ2() {
    const q1 = document.getElementById("q1").value;
    const q1Result = document.getElementById("q1Result");
    if (q1 === "yes") {
      document.getElementById("q2Block").style.display = "block";
      q1Result.innerHTML = "→ Go to Q2";
    } else {
      document.getElementById("q2Block").style.display = "none";
      q1Result.innerHTML = "❌ Not under Fibre-Based Composite scope";
      q1Result.className = "result red";
    }
  }

  function showQ3() {
    const q2 = document.getElementById("q2").value;
    const q2Result = document.getElementById("q2Result");
    if (q2 === "yes") {
      q2Result.innerHTML = "✔ Assess under Fibre-Based Composite guidance";
      q2Result.className = "result green";
      document.getElementById("q3Block").style.display = "block";
    } else {
      q2Result.innerHTML = "❌ Refer to Paper & Board guidance";
      q2Result.className = "result red";
      document.getElementById("q3Block").style.display = "none";
    }
  }

  function showQ3b() {
    const q3 = document.getElementById("q3").value;
    if (q3 === "yes") {
      document.getElementById("q4Block").style.display = "block";
      document.getElementById("q3bBlock").style.display = "none";
    } else if (q3 === "no") {
      document.getElementById("q3bBlock").style.display = "block";
      document.getElementById("q4Block").style.display = "none";
    }
  }

  function calculatePaperContent() {
    const fibre = parseInt(document.getElementById("fibre").value || 0);
    const filler = parseInt(document.getElementById("filler").value || 0);
    const water = parseInt(document.getElementById("water").value || 0);
    const additives = parseInt(document.getElementById("additives").value || 0);
    const coating = parseInt(document.getElementById("coating").value || 0);
    const total = parseInt(document.getElementById("totalMass").value || 0);

    const numerator = fibre + filler + water + additives + coating;
    const percentage = total ? ((numerator / total) * 100).toFixed(2) : 0;
    const contentResult = document.getElementById("contentResult");

    contentResult.innerHTML = `📐 Paper Content = ${percentage}%<br>`;
    if (percentage >= 95) {
      contentResult.innerHTML += "✅ Classify under Fibre-Based Composite guidance";
      contentResult.className = "result green";
    } else {
      contentResult.innerHTML += "⚠ Refer to specialist test protocols";
      contentResult.className = "result amber";
    }
  }
</script>

</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>UK RAM – Plastic (Flexibles & Rigids) 2025</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      margin: 20px;
      color: #333;
    }
    h1, h2 {
      background-color: #004b8d;
      color: white;
      padding: 10px;
      border-radius: 5px;
    }
    .section {
      margin-top: 30px;
      padding: 15px;
      background: #fff;
      border-left: 5px solid #004b8d;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    label, select {
      margin-top: 10px;
      display: block;
    }
    select, input[type="checkbox"] {
      margin-left: 10px;
    }
    .result {
      font-weight: bold;
      margin-top: 15px;
    }
    .green { color: green; }
    .amber { color: orange; }
    .red { color: red; }
  </style>
</head>
<body>

<h1>Plastic (Flexibles & Rigids) – UK RAM Questionnaire (2025)</h1>

<!-- FLEXIBLES -->
<h2>🟦 Plastic (Flexibles)</h2>

<div class="section">
  <h3>1. Classification</h3>
  <label>Q1. Is it a flexible plastic item?</label>
  <select id="flex_q1" onchange="checkFlexQ1()">
    <option value="">Select</option>
    <option value="yes">Yes</option>
    <option value="no">No</option>
  </select>
  <div id="flex_q1_result" class="result"></div>

  <div id="flex_q2_block" style="display:none;">
    <label>Q2. Is PE/PP the primary polymer?</label>
    <select id="flex_q2" onchange="checkFlexQ2()">
      <option value="">Select</option>
      <option value="yes">Yes</option>
      <option value="no">No</option>
    </select>
    <div id="flex_q2_result" class="result"></div>
  </div>

  <div id="flex_q3_block" style="display:none;">
    <label>Q3. ≥80% PE/PP by weight?</label>
    <select id="flex_q3" onchange="checkFlexQ3()">
      <option value="">Select</option>
      <option value="yes">Yes</option>
      <option value="no">No</option>
    </select>
    <div id="flex_q3_result" class="result"></div>
  </div>
</div>

<div class="section">
  <h3>2. Collection</h3>
  <label>Q4. Collected by ≥75% LAs?</label>
  <select id="flex_q4">
    <option value="">Select</option>
    <option value="yes">Yes → ✔ Kerbside eligible</option>
    <option value="no">No → ❌ Not kerbside eligible</option>
  </select>

  <label>Q5. Covered under take-back scheme?</label>
  <select id="flex_q5">
    <option value="">Select</option>
    <option value="yes">⚠ Amber</option>
    <option value="no">❌ Red</option>
  </select>
</div>

<div class="section">
  <h3>3. Sortation</h3>
  <label>Q6. Carbon black in masterbatch?</label>
  <select id="flex_q6">
    <option value="">Select</option>
    <option value="yes">❌ Red</option>
    <option value="no">Go to Q7</option>
  </select>

  <label>Q7. Contains aluminium foil (not metallised)?</label>
  <select id="flex_q7">
    <option value="">Select</option>
    <option value="yes">❌ Red</option>
    <option value="no">✔ Passes sortation</option>
  </select>
</div>

<div class="section">
  <h3>4. Reprocessing</h3>
  <label>Q8. Does it contain non-reprocessable materials?</label>
  <select id="flex_q8">
    <option value="">Select</option>
    <option value="yes">❌ Red</option>
    <option value="no">✔ Go to Application</option>
  </select>
</div>

<div class="section">
  <h3>5. Application</h3>
  <label>Q9. Amber-level risks (select all that apply):</label>
  <input type="checkbox" id="flex_risk1"> Labels of different material<br>
  <input type="checkbox" id="flex_risk2"> PU adhesives >3% (PE)<br>
  <input type="checkbox" id="flex_risk3"> PU adhesives >5% (PP)<br>
  <input type="checkbox" id="flex_risk4"> Acrylic/latex/tie layers >5%<br><br>

  <label>Q10. Is it from a class with known issues?</label>
  <select id="flex_q10" onchange="checkFlexFinal()">
    <option value="">Select</option>
    <option value="yes">⚠ Amber</option>
    <option value="no">✅ Green</option>
  </select>
  <div id="flex_final_result" class="result"></div>
</div>

<!-- RIGIDS -->
<h2>🟧 Plastic (Rigids)</h2>

<div class="section">
  <h3>1. Classification</h3>
  <label>Q1. Is it a rigid plastic (bottle, tub, cap)?</label>
  <select id="rigid_q1" onchange="checkRigidQ1()">
    <option value="">Select</option>
    <option value="yes">Yes</option>
    <option value="no">No</option>
  </select>
  <div id="rigid_q1_result" class="result"></div>

  <div id="rigid_q2_block" style="display:none;">
    <label>Q2. Retains shape under normal use?</label>
    <select id="rigid_q2">
      <option value="">Select</option>
      <option value="yes">✔ Valid rigid plastic</option>
      <option value="no">❌ Not rigid</option>
    </select>
  </div>
</div>

<script>
  function checkFlexQ1() {
    const val = document.getElementById("flex_q1").value;
    const res = document.getElementById("flex_q1_result");
    if (val === "yes") {
      res.innerText = "→ Go to Q2";
      document.getElementById("flex_q2_block").style.display = "block";
    } else {
      res.innerText = "❌ Not classified as flexible plastic";
      res.className = "result red";
      document.getElementById("flex_q2_block").style.display = "none";
    }
  }

  function checkFlexQ2() {
    const val = document.getElementById("flex_q2").value;
    const res = document.getElementById("flex_q2_result");
    if (val === "yes") {
      res.innerText = "→ Go to Q3";
      document.getElementById("flex_q3_block").style.display = "block";
    } else {
      res.innerText = "❌ May not qualify as recyclable flexible plastic";
      res.className = "result red";
      document.getElementById("flex_q3_block").style.display = "none";
    }
  }

  function checkFlexQ3() {
    const val = document.getElementById("flex_q3").value;
    const res = document.getElementById("flex_q3_result");
    if (val === "yes") {
      res.innerText = "✔ Eligible for reprocessing";
      res.className = "result green";
    } else {
      res.innerText = "❌ Red";
      res.className = "result red";
    }
  }

  function checkFlexFinal() {
    const val = document.getElementById("flex_q10").value;
    const result = document.getElementById("flex_final_result");
    const riskSelected =
      document.getElementById("flex_risk1").checked ||
      document.getElementById("flex_risk2").checked ||
      document.getElementById("flex_risk3").checked ||
      document.getElementById("flex_risk4").checked;

    if (riskSelected || val === "yes") {
      result.innerText = "⚠ Amber – Review risks and confirm recyclability.";
      result.className = "result amber";
    } else if (val === "no") {
      result.innerText = "✅ Green – Passes all criteria.";
      result.className = "result green";
    } else {
      result.innerText = "";
    }
  }

  function checkRigidQ1() {
    const val = document.getElementById("rigid_q1").value;
    const res = document.getElementById("rigid_q1_result");
    if (val === "yes") {
      res.innerText = "→ Go to Q2";
      document.getElementById("rigid_q2_block").style.display = "block";
    } else {
      res.innerText = "❌ Not Plastic (Rigids)";
      res.className = "result red";
      document.getElementById("rigid_q2_block").style.display = "none";
    }
  }
</script>

</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>UK RAM – Steel, Aluminium, Glass (2025)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f6f8fa;
      padding: 20px;
      color: #333;
    }
    h1, h2 {
      background: #003366;
      color: white;
      padding: 10px;
      border-radius: 5px;
    }
    .section {
      margin-top: 30px;
      padding: 15px;
      background: #fff;
      border-left: 5px solid #003366;
      box-shadow: 0 0 5px rgba(0,0,0,0.05);
    }
    label, select, input[type="checkbox"] {
      display: block;
      margin: 10px 0;
    }
    select {
      padding: 4px;
      width: 250px;
    }
    .result {
      font-weight: bold;
      margin-top: 10px;
    }
    .green { color: green; }
    .amber { color: orange; }
    .red { color: red; }
  </style>
</head>
<body>

<h1>UK RAM Assessment Tool – Steel, Aluminium, Glass (2025)</h1>

<!-- STEEL -->
<h2>🟦 Steel Packaging</h2>
<div class="section">
  <label>Q1. Is the packaging made of steel (tins, aerosols, lids)?</label>
  <select id="steel_q1" onchange="showSteelQ2()">
    <option value="">Select</option>
    <option value="yes">Yes → Go to Q2</option>
    <option value="no">❌ Not classified as steel</option>
  </select>
  <div id="steel_q1_result" class="result"></div>
</div>

<div class="section" id="steel_q2_block" style="display:none;">
  <label>Q2. Collected by ≥75% LAs?</label>
  <select id="steel_q2">
    <option value="">Select</option>
    <option value="green">Yes → ✔ Green</option>
    <option value="amber1">50-74% → ⚠ Amber</option>
    <option value="amber2">Take-back scheme → ⚠ Amber</option>
    <option value="red">No → ❌ Red</option>
  </select>

  <label>Q3. Does it exceed 300mm?</label>
  <select id="steel_q3">
    <option value="">Select</option>
    <option value="no">No → ✔ Passes</option>
    <option value="yes">Yes – Check below</option>
  </select>

  <label>If Yes, can it be dismantled or in take-back scheme?</label>
  <select id="steel_q3b">
    <option value="">Select</option>
    <option value="yes">⚠ Amber</option>
    <option value="no">❌ Red</option>
  </select>

  <label>Q4. Has it passed to reprocessing?</label>
  <select>
    <option>✅ Yes → ✔ Proceed (Accepted contaminants)</option>
  </select>

  <label>Q5. Non-steel content >30%?</label>
  <select>
    <option value="">Select</option>
    <option>Yes → ⚠ Amber</option>
    <option>No → ✅ Green</option>
  </select>
</div>

<!-- ALUMINIUM -->
<h2>🟧 Aluminium Packaging</h2>
<div class="section">
  <label>Q1. Is it aluminium (tubes, tins, closures)?</label>
  <select id="alu_q1" onchange="showAluQ2()">
    <option value="">Select</option>
    <option value="yes">Yes → Go to Q2</option>
    <option value="no">❌ Not classified as aluminium</option>
  </select>
  <div id="alu_q1_result" class="result"></div>
</div>

<div class="section" id="alu_q2_block" style="display:none;">
  <label>Q2. Collected by ≥75% LAs?</label>
  <select>
    <option value="">Select</option>
    <option>Yes → ✔ Green</option>
    <option>50-74% → ⚠ Amber</option>
    <option>Take-back scheme → ⚠ Amber</option>
    <option>No → ❌ Red</option>
  </select>

  <label>Q3. ≤300mm in dimensions?</label>
  <select>
    <option value="">Select</option>
    <option>Yes → ✔ Passes</option>
    <option>No – If dismantled or scheme → ⚠ Amber</option>
    <option>No → ❌ Red</option>
  </select>

  <label>Q4. Passed to reprocessing?</label>
  <select>
    <option>✅ Yes → ✔ Proceed</option>
  </select>

  <label>Q5. Non-aluminium content >30%?</label>
  <select>
    <option value="">Select</option>
    <option>Yes → ⚠ Amber</option>
    <option>No → ✅ Green</option>
  </select>
</div>

<!-- GLASS -->
<h2>🟪 Glass Packaging</h2>
<div class="section">
  <label>Q1. Is it glass (bottle, jar)?</label>
  <select id="glass_q1" onchange="showGlassQ2()">
    <option value="">Select</option>
    <option value="yes">Yes → Go to Q2</option>
    <option value="no">❌ Not classified as glass</option>
  </select>
  <div id="glass_q1_result" class="result"></div>
</div>

<div class="section" id="glass_q2_block" style="display:none;">
  <label>Q2. Standard bottle/jar?</label>
  <select id="glass_q2" onchange="showGlassExclusion()">
    <option value="">Select</option>
    <option value="yes">✔ Green</option>
    <option value="no">→ Check exclusions</option>
  </select>

  <label id="glass_exclusion_block" style="display:none;">Any exclusions (mirrored, decorative, contaminated)?</label>
  <select>
    <option value="">Select</option>
    <option>No → ✔ Green</option>
    <option>Yes – Valid scheme? ⚠ Amber</option>
    <option>No scheme → ❌ Red</option>
  </select>

  <label>Q3. Passed to sortation?</label>
  <select>
    <option>✅ Yes → ✔ No issue → Proceed</option>
  </select>

  <label>Q4. Passed to reprocessing?</label>
  <select>
    <option>✅ Yes → ✔ Proceed</option>
  </select>

  <label>Q5. Any of these attachments?</label><br>
  <input type="checkbox"> Ceramic stoppers<br>
  <input type="checkbox"> Non-removable attachments<br>
  <input type="checkbox"> Pump/dispenser heads<br>
  <input type="checkbox"> Colour: other than clear, green, blue, amber<br>

  <label>Impact?</label>
  <select>
    <option value="">Select</option>
    <option>Yes → ⚠ Amber</option>
    <option>No → ✅ Green</option>
  </select>
</div>

<script>
  function showSteelQ2() {
    const val = document.getElementById("steel_q1").value;
    document.getElementById("steel_q1_result").innerText = val === "yes"
      ? "→ Go to Q2"
      : "❌ Not classified as steel";
    document.getElementById("steel_q2_block").style.display = val === "yes" ? "block" : "none";
  }

  function showAluQ2() {
    const val = document.getElementById("alu_q1").value;
    document.getElementById("alu_q1_result").innerText = val === "yes"
      ? "→ Go to Q2"
      : "❌ Not classified as aluminium";
    document.getElementById("alu_q2_block").style.display = val === "yes" ? "block" : "none";
  }

  function showGlassQ2() {
    const val = document.getElementById("glass_q1").value;
    document.getElementById("glass_q1_result").innerText = val === "yes"
      ? "→ Go to Q2"
      : "❌ Not classified as glass";
    document.getElementById("glass_q2_block").style.display = val === "yes" ? "block" : "none";
  }

  function showGlassExclusion() {
    const val = document.getElementById("glass_q2").value;
    document.getElementById("glass_exclusion_block").style.display = val === "no" ? "block" : "none";
  }
</script>

</body>
</html>




<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>UK RAM – Wood & Other Materials (2025)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f7fa;
      padding: 20px;
      color: #333;
    }
    h1, h2 {
      background: #005b4f;
      color: #fff;
      padding: 10px;
      border-radius: 5px;
    }
    .section {
      margin-top: 25px;
      padding: 15px;
      background: #fff;
      border-left: 5px solid #005b4f;
      box-shadow: 0 2px 4px rgba(0,0,0,0.05);
    }
    label, select, input[type="checkbox"] {
      display: block;
      margin: 10px 0;
    }
    select {
      padding: 6px;
      width: 280px;
    }
    .result {
      font-weight: bold;
      margin-top: 10px;
    }
    .green { color: green; }
    .amber { color: orange; }
    .red { color: red; }
  </style>
</head>
<body>

<h1>UK RAM Questionnaire – Wood & Other Materials (2025)</h1>

<!-- WOOD SECTION -->
<h2>🟫 WOOD</h2>
<div class="section">
  <label>Q1. Is the component made primarily of wood (e.g., trays, pallets)?</label>
  <select id="wood_q1" onchange="showWoodSection()">
    <option value="">Select</option>
    <option value="yes">Yes → Go to Collection</option>
    <option value="no">❌ Not classified as wood</option>
  </select>
  <div id="wood_q1_result" class="result"></div>
</div>

<div class="section" id="wood_followup" style="display:none;">
  <label>Q2. Collected by ≥75% of local authorities?</label>
  <select id="wood_q2" onchange="showWoodSortation()">
    <option value="">Select</option>
    <option value="yes">✔ Green</option>
    <option value="no">No – Check take-back</option>
  </select>

  <label>Take-back scheme available?</label>
  <select id="wood_q2b">
    <option value="">Select</option>
    <option value="yes">⚠ Amber</option>
    <option value="no">❌ Red</option>
  </select>

  <label>Q3. Scaled UK sortation process?</label>
  <select>
    <option value="">Select</option>
    <option value="no">❌ Red (no sortation defined)</option>
  </select>

  <label>Q4. Reprocessed at scale in UK?</label>
  <select>
    <option value="">Select</option>
    <option value="no">❌ Red</option>
  </select>

  <label>Q5. Expected to reach application stage?</label>
  <select>
    <option value="">Select</option>
    <option value="no">❌ Red</option>
  </select>
</div>

<!-- OTHER MATERIALS SECTION -->
<h2>🟨 OTHER MATERIALS</h2>
<div class="section">
  <label>Q1. Is the component made of cork, bamboo, ceramic, silicone, rubber, hemp, copper, etc.?</label>
  <select id="other_q1" onchange="showOtherSection()">
    <option value="">Select</option>
    <option value="yes">Yes → Classify as "Other"</option>
    <option value="no">❌ Not applicable under Other</option>
  </select>
  <div id="other_q1_result" class="result"></div>
</div>

<div class="section" id="other_followup" style="display:none;">
  <label>Q2. Collected by ≥75% of local authorities?</label>
  <select id="other_q2" onchange="showOtherSortation()">
    <option value="">Select</option>
    <option value="yes">✔ Green</option>
    <option value="no">No – Check take-back</option>
  </select>

  <label>Take-back scheme available?</label>
  <select id="other_q2b">
    <option value="">Select</option>
    <option value="yes">⚠ Amber</option>
    <option value="no">❌ Red</option>
  </select>

  <label>Q3. Scalable sortation process?</label>
  <select>
    <option value="">Select</option>
    <option value="no">❌ Red (No sortation exists)</option>
  </select>

  <label>Q4. Reprocessed at scale in household recycling?</label>
  <select>
    <option value="">Select</option>
    <option value="no">❌ Red</option>
  </select>

  <label>Q5. Are defined applications present?</label>
  <select>
    <option value="">Select</option>
    <option value="no">❌ Red (Not expected to reach this stage)</option>
  </select>
</div>

<script>
  function showWoodSection() {
    const val = document.getElementById("wood_q1").value;
    document.getElementById("wood_q1_result").innerText =
      val === "yes" ? "→ Proceed to collection" : "❌ Not classified as wood";
    document.getElementById("wood_followup").style.display = val === "yes" ? "block" : "none";
  }

  function showOtherSection() {
    const val = document.getElementById("other_q1").value;
    document.getElementById("other_q1_result").innerText =
      val === "yes" ? "→ Proceed to collection" : "❌ Not applicable under 'Other'";
    document.getElementById("other_followup").style.display = val === "yes" ? "block" : "none";
  }

  function showWoodSortation() {
    const val = document.getElementById("wood_q2").value;
    document.getElementById("wood_q2b").style.display = val === "no" ? "block" : "none";
  }

  function showOtherSortation() {
    const val = document.getElementById("other_q2").value;
    document.getElementById("other_q2b").style.display = val === "no" ? "block" : "none";
  }
</script>

</body>
</html>
