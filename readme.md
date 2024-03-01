# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.

	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
    SELECT o.ograd, o.ogrsoyad ,i.atarih FROM ogrenci AS o, islem AS i 
    WHERE o.ogrno = i.ogrno
    ORDER BY o.ograd, o.ogrsoyad ASC

	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
    SELECT k.kitapadi, k.turno, t.turno FROM kitap AS k, tur AS t
    WHERE k.turno = t.turno  
    AND t.turadi IN ('Fıkra','Hikaye')



	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

    SELECT o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi,o.sinif
    FROM ogrenci as o, kitap as k, islem as i
	WHERE o.ogrno=i.ogrno
	AND i.kitapno = k.kitapno
	AND o.sinif in ('10B','10C')
	order by o.ograd;
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    SELECT o.ograd, o.ogrsoyad, i.atarih 
	FROM ogrenci AS o
	INNER JOIN islem AS i
	ON o.ogrno = i.ogrno
	ORDER BY o.ograd, o.ogrsoyad ASC
	
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
  
    SELECT k.kitapadi, k.turno 
	FROM kitap AS k
	INNER JOIN tur AS t
	ON k.turno = t.turno
	ORDER BY k.kitapadi ASC 
	
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
    SELECT o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi,o.sinif
    FROM ogrenci as o
    INNER JOIN islem AS i ON o.ogrno=i.ogrno
	INNER JOIN kitap AS k ON i.kitapno = k.kitapno
	AND o.sinif in ('10B','10C')
	ORDER BY o.ograd;
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
    SELECT o.ograd, o.ogrsoyad, i.atarih
    FROM ogrenci AS o
    LEFT JOIN islem AS i ON o.ogrno = i.ogrno
    ORDER BY o.ograd

	8) Kitap almayan öğrencileri listeleyin.
	SELECT o.ograd, o.ogrsoyad, i.atarih
    FROM ogrenci AS o
    LEFT JOIN islem AS i 
    ON o.ogrno = i.ogrno
    WHERE i.ogrno IS NULL

	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	
    
    SELECT k.kitapno, k.kitapadi , COUNT(i.kitapno) AS  'ALIM SAYISI' from kitap k
    INNER JOIN islem i
    ON k.kitapno = i.kitapno 
    GROUP BY k.kitapno,k.kitapadi 
    ORDER BY k.kitapno ASC
   
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
    SELECT k.kitapno, k.kitapadi, COUNT(i
    
    me: 
    SELECT k.kitapno, k.kitapadi ,
    COUNT(i.kitapno) AS total
    FROM kitap AS k
    left join islem AS i
    ON i.kitapno = k.kitapno
    GROUP BY k.kitapno, k.kitapadi
    ORDER BY k.kitapno DESC;  
 
	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
    SELECT o.ograd, o.ogrsoyad, k.kitapadi
    FROM ogrenci AS o
    INNER JOIN islem AS i
    ON o.ogrno = i.ogrno
    INNER JOIN kitap as k 
    on k.kitapno = i.kitapno
    
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
    SELECT o.ograd, o.ogrsoyad, k.kitapadi, y.yazarad, y.yazarsoyad, t.turadi , i.atarih
    FROM ogrenci AS o
    LEFT JOIN islem AS i 
    ON o.ogrno = i.ogrno
    LEFT JOIN kitap AS k
    ON k.kitapno = i.kitapno
    LEFT JOIN yazar AS y 
    ON y.yazarno = k.yazarno
    LEFT JOIN tur AS t
    ON t.turno = k.turno
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	
    SELECT o.ograd, o.ogrsoyad, COUNT(i.kitapno) AS total_read
    FROM ogrenci AS o
    INNER JOIN islem AS i
    ON 	o.ogrno = i.ogrno
    WHERE o.sinif IN ('10A','10B')
    GROUP BY o.ograd, o.ogrsoyad
    ORDER BY total_read DESC

	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG

    SELECT ROUND(AVG(sayfasayisi),2) FROM kitap
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
    
    SELECT k.kitapadi FROM kitap AS k 
    WHERE k.sayfasayisi > (SELECT AVG(sayfasayisi) FROM kitap as k)
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
    SELECT COUNT(ogrno) FROM ogrenci
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	
    SELECT COUNT(ogrno) AS toplam_ogrenci FROM ogrenci
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
    SELECT DISTINCT(o.ograd) FROM ogrenci AS o;
   
    SELECT o.ograd,COUNT(o.ograd) FROM
    OGRENCI AS o
    GROUP BY o.ograd;
 	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
    SELECT MAX(k.sayfasayisi) AS en_cok_sayfa_sayisi FROM kitap AS k
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
    SELECT k.kitapadi, MAX(k.sayfasayisi) AS en_cok_sayfa_sayisi 
	FROM kitap AS k
	GROUP BY k.kitapadi
	ORDER BY en_cok_sayfa_sayisi DESC
	LIMIT 1;


	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
    SELECT MIN(k.sayfasayisi) AS en_az_sayfa_sayisi FROM kitap AS k
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
    SELECT k.kitapadi, MIN(k.sayfasayisi) AS en_az_sayfa_sayisi 
	FROM kitap AS k
	GROUP BY k.kitapadi
	ORDER BY en_az_sayfa_sayisi ASC
	LIMIT 1;
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
	
    SELECT k.kitapadi, MAX(k.sayfasayisi) AS least 
    FROM kitap AS k
    INNER JOIN tur AS t
    ON k.turno = t.turno 
    WHERE t.turadi = 'Dram'
    GROUP BY k.kitapadi 
    ORDER BY least DESC
    LIMIT 1;
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	
    SELECT o.ograd, o.ogrsoyad, SUM(k.sayfasayisi) 
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
    
    SELECT o.ograd, COUNT(o.ograd) FROM ogrenci o
    GROUP BY o.ograd
    ORDER BY o.ograd
	
	26) Her sınıftaki öğrenci sayısını bulunuz.
	
    SELECT o.sinif, COUNT(o.ogrno) FROM ogrenci o 
    GROUP BY o.sinif
    ORDER BY o.sinif
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
    SELECT o.sinif, o.cinsiyet, COUNT(o.ogrno) FROM ogrenci o 
    GROUP BY o.sinif, o.cinsiyet
    ORDER BY o.sinif, o.cinsiyet
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
    SELECT o.ograd,o.ogrsoyad, SUM(k.sayfasayisi) AS total_read FROM ogrenci o
	INNER JOIN islem i ON o.ogrno = i.ogrno
	INNER JOIN kitap k ON k.kitapno = i.kitapno
	GROUP BY  o.ograd,o.ogrsoyad
	ORDER BY  total_read DESC;	

	29) Her öğrencinin okuduğu kitap sayısını getiriniz.
   
    SELECT o.ograd,o.ogrsoyad, COUNT(i.islemno) AS total_book FROM ogrenci o
	INNER JOIN islem i ON o.ogrno = i.ogrno
	GROUP BY  o.ograd,o.ogrsoyad
	ORDER BY  total_book DESC;