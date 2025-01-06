# Saklı Yordamlar ve Fonksiyonlar (MYP217)

---

## 1. Yordam (Procedure) Tanımı ve Kullanımı

**Yordam (Procedure):** Belirli bir işlemi gerçekleştiren ve tekrar kullanılabilir kod bloklarıdır. Yordamlar parametre alabilir ve bu parametreler yardımıyla işlemler gerçekleştirilebilir.

### Yordam Tanımlama Formatı:
```sql
CREATE OR REPLACE PROCEDURE procedure_name
AS
BEGIN
   -- Yordam içeriği
END;
```

**Örnek Yordam:**
```sql
CREATE OR REPLACE PROCEDURE selamlama
AS
BEGIN
   DBMS_OUTPUT.PUT_LINE('Merhaba, Dünya!');
END;
```

Yukarıdaki örnekte, `selamlama` adında bir yordam tanımlanmıştır ve bu yordam çalıştırıldığında ekrana "Merhaba, Dünya!" çıktısını verir.

### Yordam Çağırma:
```sql
BEGIN
   procedure_name;
END;
/
```

**Örnek:**
```sql
BEGIN
   selamlama;
END;
/
```

---

## 2. Fonksiyon Tanımı ve Kullanımı

**Fonksiyon (Function):** Bir değer döndüren yordam türüdür. Fonksiyonlar genellikle hesaplamalar yapmak veya belirli bir sonucu döndürmek için kullanılır.

### Fonksiyon Tanımlama Formatı:
```sql
CREATE OR REPLACE FUNCTION function_name
RETURN data_type
AS
BEGIN
   -- Fonksiyon içeriği
   RETURN sonuc;
END;
```

**Örnek Fonksiyon:**
```sql
CREATE OR REPLACE FUNCTION kare_al(sayi NUMBER)
RETURN NUMBER
AS
BEGIN
   RETURN sayi * sayi;
END;
```

Bu örnekte, `kare_al` adında bir fonksiyon tanımlanmıştır. Fonksiyona bir sayı gönderildiğinde, bu sayı karesi alınarak döndürülür.

### Fonksiyon Çağırma:
```sql
SELECT function_name(parametre) FROM DUAL;
```

**Örnek:**
```sql
SELECT kare_al(5) FROM DUAL;
```
Bu sorgunun sonucu `25` olacaktır.

---

## 3. Parametreler ve Değer Döndürme

**Yordam ve Fonksiyonlarda Parametre Kullanımı:**
- **IN:** Girdi parametresi.
- **OUT:** Çıktı parametresi.
- **IN OUT:** Hem girdi hem de çıktı parametresi.

### Parametreli Yordam Tanımlama:
```sql
CREATE OR REPLACE PROCEDURE bilgileri_goster(ad VARCHAR2, soyad VARCHAR2)
AS
BEGIN
   DBMS_OUTPUT.PUT_LINE('Ad: ' || ad || ', Soyad: ' || soyad);
END;
```

**Yordam Çağırma:**
```sql
BEGIN
   bilgileri_goster('Ali', 'Veli');
END;
/
```
Bu yordam çağırıldığında ekrana "Ad: Ali, Soyad: Veli" çıktısı basılır.

---

## 4. Yordam ve Fonksiyonların Avantajları
- Kod tekrarını önler.
- Programın okunabilirliğini artırır.
- Bakım ve güncellemeleri kolaylaştırır.

---

## 5. Örnek Kodlar ve Kullanım Senaryoları

**Matematiksel İşlem Yapan Fonksiyon:**
```sql
CREATE OR REPLACE FUNCTION faktoriyel(sayi NUMBER)
RETURN NUMBER
AS
   sonuc NUMBER := 1;
BEGIN
   FOR i IN 1..sayi LOOP
      sonuc := sonuc * i;
   END LOOP;
   RETURN sonuc;
END;
```

**Fonksiyon Çağırma:**
```sql
SELECT faktoriyel(5) FROM DUAL;
```
Bu sorgunun sonucu `120` olacaktır.

---

**Parametreli Fonksiyon Örneği:**
```sql
CREATE OR REPLACE FUNCTION maas_hesapla(maas NUMBER, zam_orani NUMBER)
RETURN NUMBER
AS
BEGIN
   RETURN maas + (maas * zam_orani / 100);
END;
```

**Fonksiyon Çağırma:**
```sql
SELECT maas_hesapla(5000, 10) FROM DUAL;
```
Bu sorgunun sonucu `5500` olacaktır.

---

Bu notlar, PDF'deki temel konulara ve örnek kodlara dayanmaktadır. Kodları doğrudan çalıştırarak test edebilirsiniz.
