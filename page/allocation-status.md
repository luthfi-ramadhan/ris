---
layout: default
title: HPC Resource Allocation Status
permalink: /allocation-status/
---

<h2>HPC Resource Allocation Status</h2>

<table id="sheet-table">
  <thead>
    <tr>
      <th onclick="sortTable(0)">HPC</th>
      <th onclick="sortTable(1)">Status</th>
      <th onclick="sortTable(2)">Start</th>
      <th onclick="sortTable(3)">End</th>
      <th onclick="sortTable(4)">Remaining Day</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<style>
#sheet-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

#sheet-table th, #sheet-table td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
}

#sheet-table th {
  background: #222;
  color: white;
  cursor: pointer;
}

.badge {
  padding: 6px 12px;
  border-radius: 14px;
  color: white;
  font-weight: 600;
  font-size: 12px;
}

/* STATUS COLORS */
.badge.available { background-color: #27ae60; }
.badge.busy { background-color: #e74c3c; }
.badge.inactive { background-color: #2c3e50; }

/* ROW COLORS (hanya untuk busy) */
.overdue { background: #ffdddd; }
.warning { background: #fff3cd; }
.safe { background: #e8f5e9; }
</style>


<script>
const csvUrl = "https://docs.google.com/spreadsheets/d/e/2PACX-1vSyP3lQOEf0soz2vU0XezrIZ9JXM_y0Jm1yczzHLGWkzRjlQX8yXCnP8Dky1_qWipQaOUE945LrlVxU/pub?gid=2090281700&single=true&output=csv";

fetch(csvUrl)
  .then(res => res.text())
  .then(text => {

    const rows = text.trim().split("\r\n");
    const tbody = document.querySelector("#sheet-table tbody");

    rows.slice(1).forEach(row => {

      const cols = row.split(",");
      if(cols.length < 5) return;

      const hpc = cols[0].trim();
      const statusRaw = cols[1].trim();
      const start = cols[2]?.trim() || "-";
      const end = cols[3]?.trim() || "-";
      const sisa = parseInt(cols[4]);

      const tr = document.createElement("tr");

      // ===== STATUS BADGE =====
      let badgeClass = "";
      let statusText = statusRaw;

      const statusLower = statusRaw.toLowerCase();

      if(statusLower === "available") {
        badgeClass = "available";
      } 
      else if(statusLower === "busy") {
        badgeClass = "busy";
      } 
      else if(statusLower === "inactive") {
        badgeClass = "inactive";
      }

      const badge = `<span class="badge ${badgeClass}">${statusText}</span>`;

      tr.innerHTML = `
        <td>${hpc}</td>
        <td>${badge}</td>
        <td>${start}</td>
        <td>${end}</td>
        <td>${isNaN(sisa) ? "-" : sisa}</td>
      `;

      // ===== ROW COLOR (HANYA JIKA BUSY) =====
      if(statusLower === "busy" && !isNaN(sisa)) {
        if(sisa < 0) {
          tr.classList.add("overdue");
        } 
        else if(sisa <= 3) {
          tr.classList.add("warning");
        } 
        else {
          tr.classList.add("safe");
        }
      }

      tbody.appendChild(tr);
    });
  });


// SORTING
function sortTable(columnIndex) {
  const table = document.getElementById("sheet-table");
  const rows = Array.from(table.rows).slice(1);
  const asc = table.getAttribute("data-sort") !== "asc";

  rows.sort((a, b) => {
    let x = a.cells[columnIndex].innerText;
    let y = b.cells[columnIndex].innerText;

    if(!isNaN(x) && !isNaN(y)) {
      return asc ? x - y : y - x;
    }

    return asc ? x.localeCompare(y) : y.localeCompare(x);
  });

  rows.forEach(row => table.tBodies[0].appendChild(row));
  table.setAttribute("data-sort", asc ? "asc" : "desc");
}
</script>
