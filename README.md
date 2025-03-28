# BambangShop Receiver App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

- Nama : Muhammad Almerazka Yocendra
- NPM : 2306241745
- Kelas : Pemrograman Lanjut - A

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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

<details>
    <summary><strong> Reflection Subscriber-1 💡 </strong></summary>
  
1. **In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?**
    > Dalam tutorial ini, kita menggunakan `RwLock<Vec<Notification>>` untuk menyinkronkan akses ke daftar notifikasi agar beberapa _thread_ dapat membaca atau menulis secara aman tanpa menyebabkan _race condition_ atau _crash_. `RwLock<>` dipilih karena memungkinkan banyak _thread_ membaca secara bersamaan, sementara hanya satu _thread_ yang boleh menulis pada satu waktu. Ini lebih efisien dibandingkan `Mutex<>`, yang mengunci akses secara penuh sehingga baik pembaca maupun penulis harus menunggu giliran. Jika menggunakan `Mutex<>`, setiap operasi baca dan tulis harus dilakukan secara eksklusif, yang berarti semua _thread_ harus antri meskipun hanya ingin membaca data, yang sebenarnya tidak mengubah apa pun.
    
    > Karena dalam kasus ini lebih banyak operasi membaca dibandingkan menulis, `RwLock<>` menjadi pilihan yang lebih optimal karena mengizinkan banyak _thread_ membaca tanpa harus saling menunggu, menjaga responsivitas aplikasi, dan mengurangi _bottleneck_ yang bisa terjadi jika setiap operasi baca harus menunggu akses eksklusif seperti pada `Mutex<>`. Dengan `RwLock<>`, kita bisa tetap menjaga sinkronisasi data sambil memastikan performa tetap cepat dan efisien, sehingga cocok untuk skenario ini.

2.  **In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?**
    >  Di **Rust**, variabel _static_ bersifat _immutable_ secara _default_ untuk memastikan _memory_ dan _thread safety_ sejak tahap kompilasi. Berbeda dengan Java, di mana variabel _static_ bisa diubah melalui metode _static_ tanpa batasan, **Rust** menerapkan aturan kepemilikan (_ownership_) dan peminjaman (_borrowing_) yang lebih ketat untuk mencegah data _race_ dan perilaku tak terdefinisi dalam program yang berjalan secara konkuren. **Rust** tidak mengizinkan mutasi langsung pada variabel _static_ karena ingin menghindari potensi _bug_ yang bisa muncul akibat akses data secara bersamaan oleh beberapa thread.
    
    > Sementara di **Java** perubahan langsung pada variabel _static_ diperbolehkan karena adanya _garbage collector_ dan _runtime checks_ yang menangani masalah memori, **Rust** memilih pendekatan eksplisit untuk memastikan keamanan data. Jika kita ingin menggunakan variabel _static_ yang bisa diubah, kita harus menggunakan mekanisme sinkronisasi tambahan seperti `lazy_static!` dengan `RwLock<>` atau `Mutex<>`. Ini memastikan bahwa akses ke data tetap aman tanpa menyebabkan korupsi memori atau kondisi balapan (_race condition_). Dengan pembatasan ini, **Rust** memberikan jaminan keamanan yang lebih kuat dalam pemrograman konkuren, sekaligus memberi _developer_ kontrol penuh atas bagaimana data dikelola dan diakses dalam aplikasi.
</details>

<details>
    <summary><strong> Reflection Subscriber-2 💡 </strong></summary>
  
