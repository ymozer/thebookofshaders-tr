<canvas id="custom" class="canvas" data-fragment-url="src/moon/moon.frag" data-textures="src/moon/moon.jpg" width="350px" height="350px"></canvas>

# The Book of Shaders
*[Patricio Gonzalez Vivo](http://patriciogonzalezvivo.com/) ve [Jen Lowe](http://jenlowe.net/)* tarafından

Bu kitap, Fragment Shader'ların soyut ve karmaşık evreninde adım adım ilerleyen uysal bir rehberdir.

<div class="header">
<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=B5FSVSHGEATCG" style="float: right;"><img src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif" alt=""></a>
</div>

## Contents

* [Bu Kitap Hakkında](00/)

* Başlangıç
    * [Shader Nedir?](01/)
    * [“Merhaba Dünya!”](02/)
    * [Uniform'lar](03/)
	* [Shader'larınızı Çalıştırma](04/)

* Algoritmik Çizim
    * [Şekillendirme Fonksiyonları](05/)
    * [Renkler](06/)
    * [Şekiller](07/)
    * [Matrisler](08/)
    * [Örüntüler](09/)

* Üretici Tasarımlar
    * [Rastgele](10/)
    * [Gürültü](11/)
    * [Hücresel Gürültü](12/)
    * [Kesirli Brownian Hareketi](13/)
    * Fraktallar

* Görüntü İşleme
    * Dokular (Texture)
    * Görüntü işlemleri
    * Çekirdek (Kernel) konvolüsyonları
    * Filtreler
    * Diğer efektler

* Simülasyon
    * Pingpong
    * Conway
    * Dalgalar
    * Su rengi
    * Reaksiyon yayılımı

* 3D grafikler
    * Işıklar
    * Normal-haritalar
    * Bump-haritalar
    * Işın Adımlama (Ray marching)
    * Çevresel Haritalar (küre ve küp)
    * Yansıtma ve Kırılma

* [Ekler:](appendix/) Bu kitabı başka yollarla kullanma
	* [Bu kitabı çevrimdışı nasıl kullanabilirim?](appendix/00/)
	* [Örnekleri Raspberry Pi üzerinde nasıl çalıştırabilirim?](appendix/01/)
	* [Bu kitabı nasıl yazdırabilirim?](appendix/02/)
    * [Nasıl katkıda bulunabilirim?](appendix/03/)
    * [JS'den gelenler için bir giriş](appendix/04/) [Nicolas Barradeau](http://www.barradeau.com/) tarafından
* [Örnekler Galerisi](examples/)
* [Sözlük](glossary/)

## Yazarlar Hakkında

[Patricio Gonzalez Vivo](http://patriciogonzalezvivo.com/) (1982, Buenos Aires, Argentina) New York merkezli bir sanatçı ve geliştiricidir. Organik ile sentetik, analog ile dijital, bireysel ile kolektif arasındaki ara alanları keşfeder. İşlerinde, birlikte daha iyi bir dünya geliştirmek amacıyla ifade dili olarak kodu kullanır.

Patricio, psikoterapi ve ifade sanatı terapisi okudu ve uyguladı. Parsons The New School'da Tasarım & Teknoloji alanında Yüksek Lisans derecesine (MFA) sahiptir, burada şu anda ders vermektedir. Şu anda Mapzen'de Grafik Mühendisi olarak çalışmakta ve açık kaynak haritalama araçları yapmaktadır.

<div class="header"> <a href="http://patriciogonzalezvivo.com/" target="_blank">Site</a> - <a href="https://twitter.com/patriciogv" target="_blank">Twitter</a> - <a href="https://github.com/patriciogonzalezvivo" target="_blank">GitHub</a> - <a href="https://vimeo.com/patriciogv" target="_blank">Vimeo</a> - <a href="https://www.flickr.com/photos/106950246@N06/" target="_blank"> Flickr</a></div>

[Jen Lowe](http://jenlowe.net/) Datatelling'de bağımsız bir veri bilimci ve veri iletişimcisi olarak insanları + sayıları + kelimeleri bir araya getirir. SVA'nın Sosyal İnovasyon için Tasarım programında ders verir, Şiirsel Hesaplama Okulu'nun kurucu ortağıdır, NYU ITP'de Sanatçılar için Matematik dersi vermiştir, Columbia Üniversitesi'nde Mekansal Bilgi Tasarım Laboratuvarı'nda araştırma yapmıştır ve Beyaz Saray Bilim ve Teknoloji Politikaları Ofisi'nde fikirler katkıda bulunmuştur. SXSW ve Eyeo'da konuşmalar yapmıştır. İşleri The New York Times ve Fast Company tarafından ele alınmıştır. Araştırması, yazıları ve konuşmaları, toplumda veri ve teknolojinin vaatleri ve etkilerini keşfeder. Uygulamalı Matematik alanında B.S. derecesine ve Bilgi Bilimi alanında Yüksek Lisans derecesine sahiptir. Sık sık karşıt görüşte olsa da, her zaman sevginin tarafındadır.

<div class="header"> <a href="http://jenlowe.net/" target="_blank">Site</a> - <a href="https://twitter.com/datatelling" target="_blank">Twitter</a> - <a href="https://github.com/datatelling" target="_blank">GitHub</a></div>

## Teşekkürler
[Tong Li](https://www.facebook.com/tong.lee.9484) ve [Yi Zhang](https://www.facebook.com/archer.zetta?pnref=story)'a [Çince çeviri (中文版)](?lan=ch) için teşekkürler.

[Jae Hyun Yoo](https://www.facebook.com/fkkcloud) ve [June Kim](https://github.com/rlawns324)'e Korece [çeviri (한국어)](?lan=kr) için teşekkürler.

Nahuel Coppero (Necsoft)'a İspanyolca [çeviri (español)](?lan=es) için teşekkürler.

[Raphaela Protásio](https://github.com/Rawphs) ve [Lucas Mendonça](https://github.com/luuchowl)'ya Portekizce [çeviri (portugues)](?lan=pt) için teşekkürler.

[Nicolas Barradeau](https://twitter.com/nicoptere) ve [Karim Naaji](http://karim.naaji.fr/)'ye Fransızca [çeviri (français)](?lan=fr) için teşekkürler.

[Andrea Rovescalli](https://www.earove.info)'ye İtalyanca [çeviri (italiano)](?lan=it) için teşekkürler.

[Michael Tischer](http://www.mitinet.de)'a Almanca [çeviri (deutsch)](?lan=de) için teşekkürler.

[Sergey Karchevsky](https://www.facebook.com/sergey.karchevsky.3)'ye Rusça [çeviri (russian)](?lan=ru) için teşekkürler.

[Vu Phuong Hoang](https://www.facebook.com/vuphuonghoang88)'a Vietnamca [çeviri (Tiếng Việt)](?lan=vi) için teşekkürler.

[Wojciech Pachowiak](https://github.com/WojtekPachowiak)'a Lehçe [çeviri (polski)](?lan=pl) için teşekkürler.

[Manoylov Andriy](https://twitter.com/ManoylovAC)'e Ukraynaca [çeviri (український переклад)](?lan=ua) için teşekkürler.

[Andy Stanton](https://andy.stanton.is/)'a [PDF/EPUB dışa aktarma hattını](https://thebookofshaders.com/appendix/02/) düzeltip iyileştirdiği için teşekkürler.

Bu projeye inanan ve [düzeltmelerle](https://github.com/patriciogonzalezvivo/thebookofshaders/graphs/contributors) ya da bağışlarla katkıda bulunan herkese teşekkürler.

## Yeni Bölümlere Ulaşın

Bildirim maili için kaydolun veya takip edin [Twitter](https://twitter.com/bookofshaders) / <a rel="me" href="https://mastodon.gamedev.place/@bookofshaders">Mastodon</a> / [Discord](shader.zone)

<div id="fd-form-623359074e5181d777e479f9"></div>
<script>
  window.fd('form', {
    formId: '623359074e5181d777e479f9',
    containerEl: '#fd-form-623359074e5181d777e479f9'
  });
</script>

## LICENSE

Telif Hakkı (c) Patricio Gonzalez Vivo, 2015 - http://patriciogonzalezvivo.com/
Tüm hakları saklıdır.
