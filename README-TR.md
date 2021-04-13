# .NET Core'da Birim Testler i�in Request.Form Mocklama

API ve web istekleriyle �al���rken form-data y� dosya ve �e�i�tli tiplerde veri transferi i�in olduk�a s�k kullan�yoruz. .Net Core'da bu dosyalara controller'lardaki Request.Form nesnesi �zerinden ula��labiliyoruz. �zetle dosyalar burada StringValues tipinde serialize edilerek tutuluyor. Genelde bunlar� kullanabilmek i�in verileri bir IFormFile de�i�kenine aktar�p Streamler yard�m�yla dosyaya yazmam�z gerekiyor. A�a��da birim testler i�in form datay� nas�l mocklayabilece�imizi g�steren �rnekler verilmi�tir.
# Dosyalar� Mocklamak

Normalde verilerimizi ihtiya� duydu�umuz i�eriklerle bir s�n�f olu�turup mocklayabiliriz. Ancak bu dosyalar Http iste�inin i�inde geldi�i i�in burada farkl� bir yol izlemeliyiz. Request.Form controller'�m�z�n HttpContext'inin bir alan�d�r. Yani bir FormCollection nesnesi olu�turup bunu Request.Form alan�na koymam�z gerek. �ncelikle bir dosyan�n bitlerini i�eren bir diziye ihtiyac�m�z var. Metodunuz tip kontrol� yap�yor veya tiplere �zel i�lemler ger�ekle�tiriyorsa bu konuda dikkatli olmal�s�n�z ��nk� **dosya bitlerinin ilk birka�� dosya tipini belirtir**. A�a��daa m�mk�n olan en k���k s�k kullan�lan veri tiplerini i�eren bir proje linkini bulabilirsiniz. 
![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/images/setformdata.png)

Yukar�da yazd���m metotta bir gif dosyas� mocklanm�� ve Request.Form'a konmu�tur. Dosyan�n bitlerini ald�ktan sonra dosyaya yazabilmek i�in bunlar� bir MemoryStream'e veriyoruz. Burada mime t�r� dosyan�n tan�nmas� i�in olduk�a �nemli bu y�zden dikkatli olmal�y�z. Ayr�ca headers alan�nda bunun bir **form-data** oldu�unu da belirtmemiz gerek. Sonras�nda tan�mlad���m�z ba�l�k ve stream ile FormFile'�m�z� olu�turuyoruz. FormFile kurucu fonksiyonuna dosya ismi ve isim alanlar�n� parametre olarak verebilirsiniz. Bunlar� kullanmayaca��m i�in ikisini de bo� string olarak verdim. Bir FormFileCollection nesnesi olu�turarak, yap�land�rd���m�z FormFile'�m�z� buraya ekliyoruz. Son olarak bunu da Request.Form'umuza at�yoruz.

![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/images/setformdata2.png)

Burada da Request.Form u File d���nda farkl� bir veri ile mocklayan versiyonu g�steriliyor. File k�sm� i�in asl�nda ayn� �eyi yap�yoruz. Parametreleri iste�inize g�re d�zenleyebilirsiniz. Burada program�n yapt��� �ey, parametre olarak gelen bir objeyi ya da de�i�keni **StringValues** tipine serialize etmek. ��nk� FormCollection nesnesine verece�imiz dictionary **StringValues** tipinde de�erlere sahip olmal�d�r. Sonras�nda bunu dictionary nesnemize **daha sonra kullanarak ula�aca��m�z ismi** key olarak vererek ekliyoruz.


## Mocklanm�� Form Dosyas�n Kullanmak

![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/images/controller1.png)

Test metotlar�n�zda as�l controller metodunu �a��rmadan hemen �nce haz�rlad���m�z metodu �a��rabilirsiniz. Verilen boolean parametre ile de Null olmayan ancak bo� olan bir Request.Form kullanmak istemeniz durumunda Form u bo�altabilirsiniz. Burada ikinci metodu FormFile �n yan�nda ekstra bir parametre ile kulland���m�z �rnek var:
![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/images/controller2.png)

Buraya "theory" olarak haz�rlad���m�z nesneyi veriyoruz. Ve b�ylece dictionaryde verdi�imiz key ile Request.Form aeklenmi� oluyor. Request.Form'daki dosyalar� mocklamak i�in yapman�z gerekenler bu kadar. Ayr�ca Request'ni di�er alanlar�n� da de�i�tirerek ihtiya�lar�m�za uydurabiliriz. Okudu�unuz i�ni te�ekk�rler. Umar�m i�lerinizde yard�mc� olur.

# Links

 - [Smallest possible [...] files](https://github.com/mathiasbynens/small)
 
