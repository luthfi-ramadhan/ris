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
  .then(res => res.text())
  .then(data => {
    const rows = data.split("\n").map(r => r.split(","));
    const body = document.querySelector("#sheet-table tbody");

    rows.slice(1).forEach(row => {
      const status = row[0];
      const sisa = parseInt(row[3]);

      if(status === "Dipinjam" && sisa > 0){
        const tr = document.createElement("tr");

        row.forEach(cell => {
          const td = document.createElement("td");
          td.textContent = cell;
          tr.appendChild(td);
        });

        body.appendChild(tr);
      }
    });
  });

</script>
