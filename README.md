<div align="center">
  <img src="https://img.shields.io/badge/Versi-1.0.0-blue?style=for-the-badge&logo=arduino" />
  <img src="https://img.shields.io/badge/Chip-ESP8266-black?style=for-the-badge&logo=espressif" />
  <img src="https://img.shields.io/badge/Protocol-MQTT-green?style=for-the-badge&logo=mosquitto" />
  <br><br>
  <img src="https://img.shields.io/badge/💡_Smart_Lamp_Control-4CH_RELAY-blue" />
</div>

<h1 align="center">🏠 SMART HOME - Lampu 4 Channel</h1>
<p align="center">
  <b>Kontrol Lampu Rumah via MQTT & Web Dashboard</b><br>
  <i>Saklar pintar berbasis Wemos D1 Mini dengan relay 4 channel, scheduler, timer, dan kontrol jarak jauh melalui internet.</i>
</p>

<hr>

<h2>🎯 Fitur Utama</h2>
<ul>
  <li><b>Kontrol 4 Lampu:</b> On/Off individual maupun semua lampu sekaligus (All On/All Off).</li>
  <li><b>Kontrol Jarak Jauh (IoT):</b> Terhubung via protokol <b>MQTT</b> ke broker cloud (<code>broker.emqx.io</code>), sehingga bisa dikontrol dari mana saja melalui internet.</li>
  <li><b>Web Dashboard Eksternal:</b> Antarmuka web modern yang di-host secara terpisah, terhubung ke perangkat melalui topic MQTT.</li>
  <li><b>Penjadwalan (Scheduler):</b> 10 slot jadwal otomatis untuk menyalakan/mematikan lampu berdasarkan hari dan jam (membutuhkan koneksi internet untuk NTP).</li>
  <li><b>Timer Otomatis:</b> Fitur countdown timer untuk mematikan semua lampu secara otomatis setelah waktu tertentu.</li>
  <li><b>Nama Lampu Custom:</b> Setiap relay bisa diberi nama custom (contoh: "Lampu Teras", "Lampu Kamar") yang tersimpan di EEPROM.</li>
  <li><b>Penyimpanan Status:</b> Status terakhir lampu (ON/OFF) tersimpan di EEPROM. Saat listrik padam dan hidup kembali, lampu akan kembali ke posisi terakhir.</li>
  <li><b>Captive Portal Setup:</b> Konfigurasi WiFi langsung dari HP tanpa perlu kabel USB. Cukup hubungkan ke WiFi <b>SMART HOME</b> dan buka browser.</li>
  <li><b>OTA Update:</b> Update firmware langsung dari browser tanpa kabel USB.</li>
  <li><b>Reset Pabrik:</b> Fitur reset ke pengaturan awal tanpa membongkar perangkat (membuat hotspot <code>SETELAN_AWAL</code> dari HP).</li>
  <li><b>Keamanan MAC Address:</b> Firmware terkunci ke MAC address tertentu. Jika MAC tidak cocok, perangkat akan berkedip terus-menerus.</li>
</ul>

<hr>

<h2>🛠️ Spesifikasi Hardware & Pinout</h2>
<p>⚠️ <b>PERINGATAN:</b> Pastikan pengkabelan sesuai dengan konfigurasi di bawah ini. Relay menggunakan tipe <b>LOW TRIGGER (Active LOW)</b>.</p>

<h3>1. Wemos D1 Mini → Relay 4CH (Low Trigger)</h3>
<table>
  <tr>
    <th>Wemos D1 Mini</th>
    <th style="text-align: center;">Relay 4CH</th>
    <th>Keterangan</th>
  </tr>
  <tr><td><b>5V (VCC)</b></td><td style="text-align: center;"><b>VCC</b></td><td>Cataya Daya Relay</td></tr>
  <tr><td><b>GND</b></td><td style="text-align: center;"><b>GND</b></td><td>Ground Bersama</td></tr>
  <tr><td><b>D4 (GPIO2)</b></td><td style="text-align: center;"><b>IN1</b></td><td>Relay Lampu 1</td></tr>
  <tr><td><b>D3 (GPIO0)</b></td><td style="text-align: center;"><b>IN2</b></td><td>Relay Lampu 2</td></tr>
  <tr><td><b>D2 (GPIO4)</b></td><td style="text-align: center;"><b>IN3</b></td><td>Relay Lampu 3</td></tr>
  <tr><td><b>D1 (GPIO5)</b></td><td style="text-align: center;"><b>IN4</b></td><td>Relay Lampu 4</td></tr>
</table>

<h3>2. Logika Relay (LOW TRIGGER)</h3>
<table>
  <tr>
    <th>Kondisi Pin</th>
    <th style="text-align: center;">Status Relay</th>
    <th style="text-align: center;">Status Lampu</th>
  </tr>
  <tr><td><code>LOW</code> (0V)</td><td style="text-align: center;">✅ ON</td><td style="text-align: center;">💡 NYALA</td></tr>
  <tr><td><code>HIGH</code> (3.3V)</td><td style="text-align: center;">❌ OFF</td><td style="text-align: center;">🌙 MATI</td></tr>
