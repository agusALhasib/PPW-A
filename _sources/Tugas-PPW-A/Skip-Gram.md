---
title: 210411100227 Agus Muhammad Al Hasib

---

## Word Embedding
Word embeddings adalah proses konversi kata yang berupa karakter alphanumeric kedalam bentuk vector. Setiap kata adalah vector yang merepresentasikan sebuah titik pada space dengan dimensi tertentu. Dengan word embedding, kata-kata yang memiliki properti tertentu, misalnya berada pada konteks yang sama, atau memiliki semantic meaning yang sama berada tidak jauh satu sama lain pada space tersebut.

Beberapa contoh model yang digunakan untuk membangun word embedding meliputi:

* Word2Vec: Model yang mampu memprediksi kata-kata di sekitar berdasarkan kata yang diberikan.
* Skip-gram: Bagian dari Word2Vec yang mencoba memprediksi kata di sekitar (context) dari sebuah kata target.
* GloVe (Global Vectors for Word Representation): Model yang memanfaatkan statistik global dari sebuah corpus teks untuk menghasilkan vektor representasi kata.

## Skip-Gram
**Skip-gram** adalah sebuah metode dalam **Word2Vec** yang digunakan untuk mengubah kata-kata menjadi angka-angka (vektor) sehingga komputer bisa memahaminya. Cara kerjanya sederhana: metode ini mempelajari hubungan antar kata dengan melihat kata-kata yang sering muncul di sekitar satu kata tertentu dalam sebuah kalimat.

Misalnya, jika kita punya kalimat **"Saya suka makan nasi"**, dan kata targetnya adalah "makan", metode skip-gram akan mencoba menebak kata-kata di sekitar "makan", yaitu "saya", "suka", dan "nasi". Jadi, skip-gram mempelajari hubungan kata berdasarkan seberapa sering kata-kata itu muncul berdekatan.

Dengan begitu, kata-kata yang sering muncul bersama akan memiliki representasi vektor yang mirip atau dekat satu sama lain, membuat komputer bisa lebih memahami hubungan antara kata-kata dalam teks.

## Arsitektur Skip-Gram
Arsitektur model skip-gram terdiri dari lapisan input, lapisan output, dan hidden layer.

![1_yuMyr323aSnzivITzCSt_g](https://hackmd.io/_uploads/ryKoz1SRR.png)
Tujuan dari arsitektur skip-gram adalah untuk memprediksi konteks (output) di sekitar current word (input).

## Perhitungan Skip-Gram
**Contoh Perhitungan Model Skip-Gram**
Contoh Kalimat = "**Andy makan soto lamongan dengan teman**"

**Representasi One-Hot Encoding**

mempresentasikan ke dalam bentuk vektor. membuat representasi one-hot encoding untuk setiap kata dalam kalimat tersebut.

| Kata      | Andy | Makan | Soto | Lamongan | Dengan | Teman |
|-----------|------|-------|------|----------|--------|-------|
| Andy      |  1   |   0   |  0   |    0     |   0    |   0   |
| Makan     |  0   |   1   |  0   |    0     |   0    |   0   |
| Soto      |  0   |   0   |  1   |    0     |   0    |   0   |
| Lamongan  |  0   |   0   |  0   |    1     |   0    |   0   |
| Dengan    |  0   |   0   |  0   |    0     |   1    |   0   |
| Teman     |  0   |   0   |  0   |    0     |   0    |   1
|

## Pasangan Kata
Berikut adalah pasangan kata dan tetangganya

| Target  | Tetangga               |
|---------|------------------------|
| Andy    | Makan                  |
| Makan   | Andy, Soto             |
| Soto    | Makan, Lamongan        |
| Lamongan| Soto, Dengan           |
| Dengan  | Lamongan, Teman        |
| Teman   | Dengan                 |

## Membuat Vektor Embedding
Selanjutnya yaitu rubah kata menjadi bentuk vektor. Lalu tentukan ukuran dimensi nya menjadi = 3
Berikut adalah nilai embedding acak untuk setiap kata sebagai berikut:


| Kata      | Vektor Embedding     |
|-----------|----------------------|
| Andy      | [0.1, 0.2, 0.3]      |
| Makan     | [0.4, 0.5, 0.6]      |
| Soto      | [0.7, 0.8, 0.9]      |
| Lamongan  | [0.2, 0.3, 0.1]      |
| Dengan    | [0.5, 0.7, 0.6]      |
| Teman     | [0.3, 0.6, 0.4]      |

## Iterasi 1
menghitung representasi untuk kata "makan" menggunakan tetangga kiri dan kanan yaitu 'Andy' dan 'soto'.

1. Untuk "makan" dengan tetangga kiri 'Andy':
   $$ 
   \text{Vector } makan = [0.4, 0.5, 0.6] 
   $$
   $$ 
   \text{Vector } Andy = [0.1, 0.2, 0.3] 
   $$

2. Untuk "makan" dengan tetangga kanan 'soto':
   $$ 
   \text{Vector } soto = [0.7, 0.8, 0.9] 
   $$

   Sehingga didapatkan representasi awal dari kata *makan* dengan rata-rata tetangganya:
   $$ 
   \text{Vector } makan = \frac{[0.1, 0.2, 0.3] + [0.7, 0.8, 0.9]}{2} = [0.4, 0.5, 0.6] 
   $$

**note** :
Perhitungan di atas merupakan bagian dari iterasi pertama, di mana kita menggunakan pasangan kata-tetangga untuk membentuk representasi awal dari kata. Setelah beberapa iterasi, representasi vektor akan diperbarui dan menjadi lebih akurat dalam merepresentasikan hubungan semantik antar kata.


# Iterasi 2
Menggunakan contoh kata yang sama dengan kata target "makan" dan tetangganya "Andy" serta "soto". Setelah iterasi pertama, kita menghitung error dan melakukan pembaruan vektor pada kata "makan".

**Vektor Awal dari Iterasi Pertama:**
Setelah iterasi pertama, kita telah menghitung representasi sementara sebagai berikut:
- $$ \text{Vektor } makan = [0.4, 0.5, 0.6] $$
- $$ \text{Vektor } Andy = [0.1, 0.2, 0.3] $$
- $$ \text{Vektor } soto = [0.7, 0.8, 0.9] $$

**Pembaruan Vektor Embedding untuk "makan"**
Untuk memperbarui vektor embedding untuk kata target "makan", kita menggunakan rumus:

$$ 
\text{vektor(makan)} = \text{vektor(makan)} - \text{learning rate} \times \text{gradien error} 
$$

Misalkan gradien yang dihasilkan adalah 0.05 untuk setiap dimensi vektor, dan kita menggunakan learning rate sebesar 0.1. ]erhitungannya sebagai berikut :

