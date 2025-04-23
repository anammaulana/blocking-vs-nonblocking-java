
# penjelasan blocking dan non blocking di java

# 🔄 Blocking vs Non-Blocking di Java

Dokumen ini menjelaskan perbedaan antara pendekatan **blocking** (imperative) dan **non-blocking** (reactive) dalam pemrograman Java, khususnya saat mengakses data atau melakukan operasi I/O.

---

## 📌 Apa itu Blocking?

**Blocking** berarti program akan **berhenti sementara dan menunggu** suatu operasi selesai sebelum lanjut ke proses berikutnya.

### Contoh (Blocking)
```java
public class BlockingExample {
    public static void main(String[] args) {
        System.out.println("Start");

        String user = getUserData(); // program berhenti di sini sampai selesai
        System.out.println("User: " + user);

        System.out.println("End");
    }

    static String getUserData() {
        try {
            Thread.sleep(2000); // simulasi delay 2 detik
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "Anam Maulana";
    }
}
```

### Output
```
Start
(User data loading 2 detik...)
User: Anam Maulana
End
```

---

## ⚡ Apa itu Non-Blocking (Reactive)?

**Non-blocking** berarti program tidak perlu menunggu. Operasi berjalan **asynchronous**, dan program bisa melanjutkan pekerjaan lain sembari menunggu hasilnya datang.

### Contoh (Non-Blocking) dengan Mutiny (Quarkus)
```java
import io.smallrye.mutiny.Uni;

public class NonBlockingExample {
    public static void main(String[] args) {
        System.out.println("Start");

        getUserData()
            .subscribe().with(user -> {
                System.out.println("User: " + user);
            });

        System.out.println("End");
    }

    static Uni<String> getUserData() {
        return Uni.createFrom().item("Anam Maulana")
                   .onItem().delayIt().by(java.time.Duration.ofSeconds(2));
    }
}
```

### Output
```
Start
End
(User muncul setelah 2 detik)
User: Anam Maulana
```

---

## 🔍 Perbandingan

| Fitur          | Blocking                 | Non-Blocking (Reactive)       |
|----------------|--------------------------|-------------------------------|
| Cara kerja     | Tunggu selesai dulu      | Lanjut, hasil ditangani nanti |
| Resource       | Lebih boros (CPU/thread) | Lebih efisien                 |
| Cocok untuk    | Aplikasi sederhana       | Aplikasi dengan trafik tinggi |
| Contoh library | JDBC, Hibernate          | Mutiny, R2DBC, Hibernate Reactive |

---

## 📘 Kesimpulan

- Gunakan **blocking** saat membuat aplikasi kecil atau sederhana.
- Gunakan **non-blocking** untuk aplikasi modern, high-performance, seperti microservices atau real-time apps.
```

