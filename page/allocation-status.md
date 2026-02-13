---
layout: default
title: Status Peminjam
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
  padding: 5px 10px;
  border-radius: 12px;
  color: white;
  font-weight: bold;
}

.badge.dipinjam { background: #e67e22; }
.badge.ready { background: #27ae60; }
.badge.mati { background: #c0392b; }

.overdue { background: #ffdddd; }
.warning { background: #fff3cd; }
.safe { background: #e8f5e9; }
</style>

<script>
const csvUrl = "https://docs.google.com/spreadsheets/d/e/2PACX-1vSyP3lQOEf0soz2vU0XezrIZ9JXM_y0Jm1yczzHLGWkzRjlQX8yXCnP8Dky1_qWipQaOUE945LrlVxU/pub?gid=2090281700&single=true&output=csv";

fetch(csvUrl)
  .then(res => res.text())
  .then(data => {
    const rows = data.trim().split("\n").map(r => r.split(","));
    const tbody = document.querySelector("#sheet-table tbody");

    rows.slice(1).forEach(row => {
      const hpc = row[0].trim();
      const status = row[1].trim();
      const start = row[2];
      const end = row[3];
      const sisa = parseInt(row[4]);

      // hanya tampilkan yang sedang Dipinjam
    //   if(status !== "Dipinjam") return;

      const tr = document.createElement("tr");

      // badge status
      const badge = `<span class="badge dipinjam">${status}</span>`;

      tr.innerHTML = `
        <td>${hpc}</td>
        <td>${badge}</td>
        <td>${start}</td>
        <td>${end}</td>
        <td>${sisa}</td>
      `;

      // warna otomatis
      if(sisa < 0) {
        tr.classList.add("overdue");
      } else if(sisa <= 3) {
        tr.classList.add("warning");
      } else {
        tr.classList.add("safe");
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
