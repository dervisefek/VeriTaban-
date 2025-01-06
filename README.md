# İmleçler (MYP217)

---

## 1. İmleç (Cursor) Tanımı ve Kullanımı

**İmleç (Cursor):** Bir SQL sorgusunun sonuç kümesini satır satır işlemek için kullanılan yapıdır. İmleçler, veri tabanından çekilen çoklu kayıtlarla çalışmak için kullanılır.

### İmleç Tipleri
1. **Statik İmleçler:** Sorgu sonucunu bellekte saklar ve sonuçlar değişmez.
2. **Dinamik İmleçler:** Veri tabanındaki değişikliklere bağlı olarak güncellenebilir.

---

## 2. İmleç Tanımlama ve Kullanımı

### İmleç Tanımlama Formatı:
```sql
DECLARE
   cursor_name CURSOR FOR SELECT_query;
```

**Örnek İmleç Tanımı:**
```sql
DECLARE
   emp_cursor CURSOR FOR
   SELECT employee_id, first_name, last_name FROM employees;
```
Bu örnekte `emp_cursor` adında bir imleç tanımlanmıştır.

---

## 3. İmlecin Açılması ve Kapatılması

- **İmleç Açma:** `OPEN` komutu ile yapılır.
- **İmleç Kapatma:** `CLOSE` komutu ile yapılır.

### İmleç Açma ve Kapatma Örnekleri:
```sql
BEGIN
   OPEN emp_cursor;
   CLOSE emp_cursor;
END;
```
---

## 4. FETCH Komutu ile Veri Okuma

**FETCH:** İmleçten bir satır çekmek için kullanılır.

### FETCH Kullanımı:
```sql
FETCH cursor_name INTO variable_list;
```

**Örnek:**
```sql
DECLARE
   emp_cursor CURSOR FOR
   SELECT employee_id, first_name FROM employees;
   v_id employees.employee_id%TYPE;
   v_name employees.first_name%TYPE;
BEGIN
   OPEN emp_cursor;
   FETCH emp_cursor INTO v_id, v_name;
   CLOSE emp_cursor;
END;
```
Bu örnekte, `emp_cursor` imleci açılmış, bir kayıt okunmuş ve daha sonra kapatılmıştır.

---

## 5. İmleç Kullanımı ile Döngü

İmleçler genellikle döngülerle birlikte kullanılır. Bu sayede sonuç kümesindeki tüm kayıtlar işlenebilir.

### İmleç ile Döngü Formatı:
```sql
LOOP
   FETCH cursor_name INTO variable_list;
   EXIT WHEN cursor_name%NOTFOUND;
END LOOP;
```

**Örnek:**
```sql
DECLARE
   emp_cursor CURSOR FOR
   SELECT employee_id, first_name FROM employees;
   v_id employees.employee_id%TYPE;
   v_name employees.first_name%TYPE;
BEGIN
   OPEN emp_cursor;
   LOOP
      FETCH emp_cursor INTO v_id, v_name;
      EXIT WHEN emp_cursor%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE('ID: ' || v_id || ', Name: ' || v_name);
   END LOOP;
   CLOSE emp_cursor;
END;
```
Bu örnekte, `emp_cursor` imleci açılmış, döngü içinde tüm kayıtlar okunmuş ve ekrana yazdırılmıştır.

---

## 6. Parametreli İmleçler

Parametreli imleçler, dışarıdan değer alarak çalıştırılan imleçlerdir.

### Parametreli İmleç Tanımlama:
```sql
DECLARE
   cursor_name (parametre_değişkeni DATATYPE) CURSOR FOR SELECT_query;
```

**Örnek:**
```sql
DECLARE
   emp_cursor(p_department_id NUMBER) CURSOR FOR
   SELECT employee_id, first_name FROM employees WHERE department_id = p_department_id;
BEGIN
   OPEN emp_cursor(50);
   -- İşlemler
   CLOSE emp_cursor;
END;
```
Bu örnekte, `emp_cursor` imleci bir departman numarası parametresi ile çalıştırılmıştır.

---

## 7. İmleç Özellikleri

- **%FOUND:** Son `FETCH` işlemi bir kayıt döndürdü mü?
- **%NOTFOUND:** Son `FETCH` işlemi kayıt döndürmedi mi?
- **%ROWCOUNT:** İmleç ile kaç satır işlendiğini döndürür.

### Örnek Kullanım:
```sql
BEGIN
   OPEN emp_cursor;
   FETCH emp_cursor INTO v_id, v_name;
   IF emp_cursor%FOUND THEN
      DBMS_OUTPUT.PUT_LINE('Kayıt bulundu.');
   END IF;
   CLOSE emp_cursor;
END;
```

---

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
