---
$title: "Analytics: Temel Bilgiler"
---

AMP analitik hakkında temel bilgileri öğrenmek için buraya tıklayın.

[TOC]

## Amp-piksel veya amp-analitik kullanıyor musunuz?

AMP analitik ve ölçüm ihtiyaçlarınızı karşılamak için iki bileşen kullanır:
[amp-piksel](/docs/reference/components/amp-pixel.html) ve
[amp-analitik](/docs/reference/components/amp-analytics.html).
Her iki seçenek de tanımlı bir son noktaya analitik verileri gönderir.

[Piksel izleme](https://en.wikipedia.org/wiki/Web_beacon#Implementation) gibi basit bir davranış arıyorsanız,
`amp-pixel` bileşeni temel sayfa görünümü izlemesi sağlar;
sayfa görünümü verileri tanımlı bir URL›ye gönderilir.
Satıcıyla bazı entegrasyonlar bu bileşeni çağırabilir,
bu durumda URL son noktası tam olarak belirlenir.

En fazla analitik çözüm için, `amp-analytics` kullanın.
`amp-analytics`‹de sayfa görünümü izleme de çalışır.
Ancak,
bağlantılara ve düğmelere tıklama dahil, kullanıcı katılımını herhangi bir sayfa içeriği türüyle izleyebilirsiniz.
Ayrıca, kullanıcının sayfayı ne kadar kaydırdığını,
sosyal medyayla etkileşim kurup kurmadığını ve daha birçok özelliği ölçebilirsiniz
(bkz.
[AMP Analytics Derinlemesine Giriş]({{g.doc('/content/docs/analytics/analytics_amp/deep_dive_analytics.md', locale=doc.locale).url.path}})).

AMP platformu entegrasyonunun bir parçası olarak,
sağlayıcılar
verilerin toplanmasını ve izleme araçlarına gönderilmesini kolaylaştırmak amacıyla ön tanımlı `amp-analytics` yapılandırmaları sunmuştur.

[Amp-analitik spesifikasyonu](/docs/reference/components/amp-analytics.html) bölümünden satıcı belgelerine erişebilirsiniz.

Sayfalarınızda hem `amp-pixel` hem de`amp-analytics` kullanabilirsiniz:
Basit sayfa görünümüz izleme için `amp-pixel`
ve diğer her şey için `amp-analytics`.
Aynı zamanda, her etiket için birden çok ekleyebilirsiniz.
Birden çok analitik sağlayıcı ile çalışıyorsanız,
her çözüm için bir etiket gerekir.
Daha basit AMP sayfalarının kullanıcılar için daha iyi olduğunu unutmayın;
diğer bir deyişle, ekstra etiketlere ihtiyacınız yoksa kullanmayın.

## Basit bir analitik yapılandırması oluşturun

Basit bir
[amp-piksel](/docs/reference/components/amp-pixel.html) ve
[amp-analitik](/docs/reference/components/amp-analytics.html) yapılandırmasının nasıl oluşturulacağını öğrenin.

### Basit bir amp-piksel yapılandırması

Basit bir `amp-pixel` yapılandırması oluşturmak için,
AMP sayfanızın gövdesine aşağıdakine benzer bir şey ekleyin:

```html
<amp-pixel src="https://foo.com/pixel?RANDOM"></amp-pixel>
```

Bu örnekte,
sayfa görünümü verileri rastgele bir sayıyla birlikte tanımlanan URL›ye gönderilir.
`RANDOM` değişkeni,
[AMP platformundaki değiştirme değişkenlerinden](https://github.com/ampproject/amphtml/blob/master/spec/amp-var-substitutions.md) bir tanesidir.

[Değişken değiştirme](/tr/docs/analytics/analytics_basics.html#değişken-değiştirme) hakkında buradan daha fazla bilgi edinebilirsiniz.

[Amp-piksel](/docs/reference/components/amp-pixel.html) bileşeni yerleşiktir,
böylece `amp-analytics` de dahil AMP›nin uzantılı bileşenlerinde olduğu gibi bir ekleme bildirimi gerekli değildir.
`amp-pixel` etiketini
`<body>` başına mümkün olduğunca yakın yerleştirmeniz gerekmektedir.
İzleme pikseli yalnızca etiket kendini görüntülediğinde uyarı verecektir.
`amp-pixel` sayfanın altına doğru yerleştirildiyse,
uyarı vermeyebilir.

### Basit amp-analitik yapılandırması

Basit bir
[amp-analytics](/docs/reference/components/amp-analytics.html) yapılandırması oluşturmak için,
ilk önce bu `custom-element` bildirimini
`<head>` AMP belgesine eklemeniz gerekir (ayrıca bkz.
[Bileşen ekleme bildirimi](/docs/reference/components.html)):

```html
<script async custom-element="amp-analytics" src="https://cdn.ampproject.org/v0/amp-analytics-0.1.js"></script>
```

Aşağıdaki örnek [`amp-pixel` örneği](/tr/docs/analytics/analytics_basics.html#basit-bir-amp-piksel-yapılandırması) ile benzerdir.
Bir sayfa her görünür olduğunda,
tetikleme etkinliği uyarı verir ve
sayfa görüntüleme verilerini rastgele bir ID ile birlikte tanımlı URL›ye gönderir:

```html
<amp-analytics>
<script type="application/json">
{
  "requests": {
    "pageview": "https://foo.com/pixel?RANDOM",
  },
  "triggers": {
    "trackPageview": {
      "on": "visible",
      "request": "pageview"
    }
  }
}
</script>
</amp-analytics>
```

Yukarıdaki örnekte, sayfa görünümü `https://foo.com/pixel?RANDOM` olacak şekilde bir istek tanımladık. Daha önce belirtildiği gibi, RANDOM rastgele bir sayı ile değiştirilmiştir, böylece istek aslında `https://foo.com/pixel?0.23479283687235653498734` olarak görüntülenecektir.

Sayfa görünür olduğunda
(`visible` tetikleme anahtar kelimesinin kullanımıyla belirtildiği gibi),
bir etkinlik tetiklenir ve `pageview` isteği gönderilir.
Tetikleme sayfa görüntüleme isteği uyarısının ne zaman çıkacağını belirler.
[İstekler ve tetikleme](/tr/docs/analytics/deep_dive_analytics.html#istek,-tetikleme-ve-taşıma) hakkında daha fazla öğrenin.

## Değişken değiştirme

Hem [amp-piksel](/docs/reference/components/amp-pixel.html) hem de
[amp-analitik](/docs/reference/components/amp-analytics.html) bileşenleri
standart URL değişken değiştirmelerin tamamına izin verir (bkz.
[AMP HTML Değişken Değiştirme](https://github.com/ampproject/amphtml/blob/master/spec/amp-var-substitutions.md)).
Aşağıdaki örnekte,
sayfa görünümü isteği
mevcut AMP belgelerinin standart URL›si, başlığı ve bir
[istemci kimliği](/tr/docs/analytics/analytics_basics.html#kullanıcı-tanımlama) ile birlikte URL›ye gönderilir:

```html
<amp-pixel src="https://example.com/analytics?url=${canonicalUrl}&title=${title}&clientId=${clientId(site-user-id)}"></amp-pixel>
```

Basitliği nedeniyle, `amp-pixel` etiketi yalnızca platform tarafından tanımlanan veya AMP çalışma zamanının AMP sayfasından ayrıştırabileceği değişkenleri içerebilir. Yukarıdaki örnekte, platform hem
`canonicalURL` hem de `clientId(site-user-id)` için değerler üretir.
`amp-analytics` etiketi `amp-pixel` ile aynı değerleri,
aynı zamanda etiket yapılandırması içerisinde özel olarak tanımlanan değişkenleri içerebilir.

`${varName}` formatını bir sayfa
 ya da platform tanımlı değişken için bir istek dizesi içinde kullanın.
`amp-analytics` etiketi, şablonu
analitik isteğinin oluşturulduğu zamandaki gerçek değeri ile değiştirecektir (ayrıca bkz.
[Amp-analitik içinde desteklenen değişkenler](https://github.com/ampproject/amphtml/blob/master/extensions/amp-analytics/analytics-vars.md)).

Aşağıdaki `amp-analytics` örneğinde,
sayfa görünümü isteği, `amp-analytics` yapılandırması içerisinde
bir kısmı platformdan
bir kısmı da tanımlanan satır içinden gelen değişken değiştirmelerden çıkartılan
ek verilerle birlikte
URL›ye gönderilir:

```html
<amp-analytics>
<script type="application/json">
{
  "requests": {
    "pageview":"https://example.com/analytics?url=${canonicalUrl}&title=${title}&acct=${account}&clientId=${clientId(site-user-id)}",
  },
  "vars": {
    "account": "ABC123",
  },
  "triggers": {
    "someEvent": {
      "on": "visible",
      "request": "pageview",
      "vars": {
        "title": "My homepage",
      }
    }
  }
}
</script>
</amp-analytics>
```

Yukarıdaki örnekte,
değişkenler `account` ve`title`
`amp-analytics` yapılandırmasında tanımlanmıştır.
`canonicalUrl` ve `clientId` değişkenleri yapılandırmada tanımlı değildir,
bu nedenle değerleri platform tarafından değiştirilir.

**Önemli:** Değişken değiştirme esnekliğe sahiptir;
aynı değişken farklı lokasyonlarda tanımlanmış olabilir
ve AMP çalışma zamanı öncelik sırasına bu değerleri ayrıştıracaktır
(bkz. [Değişken değiştirme sıralaması](/tr/docs/analytics/deep_dive_analytics.html#değişken-değiştirme-sıralaması)).

## Kullanıcı tanımlama

Web siteleri tarayıcıda bir kullanıcıya özgü bilgileri saklamak için çerezler kullanır.
Çerezler kullanıcının bir internet sitesini daha önce ziyaret edip etmediğini belirlemek için kullanılabilir.
AMP›de,
sayfalar bir yayıncının web sitesinden ya da bir önbellekten
(Google AMP Cache gibi) sağlanabilir.
Yayıncının web sitesi ile önbelleğin farklı etki alanlarına sahip olması olasıdır.
Güvenlik nedenleriyle,
tarayıcılar bir başka etki alanının çerezlerine erişimi kısıtlayabilir (ve genellikle de kısıtlarlar)
(ayrıca bkz.
[Kaynaklar arasında kullanıcıları izleme](https://github.com/ampproject/amphtml/blob/master/extensions/amp-analytics/cross-origin-tracking.md)).

Varsayılan olarak,
AMP sayfaya bir yayıncının orjininal web sitesinden mi yoksa bir önbellekten mi erişildiğine göre istemci kimliğinin sağlanmasını yönetecektir.
AMP tarafından oluşturulan istemci kimliğinde bir `"amp-"` değeri ile
rastgele bir `base64` şifreli dize vardır ve
kullanıcı tekrar ziyaret ettiğinde aynı kalır.

AMP tüm durumlarda istemci kimliğinin okunmasını ve yazılmasını yönetir.
Bu özellikle, bir sayfa
önbellekle sağlandığında geçerlidir, aksi halde yayıncının orijinal sitesinin görüntüleme bağlamının
 dışında gösterilir.
Bu durumda, yayıncının internet sitesinin çerezlerine erişilemez.

AMP sayfası bir yayıncı internet sitesinden sağlandığında,
AMP›nin kullandığı istemci kimlik çerçevesinin
bir son çare çerezi arayıp kullanması istenebilir.
Bu durumda,
`clientId` değişkeninin`cid-scope-cookie-fallback-name` bağımsız değişkeni
bir çerez adı olarak yorumlanabilir.
Format
`CLIENT_ID(cid-scope-cookie-fallback-name)` ya da
`${clientId(cid-scope-cookie-fallback-name)}` olarak görünebilir.

Örneğin;

```html
<amp-pixel src="https://foo.com/pixel?cid=CLIENT_ID(site-user-id-cookie-fallback-name)"></amp-pixel>
```

AMP tarafından bu çerezin ayarlandığı bulunursa,
istemci ID değişikliği çerez değerini döndürür.
AMP tarafından bu çerezin ayarlı olmadığı bulunursa,
bu durumda AMP bir `amp-` formu değeri ile ardından
rastgele bir base64 şifreli dize oluşturacaktır.

İsteğe bağlı bir kullanıcı bildirim kimliğinin nasıl ekleneceği dahil,
istemci kimliği değişikliği hakkında daha fazla bilgi için,
bkz. [AMP analitikte desteklenen değişkenler](https://github.com/ampproject/amphtml/blob/master/extensions/amp-analytics/analytics-vars.md).
