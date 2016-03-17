<!--- Copyright (C) 2009-2013 Typesafe Inc. <http://www.typesafe.com> -->
# Bir Play uygulamasının anatomisi

## Standart uygulama düzeni

Bir Play uygulamasının düzeni bir şeyleri olabildiğince basit tutmak için standartlaştırılmıştır . İlk başarılı derlemenin ardından, standart bir Play uygulaması şu şekilde görünür:

```
app                      → Uygulama kaynakları
 └ assets                → Derlenmiş kaynaklar
    └ stylesheets        → Tipik LESS CSS kaynakları
    └ javascripts        → Tipik CoffeeScript kaynakları
 └ controllers           → Uygulama kontrolörleri
 └ models                → Uygulama iş katmanı
 └ views                 → Şablonlar
build.sbt                → Uygulama kurulum betiği
conf                     → Yapılandırma dosyaları ve diğer derlenmemiş kaynaklar (sınıfyolu üzerind)
 └ application.conf      → Ana yapılandırma dosyası
 └ routes                → Rota tanımlama
public                   → Açık varlıklar
 └ stylesheets           → CSS dosyaları
 └ javascripts           → Javascript dosyaları
 └ images                → Görüntü dosyaları
project                  → sbt yapılandırma dosyaları
 └ build.properties      → sbt projesi için belirteç
 └ plugins.sbt           → Beyanı dahil olmak üzere Play için sbt eklentileri
lib                      → Yönetilmemiş kütüphane bağımlılıkları
logs                     → Standart günlük klasörü
 └ application.log       → Varsayılan günlük dosyası
target                   → Oluşturulmuş sayfalar
 └ scala-2.10.0            
    └ cache              
    └ classes            → Derlenmiş sınıf dosyaları
    └ classes_managed    → Yönetilen sınıf dosyaları (şablonlar, ...)
    └ resource_managed   → Yönetilen kaynaklar (less, ...)
    └ src_managed        → Oluşturulmuş kaynaklar (şablonlar, ...)
test                     → Fonksiyonel veya birim testler için kaynak klasörü
```

## app/ Dizini

`app` dizini tüm çalıştırılabilir artifaktları içerir: Java ve Scala kaynak kodu, şablonlar ve derlenmiş varlık’ kaynakları.

`app` dizininde üç standart paket bulunur, her bir bileşen MVC yazılım deseninin bir parçasıdır: 

- `app/controllers`
- `app/models`
- `app/views`

Tabiki kendi paketlerinizi ekleyebilirsiniz, örneğin bir `app/utils` paketi.

> Play'de controllers, models ve views paket adlarını yazıldığı gibi kullanma eğilimi vardır ve ihtiyaç duyulursa değiştirilebilirler(Herşeyin başına `com.yourcompany` ekleme gibi )

Ayrıca isteğe bağlı olarak kullanılan, [LESS kaynakları](http://lesscss.org/) ve [CoffeeScript kaynakları](http://coffeescript.org/) gibi derlenmiş nesneler için kullanılan `app/assets` dizini mevcuttur.

## public/ dizini

`public` dizininde depolanan kaynaklar direkt olarak Web sunucularına hizmet eden statik nesnelerdir.

Bu dizin görüntüler(images), CSS dosyaları(stylesheets) ve Javascript dosyaları için üç alt dizine ayrılır. Tüm play uygulamalarını tutarlı bir halde tutabilmek için tüm statik nesnelerinizi bu şekilde organize etmelisiniz.

> Yeni oluşturulan uygulamada, `/public` dizini `/assets` URL adresi ile ilişkilendirilir, fakat bu durumu kolaylıkla değiştirebilir ya da statik nesneler için birbirinden farklı dizinler bile kullanabilirsiniz.

## conf/ dizini

`conf` dizini uygulamanın konfigürasyon dosyalarını barındırır. İki tane konfigürasyon dosyası vardır:

- `application.conf`, uygulama için ana konfigürasyon dosyası, standart konfigürasyon parametrelerini içerir.
- `routes`, rota tanım dosyası

Eğer uygulamanıza özel bir ayar seçeneği eklemeye ihtiyacınız varsa, `application.conf` dosyasına daha fazla seçenek eklemek iyi bir fikirdir.

Eğer bir kütüphane özel bir ayar dosyasına ihtiyaç duyuyorsa, o dosyayı `conf` dizininin altına koymayı deneyin.

## lib/ dizini

`lib` dizini isteğe bağlıdır ve build sistemin dışında, manuel olarak yönetmek istediğiniz tüm jar dosyaları gibi yönetilmeyen kütüphane bağımlılıklarını barındırır. Jar dosyaları bu dizine bırakılarak uygulama classpath'ine eklenebilirler.

## build.sbt dosyası

Projenizin ana build tanımlamaları genellikle proje kökünde `build.sbt`dosyasında bulunur. `project/` dizinindeki `.scala` dosyalarıda projenin build tanımlamalarını yapmak için kullanılabilir.

## project/ dizini

`project` dizini sbt oluşturma tanımlarını içerir:

- `plugins.sbt` bu projede kuullanılan sbt eklentilerini tanımlar.
- `build.properties` uygulamanızı oluşturmak için kullanılan sbt sürümünü içerir.

## target/ dizini

`target` dizini build sistem tarafından oluşturulan herşeyi içerir. Burada neler oluşturulduğunu bilmek faydalı olabilir.

- `classes/` tüm derlenmiş sınıfları içerir (Java ve Scala kaynaklarının her ikisinden).
- `classes_managed/` talnızca framework tarafından yönetilen sınıfları içerir (router ya da template sistem tarafından oluşturulan sınıflar gibi). Bu sınıf dizinini IDE projenize harici sınıf dizini olarak eklemek faydalı olabilir.
- `resource_managed/` Özellikle LESS, CSS ve CoffeeScript gibi nesnelerin derlenme çıktıları gibi oluşturulmuş kaynakları içerir.
- `src_managed/` Template sistem tarafından oluşturulan Scala kaynakları gibi oluşturulmuş kaynakları içerir.
- `web/` `app/assets` ve `public` dizinlerinde bulunanlar gibi [sbt-web](https://github.com/sbt/sbt-web#sbt-web) tarafından işlenen nesneleri içerir.

## Tipik .gitignore dosyası

Oluşturulan dizinler versiyon kontrol sisteminiz tarafından yoksayılmalıdır. Bir Play uygulaması için tipik `.gitignore` dosyası:

```txt
logs
project/project
project/target
target
tmp
dist
.cache
```

> **Sonraki:** [[Play konsolunu kullanmak | PlayConsole ]]