**Perhitungan**
1. Dimensi pertama vektor "makan":
   $$ 
   \text{vektor makan (baru)} = 0.4 - (0.1 \times 0.05) = 0.395 
   $$
   
2. Dimensi kedua vektor "makan":
   $$ 
   \text{vektor makan (baru)} = 0.5 - (0.1 \times 0.05) = 0.495 
   $$
   
3. Dimensi ketiga vektor "makan":
   $$ 
   \text{vektor makan (baru)} = 0.6 - (0.1 \times 0.05) = 0.595 
   $$

Setelah pembaruan, vektor embedding untuk "makan" adalah:
$$ 
\text{Vektor } "makan" = [0.395, 0.495, 0.595] 
$$

**Pembaruan Vektor "Andy"**
1. Dimensi pertama vektor "Andy":
   $$ 
   \text{vektor Andy (baru)} = 0.1 - (0.1 \times 0.05) = 0.095 
   $$
   
2. Dimensi kedua vektor "Andy":
   $$ 
   \text{vektor Andy(baru)} = 0.2 - (0.1 \times 0.05) = 0.195 
   $$
   
3. Dimensi ketiga vektor "Andy":
   $$ 
   \text{vektor Andy (baru)} = 0.3 - (0.1 \times 0.05) = 0.295 
   $$

Hasil Vektor "Andy" setelah Iterasi Kedua:
$$ 
\text{Vektor } Andy = [0.095, 0.195, 0.295] 
$$

**Pembaruan Vektor "soto"**
1. Dimensi pertama vektor "soto":
   $$ 
   \text{vektor soto (baru)} = 0.7 - (0.1 \times 0.05) = 0.695 
   $$
   
2. Dimensi kedua vektor "soto":
   $$ 
   \text{vektor soto (baru)} = 0.8 - (0.1 \times 0.05) = 0.795 
   $$
   
3. Dimensi ketiga vektor "soto":
   $$ 
   \text{vektor soto (baru)} = 0.9 - (0.1 \times 0.05) = 0.895 
   $$

Setelah pembaruan, vektor embedding "soto" menjadi:
$$ 
\text{Vektor } soto = [0.695, 0.795, 0.895] 
$$

**Rekapitulasi Vektor Embedding**

Setelah iterasi kedua, berikut adalah vektor embedding yang diperbarui untuk kata "makan", "Andy", dan "soto":
* Vektor makan = [0.395, 0.495, 0.595] $$
* Vektor soto = [0.695, 0.795, 0.895] $$
* Vekrto Andy = [0.095, 0.195, 0.295] $$

### Kesimpulan
Pada iterasi kedua, kita telah memperbarui vektor embedding untuk kata "makan", "Andy", dan "soto" berdasarkan hasil dari iterasi pertama dengan menggunakan backpropagation dan pembaruan bobot (embedding).














