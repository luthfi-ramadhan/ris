---
layout: default
title: Formulir
permalink: /form/
---
<div class="form-container">
  <h2>Formulir Pendaftaran</h2>

  <!-- Ganti action dengan endpoint Formspree -->
  <form action="https://formspree.io/f/YOUR_ID" method="POST">

    <label>Nama Lengkap</label>
    <input type="text" name="nama" required>

    <label>Email</label>
    <input type="email" name="email" required>

    <label>Nomor HP</label>
    <input type="tel" name="no_hp">

    <label>Keperluan</label>
    <select name="keperluan">
      <option value="konsultasi">Konsultasi</option>
      <option value="pendaftaran">Pendaftaran</option>
      <option value="lainnya">Lainnya</option>
    </select>

    <label>Pesan</label>
    <textarea name="pesan" rows="4"></textarea>

    <button type="submit">Kirim</button>

  </form>
</div>
