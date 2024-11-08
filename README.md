﻿# Tugas Praktikum Mobile - Pertemuan 8

**1. Halaman Tampil Mahasiswa**
   
![image](https://github.com/user-attachments/assets/fb8e52c7-297c-4c2b-8e1f-f52e826bd218)
![image](https://github.com/user-attachments/assets/1be8889b-c445-48cc-858a-3350b5114bec)

Pada halaman tampil data, proses yang terjadi dapat dijelaskan dalam beberapa tahap utama sebagai berikut: 

Ketika halaman tampil data dibuka, aplikasi Ionic memanggil fungsi getMahasiswa(). Fungsi ini bertanggung jawab mengirimkan permintaan (request) HTTP ke API tampil.php yang ada di backend PHP untuk mengambil data mahasiswa dari database. Fungsi ini menggunakan metode this.api.tampil() untuk memulai proses komunikasi dengan backend. 

Pada proses backend terjadi proses pengambilan data melalui file tampil.php menerima permintaan dari frontend dan menjalankan query SQL untuk mengambil seluruh data dari tabel mahasiswa. Data yang diambil dari database disimpan ke dalam array $data, yang menampung setiap baris hasil sebagai objek. 

Setelah semua data mahasiswa berhasil dimasukkan ke dalam array $data, PHP mengonversinya menjadi format JSON dengan json_encode($data). Data dalam format JSON ini dikirim sebagai respons balik ke frontend Ionic.

Ketika respons JSON diterima oleh fungsi getMahasiswa(), aplikasi menyimpannya dalam variabel dataMahasiswa. Variabel ini kemudian digunakan untuk menampilkan daftar mahasiswa pada halaman, sehingga pengguna dapat melihat data tersebut secara langsung di aplikasi. 

Jika terjadi masalah dalam pengambilan data—baik di sisi backend (misalnya query gagal) atau di sisi frontend (misalnya koneksi terputus)—error tersebut dicatat dalam console log untuk memudahkan proses debugging.

![image](https://github.com/user-attachments/assets/8a2cb487-ed61-462e-8006-d75c292fbf71)


**2. Halaman Tambah Data** 

![image](https://github.com/user-attachments/assets/cc2c725c-1c09-4d94-ba37-451277ba0002)
![image](https://github.com/user-attachments/assets/5926d8ee-1280-4181-8f2c-70756492f95b)

Pada halaman tambah data, prosesnya melibatkan pengumpulan data dari frontend, pengiriman ke backend untuk disimpan ke database, dan pembaruan tampilan data setelah penambahan. Berikut penjelasan dari setiap langkahnya:

Ketika pengguna memasukkan nama dan jurusan, fungsi tambahMahasiswa() di frontend memeriksa apakah kedua input tersebut tidak kosong. Jika validasi berhasil (nama dan jurusan sudah diisi), data tersebut disimpan dalam objek data, yang berisi nama dan jurusan.

Fungsi tambahMahasiswa() kemudian memanggil metode this.api.tambah(data, 'tambah.php') untuk mengirimkan data mahasiswa baru ini ke backend, yaitu file tambah.php. Permintaan ini menggunakan HTTP POST dengan data sebagai isinya, yang nantinya akan diproses oleh backend.

Pada file tambah.php, backend menerima data input melalui php://input, yang kemudian dikonversi menjadi array PHP menggunakan json_decode. Variabel $nama dan $jurusan kemudian diekstrak dari data yang diterima, dan whitespace dihilangkan dengan trim().

Setelah menerima data, backend memeriksa apakah $nama dan $jurusan tidak kosong. Jika data valid, backend menjalankan query SQL untuk menyisipkan data baru ke tabel mahasiswa menggunakan INSERT INTO. Jika berhasil, variabel $pesan diatur ke true, yang menandakan bahwa penambahan data berhasil. Jika ada data kosong, $pesan diatur ke false.

Hasil dari operasi penyimpanan ini (berhasil atau tidak) dikirim kembali ke frontend dalam format JSON. Di sisi frontend, jika operasi berhasil (hasil: any menunjukkan nilai true), aplikasi menutup modal, memanggil this.getMahasiswa() untuk memperbarui tampilan daftar mahasiswa, dan mencetak pesan keberhasilan ke console log. Jika gagal, pesan kesalahan dicetak ke console.

Penanganan Error dan Pesan Kosong: Jika terjadi kesalahan dalam pengiriman data atau ada data kosong saat penambahan, fungsi akan mencetak pesan ke console log untuk memudahkan dalam proses debugging dan menginformasikan bahwa proses penambahan tidak berhasil.

**3. Halaman Edit Data**

![image](https://github.com/user-attachments/assets/189cbd39-2557-452f-b43e-bf6d45aeda39)
![image](https://github.com/user-attachments/assets/e6866d93-1ad8-4ef5-ab4a-0d5154f9a42d)

Pada halaman edit data, prosesnya melibatkan pengiriman data yang telah diperbarui dari frontend ke backend untuk memperbarui data di database. Berikut penjelasan lengkap prosesnya:

Pada frontend, setelah pengguna memasukkan data baru (nama dan jurusan) yang ingin diperbarui untuk mahasiswa tertentu, aplikasi mengumpulkan data tersebut dalam bentuk objek yang berisi ID mahasiswa, nama baru, dan jurusan baru. Data ini kemudian dikirim ke API edit.php di backend untuk diproses lebih lanjut.

Pada edit.php, backend menerima data yang dikirim dari frontend melalui php://input. Data tersebut dikonversi menjadi array PHP menggunakan json_decode, dan variabel $id, $nama, dan $jurusan diekstrak dari array. Setiap variabel di-trim untuk menghilangkan whitespace di awal dan akhir data.

Setelah data diterima, backend memeriksa apakah $nama dan $jurusan tidak kosong. Jika validasi berhasil, backend menjalankan query SQL UPDATE untuk memperbarui data mahasiswa dengan ID yang sesuai (id='$id'). Data nama dan jurusan pada mahasiswa tersebut akan diperbarui di tabel mahasiswa. Jika pembaruan berhasil, variabel $pesan diatur ke true sebagai indikasi keberhasilan; jika tidak, $pesan diatur ke false.

Setelah operasi pembaruan, backend mengirimkan hasilnya kembali ke frontend dalam format JSON. Jika operasi berhasil ($pesan bernilai true), frontend akan menerima respons sukses yang kemudian ditindaklanjuti dengan memperbarui tampilan data di aplikasi Ionic agar perubahan terbaru terlihat oleh pengguna. Jika pembaruan gagal, pesan error dicetak di console log untuk membantu dalam proses debugging.

Jika terjadi kesalahan dalam proses query (misalnya kesalahan sintaks atau koneksi database), pesan error tersebut dicetak di backend menggunakan mysqli_error($koneksi). Informasi ini membantu dalam proses debugging, meskipun umumnya tidak ditampilkan kepada pengguna.

![image](https://github.com/user-attachments/assets/8c3ebe96-f64e-45bd-b2e5-bbad97506ea4)
![image](https://github.com/user-attachments/assets/98cf2ae1-5beb-457d-ba50-ad79eb48e3c8)

**4. Proses Hapus Data**

![image](https://github.com/user-attachments/assets/43d79e61-a60f-4b7b-91b1-547729223c97)
![image](https://github.com/user-attachments/assets/562bee44-282a-4c44-97a5-875c6d3c7c2f)
![image](https://github.com/user-attachments/assets/fa23b999-09b1-4126-a911-4a6b56fe9532)

Pada proses hapus data mahasiswa, frontend dan backend bekerja sama untuk memastikan data dihapus dari database sesuai permintaan pengguna. Berikut adalah alur proses lengkapnya:

Pada frontend, fungsi confirmHapusMahasiswa(id) menampilkan dialog konfirmasi menggunakan alertController. Dialog ini bertanya kepada pengguna apakah mereka yakin ingin menghapus data mahasiswa. Jika pengguna memilih "Hapus", maka fungsi hapusMahasiswa(id) akan dipanggil dengan ID mahasiswa yang ingin dihapus. Jika pengguna memilih "Batal", pesan pembatalan dicetak di console log, dan proses penghapusan dihentikan.

Jika pengguna mengonfirmasi penghapusan, fungsi hapusMahasiswa(id) dipanggil. Fungsi ini menggunakan this.api.hapus(id, 'hapus.php?id=') untuk mengirim permintaan HTTP DELETE ke backend, yaitu ke file hapus.php. ID mahasiswa yang ingin dihapus ditambahkan sebagai parameter dalam URL (hapus.php?id=<id_mahasiswa>).

Pada backend, file hapus.php menerima permintaan hapus dan mengambil ID mahasiswa dari parameter URL $_GET['id']. Kemudian, query SQL DELETE dijalankan untuk menghapus mahasiswa yang memiliki ID sesuai dengan yang diberikan dari tabel mahasiswa.

a. Jika query berhasil, server mengatur kode respons HTTP menjadi 201 (Created) dan menyimpan status sukses dalam array $pesan.

b. Jika query gagal, server mengatur kode respons HTTP menjadi 422 (Unprocessable Entity) dan menyimpan status gagal dalam $pesan.

c. Hasil dari operasi ini dikonversi ke JSON dan dikirim kembali ke frontend. Jika terjadi kesalahan dalam query, pesan kesalahan dicetak oleh mysqli_error($koneksi) untuk tujuan debugging.


Pada frontend, jika operasi penghapusan berhasil (res menunjukkan status sukses), maka:

a. Fungsi getMahasiswa() dipanggil untuk memperbarui daftar mahasiswa sehingga data yang sudah dihapus tidak lagi terlihat di antarmuka aplikasi.

b. Pesan keberhasilan dicetak di console log.

Jika penghapusan gagal, pesan kegagalan dicetak di console log, memberi tahu pengembang atau pengguna bahwa operasi tidak berhasil.

![image](https://github.com/user-attachments/assets/fe732616-3fb3-42c8-9d98-ed1515dc2c3e)
![image](https://github.com/user-attachments/assets/a2b8852b-33ba-4577-ae7b-2e9c187935b0)
