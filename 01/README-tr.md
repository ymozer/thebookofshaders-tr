# Giriş
## Fragment (Piksel) Shader'ları nedir?

Önceki bölümde shader'ları, grafikler için bir matbaa gibi tanımlamıştık. Neden? Ve daha da önemlisi: shader nedir?

![Harf Harften, Sağdaki: William Blades (1891).Sayfadan Sayfaya, Soldaki: Rolt-Wheeler (1920).](print.png)

Daha önceden bilgisayar üzerinden çizim yaptıysanız, bu sürecin bir çember, ardından bir dikdörtgen, bir çizgi, bazı üçgenler çizmek olduğunu bilirsiniz. Bu süreç, bir mektup veya kitap yazmak gibidir - birbirini takip eden bir dizi görevlerdir.

Shader'lar da bir dizi talimatlar kümesidir, ancak talimatlar ekrandaki her piksel için aynı anda çalıştırılır. Bu; yazdığınız kodun, ekrandaki pikselin konumuna bağlı olarak farklı davranması gerektiği anlamına gelir. Bir matbaa gibi, programınız, bir konum alır ve bir renk döndürür. Derlendiğindeyse olağanüstü hızlı çalışır.

![Çince hareket ettirilebilir baskı](typepress.jpg)

## Shader'lar neden bu kadar hızlı?

Bu soruyu yanıtlamak için, "paralel işleme" mucizelerini sunuyorum.

Bir bilgisayarın işlemcisi (CPU) büyük bir endüstriyel boru gibi ve her görev, borudan geçen bir şey gibi - bir fabrika hattı gibi - düşünülebilir. Bazı görevler diğerlerinden daha büyüktür, bu da onların daha fazla zaman ve enerji gerektirdiği anlamına gelir. Onlara daha fazla işlem gücü gerekir. Bilgisayarların mimarisi nedeniyle, görevlerin sırayla çalışması zorunludur; her görevin sırasıyla bitirilmesi gerekmektedir. Modern bilgisayarlar genellikle dört işlemci grubuna sahiptir ve her biri bu borular gibi çalışarak görevleri sırayla tamamlar. Her boru aynı zamanda bir *iş parçacığı (thread)* olarak da bilinir.

![CPU](00.jpeg)

Video oyunları ve diğer grafik uygulamaları, diğer programlardan çok daha fazla işlem gücü gerektirir. Grafiksel içerikleri nedeniyle, piksel piksel işlemler yapmak zorundadırlar. Ekranın her pikseli hesaplanmalıdır ve 3D oyunlarda buna ek olarak geometri ve perspektifler de hesaplanmalıdır.

Hadi boru ve görevler metaforumuza geri dönelim. Ekranın her pikseli, basit bir küçük görevi temsil eder. Tek başına her piksel görevi, CPU için bir sorun değildir, ancak (ve işte sorun burada) küçük görevin her piksel için yapılması gerekmektedir! Bu, eski bir 800x600 ekranında, saniyede 14,400,000 hesaplamaya denk gelir! Evet! Bu, bir mikroişlemciyi aşırı yükleyebilecek kadar büyük bir sorundur. 2880x1800 retina ekranında 60 kare/saniyede çalışan modern bir ekranda bu hesaplama, saniyede 311,040,000 hesaplamaya denk gelir. Grafik mühendisleri bu sorunu nasıl çözerler?

![](03.jpeg)

Tam burada paralel işleme devreye girer. Birkaç büyük ve güçlü mikroişlemci - ya da borular - yerine, aynı anda paralel olarak çalışan birçok küçük mikroişlemciye sahip olmak daha akıllıcadır. İşte Grafik İşlemci Birimi (GPU) budur.

![GPU](04.jpeg)