</table>
<p><i>* Saat booting/startup, Wemos otomatis set semua pin <code>HIGH</code> agar lampu tidak flicker.</i></p>

<h3>3. Skema Koneksi AC 220V</h3>
<table>
  <tr>
    <th>Sumber</th>
    <th style="text-align: center;">Tujuan</th>
  </tr>
  <tr><td>AC 220V (Fase)</td><td style="text-align: center;">→ Terminal <b>COM</b> Relay</td></tr>
  <tr><td>Terminal <b>NO</b> Relay</td><td style="text-align: center;">→ Lampu (Fase)</td></tr>
  <tr><td>AC 220V (Netral)</td><td style="text-align: center;">→ Lampu (Netral)</td></tr>
  <tr><td>Terminal <b>NC</b></td><td style="text-align: center;">→ <i>Dibiarkan Kosong</i></td></tr>
</table>

<div align="center">
  <table>
    <tr>
      <td>
        <img src="https://img.shields.io/badge/⚠️_PERINGATAN_220V_AC-red?style=for-the-badge" />
      </td>
    </tr>
  </table>
  <p><b>Pastikan sambungan AC 220V terisolasi rapat dengan selotip/kabel tub!</b><br>
  Jangan sentuh bagian relay saat terhubung ke listrik AC!<br>
  Gunakan <b>power supply 5V minimal 2A</b> jika 4 lampu dinyalakan bersamaan untuk menghindari drop voltage yang menyebabkan Wemos reset.</p>
</div>



<h2>📦 Cara Menggunakan</h2>

<h3>1. Konfigurasi WiFi Pertama Kali</h3>
<ol>
  <li>Nyalakan perangkat (hubungkan ke power supply 5V).</li>
  <li>Buka WiFi di HP, cari SSID: <b>SMART HOME</b> (tanpa password).</li>
  <li>Hubungkan, lalu buka browser (halaman setup akan muncul otomatis via Captive Portal).</li>
  <li>Jika tidak muncul, ketik manual: <b>http://192.168.1.1</b></li>
  <li>Pergi ke tab <b>SETTING</b>, isi <b>WiFi SSID</b> dan <b>WiFi Password</b> rumah Anda.</li>
  <li>Klik <b>SIMPAN & RESTART</b>.</li>
  <li>Perangkat akan restart dan otomatis terhubung ke WiFi rumah Anda.</li>
</ol>

<h3>2. Mengakses Web Dashboard</h3>
<ol>
  <li>Pastikan perangkat sudah terhubung ke WiFi rumah (mode Online).</li>
  <li>Buka browser di HP/Laptop yang terhubung ke WiFi yang sama.</li>
  <li>Ketik IP address Wemos (terlihat di Serial Monitor), atau buka langsung:</li>
</ol>

<h3>3. Menambahkan ke Multi Device Dashboard</h3>
<ol>
  <li>Buka halaman setup perangkat: <code>http://[IP_WEMOS]/setup</code></li>
  <li>Pergi ke tab <b>UPDATE</b>.</li>
  <li>Copy <b>MQTT TOPIC DEVICE</b> yang ditampilkan.</li>
  <li>Buka <b>Multi Device Dashboard</b> dan tambahkan topic tersebut.</li>
</ol>

<h3>4. Reset ke Pengaturan Pabrik</h3>
<ol>
  <li>Buka pengaturan Hotspot di HP Anda.</li>
  <li>Buat Hotspot baru dengan nama SSID: <b>SETELAN_AWAL</b> (tanpa password).</li>
  <li>Letakkan HP dekat perangkat (maks ~1 meter).</li>
  <li>Tunggu maksimal <b>30 detik</b> — perangkat akan otomatis mendeteksi dan mereset ke pabrik.</li>
  <li>Setelah reset, perangkat akan restart dalam mode AP (<b>SMART HOME</b>).</li>
  <li>Matikan hotspot <code>SETELAN_AWAL</code> di HP Anda setelah proses selesai.</li>
</ol>

<h3>5. Update Firmware via OTA</h3>
<ol>
  <li>Pastikan perangkat dalam mode AP (tidak terhubung WiFi rumah).</li>
  <li>Hubungkan ke WiFi <b>SMART HOME</b>, buka <code>http://192.168.1.1</code>.</li>
  <li>Pergi ke tab <b>UPDATE</b>.</li>
  <li>Pilih file <code>.bin</code> firmware baru, lalu klik <b>MULAI UPDATE</b>.</li>
  <li>Tunggu hingga selesai. Perangkat akan restart otomatis.</li>
</ol>



