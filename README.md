# Instalasi Docker
1. 'sudo yum check-update' digunakan untuk memeriksa pembaruan paket yang tersedia untuk sistem yang menjalankan CentOS. Sebelum instalasi Docker, ini direkomendasikan untuk memastikan bahwa sistem menggunakan versi terbaru dari paket yang diperlukan oleh Docker dan untuk memastikan kompatibilitas dengan sistem yang ada. Hal ini juga dapat membantu dalam mencegah konflik versi atau masalah lain yang mungkin timbul selama instalasi.
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/0eec7ecc-44b6-488c-9c9d-4fe35f09918b)
2. 'sudo yum install docker' digunakan untuk menginstal Docker pada sistem yang menjalankan CentOS dengan mengunduh dan menginstal paket-paket yang diperlukan untuk menjalankan Docker, sehingga dapat mulai menggunakan Docker untuk mengelola kontainer pada sistem.
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/a434c62b-fc18-4610-923c-c713b5202ee0)

3. 'sudo systemctl start docker' digunakan untuk start service Docker pada sistem yang menjalankan CentOS.
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/0aefc247-3c26-47aa-bff7-4f1edb409122)

5. 'sudo systemctl enable docker' digunakan untuk mengaktifkan service Docker agar otomatis di-start saat sistem boot.
6. 'docker --version' digunakan untuk menampilkan versi Docker yang terinstal pada sistem tanpa perlu masuk ke dalam kontainer atau menjalankan perintah Docker lainnya.
