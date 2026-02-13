---
layout: default
title: HPC Specifications
permalink: /hpc-specifications/
---

<div class="container">
  <h1>High Performance Computing (HPC) Specifications</h1>

  <p>
    This page provides detailed technical specifications of the available
    High Performance Computing (HPC) nodes, including processor,
    memory, storage, and network configurations.
  </p>

  <hr>

  <h2>Cluster Overview</h2>
  <ul>
    <li><strong>Total Nodes:</strong> 15</li>
    <li><strong>Network:</strong> 10 Gbps Ethernet</li>
    <li><strong>Operating System:</strong> Ubuntu Server 22.04 LTS</li>
    <li><strong>Scheduler:</strong> SLURM Workload Manager</li>
  </ul>

  <hr>

  <h2>Node Specifications</h2>

  <table border="1" cellpadding="8" cellspacing="0" width="100%">
    <thead>
      <tr>
        <th>Node ID</th>
        <th>Processor</th>
        <th>Cores</th>
        <th>RAM</th>
        <th>Storage</th>
        <th>GPU</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>L1</td>
        <td>Intel Xeon Gold 6230</td>
        <td>20 Cores</td>
        <td>128 GB</td>
        <td>1 TB SSD</td>
        <td>-</td>
      </tr>
      <tr>
        <td>M1</td>
        <td>AMD EPYC 7543</td>
        <td>32 Cores</td>
        <td>256 GB</td>
        <td>2 TB NVMe</td>
        <td>NVIDIA RTX 3090 (24GB)</td>
      </tr>
      <tr>
        <td>H2</td>
        <td>Intel Xeon Silver 4214</td>
        <td>12 Cores</td>
        <td>64 GB</td>
        <td>1 TB SSD</td>
        <td>NVIDIA Tesla V100 (16GB)</td>
      </tr>
    </tbody>
  </table>

  <hr>

  <h2>Software Environment</h2>
  <ul>
    <li>CUDA Toolkit 12.x</li>
    <li>Python 3.10</li>
    <li>MATLAB R2024a</li>
    <li>Docker & Singularity Support</li>
  </ul>

</div>

<style>
  .container {
    max-width: 1000px;
    margin: auto;
    padding: 20px;
  }

  h1, h2 {
    color: #1f2937;
  }

  table {
    border-collapse: collapse;
  }

  th {
    background-color: #f3f4f6;
  }

  th, td {
    text-align: left;
  }
</style>
