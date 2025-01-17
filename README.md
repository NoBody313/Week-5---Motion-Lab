# **Assignment Week-5 - Working with Firebase**

## 🔐 **Firebase Authentication**
Firebase Authentication menyediakan sistem autentikasi pengguna yang sangat mudah untuk digunakan, Firebase Auth memiliki berbagai metode autentikasi seperti email/sandi, Google, Facebook, Twitter, GitHub, dan lainnya. Dengan fitur ini, kamu tidak perlu membuat sistem autentikasi dari awal, karena Firebase sudah menangani proses verifikasi, pengelolaan sesi, dan perlindungan terhadap ancaman seperti brute-force attacks. Kamu juga bisa mengubah beberapa pengaturan default dan juga bisa menggunakan ekstensi yang tersedia sesuai kebutuhan kamu.

### **Kegunaan dalam Pengembangan Aplikasi**:
Firebase Authentication memudahkan kamu untuk menambahkan fitur login dan registrasi user dalam aplikasi kamu tanpa harus menulis banyak kode. Kamu pun dapat mengatur metode login yang tersedia, seperti menggunakan akun Google atau alamat email. Sistem ini juga memungkinkan kamu untuk melakukan manajemen kata sandi, reset kata sandi, dan verifikasi email, yang meningkatkan keamanan aplikasi kamu.

#### **Contoh penerapan dalam Kotlin**:
```kotlin
// Email and Password Authentication
suspend fun signInWithEmailPassword(email: String, password: String): Result<String> {
    return try {
        val authResult = auth.signInWithEmailAndPassword(email, password).await()
        Result.success(authResult.user?.uid ?: "")
    } catch (e: Exception) {
        Result.failure(e)
    }
}

// Email and Password Registration
suspend fun registerWithEmailPassword(email: String, password: String): Result<String> {
    return try {
        val authResult = auth.createUserWithEmailAndPassword(email, password).await()
        Result.success(authResult.user?.uid ?: "")
    } catch (e: Exception) {
        Result.failure(e)
    }
}
```

---

## 📚 **Firebase Firestore (Database NoSQL)**
Firebase Firestore adalah layanan database NoSQL yang memungkinkan kamu untuk menyimpan data dalam struktur dokumen dan koleksi. Firestore sangat cocok untuk aplikasi yang membutuhkan data terstruktur dengan skala yang dapat disesuaikan. Data disimpan dalam bentuk dokumen (JSON-like), yang bisa dengan mudah diakses dan dimodifikasi.

### **Kegunaan dalam Pengembangan Aplikasi**:
Firestore memungkinkan kamu untuk menyimpan dan mengelola data secara fleksibel tanpa harus menggunakan struktur database relasional dan memungkinkan juga untuk membuat datanya menjadi relasional. Hal ini sangat berguna untuk aplikasi dengan skala besar dan yang memerlukan pembaruan data secara real-time. Firestore juga mendukung kemampuan untuk melakukan query yang kompleks dan mendukung sinkronisasi data secara real-time di perangkat pengguna aplikasi kamu.

#### **Contoh penerapan dalam kotlin**:
```kotlin
private val firestore = FirebaseFirestore.getInstance()
private val userRef = firestore.collection("users")

suspend fun createUser(users: Users): Result<String> {
    return try {
        val exitingUser = userRef
            .whereEqualTo("email", users.email)
            .get()
            .await()

        if (!exitingUser.isEmpty) {
            return Result.failure(Exception("Email already used"))
        }

        val userData = users.copy(
            password = Users.hashPassword(users.password)
        )
        val resultUser = userRef.add(userData).await()
        Result.success(resultUser.id)
    } catch (e: Exception) {
        Result.failure(e)
    }
}
```
---

## 📡 **Firebase Realtime Database**
Firebase Realtime Database adalah database NoSQL yang memungkinkan data disinkronkan secara langsung dan real-time di seluruh perangkat yang terhubung. Data yang disimpan secara realtime di dalam Realtime Database akan segera tersedia di perangkat lain tanpa harus melakukan refresh atau melakukan request tambahan dari server. Ini memungkinkan dalam pengembangan aplikasi yang lebih interaktif, seperti aplikasi chating dan berbagi status seperti instagram.

### **Kegunaan dalam Pengembangan Aplikasi**:
Realtime Database ideal untuk aplikasi yang memerlukan update data secara langsung. Misalnya, aplikasi chatting, game multiplayer, atau aplikasi berbasis lokasi yang memerlukan interaksi langsung dan pembaruan data antar user. Dengan Realtime Database, kamu dapat menghemat waktu karena tidak perlu menunggu untuk mengupdate tampilan aplikasi setelah data baru dimasukkan.

---

## 🖼️ **Firebase Cloud Storage**
Firebase Cloud Storage memberikan solusi penyimpanan file yang mudah digunakan untuk menyimpan file besar, seperti gambar, video, dan audio. Layanan ini terintegrasi dengan menggunakan Firebase SDK, memungkinkan kamu untuk mengupload dan mengunduh file ataui data dengan gampang yang diberikan aplikasi mereka. Firebase Cloud Storage juga dilengkapi dengan pengaturan keamanan berbasis Firebase Authentication, yang memungkinkan sistem untuk mengkontrol akses yang lebih baik terhadap file yang diupload.

### **Kegunaan dalam Pengembangan Aplikasi**:
Firebase Cloud Storage sangat berguna untuk aplikasi yang perlu mengelola media user, seperti aplikasi sharing foto atau video. Penyimpanan cloud ini memudahkan kamu untuk menyimpan file di cloud tanpa perlu membangun infrastruktur penyimpanan sendiri. Firebase menyediakan kontrol akses detail untuk memastikan bahwa hanya pengguna yang memiliki akses yang dapat mengakses atau mengunggah file.

---

## 📲 **Firebase Cloud Messaging (FCM)**
Firebase Cloud Messaging (FCM) memungkinkan kamu untuk mengirim pemberitahuan atau pesan langsung ke aplikasi user di berbagai platform seperti Android, iOS, dan web. FCM ini mendukung pemberitahuan push (terus menerus) dan pesan data, memberikan fleksibilitas dalam cara pesan dikirim dan diterima oleh aplikasi.

### **Kegunaan dalam Pengembangan Aplikasi**:
FCM berguna untuk meningkatkan keterlibatan pengguna aplikasi dengan mengirimkan pemberitahuan secara langsung ke perangkat mereka. Ini sangat berguna dalam aplikasi yang memerlukan pembaruan langsung atau pemberitahuan, seperti aplikasi media sosial, berita, atau game yang membutuhkan pengingat atau pembaruan secara realtime. FCM juga memungkinkan kamu dapat mengirim pesan berbasis grup atau individual kepada pengguna aplikasi.

---
