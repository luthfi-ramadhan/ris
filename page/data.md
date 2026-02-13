---
layout: default
title: Data Mahasiswa
permalink: /data/
---
<h2>Data dari Google Sheets</h2>





<table border="1" id="sheet-table">
  <tbody></tbody>
</table>

<script>
fetch("https://docs.google.com/spreadsheets/d/e/2PACX-1vSyP3lQOEf0soz2vU0XezrIZ9JXM_y0Jm1yczzHLGWkzRjlQX8yXCnP8Dky1_qWipQaOUE945LrlVxU/pubhtml?gid=2090281700&single=true")
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
</script>

