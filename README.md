# BambangShop Publisher App
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
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. **Observer Pattern and the need for an interface/trait**:

    Dalam kasus BambangShop, tidak perlu menggunakan interface atau trait untuk Observer pattern karena hanya terdapat satu jenis observer, yaitu Subscriber. Namun, jika di masa depan ada penambahan jenis observer baru, menggunakan trait akan lebih fleksibel.

2. **Uniqueness of ID and URL**: 

    Penggunaan DashMap lebih disarankan daripada Vec karena memungkinkan pemetaan yang lebih baik antara id program dan url subscriber. Dengan DashMap, kita dapat dengan mudah menghubungkan setiap jenis produk ke setiap subscriber yang berlangganan. Menggunakan Vec akan memerlukan struktur data tambahan yang dapat menyulitkan dalam pengelolaan dan pembaruan data.

3. **Thread safety and Singleton pattern**: 

    Dalam konteks keamanan multithreading Rust, penggunaan DashMap merupakan pilihan yang tepat karena telah dioptimalkan untuk lingkungan multithreaded. DashMap memastikan operasi pada map SUBSCRIBERS dapat dilakukan dengan aman dan efisien oleh banyak thread. Meskipun Singleton pattern dapat memastikan hanya ada satu instance dari objek yang dibuat, hal itu tidak menjamin thread safety seperti yang disediakan oleh DashMap. Dengan menggunakan DashMap, kita dapat memastikan bahwa daftar subscriber terhadap produk kita dikelola secara thread-safe dan efisien.

#### Reflection Publisher-2

1. Memisahkan "Service" dan "Repository" dari Model dalam pola Model-View-Controller (MVC) mengikuti prinsip pemisahan kepentingan. Setiap komponen dalam aplikasi seharusnya memiliki tanggung jawab tunggal, dan memisahkan kepentingan seperti akses data dan logika bisnis dari Model meningkatkan modularitas, keberlanjutan, dan kemampuan pengujian.

    Memisahkan Service dari Repository dalam pola desain Model-View-Controller (MVC) adalah suatu keharusan untuk mematuhi prinsip tanggung jawab tunggal. Service bertugas mengelola logika bisnis dan pemrosesan data yang diperoleh dari Repository, sementara Repository bertindak sebagai antarmuka untuk mengakses dan mengubah data dalam database. Dengan memisahkan kedua lapisan ini, struktur kode menjadi lebih terorganisir, lebih mudah dimengerti, dan lebih mudah untuk dipelihara.


2. Jika hanya menggunakan Model tanpa Service dan Repository, maka akan terjadi ketergantungan yang kuat antara komponen-komponen tersebut. Hal ini akan menyebabkan kesulitan dalam melakukan perubahan karena setiap modifikasi pada Model akan berdampak pada seluruh kode. Dampaknya adalah meningkatnya kompleksitas dan penurunan fleksibilitas dalam pengembangan dan pemeliharaan kode.

3. Postman adalah alat yang sangat berguna untuk menguji dan memvalidasi fungsionalitas aplikasi yang dikembangkan. Dengan Postman, kita dapat mengirimkan permintaan HTTP ke berbagai endpoint dalam aplikasi, dan memeriksa respon yang diterima untuk memastikan keakuratan dan konsistensi data. Fitur CRUD memungkinkan kita menguji operasi dasar seperti membuat, membaca, memperbarui, dan menghapus data. Kemampuan untuk menyesuaikan permintaan dan melihat respons secara langsung membantu dalam menguji dan memperbaiki aplikasi dengan cepat dan efisien.

#### Reflection Publisher-3
