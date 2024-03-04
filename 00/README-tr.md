# Başlangıç  

<canvas id="custom" class="canvas" data-fragment-url="cmyk-halftone.frag" data-textures="vangogh.jpg" width="700px" height="320px"></canvas>

Yukarıdaki resimler farklı yollarla yapıldı. İlki Van Gogh'un elleriyle katman katman boya uygulaması ile yapıldı. Saatler sürdü. İkincisi ise saniyeler içerisinde dört piksel matrisinin birleşimiyle yapıldı: biri siyan, biri magenta, biri sarı ve biri siyah için. Ana fark, ikinci resmin o anda üretilmesi (yani adım adım değil, aynı anda).

Bu kitap dijital olarak üretilen resimleri bir sonraki seviyeye taşıyan, "fragment (piksel) shader" adı verilen devrimci hesaplama tekniği hakkındadır. Bu, grafikler için Gutenberg'in matbaası gibi düşünülebilir.

![Gutenberg'in Matbaası](gutenpress.jpg)

Fragment shader'lar, size ekranda render edilen pikseller üzerinde tam kontrol sağlar ve bunu süper hızlı bir şekilde yapar. İşte bu yüzden, cep telefonlarındaki video filtrelerinden tutun inanılmaz 3D video oyunlarına kadar hemen hemen her türlü durumda kullanılırlar.

![That Game Company tarafından geliştirilen "Journey" oyunu](journey.jpg)

Kitabın ilerleyen bölümlerinde bu tekniğin ne kadar hızlı ve güçlü olduğunu, profesyonel ve kişisel çalışmalarınıza nasıl uygulayabileceğinizi keşfedeceksiniz.

## Bu Kitap Kime Hitap Ediyor?

Bu kitap; kodlama deneyimi olan, temel lineer cebir ve trigonometri bilgisine sahip ve çalışmalarını grafik kalitesinde heyecan verici yeni bir seviyeye taşımak isteyen yaratıcı kodlayıcılar, oyun geliştiricileri ve mühendisler için yazılmıştır. (Eğer kodlamayı öğrenmek istiyorsanız, [Processing](https://processing.org/) ile başlamanızı ve daha sonra rahat hissettiğinizde buraya dönmenizi şiddetle tavsiye ederim.)

Bu kitap size projelerinize shader'ları nasıl kullanıp entegre edeceğinizi, performanslarını ve grafik kalitelerini nasıl artıracağınızı öğretecektir. GLSL (OpenGL Shading Language) shader'ları birçok platformda derlenip çalıştığı için, burada öğrendiklerinizi OpenGL, OpenGL ES veya WebGL kullanan herhangi bir ortama uygulayabileceksiniz. Yani, [Processing](https://processing.org/) çizimlerinizde, [openFrameworks](http://openframeworks.cc/) uygulamalarınızda, [Cinder](http://libcinder.org/) interaktif uygulamalarınızda ve [Three.js](http://threejs.org/) web sitelerinizde veya iOS/Android oyunlarınızda bilgilerinizi uygulayıp kullanabileceksiniz.

## Bu Kitap Neyi Kapsıyor?

Bu kitap GLSL piksel shader'larının kullanımına odaklanacaktır. Öncelikle shader'ların ne olduğunu tanımlayacağız; ardından onları kullanarak prosedürel şekiller, desenler, dokular ve animasyonlar yapmayı öğreneceğiz. Shader (Gölgeleme) dilinin temellerini öğrenecek ve bunları resim işleme (resim işlemleri, matris konvolüsyonları, bulanıklıklar, renk filtreleri, arama tabloları ve diğer efektler) ve simülasyonlar (Conway'un hayat oyunu, Gray-Scott reaksiyon-diffüzyonu, su dalgaları, suluboya efektleri, Voronoi hücreleri, vb.) gibi daha kullanışlı senaryolara uygulayacağız. Kitabın sonlarına doğru Ray Marching'e dayalı bir dizi gelişmiş teknik göreceğiz.

"Her bölümde sizin oynayabileceğiniz interaktif örnekler bulunmakta." Kodu değiştirdiğinizde, değişiklikleri hemen göreceksiniz. Kavramlar soyut ve kafa karıştırıcı olabilir, bu yüzden interaktif örnekler, materyali öğrenmenize yardımcı olmak için esastır. Kavramları ne kadar hızlı harekete geçirirseniz, öğrenme süreci o kadar kolay olacaktır.

Bu kitap neyi kapsamıyor?:

* Bu bir OpenGL veya WebGL kitabı değildir. OpenGL/WebGL, GLSL veya fragment shader'lardan daha büyük bir konudur. OpenGL/WebGL hakkında daha fazla bilgi edinmek istiyorsanız: [OpenGL Giriş](https://open.gl/introduction), [OpenGL Programlama Kılavuzu'nun 8. Baskısı](http://www.amazon.com/OpenGL-Programming-Guide-Official-Learning/dp/0321773039/ref=sr_1_1?s=books&ie=UTF8&qid=1424007417&sr=1-1&keywords=open+gl+programming+guide) (aynı zamanda kırmızı kitap olarak da bilinir) veya [WebGL: Başlangıç](http://www.amazon.com/WebGL-Up-Running-Tony-Parisi/dp/144932357X/ref=sr_1_4?s=books&ie=UTF8&qid=1425147254&sr=1-4&keywords=webgl) kitaplarına göz atmanızı öneririm.

* Bu kitap matematik kitabı değildir. Cebir ve trigonometri anlayışına dayanan birçok algoritma ve teknik ele alacağız, ancak bunları detaylı olarak açıklamayacağız. Matematikle ilgili sorularınız için yanınızda aşağıdaki kitaplardan birini bulundurmanızı öneririm: [3D Oyun Programlama ve Bilgisayar Grafikleri için Matematik](http://www.amazon.com/Mathematics-Programming-Computer-Graphics-Third/dp/1435458869/ref=sr_1_1?ie=UTF8&qid=1424007839&sr=8-1&keywords=mathematics+for+games) veya [Oyunlar ve Etkileşimli Uygulamalar için Temel Matematik](http://www.amazon.com/Essential-Mathematics-Games-Interactive-Applications/dp/0123742978/ref=sr_1_1?ie=UTF8&qid=1424007889&sr=8-1&keywords=essentials+mathematics+for+developers).

## Başlamak için neye ihtiyacınız var?

Çok fazla bir şeye ihtiyacınız yok! Eğer WebGL'i destekleyen modern bir tarayıcınız (Chrome, Firefox veya Safari gibi) ve bir internet bağlantınız varsa, bu sayfanın sonundaki "Sonraki" butonuna tıklayarak başlayabilirsiniz.

Alternatif olarak, sahip olduğunuz veya bu kitaptan neye ihtiyacınız olduğuna bağlı olarak:

- [Bu kitabın offline versiyonunu oluşturun](https://thebookofshaders.com/appendix/00/)
- [Örnekleri Raspberry Pi'da tarayıcı olmadan çalıştırın](https://thebookofshaders.com/appendix/01/)
- [Yazdırmak üzere PDF dosyası oluşturun](https://thebookofshaders.com/appendix/02/)

- Bu kitabın [GitHub repo'sunu](https://github.com/patriciogonzalezvivo/thebookofshaders) sorunlarını gidermek ve kod paylaşmak için inceleyin.
