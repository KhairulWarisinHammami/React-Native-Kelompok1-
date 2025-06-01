### Arsitektur dan Cara Kerja React Native
#### Mekanisme Bridge antara JavaScript dan Native Modules
##### Definisi Bridge
Dalam konteks React Native, bridge adalah komponen arsitektural yang berfungsi sebagai jembatan komunikasi antara dunia JavaScript dan dunia native (Java/Kotlin untuk Android, Objective-C/Swift untuk iOS). React Native memungkinkan pengembang untuk menulis logika aplikasi dalam JavaScript, namun tetap dapat memanfaatkan API dan komponen native dari sistem operasi yang mendasarinya melalui bridge ini.

##### Cara Kerja Bridge (Arsitektur Lama)
Arsitektur bridge tradisional React Native bersifat asinkron dan serial. Berikut adalah tahapan umum proses komunikasi:
1. Eksekusi Logika di JavaScript Thread
React Native menggunakan mesin JavaScript (JavaScriptCore) untuk mengeksekusi logika aplikasi, termasuk state management dan rendering UI dalam bentuk deklaratif (JSX).

2. Permintaan Akses ke Fitur Native
Ketika JavaScript membutuhkan akses ke fitur native (misalnya, kamera, sensor perangkat, penyimpanan lokal, dsb), JavaScript membuat perintah dalam bentuk objek serial (biasanya JSON) dan mengirimkannya melalui bridge.

3. Bridge Menyalurkan Perintah ke Native Module
Bridge meneruskan perintah tersebut ke native module yang sesuai. Komponen ini berada pada thread native (biasanya background thread) dan mengakses API sistem operasi.

4. Pengembalian Hasil ke JavaScript
Jika native module perlu memberikan hasil (misalnya data sensor atau respons API), hasil tersebut dikirimkan kembali melalui bridge dalam bentuk objek serial, lalu diproses kembali oleh JavaScript.

##### Kelebihan dan Kekurangan Arsitektur Lama
Kelebihan:
* Memungkinkan interaksi antara JavaScript dan komponen native tanpa perlu menulis seluruh aplikasi dalam kode native.
*Mempermudah pengembangan lintas platform.

Kekurangan:
* Komunikasi bersifat asinkron dan melalui serialisasi JSON, sehingga dapat menimbulkan bottleneck.
* Tidak cocok untuk aplikasi yang membutuhkan performa tinggi, seperti animasi kompleks atau manipulasi UI real-time.
* Debugging lintas thread dan lintas bahasa dapat menjadi kompleks.

#### Proses Rendering UI di Android dan iOS
React Native tidak merender antarmuka pengguna menggunakan WebView seperti framework hybrid lainnya (misalnya Cordova), tetapi justru menerjemahkan komponen React ke dalam native UI components masing-masing platform.

##### Tahapan Rendering:
1. Penulisan Komponen UI dalam JSX
Developer menulis antarmuka pengguna menggunakan sintaks JSX, yang merupakan ekstensi JavaScript untuk membuat elemen UI secara deklaratif.

```
<View>
  <Text>Hello, World</Text>
</View>
```

2. Transformasi ke Virtual DOM (React Element Tree)
JSX dikompilasi menjadi struktur data JavaScript (React Element Tree) yang merepresentasikan pohon UI.

3. Proses Reconciliation oleh React
React melakukan proses reconciliation untuk menentukan perubahan (diff) antara tampilan sebelumnya dan tampilan yang diinginkan. Hasilnya adalah serangkaian UI commands.

4. Pengiriman UI Commands Melalui Bridge
Perintah UI tersebut diserialisasi dan dikirimkan melalui bridge ke native UI thread.

5. Rendering oleh Native UI Thread
Native UI thread mengeksekusi perintah tersebut dan membuat/memodifikasi elemen antarmuka menggunakan komponen native sistem operasi (misalnya UIView di iOS atau View di Android).

##### Karakteristik Rendering:
* UI tetap terlihat dan berperforma seperti aplikasi native karena menggunakan komponen UI asli.
* Namun, komunikasi antara JavaScript dan UI thread melalui bridge dapat menimbulkan latensi dalam kasus kompleks (misalnya animasi atau gesture interaktif).

#### Arsitektur Baru: Fabric dan JSI
Untuk mengatasi keterbatasan bridge tradisional, React Native mengembangkan arsitektur baru yang terdiri dari beberapa komponen utama:

a. JSI (JavaScript Interface)
* JSI adalah antarmuka C++ yang menggantikan bridge lama.
* Memungkinkan JavaScript mengakses objek native secara langsung tanpa serialisasi JSON.
* Komunikasi menjadi sinkron dan lebih efisien, mengurangi latensi dan overhead.

b. TurboModules
* Modularisasi ulang native modules agar dapat diakses melalui JSI.
* Hanya modul yang diperlukan yang dimuat secara dinamis (lazy loading), sehingga lebih hemat memori.

c. Fabric (Rendering System Baru)
* Fabric menggantikan sistem rendering lama dan terintegrasi dengan JSI.
* Mendukung concurrent rendering dan memungkinkan UI di-commit secara sinkron dengan React Fiber.
* Lebih stabil dan responsif terhadap perubahan UI secara real-time.

##### Keunggulan Arsitektur Baru:
* Komunikasi antar thread lebih cepat dan efisien.
* Penggunaan memori lebih optimal.
* Mendukung fitur-fitur lanjutan dari React seperti Concurrent Mode.
* Respons UI lebih baik dan animasi lebih halus.

#### Perbandingan Arsitektur Lama vs Baru

| Aspek                | Arsitektur Lama (Bridge)     | Arsitektur Baru (Fabric + JSI)                  |
| -------------------- | ---------------------------- | ----------------------------------------------- |
| Komunikasi JS-Native | Asinkron, berbasis JSON      | Sinkron, melalui objek langsung (JSI)           |
| Efisiensi Performa   | Kurang optimal untuk animasi | Sangat efisien dan cepat                        |
| Modularisasi         | Semua native module dimuat   | Hanya modul yang dibutuhkan dimuat              |
| Rendering            | Sistem rendering lama        | Sistem Fabric (terintegrasi dengan React Fiber) |
| Dukungan Fitur Baru  | Terbatas                     | Mendukung Concurrent Rendering                  |
