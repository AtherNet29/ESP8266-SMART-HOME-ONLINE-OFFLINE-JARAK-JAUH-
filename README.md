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

<hr>

<h2>📦 Konfigurasi MQTT</h2>

<h3>Broker & Port</h3>
<table>
  <tr><th>Parameter</th><th>Nilai</th></tr>
  <tr><td>MQTT Broker</td><td><code>broker.emqx.io</code></td></tr>
  <tr><td>Port</td><td><code>1883</code></td></tr>
  <tr><td>Username</td><td><i>Tidak digunakan (kosong)</i></td></tr>
  <tr><td>Password</td><td><i>Tidak digunakan (kosong)</i></td></tr>
</table>

<h3>Topic Structure</h3>
<p>Berikut adalah struktur topic MQTT berdasarkan <code>BASE_TOPIC = "smarthome/device04"</code>:</p>
<table>
  <tr><th>Topic</th><th>Direction</th><th>Keterangan</th></tr>
  <tr><td><code>smarthome/device04/state</code></td><td>Publish (Retained)</td><td>Status 4 relay dalam format JSON</td></tr>
  <tr><td><code>smarthome/device04/names</code></td><td>Publish (Retained)</td><td>Nama custom 4 lampu dalam format JSON</td></tr>
  <tr><td><code>smarthome/device04/schedule</code></td><td>Publish (Retained)</td><td>Data jadwal aktif dalam format JSON Array</td></tr>
  <tr><td><code>smarthome/device04/cmd</code></td><td>Subscribe</td><td>Perintah kontrol relay & timer</td></tr>
  <tr><td><code>smarthome/device04/sch_cmd</code></td><td>Subscribe</td><td>Perintah tambah/hapus jadwal</td></tr>
  <tr><td><code>smarthome/device04/name_cmd</code></td><td>Subscribe</td><td>Perintah ubah nama lampu</td></tr>
</table>

<h3>Format Perintah (Topic: <code>.../cmd</code>)</h3>
<table>
  <tr><th>Perintah</th><th>Fungsi</th></tr>
  <tr><td><code>r1=1</code> / <code>r1=0</code></td><td>Nyalakan / Matikan Lampu 1</td></tr>
  <tr><td><code>r2=1</code> / <code>r2=0</code></td><td>Nyalakan / Matikan Lampu 2</td></tr>
  <tr><td><code>r3=1</code> / <code>r3=0</code></td><td>Nyalakan / Matikan Lampu 3</td></tr>
  <tr><td><code>r4=1</code> / <code>r4=0</code></td><td>Nyalakan / Matikan Lampu 4</td></tr>
  <tr><td><code>all=1</code> / <code>all=0</code></td><td>Nyalakan / Matikan Semua Lampu</td></tr>
  <tr><td><code>timer=30</code></td><td>Matikan semua lampu dalam 30 menit</td></tr>
</table>

<h3>Format Perintah Jadwal (Topic: <code>.../sch_cmd</code>)</h3>
<table>
  <tr><th>Perintah</th><th>Format</th><th>Keterangan</th></tr>
  <tr><td>Tambah Jadwal</td><td><code>add:relay:state:day:hour:minute</code></td><td>relay(0-4), state(0/1), day(0-6=Sen-Sab, 7=Setiap hari), hour(0-23), minute(0-59)</td></tr>
  <tr><td>Hapus Jadwal</td><td><code>del:id</code></td><td>id = index jadwal (0-9)</td></tr>
</table>
<p><i>Contoh: <code>add:1:1:7:06:00</code> → Nyalakan relay 1 setiap hari jam 06:00</i></p>

<h3>Format Perintah Nama (Topic: <code>.../name_cmd</code>)</h3>
<table>
  <tr><th>Perintah</th><th>Format</th><th>Keterangan</th></tr>
  <tr><td>Ubah Nama</td><td><code>set:id:nama_lampu</code></td><td>id(1-4), nama lampu (maks 31 karakter)</td></tr>
</table>
<p><i>Contoh: <code>set:1:Lampu Teras</code> → Mengubah nama relay 1 menjadi "Lampu Teras"</i></p>

<h3>Format Payload Publish</h3>

<p><b>State:</b></p>
<pre><code>{"s1":true,"s2":false,"s3":true,"s4":false}</code></pre>

<p><b>Names:</b></p>
<pre><code>{"n1":"Lampu Teras","n2":"Lampu Kamar","n3":"Lampu Dapur","n4":"Lampu Garasi"}</code></pre>

<p><b>Schedule:</b></p>
<pre><code>[{"id":0,"relay":1,"state":1,"day":7,"hour":6,"minute":0},{"id":1,"relay":1,"state":0,"day":7,"hour":22,"minute":0}]</code></pre>

<hr>

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
<div align="center">
  <code>https://potonasib20-glitch.github.io/smart-home/index.html?topic=smarthome/device04</code>
</div>
<p><i>* Ganti <code>smarthome/device04</code> sesuai BASE_TOPIC yang dikonfigurasi di kode.</i></p>

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

<hr>

