---
layout: post
title: Efisien Caching dengan Bloom Filter
date: 2021-08-21 23:39:47 +0700
categories: system-design
---
## Efisien Caching dengan Bloom Filter

Caching bisa meningkatkan performa aplikasi yang _heavy read_, karena mengurangi pembacaan ke IO seperti _database_. Bloom filter adalah struktur data yang dirancang untuk memberi tahu Anda, dengan cepat dan hemat memori, apakah suatu elemen ada dalam filter. Dalam Bloom filter ada kemungkinan terjadi _false positive_, tetapi tidak untuk _false negative_. Dengan kata lain, hasil yang dikembalikan adalah data "mungkin dalam set" atau "pasti tidak dalam set".
{: .text-justify}

Bloom filter memerlukan lebih dari fungsi hash dalam algoritmanya. Contoh – Misalkan kita ingin memasukkan “demo” ke dalam filter dengan menggunakan 2 fungsi hash dan array dengan panjang 10 bit, dimana semuanya disetel dengan nilai 0 awalnya. Adapun langkah - langkahnya sebagai berikut:
{: .text-justify}

- Pertama kita akan menghitung hash sebagai berikut:
~~~
  h1("demo") % 10 = 1
  h2("demo") % 10 = 2
~~~
![Bloom Filter 1](https://blog-firmantoari.s3.ap-southeast-1.amazonaws.com/bloom-filter-caching-1.png)
 
- Mapping hash yang diperoleh kedalam array. Untuk mengecek apakah data ada ddidalam filter maka semua nilai harus 1, jika tidak maka data tidak ada di dalam filter. Sebagai contoh sebagai berikut:
~~~
  h1("test") % 10 = 3
  h2("test") % 10 = 2
~~~
kata "test" mempunyai nilai value 3 dan 2, akan tetapi di dalam array index 3 memiliki value 0 sehingga kata tersebut tidak exist di dalam filter
![Bloom Filter 2](https://blog-firmantoari.s3.ap-southeast-1.amazonaws.com/bloom-filter-caching-2.png)


Sebagai demo, saya akan membuat pengecekan username apakah exist di dalam filter dimana untuk filter saya akan menggunakan memory cache. Adapun framework yang digunakan adalah spring boot (JAVA). Untuk kodenya sebagai berikut:
{: .text-justify}

```java
  <dependency>
	<groupId>com.google.guava</groupId>
	<artifactId>guava</artifactId>
	<version>29.0-jre</version>
  </dependency>
```
Snippet diatas adalam dependency package dimana saya menggunakan library [guava](https://mvnrepository.com/artifact/com.google.guava/guava/11.0)

```java
package com.example.demo;

import com.google.common.hash.BloomFilter;
import com.google.common.hash.Funnels;
import org.springframework.http.MediaType;
import org.springframework.util.MultiValueMap;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;

@RestController
public class HelloController {
    private BloomFilter<String> filter;

    public HelloController() {
       this.filter = BloomFilter.create(Funnels.unencodedCharsFunnel(), 100);
    }

    @GetMapping("/")
    public String index() {
        return "Greetings from Spring Boot!";
    }

    @PostMapping(
            path = "/add-username",
            consumes = {MediaType.APPLICATION_FORM_URLENCODED_VALUE})
    public boolean addUsername(@RequestBody MultiValueMap paramMap) {
        if (this.filter.mightContain((String) paramMap.getFirst("username"))) {
            return false;
        }
        this.filter.put((String) paramMap.getFirst("username"));
        return true;
    }

}
```

line 57 adalah inisialisasi Bloom filter dengan guava. Untuk logic pengecekan dimulai pada line 69 - 72. Data disimpan dalam _memory_, untuk _persistence_ data bisa menggunakan
redis.
{: .text-justify}
