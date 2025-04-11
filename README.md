# HHOD-Calorie-Calculator
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>HHOD Calorie Calculator</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <link href="https://fonts.googleapis.com/css2?family=League+Spartan:wght@400;500;700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; }
    body {
      font-family: 'League Spartan', sans-serif;
      margin: 0;
      padding: 0;
      background: #fff3eb;
      color: #333;
    }
    .container {
      max-width: 480px;
      width: 95%;
      margin: 20px auto;
      background: white;
      border-top: 6px solid #f37221;
      border-radius: 10px;
      padding: 20px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.08);
    }
    h2 {
      text-align: center;
      color: #f37221;
      margin-bottom: 20px;
      font-size: 22px;
    }
    label {
      display: block;
      margin-top: 15px;
      font-weight: bold;
      color: #f37221;
      font-size: 14px;
    }
    input, select {
      width: 100%;
      padding: 10px;
      margin-top: 6px;
      border-radius: 6px;
      border: 1px solid #ccc;
      font-family: 'League Spartan', sans-serif;
      font-size: 14px;
    }
    .unit-tabs {
      display: flex;
      margin: 15px 0;
    }
    .unit-tabs button {
      flex: 1;
      padding: 10px;
      background: #eee;
      border: none;
      cursor: pointer;
      font-weight: bold;
    }
    .unit-tabs button.active {
      background: #f37221;
      color: white;
    }
    .unit-section {
      display: none;
    }
    .unit-section.active {
      display: block;
    }
    button.submit {
      margin-top: 25px;
      width: 100%;
      padding: 12px;
      font-size: 16px;
      background-color: #f37221;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
    .results {
      margin-top: 25px;
      background: #f7f7f7;
      padding: 15px;
      border-top: 6px solid #f37221;
      border-radius: 8px;
      font-size: 14px;
      text-align: center;
    }
    footer {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      gap: 20px;
      text-align: center;
      margin-top: 40px;
      padding: 20px;
      background: #f37221;
      color: white;
      font-size: 14px;
    }
    footer a {
      color: white;
      text-decoration: none;
      font-weight: bold;
    }
    .footer-icon {
      width: 18px;
      vertical-align: middle;
      margin-right: 6px;
    }
  </style>
</head>
<body>

<div class="container">
  <h2>HHOD Calorie Calculator</h2>

  <div class="unit-tabs">
    <button type="button" id="metricTab" class="active" onclick="switchUnit('metric')">Metric</button>
    <button type="button" id="imperialTab" onclick="switchUnit('imperial')">Imperial</button>
  </div>

  <form id="calorieForm">
    <label for="gender">Gender at birth</label>
    <select id="gender" required>
      <option value="" disabled selected>Select gender</option>
      <option value="male">Male</option>
      <option value="female">Female</option>
    </select>

    <label for="age">Age</label>
    <input type="number" id="age" required placeholder="e.g. 30" />

    <div id="metricFields" class="unit-section active">
      <label>Height (cm)</label>
      <input type="number" id="heightMetric" step="0.1" placeholder="e.g. 175" />

      <label>Weight (kg)</label>
      <input type="number" id="weightMetric" step="0.1" placeholder="e.g. 70" />
    </div>

    <div id="imperialFields" class="unit-section">
      <label>Height (feet & inches)</label>
      <div style="display: flex; gap: 10px;">
        <input type="number" id="heightFeet" placeholder="Feet" style="flex:1;" />
        <input type="number" id="heightInches" placeholder="Inches" style="flex:1;" />
      </div>

      <label>Weight (stones & pounds)</label>
      <div style="display: flex; gap: 10px;">
        <input type="number" id="weightStones" placeholder="Stones" style="flex:1;" />
        <input type="number" id="weightPounds" placeholder="Pounds" style="flex:1;" />
      </div>
    </div>

    <label for="activity">Activity</label>
    <select id="activity" required>
      <option value="" disabled selected>Choose activity level</option>
      <option value="1.2">Sedentary (little or no exercise)</option>
      <option value="1.375">Lightly active (1–3 days/week)</option>
      <option value="1.55">Moderately active (3–5 days/week)</option>
      <option value="1.725">Very active (6–7 days/week)</option>
      <option value="1.9">Extra active (hard training/physical job)</option>
    </select>

    <label for="goal">Goal</label>
    <select id="goal" required>
      <option value="" disabled selected>Choose goal</option>
      <option value="maintain">Maintain weight & body composition</option>
      <option value="lose">Lose weight</option>
      <option value="gain">Gain weight & muscle</option>
    </select>

    <label for="macroSplit">Macro Split (Protein/Carbohydrates/Fats)</label>
    <select id="macroSplit" required>
      <option value="" disabled selected>Choose macro split</option>
      <option value="normal">Normal</option>
      <option value="highProtein">High protein / low carb</option>
      <option value="keto">Keto</option>
      <option value="vegan">Vegan</option>
      <option value="paleo">Paleo</option>
    </select>

    <button type="submit" class="submit">Generate Calories & Macros</button>
  </form>

  <div id="results" class="results" style="display: none;"></div>
</div>

<footer>
  <a href="https://www.instagram.com/heathhubod/" target="_blank">
    <img class="footer-icon" src="https://upload.wikimedia.org/wikipedia/commons/a/a5/Instagram_icon.png" alt="Instagram" />
    @heathhubod
  </a>
  <a href="https://www.thehealthhubgroup.com/hhod" target="_blank">www.thehealthhubgroup.com/hhod</a>
</footer>

<script>
let currentUnit = "metric";

function switchUnit(unit) {
  if (unit === currentUnit) return;
  if (unit === "imperial") {
    const cm = parseFloat(document.getElementById("heightMetric").value) || 0;
    const kg = parseFloat(document.getElementById("weightMetric").value) || 0;
    const totalInches = cm / 2.54;
    document.getElementById("heightFeet").value = Math.floor(totalInches / 12);
    document.getElementById("heightInches").value = Math.round(totalInches % 12);
    const totalPounds = kg * 2.20462;
    document.getElementById("weightStones").value = Math.floor(totalPounds / 14);
    document.getElementById("weightPounds").value = Math.round(totalPounds % 14);
    document.getElementById("metricFields").classList.remove("active");
    document.getElementById("imperialFields").classList.add("active");
    document.getElementById("imperialTab").classList.add("active");
    document.getElementById("metricTab").classList.remove("active");
    currentUnit = "imperial";
  } else {
    const feet = parseFloat(document.getElementById("heightFeet").value) || 0;
    const inches = parseFloat(document.getElementById("heightInches").value) || 0;
    const cm = ((feet * 12) + inches) * 2.54;
    document.getElementById("heightMetric").value = Math.round(cm);
    const stones = parseFloat(document.getElementById("weightStones").value) || 0;
    const pounds = parseFloat(document.getElementById("weightPounds").value) || 0;
    const kg = ((stones * 14) + pounds) / 2.20462;
    document.getElementById("weightMetric").value = Math.round(kg);
    document.getElementById("imperialFields").classList.remove("active");
    document.getElementById("metricFields").classList.add("active");
    document.getElementById("metricTab").classList.add("active");
    document.getElementById("imperialTab").classList.remove("active");
    currentUnit = "metric";
  }
}

document.getElementById("calorieForm").addEventListener("submit", function(e) {
  e.preventDefault();

  const gender = document.getElementById("gender").value;
  const age = parseFloat(document.getElementById("age").value);
  const activity = parseFloat(document.getElementById("activity").value);
  const goal = document.getElementById("goal").value;
  const macro = document.getElementById("macroSplit").value;
  let height = parseFloat(document.getElementById("heightMetric").value);
  let weight = parseFloat(document.getElementById("weightMetric").value);

  if (currentUnit === "imperial") {
    const feet = parseFloat(document.getElementById("heightFeet").value) || 0;
    const inches = parseFloat(document.getElementById("heightInches").value) || 0;
    height = ((feet * 12) + inches) * 2.54;

    const stones = parseFloat(document.getElementById("weightStones").value) || 0;
    const pounds = parseFloat(document.getElementById("weightPounds").value) || 0;
    weight = ((stones * 14) + pounds) / 2.20462;
  }

  const bmr = gender === "male"
    ? 10 * weight + 6.25 * height - 5 * age + 5
    : 10 * weight + 6.25 * height - 5 * age - 161;

  const tdee = bmr * activity;

  function getMacros(calories) {
    let protein = 0.3, carbs = 0.4, fats = 0.3;
    if (macro === "highProtein") { protein = 0.4; carbs = 0.3; }
    else if (macro === "keto") { protein = 0.25; carbs = 0.05; fats = 0.7; }
    else if (macro === "vegan") { protein = 0.25; carbs = 0.5; fats = 0.25; }
    else if (macro === "paleo") { protein = 0.35; carbs = 0.25; fats = 0.4; }

    return {
      calories: Math.round(calories),
      protein: Math.round((calories * protein) / 4),
      carbs: Math.round((calories * carbs) / 4),
      fats: Math.round((calories * fats) / 9)
    };
  }

  const result = { maintenance: getMacros(tdee) };
  if (goal === "lose") result.loss_0_5kg = getMacros(tdee - 500);
  if (goal === "gain") {
    result.gain_0_5kg = getMacros(tdee + 500);
    result.gain_1kg = getMacros(tdee + 1000);
  }

  let html = `
    <h3>Maintenance Recommendation</h3>
    <p><strong>Calories:</strong> ${result.maintenance.calories}</p>
    <p>Protein: ${result.maintenance.protein}g | Carbs: ${result.maintenance.carbs}g | Fats: ${result.maintenance.fats}g</p>
  `;

  if (goal === "lose") {
    html += `
      <h3>Weight Loss</h3>
      <p><strong>0.5kg/week:</strong> ${result.loss_0_5kg.calories} kcal - P: ${result.loss_0_5kg.protein}g | C: ${result.loss_0_5kg.carbs}g | F: ${result.loss_0_5kg.fats}g</p>`;
  }

  if (goal === "gain") {
    html += `
      <h3>Weight Gain</h3>
      <p><strong>0.5kg/week:</strong> ${result.gain_0_5kg.calories} kcal - P: ${result.gain_0_5kg.protein}g | C: ${result.gain_0_5kg.carbs}g | F: ${result.gain_0_5kg.fats}g</p>
      <p><strong>1kg/week:</strong> ${result.gain_1kg.calories} kcal - P: ${result.gain_1kg.protein}g | C: ${result.gain_1kg.carbs}g | F: ${result.gain_1kg.fats}g</p>`;
  }

  const resultsDiv = document.getElementById("results");
  resultsDiv.innerHTML = html;
  resultsDiv.style.display = "block";
});
</script>

</body>
</html>