Şimdi bu küçük mikroişlemcileri, her biri veri akışını yönlendiren boruların bulunduğu bir masa olarak hayal edin. Her bir pikselin verisi bir pinpon topuna benzer; ve düşünün ki, saniyede 14,400,000 pinpon topu, neredeyse herhangi bir boruyu tıkayabilir. Ancak, 800x600 çözünürlükteki her bir 'küçük boru', yani mikroişlemci kanalı, saniyede 480,000 piksel verisi (pinpon topu) taşıyabilir. Bu, mikroişlemcilerin paralel işleme yeteneklerini artırarak daha büyük bir veri akışını yönetebileceğimizi gösterir. Yani, daha fazla paralel mikroişlemci kanalına sahip olmak, daha yüksek hızda daha yüksek veri akışını işleyebilmek anlamına gelir.

Grafik kartlarının bir diğer "süper gücü" de donanım üzerinden hızlandırılan özel matematik fonksiyonlarıdır, bu sayede karmaşık matematiksel işlemler doğrudan mikroçipler tarafından çözülür, yazılım tarafından değil. Bu, ekstra hızlı -elektriğin gidebileceği kadar hızlı- trigonometrik ve matris işlemlerine olanak tanır.

## GLSL Nedir?

GLSL'in açılımı OpenGl Shading Language (OpenGL Gölgeleme Dili) anlamına gelir. Bu, sonraki bölümlerde göreceğiniz shader programlarının belirli bir standardıdır. Donanım ve işletim sistemlerine bağlı olarak farklı türde shader'lar bulunmaktadır. Burada, [Khronos Group](https://www.khronos.org/opengl/) tarafından düzenlenen OpenGL standartları ile çalışacağız. OpenGL'in tarihini anlamak, çoğu garip konvansiyonunun altında yatan nedenleri anlamak için faydalı olabilir. Bu konuda daha fazla bilgi edinmek için: [openglbook.com/chapter-0-preface-what-is-opengl.html](http://openglbook.com/chapter-0-preface-what-is-opengl.html) adresine göz atmanızı öneririm.

## Shader'lar Neden Bu Kadar Zor?

Ben amcanıın söylediği gibi: "Büyük güç, büyük sorumluluk getirir." Paralel işleme de bu kuralı takip eder; GPU'nun güçlü mimari tasarımı, kendi kısıtlamaları ve sınırlamaları ile gelir.

Her boruyu -iş parçacığını (thread)- paralelde çalıştırmak için, her borunun birbirinden bağımsız olması gerekir. Biz buna iş parçacıkları diğer iş parçacıklarının ne yaptığına dair *kör* diyoruz. Bu kısıtlama, verinin aynı yönde akması gerektiği anlamına gelir. Bu nedenle, herhangi bir borunun sonucunu kontrol etmek, girdi verisini değiştirmek veya bir borunun sonucunu başka bir boruya aktarmak mümkün değildir. Boru-boru iletişimine izin vermek, verinin bütünlüğünü tehlikeye atar.

Ayrıca ekran kartı, paralel mikroişlemcileri durmadan meşgul tutar; ve boş kaldıkları ilk anda yeni veri alırlar. Bir iş parçacığının önceki anlarda ne yaptığını bilmek imkansızdır. İşletim sisteminin arayüzündeki bir butonu çiziyor olabilir, sonra da oyundaki gökyüzünü render'lıyor olabilir, ardından bir e-postadaki metni gösteriyor olabilir. Yani her bir iş parçacığı sadece **kör** değil, ayrıca da **belleksiz**dir. Sonucu konumuna bağlı olarak piksel piksel değiştiren genel bir fonksiyonu kodlamak için gereken soyutlamanın yanı sıra, kör ve belleksiz kısıtlamalar shader'ların yeni başlayan programcılar arasında pek popüler olmamasına neden olur.

Fakat endişelenmeyin! İlerleyen bölümlerde basitten ileri seviyeye kadar gölgelendirme hesaplamalarını nasıl yapacağımızı adım adım öğrenceğiz. Eğer bu kitabı bir tarayıcıda okuyorsanız, interaktif örneklerle oynamak için sabırsızlanacaksınız. Eğlenceyi daha da ertelemeden *Sonraki >>* butonuna basın ve kodun içine dalalım!