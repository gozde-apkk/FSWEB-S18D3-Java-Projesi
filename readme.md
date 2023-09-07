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

      2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

      select k.kitapadi,t.turadi from kitap as k,tur as t where k.turno=t.turno and t.turadi in("Hikaye","Fıkra")

     3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
      select o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi from ogrenci as o, kitap as k, islem as i where o.ogrno=i.ogrno and i.kitapno=k.kitapno and o.sinif in('10B','10C')
     #join ile yazın
     4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
     select o.ograd, o.ogrsoyad, i.atarih from ogrenci as o INNER JOIN islem as i on o.ogrno=i.ogrno

     5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
    select k.kitapadi,t.turadi from kitap as k inner join tur as t on k.turno=t.turno where t.turadi in("Hikaye","Fıkra")

    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
    select o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi from ogrenci as o inner join  islem as i on o.ogrno=i.ogrno inner join kitap as k on i.kitapno=k.kitapno where o.sinif in('10B','10C') order by o.ograd

    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
    select o.ograd,o.ogrsoyad,i.atarih from ogrenci as o left join islem as i on o.ogrno=i.ogrno

    8) Kitap almayan öğrencileri listeleyin.
    select o.ograd,o.ogrsoyad,i.atarih from ogrenci as o left join islem as i on o.ogrno=i.ogrno where i.atarih IS NULL

    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
     select k.kitapno,k.kitapadi,count(i.kitapno) from kitap as k inner join islem i on k.kitapno=i.kitapno group by k.kitapadi,k.kitapno order by i.kitapno desc

    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
	 //select k.kitapno,k.kitapadi,count(i.kitapno) from kitap as k left join islem i on k.kitapno=i.kitapno group by k.kitapadi,k.kitapno order by i.kitapno desc

    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	 select o.ograd,o.ogrsoyad,k.kitapadi from ogrenci as o inner join islem as i on o.ogrno=i.ogrno inner join kitap as k on k.kitapno=i.kitapno

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	 select o.ograd,o.ogrsoyad,k.kitapadi,y.yazarad,y.yazarsoyad,t.turadi,i.atarih from ogrenci as o inner join islem as i on o.ogrno=i.ogrno inner join kitap as k on k.kitapno=i.kitapno inner join yazar as y on y.yazarno=k.yazarno inner join tur as t on t.turno=k.turno

    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	 select o.ograd,o.ogrsoyad,count(i.kitapno) from ogrenci as o inner join islem as i on o.ogrno=i.ogrno  where o.sinif in("10A","10B") group by o.ograd,o.ogrsoyad

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	 select avg(sayfasayisi) from kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	select k.kitapadi, k.sayfasayisi from kitap as k where k.sayfasayisi>(select avg(k.sayfasayisi) from kitap)

    16) Öğrenci tablosundaki öğrenci sayısını gösterin
	 select count(ogrno) from ogrenci

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	select count(ogrno) as toplam_sayi from ogrenci

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	select count(distinct ograd) as uniqe_name from ogrenci

    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	select sayfasayisi from kitap order by sayfasayisi desc limit 1

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	select kitapadi,sayfasayisi from kitap order by sayfasayisi desc limit 1

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	select sayfasayisi from kitap order by sayfasayisi asc limit 1

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	select kitapadi,sayfasayisi from kitap order by sayfasayisi asc limit 1

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz. 
	select max(k.sayfasayisi) from kitap as k inner join tur as t on k.turno=t.turno where t.turadi='Dram'

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	select sum(k.sayfasayisi) as toplam_sayfa from kitap as k inner join islem as i on i.kitapno=k.kitapno inner join ogrenci as o on i.ogrno=o.ogrno where o.ogrno="15"

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
	//select ograd,count(ograd) from ogrenci group by ograd

     26) Her sınıftaki öğrenci sayısını bulunuz.
	select sinif,count(ogrno) from ogrenci group by sinif

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	select sinif,cinsiyet,count(ogrno) from ogrenci where cinsiyet is not null group by sinif,cinsiyet

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	select o.ograd,o.ogrsoyad,sum(k.sayfasayisi) as toplam_sayfa from ogrenci as o inner join islem as i on i.ogrno=o.ogrno inner join kitap as k on k.kitapno=i.kitapno group by o.ograd,ogrsoyad order by toplam_sayfa desc

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.
	//select o.ograd,o.ogrsoyad,sum(k.kitapno) as toplam_kitap from ogrenci as o inner join islem as i on i.ogrno=o.ogrno inner join kitap as k on k.kitapno=i.kitapno group by o.ograd,ogrsoyad order by toplam_kitap desc