<h2>💾 Peta Memori EEPROM</h2>
<table>
  <tr><th>Alamat</th><th>Isi</th><th>Keterangan</th></tr>
  <tr><td><code>0</code></td><td>Marker <code>'C'</code></td><td>Penanda config sudah tersimpan</td></tr>
  <tr><td><code>1 - 32</code></td><td>AP SSID</td><td>Nama hotspot perangkat (maks 32 char)</td></tr>
  <tr><td><code>33 - 96</code></td><td>AP Password</td><td>Password hotspot (maks 64 char)</td></tr>
  <tr><td><code>100 - 131</code></td><td>STA SSID</td><td>Nama WiFi rumah (maks 32 char)</td></tr>
  <tr><td><code>132 - 195</code></td><td>STA Password</td><td>Password WiFi rumah (maks 64 char)</td></tr>
  <tr><td><code>200 - 259</code></td><td>Schedules</td><td>10 slot jadwal × 6 byte each</td></tr>
  <tr><td><code>300 - 427</code></td><td>Names</td><td>4 nama lampu × 32 byte each</td></tr>
  <tr><td><code>450 - 453</code></td><td>States</td><td>Status ON/OFF 4 relay (1 byte each)</td></tr>
</table>

<hr>

<h2>⚙️ Konfigurasi di Kode</h2>
<p>Sebelum upload, sesuaikan parameter berikut di bagian atas kode:</p>

<pre><code>// Ganti dengan MAC address Wemos D1 Mini Anda (huruf kapital, pemisah ':')
const String allowedMAC = "94:B9:7E:0C:32:67";

// Ganti dengan topic unik untuk setiap perangkat
String BASE_TOPIC = "smarthome/device04";

// Nama hotspot AP saat belum ada konfigurasi WiFi
String AP_SSID = "SMART HOME";

// Broker MQTT (default: broker.emqx.io)
String MQTT_SERVER = "broker.emqx.io";
int MQTT_PORT = 1883;</code></pre>

<h3>Cara Mengetahui MAC Address Wemos D1 Mini</h3>
<ol>
  <li>Upload kode kosong berikut ke Wemos:</li>
</ol>
<pre><code>void setup() {
  Serial.begin(115200);
  delay(1000);
  uint8_t mac[6];
  WiFi.macAddress(mac);
  Serial.printf("MAC: %02X:%02X:%02X:%02X:%02X:%02X\n", mac[0], mac[1], mac[2], mac[3], mac[4], mac[5]);
}
void loop() {}</code></pre>
<ol start="2">
  <li>Buka <b>Serial Monitor</b> (Baudrate: 115200).</li>
  <li>Copy MAC address yang ditampilkan, lalu paste ke <code>allowedMAC</code> di kode utama.</li>
</ol>

<hr>

<h2>📚 Library yang Dibutuhkan</h2>
<p>Install melalui <b>Library Manager</b> di Arduino IDE:</p>
<table>
  <tr><th>Library</th><th>Versi yang Digunakan</th></tr>
  <tr><td><code>ESP8266WiFi</code></td><td>Bawaan ESP8266 Board Package</td></tr>
  <tr><td><code>ESP8266WebServer</code></td><td>Bawaan ESP8266 Board Package</td></tr>
  <tr><td><code>DNSServer</code></td><td>Bawaan ESP8266 Board Package</td></tr>
  <tr><td><code>EEPROM</code></td><td>Bawaan ESP8266 Board Package</td></tr>
  <tr><td><code>PubSubClient</code></td><td>Nick O'Leary (v2.8+)</td></tr>
  <tr><td><code>NTPClient</code></td><td>Fabrice Weinberg (v3.2+)</td></tr>
</table>

<h3>Board Settings (Arduino IDE)</h3>
<table>
  <tr><th>Setting</th><th>Nilai</th></tr>
  <tr><td>Board</td><td><code>LOLIN(WEMOS) D1 R2 & mini</code></td></tr>
  <tr><td>Upload Speed</td><td><code>921600</code></td></tr>
  <tr><td>CPU Frequency</td><td><code>80 MHz</code></td></tr>
  <tr><td>Flash Size</td><td><code>4MB (1M SPIFFS)</code> atau lebih</td></tr>
  <tr><td>Flash Mode</td><td><code>DIO</code></td></tr>
  <tr><td>Flash Frequency</td><td><code>40MHz</code></td></tr>
</table>

<hr>

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
      <img src="https://raw.githubusercontent.com/USERNAME/REPO-NAMA/BRANCH/Gambar%201.jpg" width="300" alt="Gambar 1" /><br>
      <sub><b>Produk Smart Lamp</b></sub>
    </td>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/USERNAME/REPO-NAMA/BRANCH/Gambar%202.jpg" width="300" alt="Gambar 2" /><br>
      <sub><b>Detail Modul Wemos & Relay</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/USERNAME/REPO-NAMA/BRANCH/Gambar%203.jpg" width="300" alt="Gambar 3" /><br>
      <sub><b>Koneksi Kabel AC 220V</b></sub>
    </td>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/USERNAME/REPO-NAMA/BRANCH/Halaman%20Setup.jpg" width="300" alt="Halaman Setup" /><br>
      <sub><b>Halaman Setup Captive Portal</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/USERNAME/REPO-NAMA/BRANCH/Halaman%20Dashboard.jpg" width="300" alt="Halaman Dashboard" /><br>
      <sub><b>Web Dashboard Live Control</b></sub>
    </td>
    <td align="center" width="50%">
      <img src="https://raw.githubusercontent.com/USERNAME/REPO-NAMA/BRANCH/schematic.jpg" width="300" alt="Schematic" /><br>
      <sub><b>Diagram Skematik</b></sub>
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
