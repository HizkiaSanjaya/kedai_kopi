https://hizkiasanjaya.github.io/kedai_kopi/

# Kedai Kopi Senja

Hasil Coding 2 pekan 3. Isinya masih tiga halaman dari pekan 2, sekarang
ditambah satu berkas `css/style.css` yang dipakai bersama. Tidak ada atribut
`style` dan tidak ada tag `<style>` di HTML, seluruh tampilan diatur dari
berkas CSS itu.

```
kedai_kopi/
├─ index.html
├─ menu.html
├─ pesan.html
├─ css/
│  └─ style.css
├─ img/
└─ README.md
```

## Bagian yang memakai Flexbox dan Grid

Header saya susun dengan Flexbox. Isinya hanya dua kelompok, judul kedai dan
daftar link navigasi, dan keduanya memang cukup berbaris pada satu sumbu
mendatar. Dengan `display: flex` lalu `justify-content: space-between`, judul
menempel ke kiri dan nav ke kanan tanpa perlu menghitung lebar atau memakai
`float`. Saya tambahkan `align-items: baseline` supaya dasar huruf judul dan
link tetap rata walaupun ukuran fontnya berbeda. Flexbox baru aktif lewat
`@media (min-width: 768px)`, jadi di ponsel header masih memakai alur normal
dan judul serta nav tersusun ke bawah.

Daftar menu di `menu.html` saya susun dengan Grid. Card perlu dibagi ke kolom
dan baris sekaligus, dan jumlah kolomnya berubah mengikuti lebar layar. Cukup
satu deklarasi `grid-template-columns: repeat(2, 1fr)` untuk mendapat dua kolom
sama lebar, lalu diganti `repeat(3, 1fr)` di media query berikutnya. Kalau
bagian ini dikerjakan dengan Flexbox, lebar tiap card harus dihitung sendiri
memakai persen dan `gap` masih ikut menggeser hitungan itu. Grid membagi kolom
dan jarak dalam satu aturan, dan tinggi card dalam satu baris ikut menyamakan
diri sehingga kartu yang teksnya pendek tidak menggantung.

## Satu aturan CSS yang awalnya tidak berlaku

Aturan grid untuk card menu sempat tidak jalan sama sekali. Kerangka dasarnya
sudah saya tulis satu kolom, lalu saya tambahkan dua blok media query, 600px
untuk dua kolom dan 960px untuk tiga kolom. Tetapi waktu dibuka di laptop,
card tetap berbaris satu kolom.

Saya klik kanan pada card lalu pilih Inspect dan lihat panel Styles. Di situ
deklarasi `grid-template-columns: repeat(3, 1fr)` yang ada di dalam media query
terlihat dicoret, sedangkan `grid-template-columns: 1fr` dari aturan dasar
`.daftar-menu` di bawahnya tidak dicoret. Panel Computed menguatkan hal yang
sama, nilai yang dipakai memang `1fr`. Jadi media query-nya sudah aktif, hanya
saja aturannya kalah.

Penyebabnya urutan penulisan. Selector `.daftar-menu` di dalam maupun di luar
media query punya specificity yang sama, sehingga yang menang adalah aturan
yang ditulis paling akhir. Blok media query saya letakkan di atas aturan dasar,
jadi aturan dasar selalu menimpanya. Setelah aturan dasar saya pindah ke atas
dan kedua blok `@media` diturunkan ke bawahnya, card langsung tampil tiga
kolom di layar lebar.

## Catatan pekan 2

### Kenapa data form muncul di address bar

Setelah semua field diisi dan tombol Kirim ditekan, halaman yang terbuka
masih pesan.html, tetapi alamatnya bertambah panjang. Contohnya jadi seperti
ini:
http://127.0.0.1:5500/pesan.html?nama=budi&wa=08123&menu=cappuccino&jumlah=2&cara_ambil=tempat&catatan=es+sedikit

Penyebabnya karena form saya pakai `method="get"`. Di metode ini browser
mengambil semua isi field, menyusunnya jadi pasangan nama=nilai, menyambungnya
dengan tanda &, lalu menempelkannya di URL setelah tanda tanya. Spasi diubah
jadi `+` supaya tidak merusak alamat. Ini sama seperti yang dibahas di Minggu 1
soal method HTTP, bedanya `post` menyimpan data di body request jadi tidak
terlihat di address bar.

Efek sampingnya alamat itu bisa di-bookmark atau dikirim ke orang lain beserta
isinya. Jadi kalau ada data rahasia seperti password, jangan pakai get. Panjang
URL juga ada batasnya, makanya untuk data besar lebih aman post.

Waktu saya coba kirim form yang nama dan WhatsApp-nya kosong, browser langsung
menahan pengiriman dan memberi tanda di field yang kosong. Address bar tidak
berubah sampai field wajib terisi.

### Element semantic yang saya pakai

Saya pilih `<nav>` untuk bagian link Beranda, Menu, dan Pesan. Isi bagian itu
memang daftar navigasi, jadi lebih tepat ditandai nav daripada div. Pembaca
layar bisa mengenali nav sebagai landmark, jadi pengguna yang pakai keyboard
bisa langsung lompat ke navigasi tanpa membaca halaman dari atas. Kalau nanti
ada navigasi kedua, misalnya di footer, tinggal dikasih `aria-label` biar tidak
tertukar.

Div tidak saya pakai di situ karena div tidak punya arti apa pun. Browser dan
screen reader akan menganggapnya kotak biasa, jadi struktur halaman tidak
terbaca. Selain nav, saya juga pakai `<header>`, `<main>`, `<section>` untuk
kategori menu, dan `<footer>` untuk alamat serta jam buka.