<h2>🔍 Troubleshooting</h2>
<table>
  <tr><th>Masalah</th><th>Solusi</th></tr>
  <tr><td>LED built-in berkedip terus</td><td>MAC address tidak cocok. Cek dan sesuaikan <code>allowedMAC</code></td></tr>
  <tr><td>Lampu flicker saat booting</td><td>Normal untuk sesaat. Pin di-set HIGH segera setelah <code>setup()</code></td></tr>
  <tr><td>Wemos sering reset saat 4 lampu nyala</td><td>Gunakan power supply 5V minimal 2A. Jangan gunakan catu daya dari USB laptop</td></tr>
  <tr><td>Tidak bisa connect WiFi rumah</td><td>Pastikan SSID & password benar. WiFi hanya mendukung <b>2.4 GHz</b></td></tr>
  <tr><td>Captive Portal tidak muncul</td><td>Ketik manual <code>http://192.168.1.1</code> di browser</td></tr>
  <tr><td>MQTT tidak terhubung</td><td>Pastikan WiFi terhubung. Cek koneksi internet. Broker <code>broker.emqx.io</code> membutuhkan koneksi internet</td></tr>
  <tr><td>Schedule tidak berjalan</td><td>Butuh koneksi internet untuk NTP (sinkronisasi waktu). Cek <code>timeClient.isTimeSet()</code></td></tr>
  <tr><td>Reset pabrik tidak bekerja</td><td>Pastikan HP cukup dekat (&lt;1m), hotspot bernama persis <code>SETELAN_AWAL</code>, dan tunggu hingga 30 detik</td></tr>
  <tr><td>OTA update gagal</td><td>Pastikan file yang di-upload berekstensi <code>.bin</code> dan ukurannya tidak melebihi kapasitas flash</td></tr>
</table>

<hr>

<h2>📸 Pratinjau Tampilan (Gallery)</h2>

<table align="center">
  <tr>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/AtherNet29/ESP8266-SMART-HOME-ONLINE-OFFLINE-JARAK-JAUH-/ec2b1cd3a4ebd7c6aaac5d9eced1b6d4a86e9f0d/GAMBAR%201.jpg" width="300" alt="Gambar 1" /><br>
      <sub><b>Produk Smart Home 4CH</b></sub>
    </td>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/AtherNet29/ESP8266-SMART-HOME-ONLINE-OFFLINE-JARAK-JAUH-/ec2b1cd3a4ebd7c6aaac5d9eced1b6d4a86e9f0d/GAMBAR%202.jpg" width="300" alt="Gambar 2" /><br>
      <sub><b>Detail Modul Wemos & Relay</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/AtherNet29/ESP8266-SMART-HOME-ONLINE-OFFLINE-JARAK-JAUH-/ec2b1cd3a4ebd7c6aaac5d9eced1b6d4a86e9f0d/GAMBAR%203.jpg" width="300" alt="Gambar 3" /><br>
      <sub><b>Skema Koneksi Kabel</b></sub>
    </td>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/AtherNet29/ESP8266-SMART-HOME-ONLINE-OFFLINE-JARAK-JAUH-/ec2b1cd3a4ebd7c6aaac5d9eced1b6d4a86e9f0d/GAMBAR%204.jpg" width="300" alt="Gambar 4" /><br>
      <sub><b>Proses Pemasangan</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/AtherNet29/ESP8266-SMART-HOME-ONLINE-OFFLINE-JARAK-JAUH-/ec2b1cd3a4ebd7c6aaac5d9eced1b6d4a86e9f0d/GAMBAR%205.jpg" width="300" alt="Gambar 5" /><br>
      <sub><b>Halaman Captive Portal (Offline)</b></sub>
    </td>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/AtherNet29/ESP8266-SMART-HOME-ONLINE-OFFLINE-JARAK-JAUH-/ec2b1cd3a4ebd7c6aaac5d9eced1b6d4a86e9f0d/GAMBAR%206.jpg" width="300" alt="Gambar 6" /><br>
      <sub><b>Halaman Setting WiFi</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/AtherNet29/ESP8266-SMART-HOME-ONLINE-OFFLINE-JARAK-JAUH-/ec2b1cd3a4ebd7c6aaac5d9eced1b6d4a86e9f0d/GAMBAR%207.jpg" width="300" alt="Gambar 7" /><br>
      <sub><b>Web Dashboard Live Control</b></sub>
    </td>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/AtherNet29/ESP8266-SMART-HOME-ONLINE-OFFLINE-JARAK-JAUH-/ec2b1cd3a4ebd7c6aaac5d9eced1b6d4a86e9f0d/GAMBAR%208.jpg" width="300" alt="Gambar 8" /><br>
      <sub><b>Multi Device Dashboard</b></sub>
    </td>
  </tr>
</table>

<hr>

<div align="center">
  <h2>💎 Dapatkan FIRMWARE Alat Ini</h2>
  <p>Hubungi saya atau lakukan pemesanan langsung melalui Telegram untuk mendapatkan firmware full version dan bantuan teknis. Setiap pembelian mendapatkan 1 file BIN yang tidak bisa di gandakan dan Gratis Update firmware 2 x</p>
  <h3>🛒 Harga: RP 50.000</h3>
  <br>
  <a href="https://t.me/+6283141852690">
  <img src="https://img.shields.io/badge/Buy_Now-Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white" />
  </a>
  <br><br>
  <i>Klik tombol di atas untuk chat langsung dengan saya di Telegram.</i>
</div>

<hr>
<div align="center">
  <br>
  <sub>Dibuat dengan ❤️ untuk Smart Home Indonesia</sub>
</div>
