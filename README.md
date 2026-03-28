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
    -   [✅] Commit: `Create Subscriber model struct.`
    -   [✅] Commit: `Create Notification model struct.`
    -   [✅] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [✅] Commit: `Implement add function in Subscriber repository.`
    -   [✅] Commit: `Implement list_all function in Subscriber repository.`
    -   [✅] Commit: `Implement delete function in Subscriber repository.`
    -   [✅] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [] Commit: `Create Notification service struct skeleton.`
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
1. Di kasus BambangShop ini, menurut saya Subscriber tidak harus dibuat sebagai interface atau trait. Satu struct aja sudah cukup karena semua subscriber punya data dan perilaku yang sama, yaitu menyimpan url dan name lalu menerima notifikasi dengan cara yang sama. Trait baru akan lebih berguna kalau nanti ada banyak jenis subscriber dengan cara kerja update() yang berbeda.

2. Menurut saya Vec bisa dipakai kalau datanya sedikit dan kebutuhannya lebih sederhana. Tapi karena id pada Program dan url pada Subscriber harus unik, DashMap jadi lebih cocok. Dengan menggunakan DashMap, data lebih mudah dicafi, ditambah, dan dihapus berdasarkan key tanpa harus mengecek satu per satu seperti di Vec.

3. Menurut saya DashMap tetep dibutuhkan dan tidak bisa langsung diganti hanya dengan Singleton pattern. Singleton hanya memastikan bahwa kita memakai satu instance global. Masalahnya, kita juga perlu struktur data yang aman dipakai oleh banyak thread. Di sini DashMap dipakai untuk thread safety, sedangkan lazy_static sudah membantu membuat data global seperti konsep Singleton. Jadi kalau Singleton aja belum cukup tanpa DashMap.

#### Reflection Publisher-2
1. Menurut saya memisahkan Service dan Repository dari Model membuat tanggung jawab setiap bagian menjadi lebih jelas. Model fokus pada bentuk data, Repository fokus pada penyimpanan data, dan Service fokus pada business logic. Dengan memisahkannya, kode jadi lebih rapi dan lebih mudah untuk dirubah kalau misal nanti ada perubahan logic atau cara penyimpanan data.

2. Kalau hanya menggunakan Model, maka salah satu model harus menangani terlalu banyak hal sekaligus. Misalnya Program, Subscriber, dan Notification tidak hanya menyimpan data, tapi harus mengatur juga alur bisnis dan akses data, sehingga menurut saya ini hanya akan membuat tiap model menjadi lebih kompleks. Hubungan antar model juga bisa menjadi berantakan karena masing-masing model saling memanggil logic atau satu sama lain secara langsung.

3. Saya udah mencoba memakai Postman untuk test endpoint di tutorial modul ini dan menurut saya Postman sangat membantu karena saya bisa langsung mengirim request HTTP dan melihat response nya tanpa harus membuat frontend. Fitur yang mungkin sangat membantu adalah collection dan penyimpanan request, karena endpoint jadi lebih rapi dan mudah untuk dicoba ulang. Saya rasa sih tool ini akan tetap berguna untuk project-project yang memakai API.

#### Reflection Publisher-3
