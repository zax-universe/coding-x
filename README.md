# 📚 Materi Seputar Programming & Coding Reference

---

## 🗂️ Daftar Isi

- [Apa itu Pemrograman?](#-apa-itu-pemrograman)
- [Bahasa Pemrograman](#-bahasa-pemrograman)
  - [Low-Level Languages](#low-level-languages)
  - [High-Level Languages](#high-level-languages)
  - [Scripting Languages](#scripting-languages)
  - [Query Languages](#query-languages)
  - [Markup & Stylesheet Languages](#markup--stylesheet-languages)
- [Paradigma Pemrograman](#-paradigma-pemrograman)
- [Struktur Data](#-struktur-data)
- [Algoritma](#-algoritma)
- [Konsep Dasar Pemrograman](#-konsep-dasar-pemrograman)
- [Object-Oriented Programming (OOP)](#-object-oriented-programming-oop)
- [Functional Programming](#-functional-programming)
- [Version Control](#-version-control)
- [Web Development](#-web-development)
- [Mobile Development](#-mobile-development)
- [Database](#-database)
- [DevOps & Cloud](#-devops--cloud)
- [Kecerdasan Buatan & Machine Learning](#-kecerdasan-buatan--machine-learning)
- [Keamanan / Cybersecurity](#-keamanan--cybersecurity)
- [Testing](#-testing)
- [Design Patterns](#-design-patterns)
- [Clean Code & Best Practices](#-clean-code--best-practices)
- [Kompleksitas Algoritma (Big O)](#-kompleksitas-algoritma-big-o)
- [Tools & IDE](#-tools--ide)
- [Tips Belajar Pemrograman](#-tips-belajar-pemrograman)

---

## 💡 Apa itu Pemrograman?

**Pemrograman** (atau *coding*) adalah proses menulis instruksi yang dapat dijalankan oleh komputer untuk menyelesaikan tugas tertentu. Instruksi ini ditulis menggunakan **bahasa pemrograman** — bahasa formal yang memiliki sintaks dan semantik yang ketat.

```
Manusia → Kode (Bahasa Pemrograman) → Kompilator/Interpreter → Komputer
```

### Perbedaan Compiler vs Interpreter

| | Compiler | Interpreter |
|---|---|---|
| **Cara kerja** | Menerjemahkan seluruh kode sekaligus | Menerjemahkan baris per baris |
| **Kecepatan eksekusi** | Lebih cepat | Lebih lambat |
| **Contoh bahasa** | C, C++, Go, Rust | Python, JavaScript, Ruby |
| **Output** | File executable | Langsung dijalankan |

---

## 🌐 Bahasa Pemrograman

### Low-Level Languages

Bahasa yang dekat dengan bahasa mesin, memberikan kontrol penuh atas hardware.

#### Assembly Language
```asm
section .text
    global _start
_start:
    mov eax, 1        ; syscall: write
    mov ebx, 1        ; file descriptor: stdout
    mov ecx, msg      ; pointer to message
    mov edx, 13       ; message length
    int 0x80          ; call kernel
    
    mov eax, 60       ; syscall: exit
    xor ebx, ebx
    int 0x80

section .data
    msg db "Hello World!", 0x0A
```

#### C
```c
#include <stdio.h>
#include <stdlib.h>

// Fungsi rekursif Fibonacci
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

int main() {
    int n = 10;
    printf("Fibonacci(%d) = %d\n", n, fibonacci(n));
    
    // Manajemen memori manual
    int *arr = (int*)malloc(5 * sizeof(int));
    for (int i = 0; i < 5; i++) arr[i] = i * i;
    free(arr); // Wajib dibebaskan!
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

class Stack {
private:
    std::vector<int> data;
public:
    void push(int val) { data.push_back(val); }
    int pop() {
        int top = data.back();
        data.pop_back();
        return top;
    }
    bool isEmpty() { return data.empty(); }
};

int main() {
    Stack s;
    s.push(10);
    s.push(20);
    std::cout << s.pop() << std::endl; // Output: 20
    return 0;
}
```

#### Rust
```rust
fn main() {
    // Ownership system — tidak ada garbage collector!
    let s1 = String::from("hello");
    let s2 = s1; // s1 dipindah ke s2, s1 tidak bisa digunakan lagi

    let v = vec![1, 2, 3, 4, 5];
    let doubled: Vec<i32> = v.iter().map(|x| x * 2).collect();
    println!("{:?}", doubled); // [2, 4, 6, 8, 10]
}
```

---

### High-Level Languages

#### Python
```python
# Python — elegan, readable, serbaguna
from dataclasses import dataclass
from typing import List

@dataclass
class Mahasiswa:
    nama: str
    nim: str
    ipk: float

    def lulus(self) -> bool:
        return self.ipk >= 2.0

def ranking(mahasiswas: List[Mahasiswa]) -> List[Mahasiswa]:
    return sorted(mahasiswas, key=lambda m: m.ipk, reverse=True)

# List comprehension
angka = [x**2 for x in range(10) if x % 2 == 0]
print(angka)  # [0, 4, 16, 36, 64]

# Context manager
with open("data.txt", "r") as f:
    isi = f.read()
```

#### Java
```java
import java.util.*;
import java.util.stream.*;

public class Main {
    // Generics
    public static <T extends Comparable<T>> T findMax(List<T> list) {
        return list.stream()
                   .max(Comparator.naturalOrder())
                   .orElseThrow();
    }

    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6);
        
        // Stream API
        List<Integer> sorted = numbers.stream()
            .distinct()
            .sorted()
            .collect(Collectors.toList());
        
        System.out.println("Max: " + findMax(numbers)); // 9
        System.out.println("Sorted: " + sorted);
    }
}
```

#### Go (Golang)
```go
package main

import (
    "fmt"
    "sync"
)

// Goroutines & Channels — concurrency native
func worker(id int, jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
    defer wg.Done()
    for j := range jobs {
        results <- j * j
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)
    var wg sync.WaitGroup

    for w := 1; w <= 3; w++ {
        wg.Add(1)
        go worker(w, jobs, results, &wg)
    }

    for j := 1; j <= 9; j++ {
        jobs <- j
    }
    close(jobs)

    wg.Wait()
    close(results)

    for r := range results {
        fmt.Println(r)
    }
}
```

#### Kotlin
```kotlin
data class User(val name: String, val age: Int)

fun main() {
    val users = listOf(
        User("Alice", 25),
        User("Bob", 30),
        User("Charlie", 22)
    )

    // Extension functions & lambdas
    val adults = users
        .filter { it.age >= 25 }
        .sortedBy { it.name }
        .map { "${it.name} (${it.age})" }

    println(adults) // [Alice (25), Bob (30)]

    // Null safety
    val nama: String? = null
    println(nama?.length ?: "Nama kosong")
}
```

#### Swift
```swift
// Swift — bahasa utama iOS/macOS
struct Point {
    var x: Double
    var y: Double
    
    func distance(to other: Point) -> Double {
        let dx = x - other.x
        let dy = y - other.y
        return (dx*dx + dy*dy).squareRoot()
    }
}

// Optional chaining
let dict: [String: [Int]] = ["angka": [1, 2, 3]]
let first = dict["angka"]?.first ?? 0
print(first) // 1

// Closures
let numbers = [5, 3, 8, 1, 9]
let sorted = numbers.sorted { $0 < $1 }
print(sorted) // [1, 3, 5, 8, 9]
```

---

### Scripting Languages

#### JavaScript
```javascript
// Modern JS — ES2024
const fetchData = async (url) => {
    try {
        const res = await fetch(url);
        const data = await res.json();
        return data;
    } catch (err) {
        console.error("Error:", err.message);
    }
};

// Destructuring & Spread
const { name, age, ...rest } = { name: "Budi", age: 25, city: "Jakarta" };

// Array methods
const angka = [1, 2, 3, 4, 5];
const hasil = angka
    .filter(n => n % 2 !== 0)
    .map(n => n ** 2)
    .reduce((acc, n) => acc + n, 0);

console.log(hasil); // 35 (1 + 9 + 25)

// Optional chaining
const user = { address: { city: "Bandung" } };
console.log(user?.address?.zip ?? "Tidak ada zip");
```

#### TypeScript
```typescript
// TypeScript — JS dengan tipe statis
interface Repository<T> {
    findById(id: number): Promise<T | null>;
    findAll(): Promise<T[]>;
    save(entity: T): Promise<T>;
    delete(id: number): Promise<void>;
}

type ApiResponse<T> = {
    data: T;
    status: number;
    message: string;
    timestamp: Date;
};

// Generic function dengan constraint
function mergeObjects<T extends object, U extends object>(a: T, b: U): T & U {
    return { ...a, ...b };
}

const merged = mergeObjects({ name: "Budi" }, { age: 25 });
console.log(merged.name, merged.age);
```

#### PHP
```php
<?php
// PHP modern (8.x)
class DatabaseConnection {
    private static ?self $instance = null;
    
    private function __construct(
        private readonly string $host,
        private readonly string $db
    ) {}
    
    public static function getInstance(): self {
        if (self::$instance === null) {
            self::$instance = new self("localhost", "mydb");
        }
        return self::$instance;
    }
}

// Match expression (PHP 8)
$status = 2;
$label = match($status) {
    1 => "Aktif",
    2 => "Non-aktif",
    3 => "Pending",
    default => "Unknown"
};
echo $label; // Non-aktif
?>
```

#### Ruby
```ruby
# Ruby — ekspresif dan elegan
class Animal
  attr_accessor :name, :sound
  
  def initialize(name, sound)
    @name = name
    @sound = sound
  end
  
  def speak
    "#{@name} berkata: #{@sound}!"
  end
end

# Blocks & Iterators
[1, 2, 3, 4, 5].each_with_object([]) do |n, acc|
  acc << n ** 2 if n.odd?
end
# => [1, 9, 25]

# Symbol to proc
words = ["hello", "world", "ruby"]
puts words.map(&:upcase).join(", ")
# => HELLO, WORLD, RUBY
```

---

### Query Languages

#### SQL
```sql
-- Structured Query Language
-- DDL: Data Definition Language
CREATE TABLE mahasiswa (
    id          SERIAL PRIMARY KEY,
    nim         VARCHAR(15) UNIQUE NOT NULL,
    nama        VARCHAR(100) NOT NULL,
    jurusan_id  INTEGER REFERENCES jurusan(id),
    ipk         DECIMAL(3,2) CHECK (ipk BETWEEN 0.00 AND 4.00),
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- DML: Data Manipulation Language
INSERT INTO mahasiswa (nim, nama, jurusan_id, ipk)
VALUES ('2021001', 'Budi Santoso', 1, 3.75);

-- JOIN query kompleks
SELECT 
    m.nama,
    m.nim,
    j.nama_jurusan,
    AVG(n.nilai) AS rata_nilai,
    RANK() OVER (PARTITION BY j.id ORDER BY AVG(n.nilai) DESC) AS ranking
FROM mahasiswa m
JOIN jurusan j ON m.jurusan_id = j.id
LEFT JOIN nilai n ON m.id = n.mahasiswa_id
WHERE m.ipk > 3.0
GROUP BY m.id, m.nama, m.nim, j.id, j.nama_jurusan
HAVING AVG(n.nilai) >= 75
ORDER BY rata_nilai DESC;
```

---

### Markup & Stylesheet Languages

#### HTML5
```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contoh HTML5 Semantik</title>
</head>
<body>
    <header>
        <nav aria-label="Navigasi utama">
            <ul>
                <li><a href="#beranda">Beranda</a></li>
                <li><a href="#tentang">Tentang</a></li>
            </ul>
        </nav>
    </header>
    <main>
        <article>
            <h1>Judul Artikel</h1>
            <p>Konten artikel...</p>
            <figure>
                <img src="gambar.jpg" alt="Deskripsi gambar">
                <figcaption>Keterangan gambar</figcaption>
            </figure>
        </article>
        <aside>Konten sampingan</aside>
    </main>
    <footer>
        <p>&copy; 2025 Nama Website</p>
    </footer>
</body>
</html>
```

#### CSS3
```css
/* Modern CSS — Grid, Flexbox, Custom Properties */
:root {
    --primary: #2563eb;
    --secondary: #64748b;
    --font-size-base: clamp(1rem, 2.5vw, 1.25rem);
}

/* CSS Grid Layout */
.grid-layout {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 1.5rem;
}

/* Flexbox */
.card {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 1.5rem;
    border-radius: 0.75rem;
    box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
    transition: transform 0.2s ease;
}

.card:hover {
    transform: translateY(-4px);
}

/* Animasi */
@keyframes fadeInUp {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0); }
}

/* Media Query */
@media (max-width: 768px) {
    .grid-layout {
        grid-template-columns: 1fr;
    }
}
```

---

## 🧠 Paradigma Pemrograman

| Paradigma | Deskripsi | Contoh Bahasa |
|---|---|---|
| **Imperatif** | Menjelaskan *bagaimana* melakukan sesuatu langkah demi langkah | C, Pascal |
| **Prosedural** | Kode terorganisasi dalam fungsi/prosedur | C, BASIC, Fortran |
| **OOP** | Kode terorganisasi dalam objek yang memiliki state dan perilaku | Java, C++, Python |
| **Fungsional** | Fungsi murni tanpa side effects, immutability | Haskell, Erlang, Clojure |
| **Deklaratif** | Menjelaskan *apa* yang diinginkan, bukan bagaimana | SQL, HTML, Prolog |
| **Event-Driven** | Alur program ditentukan oleh event | JavaScript, C# |
| **Reaktif** | Pemrograman berbasis aliran data asinkron | RxJS, ReactiveX |
| **Logic** | Komputasi berdasarkan logika formal | Prolog |
| **Concurrent** | Eksekusi paralel | Go, Erlang |

---

## 📦 Struktur Data

### Array / List
```python
# Array statis: ukuran tetap, akses O(1)
# List dinamis: ukuran fleksibel

arr = [10, 20, 30, 40, 50]
print(arr[2])     # 30 — akses O(1)
arr.append(60)    # tambah akhir O(1)
arr.insert(0, 5)  # tambah awal O(n)
```

### Linked List
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
    
    def append(self, data):
        node = Node(data)
        if not self.head:
            self.head = node
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = node
    
    def display(self):
        elements = []
        current = self.head
        while current:
            elements.append(str(current.data))
            current = current.next
        print(" -> ".join(elements))
```

### Stack & Queue
```python
from collections import deque

# Stack (LIFO)
stack = []
stack.append(1)   # push
stack.append(2)
stack.pop()       # pop → 2

# Queue (FIFO)
queue = deque()
queue.append(1)   # enqueue
queue.append(2)
queue.popleft()   # dequeue → 1
```

### Hash Table / Dictionary
```python
# Lookup O(1) rata-rata
phone_book = {
    "Budi":  "0812-1234-5678",
    "Siti":  "0821-8765-4321",
    "Ahmad": "0856-1111-2222"
}

# Collision resolution: chaining / open addressing
```

### Tree
```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

# Binary Search Tree (BST)
def insert(root, val):
    if root is None:
        return TreeNode(val)
    if val < root.val:
        root.left = insert(root.left, val)
    else:
        root.right = insert(root.right, val)
    return root

# In-order traversal → sorted
def inorder(root):
    if root:
        inorder(root.left)
        print(root.val, end=" ")
        inorder(root.right)
```

### Graph
```python
# Representasi dengan adjacency list
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

# BFS (Breadth-First Search)
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    
    while queue:
        node = queue.popleft()
        print(node, end=" ")
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

### Heap / Priority Queue
```python
import heapq

# Min-heap
heap = []
heapq.heappush(heap, 5)
heapq.heappush(heap, 1)
heapq.heappush(heap, 3)

print(heapq.heappop(heap))  # 1 (terkecil)
```

---

## ⚡ Algoritma

### Sorting Algorithms

| Algoritma | Best | Average | Worst | Space |
|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |
| Tim Sort | O(n) | O(n log n) | O(n log n) | O(n) |

```python
# Merge Sort
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left  = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

### Searching Algorithms

```python
# Binary Search — O(log n), array harus terurut
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# Dynamic Programming — Longest Common Subsequence
def lcs(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[m][n]
```

---

## 📐 Konsep Dasar Pemrograman

### Variabel & Tipe Data
```python
# Primitif
integer  = 42
float_   = 3.14
boolean  = True
string   = "Hello, World!"
none_val = None

# Komposit
list_  = [1, 2, 3]
tuple_ = (1, 2, 3)         # immutable
dict_  = {"key": "value"}
set_   = {1, 2, 3}          # unik
```

### Kontrol Alur
```python
# Kondisional
nilai = 85
if nilai >= 90:
    grade = "A"
elif nilai >= 80:
    grade = "B"
elif nilai >= 70:
    grade = "C"
else:
    grade = "D"

# Perulangan
for i in range(5):
    print(i)

while kondisi:
    # lakukan sesuatu
    kondisi = False

# Loop control
for n in range(10):
    if n == 3: continue   # skip
    if n == 7: break      # berhenti
    print(n)
```

### Fungsi
```python
# Fungsi dengan berbagai fitur
def buat_profil(nama, usia, *hobi, kota="Jakarta", **info):
    """
    Membuat profil pengguna.
    
    Args:
        nama: Nama pengguna
        usia: Usia pengguna
        *hobi: Hobi (varargs)
        kota: Kota (keyword default)
        **info: Informasi tambahan
    """
    return {
        "nama": nama,
        "usia": usia,
        "hobi": list(hobi),
        "kota": kota,
        **info
    }

# Lambda / anonymous function
kuadrat = lambda x: x ** 2
print(list(map(kuadrat, [1, 2, 3, 4])))  # [1, 4, 9, 16]

# Decorator
def timer(func):
    import time
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end-start:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    import time; time.sleep(1)
```

---

## 🏗️ Object-Oriented Programming (OOP)

### 4 Pilar OOP

#### 1. Encapsulation
```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self._owner = owner       # protected
        self.__balance = balance  # private
    
    @property
    def balance(self):
        return self.__balance
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
        else:
            raise ValueError("Dana tidak cukup!")
```

#### 2. Inheritance
```python
class Shape:
    def area(self): raise NotImplementedError
    def perimeter(self): raise NotImplementedError

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def perimeter(self):
        return 2 * 3.14159 * self.radius

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)
```

#### 3. Polymorphism
```python
shapes = [Circle(5), Rectangle(4, 6), Circle(3)]

for shape in shapes:
    print(f"Area: {shape.area():.2f}")  # Panggil metode yang sama, hasil berbeda
```

#### 4. Abstraction
```python
from abc import ABC, abstractmethod

class DatabaseDriver(ABC):
    @abstractmethod
    def connect(self, host: str, port: int): ...
    
    @abstractmethod
    def query(self, sql: str) -> list: ...
    
    @abstractmethod
    def close(self): ...

class MySQLDriver(DatabaseDriver):
    def connect(self, host, port):
        print(f"Connecting to MySQL at {host}:{port}")
    
    def query(self, sql):
        return []  # hasil query
    
    def close(self):
        print("Connection closed")
```

---

## 🔧 Functional Programming

```python
from functools import reduce
from typing import Callable, TypeVar

T = TypeVar("T")
U = TypeVar("U")

# Pure functions — tidak ada side effects
def add(a: int, b: int) -> int:
    return a + b  # Selalu menghasilkan output yang sama untuk input yang sama

# Immutability
original = (1, 2, 3)
new_tuple = original + (4,)  # buat baru, tidak ubah yang lama

# Higher-order functions
def compose(*fns: Callable) -> Callable:
    """Komposisi fungsi: f(g(h(x)))"""
    return reduce(lambda f, g: lambda x: f(g(x)), fns)

double  = lambda x: x * 2
add_one = lambda x: x + 1
square  = lambda x: x ** 2

transform = compose(double, add_one, square)
print(transform(3))  # double(add_one(square(3))) = double(add_one(9)) = double(10) = 20

# Currying
def curry_add(a):
    return lambda b: a + b

add5 = curry_add(5)
print(add5(3))   # 8
print(add5(10))  # 15
```

---

## 📂 Version Control

### Git Workflow Lengkap
```bash
# Inisialisasi
git init
git clone https://github.com/user/repo.git

# Branching strategy (Git Flow)
git checkout -b feature/login-page    # buat branch fitur
git checkout -b hotfix/bug-123        # buat branch hotfix
git checkout -b release/v1.2.0        # buat branch release

# Staging & Committing
git add .                              # stage semua
git add src/components/Button.js       # stage file spesifik
git commit -m "feat: tambah komponen Button"
git commit --amend                     # ubah commit terakhir

# Push & Pull
git push origin feature/login-page
git pull origin main

# Merge & Rebase
git merge feature/login-page
git rebase main                        # rebase ke main

# Stash
git stash                              # simpan perubahan sementara
git stash pop                          # ambil kembali

# Informasi
git log --oneline --graph --all
git diff HEAD~1 HEAD
git blame src/app.py

# Tags
git tag v1.0.0
git push origin --tags
```

### Conventional Commits
```
feat:     fitur baru
fix:      perbaikan bug
docs:     perubahan dokumentasi
style:    formatting (tidak mengubah logika)
refactor: refactoring kode
test:     menambah/mengubah tes
chore:    update dependency, konfigurasi

Contoh:
feat(auth): tambah login dengan Google OAuth
fix(cart): perbaiki perhitungan total harga
docs: update README dengan instruksi instalasi
```

---

## 🌐 Web Development

### Frontend Stack
```
HTML/CSS/JavaScript
    ↓
Framework: React | Vue | Angular | Svelte
    ↓
Meta-framework: Next.js | Nuxt | Remix | SvelteKit
    ↓
State: Redux | Zustand | Pinia | Jotai
    ↓
Styling: Tailwind CSS | CSS Modules | Styled Components
    ↓
Testing: Jest | Vitest | Cypress | Playwright
    ↓
Build: Vite | Webpack | Turbopack
```

### Backend Stack
```
Node.js (Express / Fastify / Hapi)
Python  (Django / FastAPI / Flask)
Java    (Spring Boot)
Go      (Gin / Echo / Fiber)
PHP     (Laravel / Symfony)
Ruby    (Rails)
.NET    (ASP.NET Core)
```

### REST API Design
```http
GET    /api/v1/users           # Daftar semua user
GET    /api/v1/users/{id}      # Detail user
POST   /api/v1/users           # Buat user baru
PUT    /api/v1/users/{id}      # Update penuh
PATCH  /api/v1/users/{id}      # Update sebagian
DELETE /api/v1/users/{id}      # Hapus user

# Response format
{
    "status": "success",
    "data": { ... },
    "meta": {
        "page": 1,
        "per_page": 20,
        "total": 150
    }
}
```

### GraphQL
```graphql
type Query {
    user(id: ID!): User
    users(filter: UserFilter): [User!]!
}

type Mutation {
    createUser(input: CreateUserInput!): User!
    updateUser(id: ID!, input: UpdateUserInput!): User!
    deleteUser(id: ID!): Boolean!
}

type Subscription {
    userCreated: User!
}

type User {
    id: ID!
    name: String!
    email: String!
    posts: [Post!]!
    createdAt: DateTime!
}
```

---

## 📱 Mobile Development

| Platform | Bahasa Utama | Framework |
|---|---|---|
| **iOS Native** | Swift, Objective-C | UIKit, SwiftUI |
| **Android Native** | Kotlin, Java | Jetpack Compose, XML |
| **Cross-platform** | Dart | Flutter |
| **Cross-platform** | JavaScript | React Native |
| **Cross-platform** | C# | .NET MAUI |

```dart
// Flutter — Widget tree
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Hello Flutter')),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text('Hello, World!', style: TextStyle(fontSize: 24)),
              ElevatedButton(
                onPressed: () => print('Ditekan!'),
                child: Text('Tekan Saya'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

---

## 🗄️ Database

### Jenis Database

| Jenis | Contoh | Kegunaan |
|---|---|---|
| **Relational (SQL)** | PostgreSQL, MySQL, SQLite | Data terstruktur, transaksi ACID |
| **Document** | MongoDB, CouchDB | Data semi-terstruktur, fleksibel |
| **Key-Value** | Redis, DynamoDB | Cache, session, real-time |
| **Column-family** | Cassandra, HBase | Data time-series, IoT |
| **Graph** | Neo4j, Amazon Neptune | Relasi kompleks, social network |
| **Search** | Elasticsearch, Solr | Full-text search |
| **Time-series** | InfluxDB, TimescaleDB | Metrics, monitoring |

### ACID vs BASE

| | ACID | BASE |
|---|---|---|
| **Kepanjangan** | Atomicity, Consistency, Isolation, Durability | Basically Available, Soft state, Eventually consistent |
| **Jenis DB** | SQL (Relational) | NoSQL |
| **Prioritas** | Konsistensi | Ketersediaan |

### ORM Example
```python
# SQLAlchemy (Python)
from sqlalchemy import create_engine, Column, Integer, String, ForeignKey
from sqlalchemy.orm import declarative_base, relationship, Session

Base = declarative_base()

class User(Base):
    __tablename__ = "users"
    id       = Column(Integer, primary_key=True)
    username = Column(String(50), unique=True, nullable=False)
    email    = Column(String(100), unique=True, nullable=False)
    posts    = relationship("Post", back_populates="author")

class Post(Base):
    __tablename__ = "posts"
    id        = Column(Integer, primary_key=True)
    title     = Column(String(200), nullable=False)
    content   = Column(String)
    author_id = Column(Integer, ForeignKey("users.id"))
    author    = relationship("User", back_populates="posts")
```

---

## ☁️ DevOps & Cloud

### Docker
```dockerfile
# Multi-stage build
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
USER node
CMD ["node", "dist/index.js"]
```

### Docker Compose
```yaml
version: '3.8'
services:
  app:
    build: .
    ports: ["3000:3000"]
    depends_on: [db, redis]
    environment:
      DATABASE_URL: postgres://user:pass@db:5432/mydb
      REDIS_URL: redis://redis:6379
  
  db:
    image: postgres:15-alpine
    volumes: [pgdata:/var/lib/postgresql/data]
    environment:
      POSTGRES_PASSWORD: pass
      POSTGRES_USER: user
      POSTGRES_DB: mydb
  
  redis:
    image: redis:7-alpine
    
volumes:
  pgdata:
```

### CI/CD Pipeline (GitHub Actions)
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm test
      - run: npm run lint
  
  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to production
        run: echo "Deploy ke server produksi"
```

---

## 🤖 Kecerdasan Buatan & Machine Learning

### Roadmap ML
```
1. Matematika Dasar
   ├── Aljabar Linear (Matriks, Vektor, Eigenvalue)
   ├── Kalkulus (Gradient, Turunan Parsial)
   └── Statistika (Probabilitas, Distribusi, Hipotesis)

2. Machine Learning Klasik
   ├── Supervised: Regresi, Klasifikasi
   ├── Unsupervised: Clustering, Dimensionality Reduction
   └── Reinforcement Learning

3. Deep Learning
   ├── Neural Network (ANN)
   ├── CNN (Computer Vision)
   ├── RNN / LSTM (Sequential Data)
   └── Transformer (NLP, Vision)

4. MLOps
   ├── Experiment Tracking (MLflow)
   ├── Model Serving (FastAPI, TorchServe)
   └── Monitoring & Drift Detection
```

```python
# Contoh: Neural Network sederhana dengan PyTorch
import torch
import torch.nn as nn

class SimpleNet(nn.Module):
    def __init__(self, input_size, hidden_size, output_size):
        super().__init__()
        self.layers = nn.Sequential(
            nn.Linear(input_size, hidden_size),
            nn.ReLU(),
            nn.Dropout(0.3),
            nn.Linear(hidden_size, hidden_size),
            nn.ReLU(),
            nn.Linear(hidden_size, output_size)
        )
    
    def forward(self, x):
        return self.layers(x)

model = SimpleNet(784, 256, 10)  # MNIST classifier
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
criterion = nn.CrossEntropyLoss()
```

---

## 🔒 Keamanan / Cybersecurity

### OWASP Top 10
1. **Broken Access Control** — Kontrol akses yang lemah
2. **Cryptographic Failures** — Enkripsi tidak memadai
3. **Injection** — SQL Injection, XSS, dll
4. **Insecure Design** — Desain sistem yang tidak aman
5. **Security Misconfiguration** — Konfigurasi server yang buruk
6. **Vulnerable Components** — Komponen/library usang
7. **Auth Failures** — Autentikasi/session yang lemah
8. **Software Integrity Failures** — Masalah supply chain
9. **Logging Failures** — Kurang pencatatan dan monitoring
10. **SSRF** — Server-Side Request Forgery

```python
# Best practices keamanan
import hashlib, secrets, hmac

# ❌ JANGAN simpan password plaintext
password_salah = "mypassword123"

# ✅ Gunakan hashing yang aman
import bcrypt
hashed = bcrypt.hashpw(b"mypassword123", bcrypt.gensalt())
valid  = bcrypt.checkpw(b"mypassword123", hashed)

# ✅ Gunakan token yang aman secara kriptografis
token = secrets.token_urlsafe(32)

# ✅ Validasi & sanitasi input selalu!
def sanitize_input(user_input: str) -> str:
    import html
    return html.escape(user_input.strip())
```

---

## 🧪 Testing

### Piramida Testing
```
        /\
       /  \    E2E Tests (Sedikit, lambat)
      /----\
     / Integ \  Integration Tests
    /----------\
   /  Unit Tests \ (Banyak, cepat)
  /--------------\
```

```python
# Unit Testing dengan pytest
import pytest

def add(a, b):
    return a + b

class TestAdd:
    def test_positive_numbers(self):
        assert add(2, 3) == 5
    
    def test_negative_numbers(self):
        assert add(-1, -2) == -3
    
    def test_zero(self):
        assert add(0, 5) == 5
    
    @pytest.mark.parametrize("a,b,expected", [
        (1, 2, 3), (5, 5, 10), (-1, 1, 0)
    ])
    def test_parametrized(self, a, b, expected):
        assert add(a, b) == expected

# Mock testing
from unittest.mock import Mock, patch

def test_api_call():
    with patch('requests.get') as mock_get:
        mock_get.return_value.json.return_value = {"status": "ok"}
        result = fetch_data("https://api.example.com")
        assert result["status"] == "ok"
```

---

## 🎨 Design Patterns

### Creational
```python
# Singleton
class Singleton:
    _instance = None
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

# Factory Method
class AnimalFactory:
    @staticmethod
    def create(animal_type: str):
        if animal_type == "dog":  return Dog()
        if animal_type == "cat":  return Cat()
        raise ValueError(f"Unknown: {animal_type}")

# Builder
class QueryBuilder:
    def __init__(self): self._query = {}
    def select(self, *fields): self._query["select"] = fields; return self
    def from_table(self, table): self._query["table"] = table; return self
    def where(self, condition): self._query["where"] = condition; return self
    def build(self): return f"SELECT {','.join(self._query['select'])} FROM {self._query['table']}"
```

### Structural
```python
# Decorator Pattern
from functools import wraps

def require_auth(func):
    @wraps(func)
    def wrapper(request, *args, **kwargs):
        if not request.user.is_authenticated:
            raise PermissionError("Login required")
        return func(request, *args, **kwargs)
    return wrapper

# Adapter Pattern
class OldAPI:
    def get_data(self): return {"name": "Budi"}

class NewAPIAdapter:
    def __init__(self, old_api):
        self._old = old_api
    
    def fetch_user(self):
        data = self._old.get_data()
        return {"username": data["name"]}  # adaptasi interface
```

### Behavioral
```python
# Observer Pattern
class EventEmitter:
    def __init__(self):
        self._handlers = {}
    
    def on(self, event, handler):
        self._handlers.setdefault(event, []).append(handler)
    
    def emit(self, event, *args, **kwargs):
        for handler in self._handlers.get(event, []):
            handler(*args, **kwargs)

emitter = EventEmitter()
emitter.on("data", lambda d: print(f"Received: {d}"))
emitter.emit("data", {"user": "Budi"})
```

---

## ✨ Clean Code & Best Practices

```python
# ❌ BURUK
def f(x, y, z):
    r = []
    for i in x:
        if i > y and i < z:
            r.append(i)
    return r

# ✅ BAIK
def filter_numbers_in_range(
    numbers: list[int],
    minimum: int,
    maximum: int
) -> list[int]:
    """
    Filter angka yang berada dalam rentang [minimum, maximum].
    
    Args:
        numbers: List angka yang akan difilter
        minimum: Batas bawah (eksklusif)
        maximum: Batas atas (eksklusif)
    
    Returns:
        List angka dalam rentang yang ditentukan
    """
    return [n for n in numbers if minimum < n < maximum]
```

### Prinsip SOLID
| Huruf | Kepanjangan | Arti |
|---|---|---|
| **S** | Single Responsibility | Satu kelas, satu tanggung jawab |
| **O** | Open/Closed | Terbuka untuk ekstensi, tertutup untuk modifikasi |
| **L** | Liskov Substitution | Subclass harus bisa menggantikan superclass |
| **I** | Interface Segregation | Banyak interface spesifik lebih baik dari satu yang umum |
| **D** | Dependency Inversion | Bergantung pada abstraksi, bukan implementasi konkret |

---

## 📊 Kompleksitas Algoritma (Big O)

```
O(1)        → Konstan       — Akses array berdasarkan index
O(log n)    → Logaritmik    — Binary search
O(n)        → Linear        — Linear search, traversal
O(n log n)  → Linearitmik   — Merge sort, Heap sort
O(n²)       → Kuadratik     — Bubble sort, nested loops
O(n³)       → Kubik         — Matrix multiplication naif
O(2ⁿ)       → Eksponensial  — Subset generation
O(n!)       → Faktorial     — Permutation generation
```

```
Waktu
|
|                                    O(n!)
|                               O(2ⁿ)
|                          O(n²)
|                     O(n log n)
|                O(n)
|          O(log n)
|     O(1) _______________
|_____________________________ Input (n)
```

---

## 🛠️ Tools & IDE

### Editor / IDE Populer
| Tool | Bahasa | Keunggulan |
|---|---|---|
| **VS Code** | Semua | Gratis, extensible, ringan |
| **JetBrains IDEs** | Khusus | IntelliSense terbaik, mahal |
| **Vim / Neovim** | Semua | Sangat cepat, keyboard-driven |
| **Emacs** | Semua | Sangat dapat dikustomisasi |
| **Xcode** | Swift/ObjC | Wajib untuk iOS development |
| **Android Studio** | Kotlin/Java | Wajib untuk Android development |

### Tools Wajib Developer
```bash
# Package managers
npm / yarn / pnpm    # JavaScript
pip / poetry / uv    # Python
cargo                # Rust
maven / gradle       # Java/Kotlin
composer             # PHP
gem / bundler        # Ruby
go mod               # Go

# Productivity
tmux                 # Terminal multiplexer
fzf                  # Fuzzy finder
ripgrep              # Pencarian yang cepat
httpie / curl        # HTTP client
jq                   # JSON processor
```

---

## 📖 Tips Belajar Pemrograman

### Roadmap untuk Pemula
```
1. Pilih SATU bahasa dulu (Python / JavaScript direkomendasikan)
2. Kuasai dasar: variabel, loop, fungsi, array
3. Bangun proyek kecil (kalkulator, to-do app)
4. Pelajari struktur data & algoritma dasar
5. Pelajari Git & GitHub
6. Pelajari framework (React, Django, dll)
7. Kontribusi open source
8. Bangun portofolio
```

### Sumber Belajar
| Sumber | Jenis | Keterangan |
|---|---|---|
| [freeCodeCamp](https://freecodecamp.org) | Video + Latihan | Web dev, gratis |
| [The Odin Project](https://theodinproject.com) | Kurikulum | Full-stack, gratis |
| [LeetCode](https://leetcode.com) | Latihan soal | Algoritma & DS |
| [Exercism](https://exercism.org) | Latihan + Mentoring | 60+ bahasa |
| [MDN Web Docs](https://developer.mozilla.org) | Dokumentasi | Web tech |
| [roadmap.sh](https://roadmap.sh) | Roadmap visual | Semua jalur |

### Pola Belajar Efektif
- 🔁 **Spaced repetition** — Ulangi materi secara berkala
- 🛠️ **Learn by doing** — Bangun proyek nyata, bukan hanya tonton
- 🐛 **Embrace bugs** — Error adalah guru terbaik
- 📝 **Rubber duck debugging** — Jelaskan kode ke benda/orang lain
- 👥 **Code review** — Minta feedback dari developer lain
- 📚 **Baca source code** — Pelajari library/framework yang kamu pakai

---

## 🤝 Kontribusi

Pull request sangat diterima! Untuk perubahan besar, buka issue terlebih dahulu untuk mendiskusikan apa yang ingin kamu ubah.

```bash
# Fork repo ini
git clone https://github.com/zax-universe/coding-x.git
git checkout -b feature/tambah-topik-baru
# ... buat perubahan ...
git commit -m "feat: tambah topik X"
git push origin feature/tambah-topik-baru
# Buka Pull Request di GitHub
```

---

## 📄 Lisensi

Didistribusikan di bawah lisensi MIT. Lihat [`LICENSE`](LICENSE) untuk informasi lebih lanjut.

---

<p align="center">
  Dibuat dengan ❤️ untuk komunitas depelopeng Indonesia
  <br>
  <strong>Happy codink! 🚀</strong>
</p>
