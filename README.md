<!-- Power BI One-Page Dashboard Wireframe (Bumi Armada) -->

<div style="font-family:'Poppins',sans-serif;background-color:#f8faff;padding:20px;border-radius:12px;max-width:1200px;margin:auto;">

  <!-- Header -->
  <div style="background-color:#002B5B;color:white;padding:20px;border-radius:8px;text-align:center;margin-bottom:20px;">
    <h1>Bumi Armada – HR Onboarding & Offboarding Dashboard (Demo)</h1>
    <p style="font-size:14px;opacity:0.9;">All data are synthetic and anonymized for demonstration purposes.</p>
  </div>

  <!-- KPI Cards -->
  <div style="display:flex;justify-content:space-between;gap:10px;margin-bottom:25px;">
    <div style="flex:1;background:#00AEEF;color:white;padding:15px;border-radius:8px;text-align:center;box-shadow:0 2px 5px rgba(0,0,0,0.1);">
      <h3>Total Employees</h3><h1>50</h1>
    </div>
    <div style="flex:1;background:#FFC107;color:#002B5B;padding:15px;border-radius:8px;text-align:center;box-shadow:0 2px 5px rgba(0,0,0,0.1);">
      <h3>Avg Time-to-Start (Days)</h3><h1>13</h1>
    </div>
    <div style="flex:1;background:#28A745;color:white;padding:15px;border-radius:8px;text-align:center;box-shadow:0 2px 5px rgba(0,0,0,0.1);">
      <h3>Checklist Completion (%)</h3><h1>85%</h1>
    </div>
    <div style="flex:1;background:#20C997;color:white;padding:15px;border-radius:8px;text-align:center;box-shadow:0 2px 5px rgba(0,0,0,0.1);">
      <h3>Medical Clearance Rate</h3><h1>68%</h1>
    </div>
    <div style="flex:1;background:#DC3545;color:white;padding:15px;border-radius:8px;text-align:center;box-shadow:0 2px 5px rgba(0,0,0,0.1);">
      <h3>Offboarded Employees</h3><h1>8%</h1>
    </div>
  </div>

  <!-- Department Overview Row -->
  <div style="display:flex;gap:20px;margin-bottom:25px;">
    <div style="flex:1;background:white;padding:15px;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,0.1);">
      <h3>Avg Checklist Completion by Department</h3>
      <div style="height:180px;background:#eaf4ff;border-radius:6px;text-align:center;line-height:180px;color:#003366;">[Bar Chart]</div>
    </div>
    <div style="flex:1;background:white;padding:15px;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,0.1);">
      <h3>Avg Time-to-Start by Department</h3>
      <div style="height:180px;background:#eaf4ff;border-radius:6px;text-align:center;line-height:180px;color:#003366;">[Column Chart]</div>
    </div>
    <div style="flex:0.7;background:white;padding:15px;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,0.1);">
      <h3>Medical Status Breakdown</h3>
      <div style="height:180px;background:#eaf4ff;border-radius:6px;text-align:center;line-height:180px;color:#003366;">[Pie Chart]</div>
    </div>
  </div>

  <!-- Process & Compliance Row -->
  <div style="display:flex;gap:20px;margin-bottom:25px;">
    <div style="flex:1;background:white;padding:15px;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,0.1);">
      <h3>Onboarding Funnel (Offer → Accept → Start → Complete)</h3>
      <div style="height:180px;background:#fff5ea;border-radius:6px;text-align:center;line-height:180px;color:#663c00;">[Funnel Chart]</div>
    </div>
    <div style="flex:0.6;background:white;padding:15px;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,0.1);">
      <h3>Compliance Gauge</h3>
      <div style="height:180px;background:#fff5ea;border-radius:6px;text-align:center;line-height:180px;color:#663c00;">[Gauge]</div>
    </div>
    <div style="flex:1.2;background:white;padding:15px;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,0.1);">
      <h3>Employee Compliance Table</h3>
      <div style="height:180px;background:#fff5ea;border-radius:6px;text-align:center;line-height:180px;color:#663c00;">[Matrix Table]</div>
    </div>
  </div>

  <!-- Offboarding Insights Row -->
  <div style="display:flex;gap:20px;margin-bottom:25px;">
    <div style="flex:1;background:white;padding:15px;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,0.1);">
      <h3>Offboarded Employees by Department</h3>
      <div style="height:180px;background:#f0f9f0;border-radius:6px;text-align:center;line-height:180px;color:#003366;">[Stacked Bar Chart]</div>
    </div>
    <div style="flex:1;background:white;padding:15px;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,0.1);">
      <h3>Offboarding Duration Trend</h3>
      <div style="height:180px;background:#f0f9f0;border-radius:6px;text-align:center;line-height:180px;color:#003366;">[Line Chart]</div>
    </div>
    <div style="flex:0.6;background:white;padding:15px;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,0.1);">
      <h3>Avg Offboarding Days</h3>
      <div style="height:180px;background:#f0f9f0;border-radius:6px;text-align:center;line-height:180px;color:#003366;">[KPI Card]</div>
    </div>
  </div>

  <!-- Slicers -->
  <div style="display:flex;justify-content:center;gap:15px;">
    <div style="background:#002B5B;color:white;padding:10px 15px;border-radius:8px;">[Department Slicer]</div>
    <div style="background:#002B5B;color:white;padding:10px 15px;border-radius:8px;">[Role Slicer]</div>
    <div style="background:#002B5B;color:white;padding:10px 15px;border-radius:8px;">[MedicalStatus Slicer]</div>
    <div style="background:#002B5B;color:white;padding:10px 15px;border-radius:8px;">[StartDate Slicer]</div>
  </div>

</div>
