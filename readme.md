# Tutorial 8 - Subscriber

Nama: Rafsanjani
NPM: 2406495400

## Reflection 1

### 1. What is AMQP?

AMQP atau Advanced Message Queuing Protocol adalah protokol komunikasi yang digunakan untuk pertukaran pesan antara aplikasi melalui message broker. Dalam tutorial ini, AMQP dipakai agar program publisher dapat mengirim event ke RabbitMQ, lalu program subscriber dapat menerima dan memproses event tersebut dari queue.

### 2. What does `guest:guest@localhost:5672` mean?

URL `amqp://guest:guest@localhost:5672` berisi informasi koneksi ke RabbitMQ.

- `guest` pertama adalah username yang digunakan untuk login ke RabbitMQ.
- `guest` kedua adalah password untuk username tersebut.
- `localhost:5672` berarti program akan terhubung ke RabbitMQ yang berjalan di komputer lokal pada port `5672`.
- Port `5672` adalah port yang digunakan RabbitMQ untuk koneksi AMQP dari program.

Dengan URL tersebut, subscriber saya akan mencoba terhubung ke RabbitMQ lokal menggunakan username `guest` dan password `guest`, lalu mendengarkan event dari queue `user_created`.

## Simulating Slow Subscriber

Pada eksperimen ini, saya mengaktifkan kembali baris:

```rust
thread::sleep(ten_millis);

## Reflection and Running at Least Three Subscribers

Pada eksperimen ini, saya menjalankan minimal tiga instance subscriber yang semuanya mendengarkan queue `user_created`. Ketika publisher dijalankan beberapa kali, message tidak hanya diproses oleh satu subscriber, tetapi dibagi ke beberapa subscriber yang aktif.

Spike pada queue RabbitMQ berkurang lebih cepat karena ada lebih dari satu consumer yang mengambil message dari queue. Dengan tiga subscriber, proses konsumsi event dapat dilakukan secara paralel sehingga backlog message lebih cepat habis dibandingkan hanya menggunakan satu subscriber.

Multiple subscribers membantu scalability karena sistem dapat menambah jumlah consumer saat jumlah event meningkat. Publisher tetap cukup mengirim event ke broker, sedangkan broker akan mendistribusikan message kepada consumer yang tersedia.

Beberapa hal yang dapat diimprove dari kode publisher dan subscriber adalah menambahkan error handling yang lebih jelas, menghindari `unwrap()` agar program tidak langsung panic, menambahkan logging pada publisher agar terlihat event mana yang berhasil dikirim, dan mengganti `loop {}` kosong pada subscriber dengan mekanisme yang lebih efisien agar tidak membuang CPU.

![Three subscribers terminals](images/three-subscribers-terminals.png)

![Three subscribers RabbitMQ chart](images/three-subscribers-rabbitmq-chart.png)

![Three subscribers publisher output](images/three-subscribers-publisher-output.png)