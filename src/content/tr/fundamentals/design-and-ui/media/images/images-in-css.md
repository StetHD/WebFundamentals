project_path: /web/_project.yaml
book_path: /web/fundamentals/_book.yaml
description: CSS `background` özelligi; birden çok resim eklemeyi kolaylastiran, tekrarlanmalarini saglayan ve baska olanaklar sunan, ögelere karmasik resimler eklemek için güçlü bir araçtir.

{# wf_review_required #}
{# wf_updated_on: 2014-04-29 #}
{# wf_published_on: 2000-01-01 #}

# CSS içindeki resimler {: .page-title }

{% include "_shared/contributors/TODO.html" %}



CSS `background` özelligi; birden çok resim eklemeyi kolaylastiran, tekrarlanmalarini saglayan ve baska olanaklar sunan, ögelere karmasik resimler eklemek için güçlü bir araçtir.  Medya sorgulariyla birlestirildiginde, arka plan özelligi daha da güçlü hale gelerek ekran çözünürlügü, görüntü alani boyutu ve diger unsurlara dayanarak kosullu resim yüklenebilmesini saglar.



## TL;DR {: .hide-from-toc }
- 'Görüntünün özellikleri için en iyi resmi kullanin; ekran boyutu, cihaz çözünürlügü ve sayfa yerlesimini dikkate alin.'
- <code>min-resolution</code> ve <code>-webkit-min-device-pixel-ratio</code> ile medya sorgulari kullanan yüksek DPI'ya sahip görüntüler için CSS`deki <code>background-image</code> özelligini degistirin.
- Biçimlendirmede 1x resme ek olarak yüksek çözünürlüklü resimler saglamak için srcset tanimlayicisini kullanin.
- JavaScript resim degistirme tekniklerini kullanirken veya son derece sikistirilmis yüksek çözünürlüklü resimleri düsük çözünürlüklü cihazlara sunarken performans maliyetlerini göz önünde bulundurun.


## Kosullu resim yükleme veya sanat yönetimi için medya sorgularini kullanma

Medya sorgulari yalnizca sayfa yerlesimini etkilemekle kalmaz, ayni zamanda resimleri kosullu olarak yüklemek veya görüntü alani genisligine bagli olarak sanat yönetimi saglamak için de kullanilabilir.

Örnegin, asagidaki örnekteki küçük ekranlarda yalnizca `small.png` dosyasi indirilip içerik `div` ögesine uygulanirken, büyük ekranlarda gövdeye `background-image: url(body.png)` ve içerik `div` ögesine `background-image: url(large.png)` uygulanir.

<pre class="prettyprint">
{% includecode content_path="web..//fundamentals/design-and-ui/media/images/_code/conditional-mq.html" region_tag="conditional" lang=css %}
</pre>

## Yüksek çözünürlüklü resimler saglamak için image-set islevini kullanma

CSS`deki `image-set()` islevi, `background` özelliginin davranisini gelistirerek farkli cihaz özellikleri için birden çok resim dosyasi saglanmasini kolaylastirir.  Bu, tarayicinin cihazin özelliklerine bagli olarak en iyi resmi seçmesine olanak tanir; örnegin, bant genisliginin sinirli oldugu bir agda bir 2x görüntüde bir 2x resim veya bir 2x cihazda 1x resim kullanma.


    background-image: image-set(
      url(icon1x.jpg) 1x,
      url(icon2x.jpg) 2x
    );
    

Dogru resmin yüklenmesine ek olarak, tarayici resmi uygun bir sekilde
ölçekler. Diger bir deyisle, tarayici 2x resimlerin 1x resimlerden iki kat genis oldugunu kabul ederek 2x resmi 2 katsayisiyla küçültür. Böylece, resim sayfada ayni boyutta görünür.

`image-set()` destegi hâlâ yenidir ve yalnizca Chrome ile Safari'de `-webkit` tedarikçi firma önekiyle desteklenmektedir.  `image-set()` islevi desteklenmediginde bir yedek resmin eklenmesine de dikkat edilmelidir; örnegin:

<pre class="prettyprint">
{% includecode content_path="web..//fundamentals/design-and-ui/media/images/_code/image-set.html" region_tag="imageset" lang=css %}
</pre>

Yukaridaki kod, image-set islevini destekleyen tarayicilarda uygun ögeyi, aksi halde 1x ögenin yedek resmini yükler. Dikkat edilmesi gereken nokta, `image-set()` tarayici destegi az oldugu için çogu tarayicinin 1x ögesini alacak olmasidir.

## Yüksek çözünürlüklü resimler veya sanat yönetimi saglamak için medya sorgularini kullanin

Medya sorgulari, [cihaz piksel oranina](http://www.html5rocks.com/en/mobile/high-dpi/#toc-bg) temelinde kurallar olusturarak 2x ve 1x görüntüler için farkli resimlerin belirtilmesini mümkün hale getirebilir.


    @media (min-resolution: 2dppx),
    (-webkit-min-device-pixel-ratio: 2)
    {
      /* High dpi styles & resources here */
    }
    

Chrome, Firefox ve Opera tarayicilarinin üçü de standart `(min-resolution: 2dppx)` destegine sahipken Safari ve Android Browser, `dppx` birimi içermeyen eski satici önekli sözdizimini gerektirir.  Bu stillerin yalnizca cihazin medya sorgusuyla eslesmesi durumunda yüklendigini ve temel duruma iliskin stilleri belirlemeniz gerektigini unutmayin.  Bu, tarayicinin medya sorgularina özel çözünürlügü desteklememesi durumunda bir seylerin olusturulacagindan emin olma avantajini da saglar.

<pre class="prettyprint">
{% includecode content_path="web..//fundamentals/design-and-ui/media/images/_code/media-query-dppx.html" region_tag="mqdppx" lang=css %}
</pre>

Ayrica, görüntü alani boyutuna bagli olarak alternatif resimler görüntülemek için min-width sözdizimini de kullanabilirsiniz.  Bu teknik, medya sorgusu eslesmezse resmin indirilmemesi avantajina sahiptir. Örnegin, `bg.png` dosyasi yalnizca tarayici genisligi 500 piksel veya daha büyük oldugunda indirilir ve `body` ögesine uygulanir:


    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    	


