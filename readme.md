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