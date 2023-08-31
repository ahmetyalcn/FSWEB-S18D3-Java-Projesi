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

		SELECT ograd,ogrsoyad,atarih FROM ogrenci AS o, islem AS i WHERE o.ogrno=i.ogrno

	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
		SELECT kitapadi,turadi FROM kitap AS k, tur AS t WHERE k.turno=t.turno and turadi IN('Fıkra' , 'Hikaye')

	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

		SELECT o.ogrno,o.ograd,o.ogrsoyad, k.kitapadi FROM ogrenci AS o, islem AS i, kitap AS k WHERE o.ogrno=i.ogrno and i.kitapno=k.kitapno and sinif IN('10B' , '10C') ORDER BY ograd,ogrsoyad
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

		SELECT ograd,ogrsoyad,atarih FROM ogrenci AS o  INNER JOIN islem AS i ON o.ogrno=i.ogrno
	
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

		SELECT kitapadi,turadi FROM kitap AS k INNER JOIN tur AS t ON k.turno=t.turno and turadi IN('Fıkra' , 'Hikaye')
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

		SELECT o.ogrno,o.ograd,o.ogrsoyad, k.kitapadi 
		FROM ogrenci AS o 
		INNER JOIN islem AS i 
		ON o.ogrno=i.ogrno 
		INNER JOIN kitap AS k 
		ON i.kitapno=k.kitapno 
		WHERE o.sinif IN('10B' , '10C') 
		ORDER BY ograd,ogrsoyad
	
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

		SELECT o.ograd,o.ogrsoyad, i.atarih 
		FROM ogrenci AS o 
		LEFT JOIN islem AS i 
		ON o.ogrno= i.ogrno
	
	
	8) Kitap almayan öğrencileri listeleyin.
	
		SELECT o.ograd,o.ogrsoyad, i.atarih 
		FROM ogrenci AS o 
		LEFT JOIN islem AS i 
		ON o.ogrno= i.ogrno
		WHERE i.atarih is null
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	
		SELECT k.kitapno,k.kitapadi, count(i.islemno)
		FROM kitap AS k
		INNER JOIN islem AS i 
		ON k.kitapno= i.kitapno
		GROUP BY k.kitapno, k.kitapadi
		ORDER BY k.kitapno
		
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

		SELECT k.kitapno,k.kitapadi, count(i.islemno)
		FROM kitap AS k
		LEFT JOIN islem AS i 
		ON k.kitapno= i.kitapno
		GROUP BY k.kitapno, k.kitapadi
		ORDER BY k.kitapno

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
		SELECT o.ograd,o.ogrsoyad, k.kitapadi 
		FROM ogrenci AS o 
		INNER JOIN islem AS i 
		ON o.ogrno=i.ogrno 
		INNER JOIN kitap AS k 
		ON i.kitapno=k.kitapno 

	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
		SELECT o.ograd,o.ogrsoyad, k.kitapadi ,t.turadi, y.yazarad, y.yazarsoyad, i.atarih
		FROM ogrenci AS o 
		LEFT JOIN islem AS i 
		ON o.ogrno=i.ogrno 
		LEFT JOIN kitap AS k 
		ON i.kitapno=k.kitapno
		LEFT JOIN yazar AS y
		ON k.yazarno=y.yazarno  
		LEFT JOIN tur AS t
		ON k.turno=t.turno
		
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

		SELECT o.ograd , o.ogrsoyad, count(i.ogrno) as kitapsayisi
		FROM ogrenci AS o 
		INNER JOIN islem AS i 
		ON o.ogrno=i.ogrno 
		WHERE o.sinif IN('10A' , '10B') 
		GROUP BY ograd,ogrsoyad
		ORDER BY ograd,ogrsoyad
		
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG

		SELECT avg(sayfasayisi) FROM kitap

	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	
		SELECT kitapadi, sayfasayisi  FROM kitap WHERE sayfasayisi > (SELECT  avg(sayfasayisi) AS ortalama FROM kitap)
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin

		SELECT count(ogrno) FROM ogrenci
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

		SELECT count(ogrno) as "toplam sayı" FROM ogrenci
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

		SELECT count(DISTINCT ograd) FROM ogrenci 
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

		SELECT max(sayfasayisi) FROM kitap
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

		SELECT kitapadi,sayfasayisi FROM kitap ORDER BY sayfasayisi DESC LIMIT 1
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

		SELECT min(sayfasayisi) FROM kitap
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

		SELECT kitapadi,sayfasayisi FROM kitap ORDER BY sayfasayisi ASC LIMIT 1
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

		SELECT max(sayfasayisi) FROM kitap as k INNER JOIN tur as t ON k.turno=t.turno WHERE turadi = 'Dram'
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

		SELECT sum(sayfasayisi)
		FROM kitap AS k 
		INNER JOIN islem AS i
		ON k.kitapno=i.kitapno 
		WHERE i.ogrno=15
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

		SELECT ograd, count(ograd) as adet
		FROM ogrenci
		GROUP BY ograd

	26) Her sınıftaki öğrenci sayısını bulunuz.
	
		SELECT sinif, count(ogrno) as ogrenci
		FROM ogrenci
		GROUP BY sinif
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

		SELECT sinif,cinsiyet, count(*) as ogrenci
		FROM ogrenci
		WHERE cinsiyet is not null
		GROUP BY sinif, cinsiyet
			
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
		SELECT o.ograd,o.ogrsoyad, sum(k.sayfasayisi) as total
		FROM ogrenci AS o 
		INNER JOIN islem AS i 
		ON o.ogrno=i.ogrno 
		INNER JOIN kitap AS k 
		ON i.kitapno=k.kitapno 
		GROUP BY o.ograd,o.ogrsoyad
		ORDER BY total DESC	
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.

		SELECT o.ogrno,o.ograd,o.ogrsoyad, count(i.kitapno) as total
		FROM ogrenci AS o 
		INNER JOIN islem AS i 
		ON o.ogrno=i.ogrno 
		GROUP BY o.ogrno,o.ograd,o.ogrsoyad

