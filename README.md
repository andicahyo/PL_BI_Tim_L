# PL_BI_Tim_L
Project Lab BI untuk dashboard Kesehatan Ibu dan Anak

# **Dashboard Kesehatan Ibu dan Anak**

## **1. Background**
Kesehatan ibu dan anak menjadi isu penting di Provinsi Jawa Barat dimana ditemukan beberapa kasus mengenai kesehatan bayi dan balita di Provinsi Jawa barat ini cenderung rendah. Partisipasi keikutsertaan ibu hamil untuk melakukan pengecekan kesehatan ke posyandu dalam kategori rendah juga. Pihak penentu kebijakan dalam hal ini pemerintah Provinsi Jawa Barat mengalami kesulitan karena terdapat juga data yang masih belum terhubung satu sama lain.

## **2. Objective**
## **2.1 Project Objective**
Dengan adanya dashboard yang telah dibuat ini, harapannya adalah dapat membantu pencatatan kesehatan Ibu dan Anak kemudian dapat menyajikan Informasi kesehatan Ibu dan anak di wilayah Provinsi Jawa Barat khususnya Kabupaten/kota dan kedepannya bisa sampai ke daerah kecamatan dan kelurahan dengan bantuan digitalisasi pencatatan kesehatan Ibu dan Anak.

## **2.2 User Persona**

### Petugas Posyandu
(write/edit access):
Who are they?
1. Perempuan.
2. Usia 19 tahun ke atas.
3. Petugas posyandu.
4. Memiliki HP smartphone ram 2/24 ke atas.

What do they do?
Mencatat data kesehatan ibu dan anak anggota Posyandu di wilayah RT/RW lokasi kerja.
Melaporkan data kesehatan ibu dan anak kepada Puskesmas.
Menyelenggarakan acara penyuluhan dengan topik mengenai kesehatan ibu, bayi, serta balita.
Melakukan pembinaan kesehatan terhadap ibu, bayi, serta balita.

What do they need?
Mengetahui status kesehatan ibu dan anak anggota Posyandu di wilayah RT/RW lokasi kerja.
Mengetahui target partisipan dalam acara yang akan diselenggarakan oleh Posyandu.


### Puskesmas
(write/edit access):
Who are they?
1. Laki / Perempuan.
2. Usia 19 tahun ke atas.
3. Petugas Puskesmas.
4. Memiliki HP smartphone ram 2/24 ke atas.

What do they do?
Memberikan pelayanan kesehatan secara menyeluruh kepada bayi dan ibu hamil.
Tenaga medis untuk pemberian imunisasi pada bayi.

What do they need?
Data status kesehatan bayi dan ibu hamil di wilayah sekitar Puskesmas.
Data mengenai isu kesehatan bayi dan ibu hamil yang banyak dijumpai di wilayah sekitar Puskesmas.

### Masyarakat Umum
(view only):
Who are they?
1. Laki / Perempuan
2. Tidak / sudah memiliki anak.
3. Tidak / merupakan anggota posyandu.
4. Usia 19 tahun ke atas.

What do they do?
Ingin mengetahui data kesehatan ibu dan anak yang tercatat oleh Posyandu di daerah Jawa Barat.

What do they need?
Ingin mengetahui data kesehatan ibu dan anak yang tercatat oleh Posyandu di daerah Jawa Barat.


## 3. Overview Process
### 3.1 Proses Pengerjaan Projek
  * Explore dataset yang telah disajikan
  * Mendiskusikan dataset yang ada serta mencoba mengatasi masalah yang ditimbulkan dari dataset tersebut
  * Dari penentuan masalah, selanjutnya menentukan persona yang akan menggunakan dashboard nantinya
  * Membuat user flow serta Hi-Fi mockup
  * Pengolahan dataset dengan menggunakan pemrograman python
  * Dataset ready dan visualisasi menggunakan Tableau Public.

### 3.1.1 EDA + Wrangling data
```python
# DROP KOLOM YANG TIDAK DIBUTUHKAN DARI DATASET
gizi_kurang.drop(['kode_provinsi', 'nama_provinsi', 'satuan'], axis=1, inplace=True)
gizi_buruk.drop(['kode_provinsi', 'nama_provinsi', 'satuan'], axis=1, inplace=True)
status_kelahiran.drop(['kode_provinsi', 'nama_provinsi', 'satuan'], axis=1, inplace=True)

# AMBIL TAHUN >=2017 DARI DATASET 
gizi_buruk = gizi_buruk[gizi_buruk['tahun'] >=2017]
status_kelahiran = status_kelahiran[status_kelahiran['status_kelahiran'] == 'HIDUP']
status_kelahiran = status_kelahiran[status_kelahiran['tahun'] >=2017]

# LAKUKAN GROUPING DATA UNTUK MENENTUKAN JUMLAH DATA KELAHIRAN
gizi_kurang = gizi_kurang.groupby(['tahun', 'kode_kabupaten_kota','nama_kabupaten_kota'])['jumlah_balita'].sum().reset_index()
gizi_buruk = gizi_buruk.groupby(['tahun', 'kode_kabupaten_kota','nama_kabupaten_kota'])['jumlah_bayi'].sum().reset_index()
status_kelahiran = status_kelahiran.groupby(['tahun', 'kode_kabupaten_kota','nama_kabupaten_kota'])['jumlah_kelahiran'].sum().reset_index()

# MERGE FINAL DATA DIATAS
final_data = pd.merge(status_kelahiran, data_gizi, how='outer', on =['kode_kabupaten_kota', 'nama_kabupaten_kota', 'tahun'])
final_data['tahun']= pd.to_datetime(final_data['tahun'], format= '%Y')

final_data.index.name = "nomor"
```
  Full wrangling data bisa dilihat di dalam folder 'notebook'

