## Daftar Isi
* [Identitas](#identitas-penulis)
* [Langkah-langkah Pembuatan Aplikasi Django Baru](#langkah-langkah-pembuatan-aplikasi-baru)
* [Bagan Request Client dan Penjelasan](#bagan-request-client)
* [Alasan Penggunaan Python *Virtual Environment*](#alasan-penggunaan-python-virtual-environment)
* [*MVC*, *MVT*, dan *MVVM*](#mvc-mvt-dan-mvvm)
# Identitas Penulis
Nama    : Patrick Samuel Evans Simanjuntak
NPM     : 2206028251
Kelas   : PBP C

# Langkah-langkah Pembuatan Aplikasi Baru
Berikut adalah bagaimana saya mengimplementasikan 
## Membuat sebuah proyek Django baru.
Pertama, saya membuat direktori lokal bernama **pokemon_bo** dan direktori git bernama **pokemon_bo**. Setelah itu, saya menghubungkan kedua direktori tersebut. Untuk persiapan pembuatan proyek Django, saya melakukan instalasi beberapa *dependencies*. Saya juga menggunakan Python *virtual environment* untuk membantu mengisolasi *dependencies* satu proyek dengan yang lainnya. Untuk membuat proyek Django-nya sendiri, saya menggunakan *syntax*:
```
django-admin startproject pokemon_bo .
```

## Membuat aplikasi dengan nama **main** pada proyek pokemon_bo
Kedua, saya membuat aplikasi **main** pada proyek pokemon bo dengan menggunakan *syntax*:
```
python manage.py startapp main
```
Syntax tersebut berfungsi untuk membuat suatu aplikasi "kosong" dan masih bersifat kerangka. Aplikasi yang dihasilkan berupa folder bernama **main** yang berisi beberapa file dengan format Python yang dibutuhkan untuk membuat suatu aplikasi. Setelah selesai membuat aplikasi **main**, saya menambahkan aplikasi tersebut ke dalam variabel **INSTALLED_APPS** untuk mendaftarkan aplikasi tersebut pada proyek **pokemon bo**.

## Melakukan routing pada proyek agar dapat menjalankan aplikasi main
Ketiga, saya membuka **urls.py** yang terdapat pada direktori proyek. Setelah itu, saya menambahkan kode berikut:
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('main/', include('main.urls')),
    path('admin/', admin.site.urls),
]
```
Berkas tersebut berfungsi untuk mengatur rute URL pada tingkatan proyek. Oleh karena itu, saya menambahkan "path('main/', include('main.urls'))" untuk mengarahkan *request* ke rute yang telah diatur pada berkas **urls.py** aplikasi **main**. Fungsi include sendiri berfungsi untuk mengimpor rute URL dari aplikasi menuju ke **urls.py** proyek.

## Membuat model pada aplikasi main dengan nama Item
Keempat, saya membuat suatu model pada aplikasi main, tepatnya pada **models.py**. Pembuatan model dilakukan untuk menentukan kerangka dari database yang berisi data-data yang akan dimuat oleh aplikasi nantinya. Pembuatan model dilakukan dengan membuat kelas Item yang merupakan turunan dari kelas **models.Model**. Di dalam kelas tersebut, saya menambahkan atribut seperti name, date_added, price, amount, description.

## Membuat sebuah fungsi pada views.py untuk dikembalikan ke dalam sebuah template HTML yang menampilkan nama aplikasi serta nama dan kelas saya
Kelima, saya membuka berkas views.py lalu melakukan import render yang berasal dari django.shortcuts. Setelah itu, saya membuat suatu fungsi bernama show_main dengan parameter **request**. Fungsi ini berguna untuk mengatur permintaan HTTP dan mengembalikan tampilan yang diinginkan HTTP. Di dalam kelas tersebut, saya menambahkan variabel **context** yang merupakan sebuah dictionary yang berisi dua key, yaitu name dan class. Key name memiliki value berupa nama saya dan key class memiliki value berupa kelas PBP saya. Terakhir, fungsi show_main ini akan melakukan return dengan memanggil fungsi render dengan *syntax*:
```
return render(request, "main.html", context)
```
Kode tersebut berguna untuk melakukan render tampilan dari main.html melalui pemanggilan fungsi render. Fungsi render sendiri memiliki tiga parameter. Parameter request merupakan objek dari pengguna yang berupa permintaan HTTP. Parameter main.html merupakan berkas yang berfungsi untuk melakukan render tampilan. Parameter context merupakan dictionary yang berguna untuk meneruskan data-data yang dimuat didalamnya untuk digunakan pada tampilan secara dinamis.

##  Membuat sebuah routing pada urls.py aplikasi main untuk memetakan fungsi yang telah dibuat pada views.py
Pertama, saya membuka berkas urls.py di dalam direktori main. Lalu, saya melakukan import beberapa library yang dibutuhkan, seperti path dan show_main.
```
from django.urls import path
from main.views import show_main
```
Setelah itu, saya menginstansiasi variabel app_name dengan value 'main'. 
```
app_name = 'main'
```
Hal ini dilakukan untuk membantu Django membuat penamaan URL masing-masing aplikasi secara unik untuk mencegah konflik pada URL antar aplikasi. Setelah itu, saya membuat variabel baru bernama urlpatterns yang berisi pola URL pada masing-masing aplikasi. Fungsi path di dalam 'urlpatterns' berfungsi untuk menentukan pola URL pada masing-masing fungsi yang bertugas mengelola tampilan. Dalam kode ini, fungsi yang dikelola adalah fungsi 'show_main'.
Berikut adalah keseluruhan kode dari urls.py saya:
```
from django.urls import path
from main.views import show_main

app_name = 'main'

urlpatterns = [
    path('', show_main, name='show_main'),
]
```

## Melakukan deployment ke Adaptable terhadap aplikasi yang sudah dibuat
Pertama, saya masuk ke laman adaptable.io dengan menggunakan akun github. Selanjutnya, saya memilih untuk melakukan deploy dari *repository* github saya. Lalu, saya memilih *repository* pokemon-bo dan branch main. Saya pun memilih Python App Template sebagai template deployment dan PostgreSQL sebagai tipe dari basis data yang akan digunakan. Selanjutnya, saya memiliih versi Python sesuai dengan versi Python yang saya gunakan dalam projek ini, yaitu versi 3.11. Selanjutnya saya menambahkan kode berikut pada kolom Start Command:
```
python manage.py migrate && gunicorn pokemon_bo.wsgi
```

# Bagan *Request Client*
Berikut adalah bagan yang berisi *request client* ke *web* aplikasi berbasis Django beserta responnya:
![image](./bagan.png)
Bagan diatas menunjukkan bagaimana *client* melakukan *request* dan mendapatkan umpan balik dari *web*. **urls.py** merupakan berkas yang berfungsi untuk menentukan pola URL yang akan digunakan pada aplikasi. Dapat dilihat pada bagan, **urls.py** menerima HTTP *request* dan menangani *request* tersebut dengan meneruskan *request* tersebut ke **view.py**. Model (**models.py**) adalah berkas yang berisi definisi dari model yang terdiri dari berbagai jenis data. **models.py** dapat membaca maupun menulis data pada **views.py**. Berkas HTML (**main.html**) berfungsi untuk menyusun dan membuat desain tampilan pada *web*. Nantinya, data-data dari **main.html** akan dirender oleh **views.py**. Setelah **views.py** memproses data-data yang diterima dari **urls.py**, **model.py**, dan **main.html**. **views.py** akan memproses dan mengatur bagaimana data-data tersebut akan ditampilkan. Lalu setelah selesai, **views.py** akan mengeluarkan *output* berupa HTTP *response* (HTML).

# Alasan Penggunaan Python *Virtual Environment*
Python *virtual environment* adalah sebuah aplikasi yang berguna untuk mengisolasi suatu proyek Python dari sistem Python yang telah terinstalasi pada sistem. Alasan saya menggunakan *virtual environment* adalah adanya isolasi lingkungan dimana setiap proyek dapat berjalan sendiri-sendiri dengan versi *library*-nya masing-masing. Contohnya, saya memiliki proyek A dan B. Saya menggunakan Django 1.0 pada proyek A dan Django 1.2 pada proyek B. Selain itu, *virtual environment* juga dapat membantu saya dalam mengelola dependensi dimana terdapat **requirements.txt** yang dapat kita instal dan perbarui dengan mudah. Pembuatan proyek dan aplikasi Django tanpa *virtual environment* masih dimungkinkan. Akan tetapi, hal tersebut memiliki risiko tinggi karena memungkinkan terjadinya ketidakcocokan *library* satu dengan yang lain, sehingga dapat menyebabkan eror.

# MVC, MVT, dan MVVM
Berikut adalah penjelasan mengenai MVC, MVT, dan MVVM. Ketiga pola tersebut merupakan pola-pola arsitektur yang biasa digunakan pada *framework-framework* perangkat lunak.
## MVC (Model-View-Controller)
MVC adalah suatu pola perangkat lunak yang membagi kode menjadi tiga bagian.
Pada MVC, model berfungsi untuk menyimpan data-data dari aplikasi tanpa mengetahui apa yang terjadi pada *interface*. View berguna sebagai pengendali dari *interface* yang mengatur semua hal yang berkaitan dengan tampilan serta interaksi dengan pengguna. Controller berfungsi untuk menjembatani View dan Model. Controller mengandung logika dari aplikasi. Controller juga mendapatkan *update* dari respon pengguna dan memperbarui Model bila dibutuhkan. *Framework* yang menggunakan MVC adalah Ruby, Spring, dan Express.js.

## MVT (Model-View-Template)
MVT adalah pola perangkat lunak yang membagi kode menjadi tiga, yaitu Model, View, dan Template. Model berfungsi untuk mengurus *database* dari aplikasi. View berfungsi untuk memproses logika dan berinteraksi dengan model untuk membawa data dan me-*render* suatu *template*. Template sendiri merupakan suatu bagian yang mengurus interaksi dengan pengguna. Contoh *framework* yang menggunakan MVT adalah Django.

## MVVM (Model-View-ViewModel)
Pola arsitektur ini terbagi menjadi tiga, yaitu Model, View, dan ViewModel. Model berfungsi untuk melakukan abstraksi pada sumber data dan merupakan pusat logika dari program. View merupakan bagian yang berguna untuk mengontrol elemen-elemen visual dan elemen-elemen yang berhubungan dengan interaksi pengguna. ViewModel merupakan bagian yang berfungsi untuk menghubungkan View dan Model. *Framework* yang menggunakan MVVM adalah Angular, Vue.js, dan Knockout.js.

## Perbedaan Ketiga Pola Arsitektur
MVC membagi pengelolaan aplikasi menjadi tiga bagian, yaitu Model, View, dan Controller dimana View mengurus tampilan dan Controller mengurus alur kontrol aplikasi. Pada MVT, View menjadi bagian yang mengontrol logika dari aplikasi dan Template bertugas untuk mengelola visual aplikasi. Pada MVVM, view berfungsi sebagai pengatur elemen visual, tetapi MVVM memiliki satu bagian lain, yaitu ViewModel. ViewModel berfungsi sebagai penghubung antara View dan Model. Jadi, setiap pola arsitektur memiliki bagian-bagian yang memiliki fungsi yang berbeda-beda. Walaupun terdapat bagian dengan nama yang sama, bagian tersebut memiliki fungsi yang berbeda-beda.

Sekian pembahasan dari saya. Mohon maaf bila ada kesalahan. Have a good day!:grinning::smile: