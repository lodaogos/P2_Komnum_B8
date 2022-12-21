### Praktikum 2 [ Komputasi Numerik B ] Kelompok 8
| Nama                      | NRP           |
|---------------------------|---------------|
|Moh rosy haqqy aminy       |5025211012     |
|Charles                    |5025211082     |
|Hammuda Arsyad             |5025211146     |

### Library Yang Digunakan
Library yang digunakan adalah pandas, numpy, matplotlib, dan Equation

### Code

```python
# Tugas Praktikum Membuat Metode Romberg Menggunakan Python Kelompok 8

# Ditulis oleh :
#   Moh. Rosy Haqqy Aminy (5025211012)
#   Hammuda Arsyad (5025211146)
#   Charles (5025211082)

# Problem : Membuat metode romberg dan mengerjakan soal yang telah diberikan sebelumnya
# Solve : Menggunakan library python pandas sebagai representasi tabel kemudian buat
#         algoritma rombergnya untuk menyelesaikan masalah


import math
import numpy as np
import pandas as pd
from Equation import Expression

# fungsi untuk mengembalikan suatu string persamaan menjadi persamaan matematika
def f(x):
    return (Expression(x))

# fungsi untuk mencari luas dengan cara trapezoida
def trapezoidal(start, end, n, f):
    h = float((end - start) / n)
    ans = f(start) + f(end)
    while(start + h != end):
        ans += 2 * f(start + h)
        start += h
    return ans * (h / 2)


# fungsi untuk mencari integrasi romberg
def romberg(equ, a, b, n):

    # Pertama kita cari dulu nilai pada setiap titik fungsi
    fx = f(equ)
    tabel = pd.DataFrame()
    h = (b-a)/n
    step = np.arange(a, b + h, h)
    for i in step:
        x = i
        y = fx(i)
        tab = pd.DataFrame([[i, x, y]], columns=['n', 'x', 'y'])
        tabel = pd.concat([tabel, tab], ignore_index=True)
        

    # Tampilkan nilai-nilai yang didapatkan dalam bentuk tabel
    tabel2 = tabel.to_string(index=False)
    print(f"\nTabel dalam selang [{a},{b}] dengan h = {h}")
    print(tabel2)

    # Buat sebuah 2d array/list untuk menyimpan nilai yang kita butuhkan nnti
    listRomberg = list()

    # Cari nilai dengan metode trapezoida
    count = 0
    tabelRomberg = pd.DataFrame()
    i = 1
    arr = list()
    while i <= n:
        print(trapezoidal(a, b, i, fx))
        arr.append(trapezoidal(a, b, i, fx))
        i = i*2
        count = count + 1
    listRomberg.append(arr)


    # Menggunakan ekstrapolasi richardson
    totalKosong = 1
    while totalKosong != count:
        j = totalKosong
        arrTemp = list()
        for i in range(j):
            arrTemp.append(None)
        while j < count:
            arrTemp.append(listRomberg[totalKosong-1][j]+(listRomberg[totalKosong-1][j]-listRomberg[totalKosong-1][j-1])/(pow(2,2*totalKosong)-1))
            j = j + 1
        s = "O(h^" + str(2*(totalKosong+1)) + ")"
        listRomberg.append(arrTemp)
        totalKosong = totalKosong + 1


    # Mengubah semua yang ada di dalam list tersebut ke dalam bentuk tabel dengan nama kolom tertentu
    x = 1
    for i in listRomberg:
        s = "O(h^" + str(2*x) + ")"
        x = x + 1
        tabelRomberg[s] = i

    # Tampilkan tabel
    print("\nTabel Romberg : ")
    print(tabelRomberg)

    # Cari jawaban dan tampilkan
    ans = listRomberg[len(listRomberg)-1][len(listRomberg[len(listRomberg)-1])-1]
    print(f"\nJawaban : {ans}")


# Input dari user
print("Program Integrasi Romberg!")
equ = input("Masukkan persamaan (contoh : 1/(9+x)) : ")
print("Masukkan batas bawah dan batas atas")
a = int(input("Batas bawah : "))
b = int(input("Batas atas : "))
n = int(input("Jumlah n : "))

# Panggil metode romberg
romberg(equ, a, b, n)