### 3.2 User Flow
  User Flow project ini dibagi dalam 2 kelompok berdasarkan aplikasi yang digunakan. Kelompok pertama, yaitu penggunaan Dashboard untuk melihat data kesehatan melalui Tableau. Kelompok kedua, yaitu proses pengumpulan data kesehatan Ibu dan Anak melalui aplikasi Google Form.
  Berdasarkan user persona yang telah dibuat, terdapat 2 tipe pengguna dashboard. Tipe user pertama adalah masyarakat umum yang ingin mengetahui gambaran informasi kesehatan Ibu dan Anak di wilayah Provinsi Jawa Barat. Tipe user kedua merupakan Petugas Posyandu ataupun Petugas Puskesmas yang menggunakan dashboard untuk menyusun program penyuluhan kesehatan yang tepat sasaran berdasarkan isu di wilayah Kabupaten/Kota tersebut.

### 3.2.1 User Flow Tableau 
  Secara garis besar, tipe pengguna dibagi ke dalam dua kelompok, yaitu masyarakat umum yang ingin mengetahui 
 status kesehatan dan kelompok kedua merupakan petugas kesehatan yang ingin membuat rencana kegiatan penyuluhan.
 
 A. Alur Penggunaan Tableau dashboard untuk masyarakat umum
![](https://cdn.discordapp.com/attachments/1013003936212459561/1018065561470648340/FLOW_01.png)

B. Alur Penggunaan untuk menyebarkan undangan penyuluhan
![](https://cdn.discordapp.com/attachments/1013003936212459561/1018065562053644328/FLOW_03.png)

C. Alur untuk alokasi sumber daya oleh Puskesmas
![](https://cdn.discordapp.com/attachments/1013003936212459561/1018065562288521236/FLOW_04.png)


### 3.2.2 User flow google form
User Flow berikut menggambarkan proses pengumpulan data yang dilakukan oleh Petugas Posyandu melalui aplikasi Google Form. 
Data yang telah terkumpul akan diolah oleh tim Data untuk memperbaharui informasi yang disajikan pada Dashboard Kesehatan Ibu dan Anak.
![](https://cdn.discordapp.com/attachments/1013003936212459561/1018065561734885386/FLOW_02.png)
## 4.1 Dashboard Kesehatan Ibu dan Anak - MVP Demonstration
 * Control Panel: Panel kendali untuk mengatur rentang variabel pada dashboard
 * Geomaps: Menampilkan peta kabupaten/kota pada provinsi Jawa barat beserta informasi kesehatan pada tooltipsnya
 * Bar dan Line chart: untuk menyajikan grafik pada variabel tertentu.

Berikut tampilan dashboard yang telah dibuat:

![](https://cdn.discordapp.com/attachments/712435030781067264/1018459729065947186/unknown.png)

## 4.2. Form Kesehatan Ibu dan Anak - Application Demonstration
Melihat adanya kekosongan data mengenai informasi kesehatan yang bersumber dari publikasi pemerintahan, kelompok kami merancang sebuah aplikasi ringkas untuk mempermudah proses pengumpulan data informasi kesehatan Ibu dan Anak.
Untuk mengatasi hal tersebut, kami merancang aplikasi berbasis Google Form dengan keunggulan: (1) desain ringan, mudah diakses, dan hemat pemakaian kuota data, (2) data dalam formulir aplikasi tersebut dapat diatur berdasarkan kebutuhan, (3) data disimpan dalam cloud server secara otomatis, (4) data formulir langsung tersimpan dalam bentuk excel, dan (5) data sharing dapat dilakukan dengan mudah melalui Google Drive.

![FLOW_GFORM](https://user-images.githubusercontent.com/102814373/189521890-1a8777ac-58d0-4c4e-aee2-9a254b44d950.png)

![recap_gform](https://user-images.githubusercontent.com/102814373/189521909-69e272c5-e06c-4553-af24-38c1fa92b18f.png)


### Referense 
- Brainstorming board: [miro](https://miro.com/app/board/uXjVPceKE1w=/?share_link_id=9592509117)
- MVP - Dashboard: [tableau](https://public.tableau.com/app/profile/andi.cahyono/viz/DashboardKesehatanIbudanAnak/MVP_Dashboard#1)
