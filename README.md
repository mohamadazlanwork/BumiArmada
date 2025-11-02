<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bumi Armada Employee Lifecycle Dashboard (Live)</title>

<!-- External Libraries -->
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">

<style>
:root {
  --green: #00A86B;
  --blue: #1E3A8A;
  --glass-bg: rgba(255, 255, 255, 0.15);
  --text: #e2e8f0;
}

body {
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(135deg, var(--blue), #0f172a);
  color: var(--text);
  margin: 0;
  padding: 40px;
  overflow-x: hidden;
}

/* === Header === */
h1 {
  text-align: center;
  color: white;
  font-size: 36px;
  font-weight: 700;
  letter-spacing: 1px;
}
#lastUpdated {
  text-align: center;
  color: #cbd5e1;
  font-size: 14px;
  margin-bottom: 30px;
}

/* === Dashboard Cards === */
.dashboard {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}
.card {
  background: var(--glass-bg);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 20px;
  text-align: center;
  color: white;
  border: 1px solid rgba(255,255,255,0.2);
  box-shadow: 0 4px 20px rgba(0,0,0,0.25);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.card:hover {
  transform: translateY(-6px);
  box-shadow: 0 8px 25px rgba(0,0,0,0.4);
}
.card h3 {margin: 0;font-size: 16px;opacity: 0.9;}
.card p {margin-top: 10px;font-size: 28px;font-weight: 700;color: var(--green);}

/* === Filters === */
.filters {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  justify-content: center;
  margin-bottom: 25px;
}
.filter-box {
  background: var(--glass-bg);
  backdrop-filter: blur(12px);
  border-radius: 10px;
  padding: 8px 12px;
  border: 1px solid rgba(255,255,255,0.25);
}
select, input {
  background: transparent;
  border: none;
  color: white;
  font-family: inherit;
  font-size: 14px;
  outline: none;
  width: 160px;
}
option { color: black; }
button {
  background: var(--green);
  border: none;
  border-radius: 8px;
  padding: 10px 18px;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: 0.3s;
}
button:hover { background: #059669; }

/* === Table === */
table {
  width: 100%;
  border-collapse: collapse;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(12px);
  border-radius: 12px;
  overflow: hidden;
  color: #f1f5f9;
  border: 1px solid rgba(255,255,255,0.2);
  box-shadow: 0 0 15px rgba(0,0,0,0.3);
}
thead {
  background: rgba(0,0,0,0.3);
}
th, td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid rgba(255,255,255,0.1);
  color: #f8fafc;
}
thead th {
  color: #ffffff;
  font-weight: 600;
}
tbody tr:hover td {
  color: #ffffff;
  background: rgba(255,255,255,0.2);
}

/* === Note === */
.note {
  margin-top: 25px;
  background: var(--glass-bg);
  backdrop-filter: blur(8px);
  border: 1px solid rgba(255,255,255,0.25);
  padding: 15px;
  border-radius: 10px;
  text-align: center;
  color: #cbd5e1;
  font-size: 14px;
}
</style>
</head>
<body>

<h1>🚢 Bumi Armada Employee Lifecycle Dashboard</h1>
<div id="lastUpdated">Loading live data...</div>

<!-- KPI CARDS -->
<div class="dashboard">
  <div class="card"><h3>Total Employees</h3><p id="totalEmp">0</p></div>
  <div class="card"><h3>Average Checklist %</h3><p id="avgChecklist">0%</p></div>
  <div class="card"><h3>Pending Medical</h3><p id="pendingMed">0</p></div>
  <div class="card"><h3>Further Check</h3><p id="furtherCheck">0</p></div>
  <div class="card"><h3>Cleared</h3><p id="cleared">0</p></div>
</div>

<!-- FILTERS -->
<div class="filters">
  <div class="filter-box"><select id="filterCountry"><option value="">Filter by Country</option></select></div>
  <div class="filter-box"><select id="filterDept"><option value="">Filter by Department</option></select></div>
  <div class="filter-box"><select id="filterMed"><option value="">Filter by Medical Status</option></select></div>
  <div class="filter-box"><input type="text" id="searchName" placeholder="Search by Name"></div>
  <button id="clearFilters">Clear Filters</button>
</div>

<!-- TABLE -->
<table id="empTable">
  <thead>
    <tr>
      <th>ID</th><th>Full Name</th><th>Country</th><th>Department</th>
      <th>Role</th><th>Checklist %</th><th>Medical Status</th><th>Start Date</th><th>Notes</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<div class="note">⚠️ Dashboard pulls live data from Google Sheets (non-confidential demo).</div>

<script>
// === 1️⃣ CONNECT TO GOOGLE SHEETS ===
const sheetURL = "https://docs.google.com/spreadsheets/d/17TRf1LsFJbccLUwnWD7PWHGrkeclVJh0/gviz/tq?tqx=out:csv";

Papa.parse(sheetURL, {
  download: true,
  header: true,
  complete: function(results) {
    const data = results.data.filter(r => r.EmployeeID);
    document.getElementById("lastUpdated").innerText =
      "Last updated: " + new Date().toLocaleString();
    buildDashboard(data);
  }
});

// === 2️⃣ BUILD DASHBOARD ===
function buildDashboard(parsed) {
  const tbody = document.querySelector("#empTable tbody");
  tbody.innerHTML = "";

  parsed.forEach(emp => {
    const tr = document.createElement("tr");
    tr.innerHTML = `
      <td>${emp.EmployeeID}</td>
      <td>${emp.FullName}</td>
      <td>${emp.Country}</td>
      <td>${emp.Department}</td>
      <td>${emp.Role}</td>
      <td>${emp.ChecklistCompletionPct}%</td>
      <td>${emp.MedicalStatus}</td>
      <td>${emp.StartDate}</td>
      <td>${emp.Notes}</td>`;
    tbody.appendChild(tr);
  });

  // Populate Filters
  function uniqueValues(field){return [...new Set(parsed.map(x=>x[field]).filter(Boolean))];}
  function fillSelect(id,arr){const sel=document.getElementById(id);sel.innerHTML=`<option value="">Filter by ${id.replace("filter","")}</option>`;arr.forEach(v=>{const opt=document.createElement("option");opt.textContent=v;sel.appendChild(opt);});}
  fillSelect("filterCountry", uniqueValues("Country"));
  fillSelect("filterDept", uniqueValues("Department"));
  fillSelect("filterMed", uniqueValues("MedicalStatus"));

  // Stats
  function updateCards(data) {
    const total = data.length;
    const cleared = data.filter(x => x.MedicalStatus === "Clear").length;
    const pending = data.filter(x => x.MedicalStatus === "Pending").length;
    const further = data.filter(x => x.MedicalStatus === "Further Check").length;
    const avgChecklist = (data.reduce((s,x)=>s+Number(x.ChecklistCompletionPct||0),0)/total).toFixed(1);
    totalEmp.textContent = total;
    avgChecklist.textContent = avgChecklist + "%";
    pendingMed.textContent = pending;
    furtherCheck.textContent = further;
    cleared.textContent = cleared;
  }
  updateCards(parsed);

  // Filters Logic
  const searchName = document.getElementById("searchName");
  const fCountry = document.getElementById("filterCountry");
  const fDept = document.getElementById("filterDept");
  const fMed = document.getElementById("filterMed");
  const clearBtn = document.getElementById("clearFilters");

  function applyFilters() {
    const search = searchName.value.toLowerCase();
    const c = fCountry.value, d = fDept.value, m = fMed.value;
    const rows = document.querySelectorAll("#empTable tbody tr");
    const filteredData = parsed.filter(x =>
      (!c||x.Country===c)&&(!d||x.Department===d)&&(!m||x.MedicalStatus===m)&&x.FullName.toLowerCase().includes(search)
    );
    rows.forEach(row => {
      const name = row.children[1].textContent.toLowerCase();
      const country = row.children[2].textContent;
      const dept = row.children[3].textContent;
      const med = row.children[6].textContent;
      const match = (!c||country===c)&&(!d||dept===d)&&(!m||med===m)&&name.includes(search);
      row.style.display = match ? "" : "none";
    });
    updateCards(filteredData);
  }

  [searchName,fCountry,fDept,fMed].forEach(el=>el.addEventListener("input",applyFilters));
  clearBtn.addEventListener("click",()=>{[searchName,fCountry,fDept,fMed].forEach(el=>el.value="");applyFilters();});
}
</script>

</body>
</html>
