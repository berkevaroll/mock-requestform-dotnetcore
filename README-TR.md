# .NET Core'da Birim Testler için Request.Form Mocklama

API ve web istekleriyle çalýþýrken form-data yý dosya ve çeþiþtli tiplerde veri transferi için oldukça sýk kullanýyoruz. .Net Core'da bu dosyalara controller'lardaki Request.Form nesnesi üzerinden ulaþýlabiliyoruz. Özetle dosyalar burada StringValues tipinde serialize edilerek tutuluyor. Genelde bunlarý kullanabilmek için verileri bir IFormFile deðiþkenine aktarýp Streamler yardýmýyla dosyaya yazmamýz gerekiyor. Aþaðýda birim testler için form datayý nasýl mocklayabileceðimizi gösteren örnekler verilmiþtir.
# Dosyalarý Mocklamak

Normalde verilerimizi ihtiyaç duyduðumuz içeriklerle bir sýnýf oluþturup mocklayabiliriz. Ancak bu dosyalar Http isteðinin içinde geldiði için burada farklý bir yol izlemeliyiz. Request.Form controller'ýmýzýn HttpContext'inin bir alanýdýr. Yani bir FormCollection nesnesi oluþturup bunu Request.Form alanýna koymamýz gerek. Öncelikle bir dosyanýn bitlerini içeren bir diziye ihtiyacýmýz var. Metodunuz tip kontrolü yapýyor veya tiplere özel iþlemler gerçekleþtiriyorsa bu konuda dikkatli olmalýsýnýz çünkü **dosya bitlerinin ilk birkaçý dosya tipini belirtir**. Aþaðýdaa mümkün olan en küçük sýk kullanýlan veri tiplerini içeren bir proje linkini bulabilirsiniz. 
![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/images/setformdata.png)

Yukarýda yazdýðým metotta bir gif dosyasý mocklanmýþ ve Request.Form'a konmuþtur. Dosyanýn bitlerini aldýktan sonra dosyaya yazabilmek için bunlarý bir MemoryStream'e veriyoruz. Burada mime türü dosyanýn tanýnmasý için oldukça önemli bu yüzden dikkatli olmalýyýz. Ayrýca headers alanýnda bunun bir **form-data** olduðunu da belirtmemiz gerek. Sonrasýnda tanýmladýðýmýz baþlýk ve stream ile FormFile'ýmýzý oluþturuyoruz. FormFile kurucu fonksiyonuna dosya ismi ve isim alanlarýný parametre olarak verebilirsiniz. Bunlarý kullanmayacaðým için ikisini de boþ string olarak verdim. Bir FormFileCollection nesnesi oluþturarak, yapýlandýrdýðýmýz FormFile'ýmýzý buraya ekliyoruz. Son olarak bunu da Request.Form'umuza atýyoruz.

![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/images/setformdata2.png)

Burada da Request.Form u File dýþýnda farklý bir veri ile mocklayan versiyonu gösteriliyor. File kýsmý için aslýnda ayný þeyi yapýyoruz. Parametreleri isteðinize göre düzenleyebilirsiniz. Burada programýn yaptýðý þey, parametre olarak gelen bir objeyi ya da deðiþkeni **StringValues** tipine serialize etmek. Çünkü FormCollection nesnesine vereceðimiz dictionary **StringValues** tipinde deðerlere sahip olmalýdýr. Sonrasýnda bunu dictionary nesnemize **daha sonra kullanarak ulaþacaðýmýz ismi** key olarak vererek ekliyoruz.


## Mocklanmýþ Form Dosyasýn Kullanmak

![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/images/controller1.png)

Test metotlarýnýzda asýl controller metodunu çaðýrmadan hemen önce hazýrladýðýmýz metodu çaðýrabilirsiniz. Verilen boolean parametre ile de Null olmayan ancak boþ olan bir Request.Form kullanmak istemeniz durumunda Form u boþaltabilirsiniz. Burada ikinci metodu FormFile ýn yanýnda ekstra bir parametre ile kullandýðýmýz örnek var:
![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/images/controller2.png)

Buraya "theory" olarak hazýrladýðýmýz nesneyi veriyoruz. Ve böylece dictionaryde verdiðimiz key ile Request.Form aeklenmiþ oluyor. Request.Form'daki dosyalarý mocklamak için yapmanýz gerekenler bu kadar. Ayrýca Request'ni diðer alanlarýný da deðiþtirerek ihtiyaçlarýmýza uydurabiliriz. Okuduðunuz içni teþekkürler. Umarým iþlerinizde yardýmcý olur.

# Links

 - [Smallest possible [...] files](https://github.com/mathiasbynens/small)
 
