# BambangShop Receiver App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:


Berikut adalah versi yang telah saya parafrase dengan tetap mempertahankan makna inti:  

---  

## **Refleksi Wajib (Subscriber)**  

### **Refleksi Subscriber-1**  

1. Dalam tutorial ini, kita menggunakan `RwLock<Vec<Notification>>` untuk mengatur akses yang aman ke daftar notifikasi dalam lingkungan multi-threading. `RwLock` memungkinkan beberapa thread untuk membaca data secara bersamaan, tetapi membatasi hanya satu thread yang dapat menulis pada satu waktu. Hal ini membantu mencegah *race condition* dan meningkatkan efisiensi akses data. Jika kita menggunakan `Mutex<Vec<Notification>>`, setiap proses baca atau tulis akan dikunci, sehingga dapat memperlambat kinerja sistem karena semua akses harus menunggu giliran.  

2. Dalam bahasa pemrograman Java, variabel statis dapat diakses oleh semua instance dari sebuah kelas dan dapat diubah tanpa pembatasan khusus. Namun, Rust memiliki aturan ketat untuk mencegah akses memori yang tidak aman dan potensi *race condition*. Oleh karena itu, dalam tutorial ini, kita menggunakan `lazy_static!` untuk mendefinisikan `NOTIFICATIONS` sebagai `RwLock<Vec<Notification>>`, yang memungkinkan akses data secara aman dalam lingkungan multi-threading. Rust tidak mengizinkan modifikasi langsung pada variabel statis tanpa mekanisme sinkronisasi seperti ini karena dapat menyebabkan perilaku yang tidak terduga dalam eksekusi program.  

### **Refleksi Subscriber-2**  

1. Saat mengerjakan tutorial ini, saya mengikuti langkah-langkah yang diberikan tanpa terlalu banyak mengeksplorasi file lain, seperti `src/lib.rs`. Saya merasa bahwa panduan yang tersedia sudah cukup jelas dan membantu dalam memahami konsep serta implementasinya. Namun, jika ada kesempatan untuk eksplorasi lebih lanjut, saya mungkin akan mencoba memahami bagian lain dari kode untuk mendapatkan wawasan yang lebih mendalam, terutama jika diminta dalam latihan tambahan.  

2. Pola desain Observer memudahkan penambahan subscriber karena publisher tidak perlu mengetahui secara langsung siapa saja yang telah berlangganan notifikasi. Subscriber dapat memilih untuk menerima notifikasi berdasarkan jenis produk tertentu, sementara publisher cukup mengirimkan pembaruan saat terjadi perubahan. Namun, ketika terdapat banyak instance dari aplikasi utama (*Main app*), tantangan baru muncul, yaitu bagaimana memastikan konsistensi notifikasi di antara semua instance. Jika tidak ada mekanisme sinkronisasi, beberapa subscriber mungkin menerima informasi yang tidak seragam. Salah satu solusi yang bisa digunakan adalah dengan menerapkan *shared database* atau menggunakan *message broker* untuk mengatur distribusi notifikasi dengan lebih baik.  

3. Saya melakukan beberapa modifikasi pada koleksi Postman untuk mengirim permintaan pembuatan produk dengan berbagai jenis. Hal ini saya lakukan agar bisa menguji apakah sistem notifikasi bekerja dengan baik pada beberapa instance receiver yang berjalan di port berbeda, seperti 8001, 8002, dan 8003. Dengan cara ini, saya dapat memastikan bahwa setiap subscriber hanya menerima notifikasi untuk kategori produk yang sesuai dengan langganannya. Pengujian ini membantu saya memahami bagaimana sistem Observer bekerja dalam skenario nyata yang melibatkan banyak subscriber.  

