# Implementasi Aplikasi dengan React Native

Bagian ini menjelaskan langkah-langkah lengkap untuk membangun, menjalankan, dan mengorganisir aplikasi React Native Anda.

---

## 1. Persiapan Tools

Pastikan Anda telah menginstal dan mengonfigurasi komponen berikut:

- **Node.js** (v16.x atau lebih baru)  
- **npm** atau **Yarn**  
- **Watchman** (khusus macOS)  
- **Java JDK** (untuk Android)  
- **Android Studio** dengan SDK dan emulator yang sesuai  
- **Xcode** (hanya macOS, untuk build iOS)
- **React Native CLI &/atau Expo CLI**  

```bash

# Cek versi Node.js dan npm
node -v && npm -v

# (Opsional) Instalasi Watchman di macOS
brew install watchman
```

## 2. Implementasi

Dalam membuat proyek baru kali ini kami menggunakan Expo Cli namun anda juga bisa menggunakan React Native Cli. Menggunakan Expo Cli bisa di bilang lebih mudah dan tidak membutuhkan konfigurasi.

### a. Inisiasi 

```bash

npx expo init Contoh-App
cd Contoh-App
```
- Hasil: folder Contoh-App dan file App.js, plus konfigurasi Expo.

### b. Struktur Folder Dasar

Setelah inisiasi Berikut Struktur folder dari hasil inisiasi sebelumnya:

! [Struktur Folder](https://github.com/KhairulWarisinHammami/React-Native-Kelompok1-/blob/Waris-Implementasi_React_Native/Implementasi_React_Native/Images/Struktur%20Folder.png)

### c. Kode Contoh “Hello, React Native!” dengan Counter

Ganti isi App.js (Jika tidak ada pada directory app maka pada app/(tabs)/index.tsf) dengan kode berikut:

```javaScript
import React, { useState } from 'react';
import { StyleSheet, Text, TouchableOpacity, View } from 'react-native';

export default function HomeScreen() {
  const [count, setCount] = useState(0);

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Hello, React Native!</Text>
      <Text style={styles.counter}>Counter: {count}</Text>
      <TouchableOpacity style={styles.button} onPress={() => setCount(count + 1)}>
        <Text style={styles.buttonText}>Tambah Counter</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  title: {
    fontSize: 26,
    fontWeight: 'bold',
    marginBottom: 12,
  },
  counter: {
    fontSize: 20,
    marginBottom: 20,
  },
  button: {
    backgroundColor: '#007AFF',
    paddingVertical: 12,
    paddingHorizontal: 24,
    borderRadius: 8,
  },
  buttonText: {
    color: '#FFF',
    fontSize: 18,
  },
});
```
Penjelasan Kode:
- `useState(0)` : Hook untuk membuat state count dengan nilai awal 0.
- `SafeAreaView` & `StatusBar` : Menjaga tampilan tidak tertutup notch/status bar.
- `View` : Container untuk mengelompokkan komponen.
- `Text` : Untuk menampilkan teks.
- `TouchableOpacity` : Komponen tombol yang bisa ditekan dengan efek opacity.
- `StyleSheet.create` : Mendefinisikan gaya menggunakan Flexbox dan properti layout React Native.
- `elevation & shadow` : Menambah bayangan (Android dan iOS).

### d. Menjalankan Aplikasi 

- Android
  ```bash
  npx react-native run-android
  ```

- iOS
  ```bash
  npx react-native run-ios
  ```

- npx expo start
  ```bash
  npx expo start
  ```

  Lalu scan QR code dengan Expo Go di HP atau pilih “Run on Android/iOS simulator” di browser.
  ![Expo](https://github.com/KhairulWarisinHammami/React-Native-Kelompok1-/blob/Waris-Implementasi_React_Native/Implementasi_React_Native/Images/EXPO.png)


### e. Hasil Contoh-App Yang Sudah Di Buat

![hasil](https://github.com/KhairulWarisinHammami/React-Native-Kelompok1-/blob/Waris-Implementasi_React_Native/Implementasi_React_Native/Images/Hasil%20EXPO.jpeg)

- Bisa dilihat pada gambar aplikasi terdapat "kata hallo Native React" dan counter yang berawal dari nol dan aplikasi Contoh-App ini bisa dijalankan pada android.


![Hasil web](https://github.com/KhairulWarisinHammami/React-Native-Kelompok1-/blob/Waris-Implementasi_React_Native/Implementasi_React_Native/Images/Hasil%20EXPO%20(Web).png)

- Pada Gambar diatas adalah ketika aplikasi Contoh-App di jalankan pada web browser. dan hasilnya sama dengan hasil pada android dan disini pengguna menambahkan counter dari 0 ke 4








