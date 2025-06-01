### Bridge antara JavaScript dan Native Modules
Apa itu Bridge di React Native?
Bridge adalah mekanisme komunikasi antara kode JavaScript dan kode native (Java/Kotlin untuk Android, Objective-C/Swift untuk iOS). React Native tidak langsung mengakses API native, melainkan menggunakan Bridge untuk menjembatani komunikasi.

#### Cara Kerja Bridge (Arsitektur Lama)
1. JavaScript Thread menjalankan aplikasi (logika, state, dll.) dengan JavaScriptCore.
2. Jika butuh akses ke fitur native (kamera, sensor, dll), JS membuat pesan dan mengirimkannya ke Bridge.
3. Bridge meneruskan pesan ini ke Native Module (dalam thread-nya sendiri).
4. Native Module memproses pesan dan, jika ada hasil (callback/promise), mengirimkan balasan ke JavaScript lagi via Bridge.

Komunikasi ini bersifat asynchronous dan serial, artinya bisa terjadi bottleneck jika terlalu banyak pesan yang dikirim.

#### Contoh
``` NativeModules.DeviceInfo.getDeviceName()
  .then(name => console.log(name));
```

Akan dikirim melalui bridge, diproses di native, lalu hasilnya dikirim balik ke JS.

#### Masalah pada Arsitektur Lama:
* Latency karena komunikasi async yang berlebihan.
* Tidak efisien untuk animasi kompleks (UI kurang responsif).

### Cara React Native Merender UI di Android/iOS
Secara Umum:
* UI React Native dibangun dengan komponen React, yang ditulis dalam JavaScript.
* React Native menerjemahkan komponen ini menjadi view native murni (UIView di iOS, View di Android), bukan WebView.

#### Proses Rendering:
1. Developer menulis JSX, misalnya:

```
<View>
  <Text>Hello</Text>
</View>
```
2. JSX dikompilasi menjadi JavaScript object tree (virtual DOM React).
3. React reconciler menghitung perubahan di Virtual DOM, lalu membuat perintah untuk menciptakan/ubah/hapus elemen.
4. Perintah ini dikirim via Bridge ke Native Thread (UI Thread).
5. UI Thread menerjemahkan perintah itu menjadi komponen native Android/iOS dan merender-nya di layar.

#### Arsitektur Baru: Fabric + JSI (JavaScript Interface)
React Native kini sedang bermigrasi ke arsitektur baru:
Komponen baru:
* Fabric: Sistem rendering baru yang membuat komunikasi lebih cepat dan sinkron.
* TurboModules: Modularisasi native module yang lebih efisien.
* JSI (JavaScript Interface): Pengganti Bridge, memungkinkan akses langsung JS ke objek native tanpa serialisasi.

Keuntungan:
* Komunikasi lebih cepat (tanpa serialisasi JSON).
* UI lebih smooth, cocok untuk animasi dan gesture.
* Lebih mudah untuk concurrent rendering (React 18+).
* Diagram Singkat (Arsitektur Lama vs Baru)

```
LAMA:                         BARU:
JS Thread                    JS Thread
   |                            |
Bridge <------------->      JSI (sinkron dan cepat)
   |                            |
Native Modules           TurboModules
   |
UI Thread                UI Thread (dengan Fabric)
```