1. **Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.**
    > Ya, saya sudah mengeksplorasi bagian lain di luar langkah-langkah tutorial, khususnya di `src/lib.rs`. Saya memahami bahwa `lib.rs` biasanya digunakan untuk mendefinisikan modul yang dapat digunakan kembali di dalam proyek, terutama dalam proyek yang menggunakan **Rocket** sebagai _framework_. Dengan adanya `lib.rs`, kode menjadi lebih modular dan terorganisir, karena berbagai bagian seperti _routing_ dan _handler_ bisa dipisahkan ke dalam modul tersendiri. Selain itu, saya juga belajar bahwa banyak proyek **Rust** menempatkan logika utama dalam `lib.rs` agar dapat diuji secara terpisah dari aplikasi utama, sehingga lebih mudah untuk menulis dan menjalankan _unit test_ menggunakan atribut `#[cfg(test)]` dan `#[test]`.
   
    > Selain itu, saya juga melihat bagaimana arsitektur aplikasi disusun dengan pemisahan yang jelas antara _controller_, _service_, dan _repository_, yang membuat kode lebih rapi dan mudah dikelola. Saya juga belajar tentang penggunaan `lazy_static!` untuk membuat konfigurasi dan _client_ HTTP global lebih efisien, serta bagaimana `RwLock<>` dan `Mutex<>` digunakan untuk manajemen akses data yang lebih aman dan efisien. Intinya `src/lib.rs` sudah dirancang agar _scalable_, efisien, dan aman dari _race condition_ dalam pengaksesan data bersama. Jadi, eksplorasi yang saya lakukan cukup membantu untuk memahami bagaimana aplikasi ini bekerja di balik layar dan bagaimana cara menulis kode yang lebih modular serta _maintainable_.

2.  **Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?**
    >  _Observer pattern_ sangat membantu dalam menambahkan _subscriber_ baru ke dalam sistem karena pola ini memungkinkan pemisahan yang jelas antara _publisher_ (**Main app**) dan _subscriber_ (**Receiver**). Dengan _Observer pattern_, setiap kali ada perubahan status produk, _publisher_ cukup memberi tahu semua _subscriber_ yang telah terdaftar tanpa perlu mengetahui detail implementasi masing-masing _subscriber_. Ini memudahkan penambahan _subscriber_ baru tanpa harus mengubah kode utama aplikasi. 
    
    > Saat saya mencoba menjalankan beberapa _instance_ **Receiver**, setiap _instance_ dapat menerima notifikasi secara independen karena mereka hanya perlu berlangganan melalui _endpoint_ yang telah disediakan. Sistem ini fleksibel karena daftar _subscriber_ dikelola secara dinamis, sehingga penambahan _subscriber_ hanya memerlukan pemanggilan API tanpa perlu mengubah arsitektur _backend_. Penambahan **Receiver** baru juga simpel—tinggal jalankan _instance_ baru dengan _port_ dan nama unik, lalu daftar ke jenis produk yang diinginkan. Namun, jika ingin menambahkan lebih dari satu _instance_ **Main app**, tantangannya menjadi lebih kompleks. Diperlukan mekanisme sinkronisasi agar setiap _instance_ memiliki daftar _subscriber_ yang sama dan tidak terjadi inkonsistensi data

3.  **Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).**
    >  Ya, saya sudah mencoba membuat test sendiri di **Postman** _collection_, bahkan untuk mata kuliah lain. Fitur ini sangat membantu dalam mengotomatiskan validasi respons dan menangkap error lebih awal, sehingga tidak perlu melakukan pengecekan manual satu per satu. Dengan adanya _test_, saya bisa memastikan bahwa notifikasi terkirim dengan benar dan _response_ API sesuai dengan yang diharapkan.
    
    > Selain itu, dokumentasi di **Postman** juga sangat berguna, terutama saat bekerja dalam tim. Dengan mendokumentasikan setiap _endpoint_, parameter yang dibutuhkan, format _response_, serta kemungkinan skenario _error_, tim lain bisa lebih mudah memahami dan menggunakan API tanpa harus membaca kode secara langsung. Dokumentasi ini juga bisa langsung digunakan untuk testing di **Postman**, yang tentunya mempercepat proses _debugging_ dan validasi sistem. Secara keseluruhan, baik _test_ maupun dokumentasi di **Postman** membuat pengembangan lebih rapi, efisien, dan menghemat banyak waktu saat melakukan debugging atau kolaborasi dalam proyek.

</details>
