# SAP-ABAP-code
SAP ABAP CODE
*Dosya adı gizlenmiştir. Verilerimizi database tablosundan çektik.
REPORT *********.  

SELECT * FROM zstudent_info
INTO TABLE @DATA(lt_student).

*Ödev 1 bütün öğrencileri listeleyen bsit rapor. (Read table)


DATA: lv_index TYPE i VALUE 1,  " İndeks başlangıcı
      lv_lines TYPE i,          " Tablo satır sayısı
      ls_student TYPE zstudent_info.

DESCRIBE TABLE lt_student LINES lv_lines.

IF lv_lines > 0.
  WHILE lv_index <= lv_lines.
    READ TABLE lt_student INTO ls_student INDEX lv_index.
    IF sy-subrc = 0.
      WRITE: / ls_student-student_id,
                 ls_student-first_name,
                 ls_student-last_name,
                 ls_student-gender,
                 ls_student-status,
                 ls_student-class,
                 ls_student-department.
    ENDIF.
    lv_index = lv_index + 1.
  ENDWHILE.
ELSE.
  WRITE: / 'Hiç öğrenci kaydı bulunamadı.'.
ENDIF.



*Öğrencileri loop ile listeledik.
*LOOP AT lt_student INTO DATA(ls_student).
*
*  WRITE: / ls_student-student_id,
*           ls_student-first_name,
*           ls_student-last_name,
*           ls_student-gender,
*           ls_student-status,
*           ls_student-class,
*           ls_student-department,
*           ls_student-birthday.
*
* endloop.


*Ödev 1 ve alt maddeleri belirli bir öğrenci numarası veya isme göre öğrenci arama ve ekrana yazdırma.
* Öğrenci numarası 100002 olan öğrenciyi yazdırma (Ayşe Demiri yazdırıyor.)

*LOOP AT lt_student INTO DATA(ls_student) WHERE student_id = '100002'.
*
*  WRITE: / ls_student-student_id,
*           ls_student-first_name,
*           ls_student-last_name.
*
*
* endloop.

*First name Mehmet olanları yazdırma belirli bir isim.

*LOOP AT lt_student INTO DATA(ls_student) WHERE first_name = 'Mehmet'.
*
*  WRITE: / ls_student-student_id,
*           ls_student-first_name,
*           ls_student-last_name,
*           ls_student-gender.
*
* endloop.

*Erkek olan ya da pasif olan listeyi yazdırdı (M ve P).(Birinden birini sağlamalı.)

*LOOP AT lt_student INTO DATA(ls_student)
* WHERE gender = 'M' OR status = 'P'.
*
*  WRITE: / ls_student-student_id,
*           ls_student-first_name,
*           ls_student-last_name,
*           ls_student-gender,
*           ls_student-status.
*
* endloop.


* İsim soyisime göre öğrenci arama.

*LOOP AT lt_student INTO DATA(ls_student)
* WHERE first_name = 'Mehmet' AND last_name = 'Yavuz'.
*
*  WRITE: / ls_student-student_id,
*           ls_student-first_name,
*           ls_student-last_name,
*           ls_student-gender,
*           ls_student-status.
* endloop.





*Ödev 2 tek öğrenci için yaş hesaplama.(Ayşenin yaşını 27 bulduk.)

*DATA(lv_current_date) = sy-datum.
*DATA lv_age TYPE i.
*DATA lv_birthday type sy-datum.
*
**Günümüzdeki tarihi aldık.
*lv_current_date = sy-datum.
*
**First name ayşe olan kişinin yaşını hesaplamaya başladık.
*LOOP AT lt_student INTO DATA(ls_student) WHERE first_name = 'Ayşe'.
*  lv_birthday = ls_student-birthday.
*
**Yaş farkı için ggünümüzden doğum tarihini çıkarttık.
*  lv_age = lv_current_date(4) - lv_birthday(4).
*
**Koşullar ile ayı baz alarak yaşı hesapladık.
*  IF lv_current_date+4(2) < lv_birthday+4(2) OR
*     ( lv_current_date+4(2) = lv_birthday+4(2) AND
*       lv_current_date+6(2) < lv_birthday+6(2) ).
*    lv_age = lv_age - 1.
*  ENDIF.
*
*  WRITE: / 'Öğrenci ID:', ls_student-student_id,
*             'Adı:', ls_student-first_name,
*             'Soyadı:', ls_student-last_name,
*             'Doğum Tarihi:', lv_birthday,
*             'Yaşı:', lv_age.
*ENDLOOP.




*Ödev 2 belirli bir sınıftaki öğrenci sayısını bulma.

*11. SINIFTAKİ ÖĞRENCİ SAYISINI BULDUK(sınıf 11 öğrenci sayısını 10 olarak ekrana yazdırdı.)
*DATA: lv_student_count TYPE i,
*      lv_class        TYPE string VALUE '11'.
*
*CLEAR lv_student_count.
*
*LOOP AT lt_student INTO DATA(ls_student).
*  IF ls_student-class = lv_class.
*    lv_student_count = lv_student_count + 1.
*  ENDIF.
*ENDLOOP.
*
*WRITE: 'Sınıf: ', lv_class.
*WRITE: / 'Öğrenci Sayısı: ', lv_student_count.
*
*IF lv_student_count = 0.
*  WRITE: / 'Bu sınıfta öğrenci bulunmamaktadır.'.
*ENDIF.


*Class'ı 11 olan 10 kişiyi yukarıdaki kod ile bulmuştuk bu kod ise 10 kişiyi listeledi.
*LOOP AT lt_student INTO DATA(ls_student) WHERE class = '11'.
*  WRITE: / ls_student-student_id,
*           ls_student-first_name,
*           ls_student-last_name,
*           ls_student-gender,
*           ls_student-status.
*ENDLOOP.





**(ödev 2) Aktif ve pasif öğrenci sayılarını hesaplama Çıktısı 9'a 9 şeklinde yazırdı ekrana.

*DATA: lv_active_count  TYPE i,
*      lv_passive_count TYPE i.
*LOOP AT lt_student INTO DATA(ls_student).
*
*  CASE ls_student-status.
*
*    WHEN 'A'.
*      lv_active_count = lv_active_count + 1.
*    WHEN 'P'.
*      lv_passive_count = lv_passive_count + 1.
*
*  ENDCASE.
*
*ENDLOOP.
*
*write:  'Aktif öğrenci sayisi:', lv_active_count,
*        'Pasif öğrenci sayisi:', lv_passive_count.
