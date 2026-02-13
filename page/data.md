---
layout: default
title: Status Peminjam
permalink: /data/
---
<!-- <h2>Status Peminjaman</h2> -->


<h2>Dashboard Status HPC</h2>

<table id="sheet-table">
  <thead>
    <tr>
      <th onclick="sortTable(0)">HPC</th>
      <th onclick="sortTable(1)">Status</th>
      <th onclick="sortTable(2)">Start</th>
      <th onclick="sortTable(3)">End</th>
      <th onclick="sortTable(4)">Sisa Hari</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<style>
#sheet-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  font-size: 14px;
}

#sheet-table th, #sheet-table td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
}

#sheet-table th {
  background: #1e1e1e;
  color: white;
  cursor: pointer;
}

.badge {
  padding: 5px 10px;
  border-radius: 12px;
  color: white;
  font-weight: bold;
  font-size: 12px;
}

.dipinjam { background: #e67e22; }
.ready { background: #27ae60; }
.mati { background: #c0392b; }

.overdue { background: #ffdddd; }
.warning { background: #fff3cd; }
.safe { background: #e8f5e9; }
</style>



<script>
const csvUrl = "https://docs.google.com/spreadsheets/d/e/2PACX-1vSyP3lQOEf0soz2vU0XezrIZ9JXM_y0Jm1yczzHLGWkzRjlQX8yXCnP8Dky1_qWipQaOUE945LrlVxU/pub?gid=2090281700&single=true&output=csv";

fetch(csvUrl)
  .then(res => res.text())
  .then(text => {
    const rows = text.trim().split("\n");
    const tbody = document.querySelector("#sheet-table tbody");

    rows.slice(1).forEach(row => {

      // REGEX CSV SPLIT YANG AMAN
      const cols = row.match(/(".*?"|[^",]+)(?=\s*,|\s*$)/g);

      if(!cols || cols.length < 5) return;

      const hpc = cols[0].replace(/"/g,'').trim();
      const status = cols[1].replace(/"/g,'').trim();
      const start = cols[2].replace(/"/g,'').trim();
      const end = cols[3].replace(/"/g,'').trim();
      const sisa = parseInt(cols[4]);

      const tr = document.createElement("tr");

      let badgeClass = "";
      if(status === "Dipinjam") badgeClass = "dipinjam";
      else if(status === "ready remote") badgeClass = "ready";
      else if(status === "Mati") badgeClass = "mati";

      tr.innerHTML = `
        <td><strong>${hpc}</strong></td>
        <td><span class="badge ${badgeClass}">${status}</span></td>
        <td>${start || "-"}</td>
        <td>${end || "-"}</td>
        <td>${isNaN(sisa) ? "-" : sisa}</td>
      `;

      if(status === "Dipinjam" && !isNaN(sisa)) {
        if(sisa < 0) tr.classList.add("overdue");
        else if(sisa <= 3) tr.classList.add("warning");
        else tr.classList.add("safe");
      }

      tbody.appendChild(tr);
    });
  })
  .catch(err => console.error("Fetch error:", err));
</script>





<!-- <table border="1" id="sheet-table">
  <tbody></tbody>
</table>

<script>
fetch("https://docs.google.com/spreadsheets/d/e/2PACX-1vSyP3lQOEf0soz2vU0XezrIZ9JXM_y0Jm1yczzHLGWkzRjlQX8yXCnP8Dky1_qWipQaOUE945LrlVxU/pub?gid=2090281700&single=true&output=csv")
  .then(response => response.text())
  .then(data => {
    const rows = data.trim().split("\n");
    const tbody = document.querySelector("#sheet-table tbody");

    rows.forEach(row => {
      const cols = row.split(",");
      const tr = document.createElement("tr");

      cols.forEach(col => {
        const td = document.createElement("td");
        td.textContent = col.trim();
        tr.appendChild(td);
      });

      tbody.appendChild(tr);
    });
  })
  .catch(error => console.error("Error:", error));
</script> -->

