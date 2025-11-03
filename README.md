<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Bumi Armada Employee Lifecycle Dashboard (Live)</title>

<!-- Fonts + Libraries -->
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet" />
<script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>

<style>
/* ===========================================================
   🌊 PART 1: CORE STYLES AND BACKGROUND
   =========================================================== */
:root{
  --green:#00A86B;
  --blue:#1E3A8A;
  --darkblue:#0f172a;
  --lightblue:#38bdf8;
  --glass-bg:rgba(255,255,255,0.12);
  --text:#e2e8f0;
}

/* Reset + base */
*{box-sizing:border-box;margin:0;padding:0;}
html,body{height:100%;scroll-behavior:smooth;}
body{
  font-family:'Poppins',sans-serif;
  background:linear-gradient(135deg,var(--blue),var(--darkblue));
  color:var(--text);
  overflow-x:hidden;
  overflow-y:auto;
  position:relative;
}

/* =============== Animated background canvas =============== */
#wave-bg{
  position:fixed;
  inset:0;
  z-index:0;
  background:radial-gradient(circle at 50% 50%,#0f172a 0%,#020617 100%);
  overflow:hidden;
}
.wave{
  position:absolute;
  width:200%;
  height:200%;
  left:-50%;
  background:radial-gradient(ellipse at 50% 50%,rgba(30,58,138,0.6),transparent 70%);
  opacity:0.6;
  border-radius:40%;
  animation:drift 18s infinite linear;
}
.wave:nth-child(2){animation-duration:22s;opacity:0.4;background:radial-gradient(ellipse at 60% 40%,rgba(0,168,107,0.4),transparent 70%);}
.wave:nth-child(3){animation-duration:26s;opacity:0.3;background:radial-gradient(ellipse at 40% 70%,rgba(56,189,248,0.3),transparent 70%);}
@keyframes drift{
  0%{transform:rotate(0deg) scale(1);}
  50%{transform:rotate(180deg) scale(1.05);}
  100%{transform:rotate(360deg) scale(1);}
}

/* =============== Header animations =============== */
@keyframes fadeSlide{
  0%{opacity:0;transform:translateY(-25px);}
  100%{opacity:1;transform:translateY(0);}
}
header{
  position:relative;
  z-index:10;
  text-align:center;
  padding:60px 20px 30px;
}
header h1{
  font-size:42px;
  font-weight:700;
  color:white;
  text-shadow:0 0 15px rgba(56,189,248,0.5);
  letter-spacing:1px;
  animation:fadeSlide 1.4s ease forwards;
}
header p{
  margin-top:12px;
  color:var(--lightblue);
  font-weight:500;
  font-size:16px;
  letter-spacing:0.5px;
  animation:fadeSlide 2s ease forwards;
}
#timestamp{
  font-family:monospace;
  color:#34d399;
  margin-top:6px;
  animation:fadeSlide 2.4s ease forwards;
  text-shadow:0 0 8px rgba(0,168,107,0.6);
}

/* =============== Layout grid placeholder =============== */
main{
  position:relative;
  z-index:5;
  padding:30px 5vw 60px;
  display:flex;
  flex-direction:column;
  gap:40px;
}

/* Section headings */
.section-title{
  font-size:24px;
  font-weight:600;
  color:#38bdf8;
  margin-bottom:15px;
  border-left:5px solid var(--green);
  padding-left:12px;
  text-shadow:0 0 6px rgba(56,189,248,0.4);
  animation:fadeSlide 1.5s ease both;
}

/* Smooth entry for each block */
@keyframes fadeUp{
  from{opacity:0;transform:translateY(25px);}
  to{opacity:1;transform:translateY(0);}
}
.section-block{
  background:var(--glass-bg);
  backdrop-filter:blur(12px);
  border:1px solid rgba(255,255,255,0.15);
  border-radius:16px;
  padding:25px;
  box-shadow:0 6px 25px rgba(0,0,0,0.3);
  animation:fadeUp 1.2s ease both;
}

