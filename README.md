# Implementasi Mikrokontroler dengan RFID dan LED

## 📌 Deskripsi Proyek
Proyek ini merupakan implementasi sistem identifikasi kartu menggunakan **RFID RC522** berbasis **ESP32** dengan indikator LED sebagai output visual. Sistem bekerja menggunakan komunikasi **SPI (Serial Peripheral Interface)** untuk membaca UID kartu RFID, kemudian memberikan respons melalui LED berdasarkan status pembacaan kartu.

Pada kondisi awal, LED merah menyala sebagai indikator standby. Ketika kartu RFID terdeteksi, sistem akan membaca UID kartu, menampilkannya pada Serial Monitor, menyalakan LED hijau, dan mematikan LED merah selama beberapa detik sebelum kembali ke kondisi awal.

Proyek ini bertujuan untuk memahami konsep dasar komunikasi SPI, penggunaan RFID, pengendalian output digital, serta integrasi perangkat keras pada embedded system.

---

## 🖼️ Dokumentasi Sistem

<img width="278" height="205" alt="image" src="https://github.com/user-attachments/assets/c48d9194-0ccf-4136-bf8c-5b868677f42f" />

---

## 🛠️ Alat dan Komponen
- ESP32 Dev Module  
- RFID RC522 Module  
- RFID Card / Tag  
- LED Merah  
- LED Hijau  
- Resistor  
- Breadboard  
- Kabel Jumper  
- Kabel USB Data  

---

## 🔌 Konfigurasi Pin

### RFID RC522
| Pin RFID | ESP32 |
|-----------|--------|
| SDA       | GPIO 5 |
| RST       | GPIO 22 |
| SCK       | GPIO 18 |
| MOSI      | GPIO 23 |
| MISO      | GPIO 19 |
| VCC       | 3.3V |
| GND       | GND |

### LED
| Komponen | ESP32 |
|-----------|--------|
| LED Merah | GPIO 25 |
| LED Hijau | GPIO 26 |

---

## ⚙️ Langkah Implementasi

1. Siapkan seluruh alat dan komponen.  
2. Rangkai komponen sesuai konfigurasi pin.  
3. Hubungkan modul RFID RC522 ke ESP32 menggunakan komunikasi SPI.  
4. Sambungkan LED merah dan LED hijau melalui resistor ke GPIO yang telah ditentukan.  
5. Hubungkan seluruh jalur ground ke GND ESP32.  
6. Buka **Arduino IDE**.  
7. Tambahkan library berikut:
   - `SPI.h`
   - `MFRC522.h`

8. Pilih board:
   `Tools → Board → ESP32 Arduino → ESP32 Dev Module`

9. Pilih port ESP32:
   `Tools → Port → COM ESP32`

10. Upload program ke ESP32 hingga muncul status **Done Uploading**.

---

## 💻 Program Arduino

```cpp
#include <SPI.h>
#include <MFRC522.h>

// Pin untuk RFID RC522
#define SS_PIN 5
#define RST_PIN 22

MFRC522 rfid(SS_PIN, RST_PIN);

// Pin LED
#define LED_MERAH 25
#define LED_HIJAU 26

void setup() {
  Serial.begin(115200);
  SPI.begin();
  rfid.PCD_Init();

  pinMode(LED_MERAH, OUTPUT);
  pinMode(LED_HIJAU, OUTPUT);

  // Kondisi awal
  digitalWrite(LED_MERAH, HIGH);
  digitalWrite(LED_HIJAU, LOW);

  Serial.println("Tempelkan kartu RFID...");
}

void loop() {

  // Cek kartu RFID
  if (!rfid.PICC_IsNewCardPresent() || !rfid.PICC_ReadCardSerial()) {

    digitalWrite(LED_MERAH, HIGH);
    digitalWrite(LED_HIJAU, LOW);

    return;
  }

  // Tampilkan UID kartu
  Serial.print("UID Kartu: ");

  for (byte i = 0; i < rfid.uid.size; i++) {
    Serial.print(rfid.uid.uidByte[i] < 0x10 ? " 0" : " ");
    Serial.print(rfid.uid.uidByte[i], HEX);
  }

  Serial.println();

  // LED hijau aktif
  digitalWrite(LED_MERAH, LOW);
  digitalWrite(LED_HIJAU, HIGH);

  delay(2000);

  rfid.PICC_HaltA();
  rfid.PCD_StopCrypto1();
}
```

---

## 🔍 Cara Kerja Sistem
- Sistem melakukan pengecekan kartu RFID secara terus-menerus.  
- Pada kondisi standby, LED merah menyala dan LED hijau mati.  
- Ketika kartu RFID terdeteksi:
  - UID kartu dibaca dan ditampilkan pada Serial Monitor  
  - LED hijau menyala  
  - LED merah mati selama 2 detik  

- Setelah proses selesai, sistem kembali ke kondisi awal untuk menunggu kartu berikutnya.

---

## 📊 Hasil Pengujian
Pengujian dilakukan untuk memastikan ESP32 mampu membaca input dari modul RFID dan mengendalikan LED sesuai logika program. Hasil pengujian menunjukkan bahwa sistem berhasil:
- Mendeteksi kartu RFID dengan baik  
- Membaca dan menampilkan UID kartu pada Serial Monitor  
- Mengontrol LED sesuai kondisi pembacaan kartu  
- Mengembalikan sistem ke kondisi standby secara otomatis  

---

## 💻 Teknologi yang Digunakan
- Arduino IDE  
- ESP32  
- RFID RC522  
- SPI Communication Protocol  
- Embedded System Programming  

---

## 📚 Tujuan Pembelajaran
- Memahami komunikasi SPI pada ESP32  
- Mengimplementasikan pembacaan RFID menggunakan RC522  
- Mengontrol output digital menggunakan LED  
- Mengintegrasikan sensor dan aktuator dalam embedded system  

---

## 👨‍💻 Author
**Puspita**  
Computer Engineering Student — IPB University
