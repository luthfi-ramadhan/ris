---
layout: default
title: Data Mahasiswa
permalink: /data/
---
<h2>Data dari Google Sheets</h2>

<table id="sheet-table" border="1" cellpadding="8">
  <thead></thead>
  <tbody></tbody>
</table>

<script>
fetch("https://docs.google.com/spreadsheets/d/e/2PACX-1vSyP3lQOEf0soz2vU0XezrIZ9JXM_y0Jm1yczzHLGWkzRjlQX8yXCnP8Dky1_qWipQaOUE945LrlVxU/pubhtml?gid=2090281700&single=true")
  .then(response => response.text())
  .then(data => {
    const rows = data.split("\n").map(row => row.split(","));
    const tableHead = document.querySelector("#sheet-table thead");
    const tableBody = document.querySelector("#sheet-table tbody");

    // Header
    const headerRow = document.createElement("tr");
    rows[0].forEach(cell => {
      const th = document.createElement("th");
      th.textContent = cell;
      headerRow.appendChild(th);
    });
    tableHead.appendChild(headerRow);

    // Data
    rows.slice(1).forEach(row => {
      const tr = document.createElement("tr");
      row.forEach(cell => {
        const td = document.createElement("td");
        td.textContent = cell;
        tr.appendChild(td);
      });
      tableBody.appendChild(tr);
    });
  });
</script>