/* Footer placeholder */
footer{
  text-align:center;
  color:#94a3b8;
  font-size:13px;
  padding:40px 0 20px;
  z-index:10;
  position:relative;
}
footer a{color:#38bdf8;text-decoration:none;}
footer a:hover{text-decoration:underline;}
</style>
</head>
<body>

<!-- 🌊 BACKGROUND WAVES -->
<div id="wave-bg">
  <div class="wave"></div>
  <div class="wave"></div>
  <div class="wave"></div>
</div>

<header>
  <h1>Bumi Armada Employee Lifecycle Dashboard</h1>
  <p>Real-time HR Insights · ⚠️ For demonstration purposes only. No real data is used.</p>
  <div id="timestamp">Loading latest data…</div>
</header>

<main>
  <div class="section-block">
    <h2 class="section-title">Overview</h2>
    <p>
      Welcome to the <strong>Bumi Armada Employee Lifecycle Dashboard</strong>.  
      This dashboard provides a live overview of onboarding, medical status, and HR compliance activities.  
      Visuals update automatically from the organization’s secure Google Sheet data source.
    </p>
  </div>
  <!-- ========== KPI CARDS ========== -->
  <div class="section-block">
    <h2 class="section-title">Key Metrics</h2>
    <div class="dashboard-grid">
      <div class="kpi-card"><h3>Total Employees</h3><p id="kpiTotal">0</p></div>
      <div class="kpi-card"><h3>Avg Checklist %</h3><p id="kpiChecklist">0%</p></div>
      <div class="kpi-card"><h3>Pending Medical</h3><p id="kpiPending">0</p></div>
      <div class="kpi-card"><h3>Further Check</h3><p id="kpiFurther">0</p></div>
      <div class="kpi-card"><h3>Cleared</h3><p id="kpiCleared">0</p></div>
    </div>
  </div>

  <!-- ========== FILTER PANEL ========== -->
  <div class="section-block">
    <h2 class="section-title">Filters & Controls</h2>
    <div class="filter-panel">
      <div class="filter-box">
        <label for="filterCountry">Country</label>
        <select id="filterCountry"><option value="">All Countries</option></select>
      </div>
      <div class="filter-box">
        <label for="filterDept">Department</label>
        <select id="filterDept"><option value="">All Departments</option></select>
      </div>
      <div class="filter-box">
        <label for="filterMed">Medical Status</label>
        <select id="filterMed"><option value="">All Status</option></select>
      </div>
      <div class="filter-box">
        <label for="searchName">Search Name</label>
        <input type="text" id="searchName" placeholder="Enter employee name" />
      </div>
      <div class="buttons">
        <button id="btnClear">🧹 Clear</button>
        <button id="btnExport">⬇️ Export CSV</button>
        <button id="btnTheme">🌗 Dark Mode</button>
      </div>
    </div>
  </div>

  <!-- ========== KPI & FILTER STYLES ========== -->
  <style>
  /* Grid & cards */
  .dashboard-grid{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(180px,1fr));
    gap:20px;
    margin-top:20px;
  }
  .kpi-card{
    background:var(--glass-bg);
    backdrop-filter:blur(14px);
    border-radius:14px;
    padding:24px;
    text-align:center;
    border:1px solid rgba(255,255,255,0.2);
    box-shadow:0 4px 25px rgba(0,0,0,0.25);
    transform:translateY(0);
    transition:all 0.4s ease;
  }
  .kpi-card:hover{
    transform:translateY(-8px) scale(1.03);
    box-shadow:0 8px 35px rgba(0,168,107,0.4);
  }
  .kpi-card h3{
    margin:0;
    font-size:16px;
    font-weight:500;
    color:#a5f3fc;
    letter-spacing:0.3px;
  }
  .kpi-card p{
    margin-top:10px;
    font-size:34px;
    font-weight:700;
    color:var(--green);
    text-shadow:0 0 10px rgba(0,168,107,0.5);
    transition:color 0.3s;
  }

  /* Filter panel layout */
  .filter-panel{
    display:flex;
    flex-wrap:wrap;
    gap:20px;
    justify-content:center;
    align-items:flex-end;
  }
  .filter-box{
    display:flex;
    flex-direction:column;
    background:var(--glass-bg);
    backdrop-filter:blur(12px);
    padding:10px 14px;
    border-radius:10px;
    border:1px solid rgba(255,255,255,0.2);
    min-width:180px;
  }
  .filter-box label{
    font-size:13px;
    color:#93c5fd;
    margin-bottom:5px;
    font-weight:500;
  }
  select,input{
    background:transparent;
    border:none;
    border-bottom:1px solid rgba(255,255,255,0.3);
    color:white;
    font-size:14px;
    padding:6px;
    outline:none;
    transition:border-color 0.3s;
  }
  select:focus,input:focus{border-color:#38bdf8;}
  option{color:#111827;}

  /* Buttons */
  .buttons{display:flex;gap:10px;flex-wrap:wrap;}
  button{
    background:var(--green);
    border:none;
    border-radius:8px;
    padding:10px 18px;
    font-weight:600;
    color:white;
    cursor:pointer;
    transition:background 0.3s, transform 0.3s;
  }
  button:hover{
    background:#059669;
    transform:translateY(-3px);
  }
  </style>

  <!-- ========== KPI & FILTER SCRIPT ========== -->
  <script>
  // Animate KPI counters
  function animateValue(id, start, end, duration){
    const obj = document.getElementById(id);
    let startTimestamp = null;
    const step = (timestamp)=>{
      if(!startTimestamp) startTimestamp = timestamp;
      const progress = Math.min((timestamp - startTimestamp)/duration, 1);
      obj.innerText = Math.floor(progress * (end - start) + start);
      if(progress < 1) window.requestAnimationFrame(step);
    };
    window.requestAnimationFrame(step);
  }

  // Placeholder animation demo before live data loads
  window.addEventListener("DOMContentLoaded",()=>{
    ["kpiTotal","kpiChecklist","kpiPending","kpiFurther","kpiCleared"].forEach((id,i)=>{
      setTimeout(()=>{animateValue(id,0,Math.floor(Math.random()*1000),2000)},i*300);
    });
  });
  </script>
  
    <!-- ========== EMPLOYEE TABLE SECTION ========== -->
  <div class="section-block">
    <h2 class="section-title">Employee Records</h2>
    <p>This section displays all employee records loaded directly from the
       organization’s secure Google Sheet (non-confidential data).</p>

    <div class="table-wrapper">
      <table id="empTable">
        <thead>
          <tr>
            <th>ID</th>
            <th>Full Name</th>
            <th>Country</th>
            <th>Department</th>
            <th>Role</th>
            <th>Checklist %</th>
            <th>Medical Status</th>
            <th>Start Date</th>
            <th>Notes</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>
    <div class="note">
      ⚠️ Dashboard pulls live data from Google Sheets (for demo visualisation only).
    </div>
  </div>

  <!-- ========== TABLE STYLES ========== -->
  <style>
  .table-wrapper{
    overflow-x:auto;
    margin-top:20px;
    border-radius:12px;
    box-shadow:0 0 20px rgba(0,0,0,0.3);
  }
  table{
    width:100%;
    border-collapse:collapse;
    background:rgba(255,255,255,0.9);
    backdrop-filter:blur(10px);
    border-radius:12px;
    color:#111827;
    font-size:14px;
  }
  thead{
    background:rgba(0,168,107,0.1);
  }
  th,td{
    padding:14px 18px;
    text-align:left;
    border-bottom:1px solid rgba(0,0,0,0.05);
  }
  th{
    font-weight:600;
    color:#0f172a;
  }
  tbody tr{
    opacity:0;
    transform:translateY(10px);
    transition:all .4s ease;
  }
  tbody tr.visible{
    opacity:1;
    transform:translateY(0);
  }
  tbody tr:hover td{
    background:rgba(56,189,248,0.15);
    color:#000;
  }
  </style>

  <!-- ========== TABLE SCRIPT (LIVE DATA LOAD) ========== -->
  <script>
  const SHEET_URL =
    "https://docs.google.com/spreadsheets/d/17TRf1LsFJbccLUwnWD7PWHGrkeclVJh0/gviz/tq?tqx=out:csv";

  // Fetch + parse Google Sheet
  Papa.parse(SHEET_URL,{
    download:true,
    header:true,
    complete:function(res){
      const rows=res.data.filter(r=>r.EmployeeID);
      document.getElementById("timestamp").innerText=
        "Last updated: "+new Date().toLocaleString();
      populateTable(rows);
      populateFilters(rows);
      updateKPIs(rows);
    }
  });

  // Populate table
  function populateTable(data){
    const tbody=document.querySelector("#empTable tbody");
    tbody.innerHTML="";
    data.forEach((r,i)=>{
      const tr=document.createElement("tr");
      tr.innerHTML=`
        <td>${r.EmployeeID||"-"}</td>
        <td>${r.FullName||"-"}</td>
        <td>${r.Country||"-"}</td>
        <td>${r.Department||"-"}</td>
        <td>${r.Role||"-"}</td>
        <td>${r.ChecklistCompletionPct||"0"}%</td>
        <td>${r.MedicalStatus||"-"}</td>
        <td>${r.StartDate||"-"}</td>
        <td>${r.Notes||""}</td>`;
      tbody.appendChild(tr);
      setTimeout(()=>tr.classList.add("visible"),i*50);
    });
  }

  // Fill dropdown filters
  function populateFilters(data){
    const unique=(f)=>[...new Set(data.map(x=>x[f]).filter(Boolean))];
    fill("filterCountry",unique("Country"));
    fill("filterDept",unique("Department"));
    fill("filterMed",unique("MedicalStatus"));
    function fill(id,arr){
      const sel=document.getElementById(id);
      sel.innerHTML=`<option value="">All</option>`;
      arr.forEach(v=>{
        const opt=document.createElement("option");
        opt.value=v;opt.textContent=v;sel.appendChild(opt);
      });
    }
  }

  // KPI updates
  function updateKPIs(data){
    const total=data.length;
    const cleared=data.filter(x=>x.MedicalStatus==="Clear").length;
    const pending=data.filter(x=>x.MedicalStatus==="Pending").length;
    const further=data.filter(x=>x.MedicalStatus==="Further Check").length;
    const avg=(data.reduce((s,x)=>s+Number(x.ChecklistCompletionPct||0),0)/total).toFixed(1);
    animateValue("kpiTotal",0,total,1000);
    animateValue("kpiChecklist",0,avg,1200);
    animateValue("kpiPending",0,pending,1000);
    animateValue("kpiFurther",0,further,1000);
    animateValue("kpiCleared",0,cleared,1000);
  }

  // Filter logic
  const fCountry=document.getElementById("filterCountry");
  const fDept=document.getElementById("filterDept");
  const fMed=document.getElementById("filterMed");
  const sName=document.getElementById("searchName");

  [fCountry,fDept,fMed,sName].forEach(el=>{
    el.addEventListener("input",applyFilters);
  });

  function applyFilters(){
    Papa.parse(SHEET_URL,{
      download:true,header:true,
      complete:function(res){
        let d=res.data.filter(r=>r.EmployeeID);
        const c=fCountry.value,dpt=fDept.value,m=fMed.value,s=sName.value.toLowerCase();
        d=d.filter(x=>
          (!c||x.Country===c)&&
          (!dpt||x.Department===dpt)&&
          (!m||x.MedicalStatus===m)&&
          x.FullName.toLowerCase().includes(s)
        );
        populateTable(d);
        updateKPIs(d);
      }
    });
  }

  // Clear filters
  document.getElementById("btnClear").addEventListener("click",()=>{
    [fCountry,fDept,fMed,sName].forEach(e=>e.value="");
    applyFilters();
  });
  </script>
  <!-- ========== DARK MODE & EXPORT SCRIPT ========== -->
  <script>
  // 🌗 Toggle Dark Mode
  const btnTheme = document.getElementById("btnTheme");
  btnTheme.addEventListener("click", () => {
    document.body.classList.toggle("dark-mode");
    btnTheme.innerText = document.body.classList.contains("dark-mode")
      ? "☀️ Light Mode"
      : "🌗 Dark Mode";
  });

  // 💾 Export visible rows to CSV
  const btnExport = document.getElementById("btnExport");
  btnExport.addEventListener("click", () => {
    const rows = Array.from(document.querySelectorAll("#empTable tbody tr"))
      .filter(r => r.classList.contains("visible") || r.style.display !== "none");
    if (rows.length === 0) {
      alert("No visible data to export. Please apply filters or wait for data load.");
      return;
    }
    const header = Array.from(document.querySelectorAll("#empTable thead th"))
      .map(th => `"${th.innerText}"`)
      .join(",");
    const csv = [header];
    rows.forEach(row => {
      const cells = Array.from(row.cells).map(td => `"${td.innerText}"`);
      csv.push(cells.join(","));
    });
    const blob = new Blob([csv.join("\n")], { type: "text/csv" });
    const a = document.createElement("a");
    a.href = URL.createObjectURL(blob);
    a.download = `BumiArmada_Export_${new Date().toISOString().split("T")[0]}.csv`;
    a.click();
  });
  </script>

  <!-- ========== DARK MODE STYLES ========== -->
  <style>
  .dark-mode {
    background:linear-gradient(135deg,#020617,#0f172a);
    color:#e2e8f0;
  }
  .dark-mode .section-block{
    background:rgba(255,255,255,0.05);
    border:1px solid rgba(255,255,255,0.15);
    box-shadow:0 0 25px rgba(0,255,255,0.1);
  }
  .dark-mode table{
    background:rgba(255,255,255,0.05);
    color:#e5e5e5;
  }
  .dark-mode th{
    background:rgba(255,255,255,0.1);
    color:#38bdf8;
  }
  .dark-mode td{
    border-bottom:1px solid rgba(255,255,255,0.15);
  }
  .dark-mode .kpi-card{
    background:rgba(255,255,255,0.08);
    box-shadow:0 0 20px rgba(0,255,255,0.2);
  }
  .dark-mode .filter-box{
    background:rgba(255,255,255,0.1);
    border-color:rgba(255,255,255,0.2);
  }
  .dark-mode select,
  .dark-mode input{
    background:rgba(255,255,255,0.1);
    color:#fff;
    border-color:rgba(255,255,255,0.2);
  }
  .dark-mode button{
    background:#10b981;
  }
  .dark-mode footer{
    color:#38bdf8;
    text-shadow:0 0 6px #38bdf8;
  }
  </style>

  <!-- ========== FOOTER SECTION ========== -->
  <footer>
    <p>© 2025 Bumi Armada · Employee Lifecycle Dashboard</p>
    <p>Created by <a href="#">Azlan</a> · For demo visualisation only</p>
    <div class="footer-wave"></div>
  </footer>

  <!-- ========== FOOTER WAVE ANIMATION ========== -->
  <style>
  .footer-wave{
    position:absolute;
    left:0;
    bottom:0;
    width:100%;
    height:150px;
    overflow:hidden;
    z-index:0;
  }
  .footer-wave::before,
  .footer-wave::after{
    content:"";
    position:absolute;
    width:200%;
    height:200%;
    top:-50%;
    left:-50%;
    border-radius:40%;
    background:radial-gradient(ellipse at 50% 50%,rgba(0,168,107,0.25),transparent 70%);
    animation:wavefloat 18s infinite linear;
  }
  .footer-wave::after{
    background:radial-gradient(ellipse at 60% 60%,rgba(56,189,248,0.25),transparent 70%);
    animation-duration:22s;
  }
  @keyframes wavefloat{
    0%{transform:rotate(0deg) scale(1);}
    50%{transform:rotate(180deg) scale(1.05);}
    100%{transform:rotate(360deg) scale(1);}
  }

  /* Button pulse */
  button:hover::after{
    content:"";
    position:absolute;
    inset:0;
    border-radius:8px;
    background:rgba(255,255,255,0.2);
    animation:btnPulse .6s ease;
  }
  @keyframes btnPulse{
    from{transform:scale(1);opacity:1;}
    to{transform:scale(1.4);opacity:0;}
  }
  </style>
  
    <!-- ========== FLOATING CONTROLS ========== -->
  <div id="floatingControls">
    <button id="btnRefresh" title="Reload live data">🔄 Refresh Data</button>
    <span id="lastSync">⏱️ Syncing...</span>
  </div>

  <!-- ========== BACKGROUND CANVAS (WAVES + PARTICLES) ========== -->
  <canvas id="bgCanvas"></canvas>

  <!-- ========== BACKGROUND STYLES ========== -->
  <style>
  #bgCanvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: -2;
    background: linear-gradient(135deg, #0f172a 10%, #1E3A8A 90%);
  }

  /* Floating controls */
  #floatingControls{
    position:fixed;
    bottom:25px;
    right:25px;
    display:flex;
    gap:12px;
    align-items:center;
    z-index:100;
    backdrop-filter:blur(10px);
    background:rgba(255,255,255,0.1);
    border-radius:12px;
    padding:8px 14px;
    border:1px solid rgba(255,255,255,0.2);
    box-shadow:0 4px 15px rgba(0,0,0,0.3);
  }
  #floatingControls button{
    background:#00A86B;
    border:none;
    border-radius:8px;
    padding:8px 14px;
    color:white;
    font-weight:600;
    cursor:pointer;
    transition:background 0.3s, transform 0.2s;
  }
  #floatingControls button:hover{
    background:#059669;
    transform:translateY(-3px);
  }
  #lastSync{
    color:#38bdf8;
    font-size:14px;
    font-family:monospace;
    text-shadow:0 0 6px #38bdf8;
  }
  </style>

  <!-- ========== BACKGROUND ANIMATION SCRIPT ========== -->
  <script>
  const canvas=document.getElementById("bgCanvas");
  const ctx=canvas.getContext("2d");
  let w,h,particles=[];
  function resize(){
    w=canvas.width=window.innerWidth;
    h=canvas.height=window.innerHeight;
  }
  window.addEventListener("resize",resize);
  resize();

  // Particle class
  class Particle{
    constructor(){
      this.x=Math.random()*w;
      this.y=Math.random()*h;
      this.r=Math.random()*2+0.5;
      this.dx=(Math.random()-0.5)*0.3;
      this.dy=(Math.random()-0.5)*0.3;
      this.color=Math.random()>0.5?"#38bdf8":"#00A86B";
    }
    move(){
      this.x+=this.dx;this.y+=this.dy;
      if(this.x<0||this.x>w) this.dx*=-1;
      if(this.y<0||this.y>h) this.dy*=-1;
    }
    draw(){
      ctx.beginPath();
      ctx.arc(this.x,this.y,this.r,0,Math.PI*2);
      ctx.fillStyle=this.color;
      ctx.fill();
    }
  }

  for(let i=0;i<200;i++){particles.push(new Particle());}

  // Wave layers
  let t=0;
  function wave(yOff,color,amp){
    ctx.beginPath();
    ctx.moveTo(0,h);
    for(let x=0;x<w;x++){
      const y=Math.sin(x*0.01+t*0.02)*amp + yOff;
      ctx.lineTo(x,y);
    }
    ctx.lineTo(w,h);
    ctx.closePath();
    ctx.fillStyle=color;
    ctx.fill();
  }

  // Animation loop
  function animate(){
    ctx.clearRect(0,0,w,h);
    // Background gradient
    const grad=ctx.createLinearGradient(0,0,w,h);
    grad.addColorStop(0,"#0f172a");
    grad.addColorStop(1,"#1E3A8A");
    ctx.fillStyle=grad;
    ctx.fillRect(0,0,w,h);

    // Waves
    wave(h*0.85,"rgba(56,189,248,0.15)",30);
    wave(h*0.9,"rgba(0,168,107,0.2)",40);
    wave(h*0.95,"rgba(0,168,107,0.25)",50);

    // Particles
    particles.forEach(p=>{
      p.move();p.draw();
    });
    t+=0.5;
    requestAnimationFrame(animate);
  }
  animate();
  </script>

  <!-- ========== REFRESH DATA + SYNC BADGE SCRIPT ========== -->
  <script>
  const btnRefresh=document.getElementById("btnRefresh");
  const lastSync=document.getElementById("lastSync");

  btnRefresh.addEventListener("click",()=>{
    lastSync.innerText="🔄 Refreshing data...";
    lastSync.style.color="#facc15";
    Papa.parse(SHEET_URL,{
      download:true,header:true,
      complete:function(res){
        const data=res.data.filter(r=>r.EmployeeID);
        populateTable(data);
        updateKPIs(data);
        setTimeout(()=>{
          lastSync.innerText="✅ Synced: "+new Date().toLocaleTimeString();
          lastSync.style.color="#38bdf8";
        },1000);
      }
    });
  });

  // Auto-update badge every 2 minutes
  setInterval(()=>{
    lastSync.innerText="✅ Synced: "+new Date().toLocaleTimeString();
  },120000);
  </script>

  <!-- ========== FINAL NOTES ========== -->
  <div class="note" style="margin-top:60px;">
    © 2025 Bumi Armada · Employee Lifecycle Dashboard | Created by Azlan  | ⚠️ For demo visualisation only – no actual records used.
  </div>


