---
published: false
---
## Efisien Caching dengan Bloom Filter

	Caching bisa meningkatkan performa aplikasi yang _heavy read_, karena mengurangi pembacaan ke IO seperti _database_. Bloom filter adalah struktur data yang dirancang untuk memberi tahu Anda, dengan cepat dan hemat memori, apakah suatu elemen ada dalam satu set. Dalam Bloom filter ada kemungkinan terjadi _false positive_, tetapi tidak untuk _false negative_. Dengan kata lain, hasil yang dikembalikan adalah data "mungkin dalam set" atau "pasti tidak dalam set".

	Contoh – Misalkan kita ingin memasukkan “demo” ke dalam filter, kita menggunakan 2 fungsi hash dan sedikit array dengan panjang 10, semuanya disetel ke 0 pada awalnya. Pertama kita akan menghitung hash sebagai berikut:
    > 	h1("demo") % 10 = 1
    	





