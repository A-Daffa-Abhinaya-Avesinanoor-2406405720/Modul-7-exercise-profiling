# JMeter GUI

## All Student Name

### Graph
![All Student Name Graph](images/all-student-name/graph.png)

### Summary
![All Student Name Summary](images/all-student-name/summary.png)

### Table
![All Student Name Table](images/all-student-name/table.png)

### Tree
![All Student Name Tree](images/all-student-name/tree.png)

## Highest GPA

### Graph
![Highest GPA Graph](images/highest-gpa/graph.png)

### Summary
![Highest GPA Summary](images/highest-gpa/summary.png)

### Table
![Highest GPA Table](images/highest-gpa/table.png)

### Tree
![Highest GPA Tree](images/highest-gpa/tree.png)

# JMeter CLI

## All Student Name

![All Student Name CLI](images/all-student-name/cli.png)

## Highest GPA

![Highest GPA CLI](images/highest-gpa/cli.png)

# Refleksi

### 1. What is the difference between the approach of performance testing with JMeter and profiling with IntelliJ Profiler in the context of optimizing application performance?

JMeter digunakan untuk melakukan performance testing dari sisi user atau client. Dengan JMeter, kita bisa kirim banyak request ke endpoint aplikasi, ukur response time, throughput, error rate, dan melihat apakah aplikasi tetap stabil saat menerima beban tertentu.

IntelliJ Profiler digunakan untuk melihat apa yang terjadi di dalam aplikasi saat kode berjalan. Profiler membantu melihat method mana yang paling banyak memakan waktu, berapa banyak query yang dijalankan, dan bagian kode mana yang menjadi bottleneck.

Jadi, JMeter dapat memberi tahu  kecepatan aplikasi tersebut merespon, sedangkan IntelliJ Profiler dapat memberi tahu bagian kode mana yang membuat aplikasi lambat.

### 2. How does the profiling process help you in identifying and understanding the weak points in your application?

Profiling membantu menemukan weak point karena kita bisa melihat aktivitas aplikasi secara lebih detail. Misalnya, pada endpoint `/all-student`, proses menjadi lambat karena service melakukan query berulang untuk setiap student. Pola seperti ini disebut N+1 query.

Dengan profiling, Kita bisa melihat bagian mana yang paling sering dipanggil dan bagian mana yang menghabiskan waktu paling lama.

### 3. Do you think IntelliJ Profiler is effective in assisting you to analyze and identify bottlenecks in your application code?

Menurut saya, IntelliJ Profiler efektif untuk membantu menganalisis bottleneck di aplikasi. Tool ini memudahkan kita melihat method yang lambat dan memahami alur eksekusi program.

Dalam kasus ini, bottleneck terdapat pada proses pengambilan data student dan course. Setelah diketahui, kode bisa di-optimize dengan mengurangi jumlah query database dan menggunakan fetch join.

### 4. What are the main challenges you face when conducting performance testing and profiling, and how do you overcome these challenges?

Tantangan utama dalam performance testing adalah hasil pengukuran bisa berubah-ubah tergantung kondisi komputer, jumlah data, koneksi database, dan proses lain yang sedang berjalan. Untuk mengatasinya, pengujian dilakukan beberapa kali lalu menggunakan rata-rata hasil.

Tantangan dalam profiling adalah membedakan penyebab kelambatan yang benar-benar berasal dari kode aplikasi dengan faktor luar seperti database, jaringan, atau logging. Untuk mengatasinya, saya membandingkan hasil profiling dengan hasil JMeter dan menganalisis dan optimisasi kode yang tidak efisien.

### 5.  What are the main benefits you gain from using IntelliJ Profiler for profiling your application code?

Manfaat utama IntelliJ Profiler adalah membantu melihat performa aplikasi dari dalam kode. Dengan profiler, kita bisa mengetahui method mana yang memakan waktu paling lama, proses mana yang dipanggil terlalu sering, dan bagian mana yang perlu diprioritaskan untuk optimasi.

Profiler juga membantu membuat proses optimasi lebih terarah. Karena kita optimisasi kode berdasarkan data dari Profile run aplikasi.

### 6. How do you handle situations where the results from profiling with IntelliJ Profiler are not entirely consistent with findings from performance testing using JMeter?

Jika hasil IntelliJ Profiler dan JMeter tidak sepenuhnya sama, saya akan melihat tujuan masing-masing tool. JMeter mengukur performa dari sisi request dan response, sedangkan profiler mengukur performa dari sisi internal aplikasi.

Perbedaan hasil bisa terjadi karena JMeter juga dipengaruhi oleh ukuran response, jaringan lokal, dan kondisi server. Sementara itu, profiler lebih fokus ke waktu eksekusi method. Untuk menangani hal ini, saya menggunakan keduanya bersama-sama: JMeter untuk melihat dampak ke user, dan profiler untuk melihat penyebab teknis di dalam kodenya.

### 7. What strategies do you implement in optimizing application code after analyzing results from performance testing and profiling? How do you ensure the changes you make do not affect the application's functionality?
Strategi optimasi yang dilakukan adalah memperbaiki bagian kode yang terbukti menjadi bottleneck. Pada kasus ini, endpoint `/all-student` sebelumnya mengambil semua student, lalu melakukan query tambahan untuk setiap student. Ini menyebabkan jumlah query menjadi sangat banyak.

Optimasi dilakukan dengan mengganti pola tersebut menjadi satu query menggunakan fetch join, sehingga data `student_courses`, `students`, dan `courses` dapat diambil lebih efisien. Selain itu, method lain juga diperbaiki, seperti pencarian GPA tertinggi langsung dari repository dan penggabungan nama student menggunakan `String.join`.

Untuk memastikan perubahan tidak merusak fungsi aplikasi, saya menjalankan test dengan Maven dan membandingkan hasil response sebelum dan sesudah optimasi. Kemudian, performa endpoint diukur kembali untuk memastikan ada peningkatan minimal 20%.
