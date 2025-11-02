<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bumi Armada Employee Lifecycle Dashboard (Demo)</title>

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
  animation: fadeIn 1.5s ease;
}
h2 {
  text-align: center;
  color: #cbd5e1;
  font-weight: 400;
  margin-bottom: 40px;
  animation: fadeIn 2s ease;
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
  animation: fadeUp 1.2s ease;
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
  animation: fadeIn 2s ease;
}
thead {
  background: rgba(0,0,0,0.2);
}
th, td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid rgba(255,255,255,0.1);
}
tr:hover { background: rgba(255,255,255,0.15); }

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

/* === Animations === */
@keyframes fadeIn {from{opacity:0;}to{opacity:1;}}
@keyframes fadeUp {from{opacity:0;transform:translateY(20px);}to{opacity:1;transform:translateY(0);}}
</style>
</head>
<body>

<h1>🚢 Bumi Armada Employee Lifecycle Dashboard</h1>
<h2>All data below is demo and non-confidential</h2>

<div class="dashboard">
  <div class="card"><h3>Total Employees</h3><p id="totalEmp">0</p></div>
  <div class="card"><h3>Average Checklist %</h3><p id="avgChecklist">0%</p></div>
  <div class="card"><h3>Pending Medical</h3><p id="pendingMed">0</p></div>
  <div class="card"><h3>Further Check</h3><p id="furtherCheck">0</p></div>
  <div class="card"><h3>Cleared</h3><p id="cleared">0</p></div>
</div>

<div class="filters">
  <div class="filter-box">
    <select id="filterCountry"><option value="">Filter by Country</option></select>
  </div>
  <div class="filter-box">
    <select id="filterDept"><option value="">Filter by Department</option></select>
  </div>
  <div class="filter-box">
    <select id="filterMed"><option value="">Filter by Medical Status</option></select>
  </div>
  <div class="filter-box">
    <input type="text" id="searchName" placeholder="Search by Name">
  </div>
  <button id="clearFilters">Clear Filters</button>
</div>

<table id="empTable">
  <thead>
    <tr>
      <th>ID</th><th>Full Name</th><th>Country</th><th>Department</th>
      <th>Role</th><th>Checklist %</th><th>Medical Status</th><th>Start Date</th><th>Notes</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<div class="note">
⚠️ Demo dashboard built for <b>Bumi Armada HR Portfolio</b>. All data is simulated and for visualization only.
</div>

<script>
// === DATA (inline for demo, can replace with CSV later) ===
const data = `EmployeeID,FullName,Country,Department,Role,OfferDate,OfferAcceptedDate,StartDate,ChecklistCompletionPct,DocumentsUploaded,MedicalStatus,MedicalDate,TimeToStartDays,ExitRequestDate,ExitCompleteDate,OffboardCycleDays,Notes
BA-0001,Farah Sulaiman,Malaysia,HR,Engineer,2024-10-02,2024-10-04,2024-10-19,94,10,Clear,2024-10-12,15,,,,"Follow-up required"
BA-0002,Nicholas Tan,Germany,HR,Assistant,2024-10-19,2024-10-22,2024-11-09,75,9,Pending,,18,,,,"Smooth onboarding"
BA-0003,Hafiz Abdullah,England,IT,Coordinator,2024-10-01,2024-10-02,2024-10-11,80,8,Clear,2024-10-13,9,,,,"Smooth onboarding"
BA-0004,Marcus Lim,India,Operations,Support,2024-10-02,2024-10-03,2024-10-15,72,7,Clear,2024-10-17,12,2024-12-28,2025-01-02,5,"Smooth onboarding"
BA-0005,Rachel Tan,London,Quality,Analyst,2024-10-17,2024-10-18,2024-11-05,74,10,Further Check,2024-11-01,18,,,,"Smooth onboarding"
BA-0006,Nicholas Tan,Malaysia,HR,Executive,2024-10-07,2024-10-09,2024-10-19,89,7,Pending,,10,2025-01-16,2025-01-19,3,"All docs complete"`;

// === Parse CSV ===
const parsed = Papa.parse(data.trim(), {header: true}).data;

// === Populate Table ===
const tbody = document.querySelector("#empTable tbody");
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

// === Populate Filters ===
function uniqueValues(field) {
  return [...new Set(parsed.map(x => x[field]).filter(Boolean))];
}
function fillSelect(id, arr) {
  const sel = document.getElementById(id);
  arr.forEach(v => {
    const opt = document.createElement("option");
    opt.textContent = v;
    sel.appendChild(opt);
  });
}
fillSelect("filterCountry", uniqueValues("Country"));
fillSelect("filterDept", uniqueValues("Department"));
fillSelect("filterMed", uniqueValues("MedicalStatus"));

// === Stats ===
function updateCards(data) {
  const total = data.length;
  const cleared = data.filter(x => x.MedicalStatus === "Clear").length;
  const pending = data.filter(x => x.MedicalStatus === "Pending").length;
  const further = data.filter(x => x.MedicalStatus === "Further Check").length;
  const avgChecklist = (data.reduce((s,x)=>s+Number(x.ChecklistCompletionPct),0)/total).toFixed(1);
  document.getElementById("totalEmp").textContent = total;
  document.getElementById("avgChecklist").textContent = avgChecklist + "%";
  document.getElementById("pendingMed").textContent = pending;
  document.getElementById("furtherCheck").textContent = further;
  document.getElementById("cleared").textContent = cleared;
}
updateCards(parsed);

// === Filters Logic ===
const searchName = document.getElementById("searchName");
const fCountry = document.getElementById("filterCountry");
const fDept = document.getElementById("filterDept");
const fMed = document.getElementById("filterMed");
const clearBtn = document.getElementById("clearFilters");

function applyFilters() {
  const search = searchName.value.toLowerCase();
  const c = fCountry.value, d = fDept.value, m = fMed.value;
  const rows = document.querySelectorAll("#empTable tbody tr");
  let filtered = [];
  rows.forEach(row => {
    const cells = row.children;
    const name = cells[1].textContent.toLowerCase();
    const country = cells[2].textContent;
    const dept = cells[3].textContent;
    const med = cells[6].textContent;
    const match = (!c||country===c) && (!d||dept===d) && (!m||med===m) && name.includes(search);
    row.style.display = match ? "" : "none";
    if (match) filtered.push(row);
  });
  updateCards(parsed.filter(x =>
    (!c||x.Country===c)&&(!d||x.Department===d)&&(!m||x.MedicalStatus===m)&&x.FullName.toLowerCase().includes(search)
  ));
}
[searchName,fCountry,fDept,fMed].forEach(el=>el.addEventListener("input",applyFilters));
clearBtn.addEventListener("click",()=>{
  [searchName,fCountry,fDept,fMed].forEach(el=>el.value="");
  applyFilters();
});
</script>

</body>
</html>
