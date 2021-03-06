# #nededimben - NLPIFY
![All Text](https://i.ibb.co/5109wQ5/logo.png)
Herkesin bize ait bilgileri bilmesini istemeyiz. Ancak sosyal medyayı kullanırken farkında olmadan kendimize ait birçok kişisel bilgiyi de paylaşmaktayız. Doğal dil işleme alanındaki gelişmeler ile kişilerin paylaştıkları mesajlardan da siyasi görüş, psikolojik durum, konum, meslek gibi kişisel bilgilerine dair başarılı tahminler  yapabilen modeller geliştirilmektedir. Sosyal medya platformlarının uzun ve karmaşık kişisel veri kullanımı sözleşmeleri insanların kendilerine dair ne tür bilgilerin çıkarılabildiklerini anlamalarına yardımcı olmamaktadır.

Her internet kullanıcısının kendisine dair ne tür bilgilerin çıkarılabildiğini bilme hakkı olduğuna inanıyoruz.  
**Çünkü bilgi güçlü bir silahtır**

## Proje Süreci
Doğal dil işleme üzerine yapılan araştırmalar sonucu birçok model ile Türkçe üzerine dil analizi yapmak mümkündür. Özellikle Google tarafından geliştirilen BERT metodu kullanarak eğitilen modeller ile duygu analizi, konum, meslek vb. alanlarda güzel sonuçlar elde edebilmekteyiz.

Ancak bunları yaparken kullandığımız hazır-önceden eğitilimiş- modeller Türkçe harici bir dil üzerinden eğitilmiş olduğundan sonuç Türkçe dilinin yapısal özelliklerinden uzak olmaktadır. Bundan dolayı ise Türkçe veri kullanarak eğitilen [BERTurk](https://github.com/stefan-it/turkish-bert) modeli sunulmuştur.

BERTurk; çoğunlukla Wikipedia verisi kullanarak eğitildiğinden dili çok düzgün ve kurallıdır. **Ancak sosyal medya paylaşımlarının dili genel olarak dilbilgisi bakımından hatalı ve devriktir.**

Bu duruma çözüm/alternatif olarak kendi eğittiğimiz BERTurk-Social modelini Türkçe Doğal Dil İşleme topluluğuna sunmaktayız.  

Oluşturduğumuz modele ise kullanıcıların duygu, saldırganlık ve teyide muhtaçlık analizleri için fine-tune işlemi uyguladık. Bu sayede kullanıcılar paylaşımlarının ne kadar duygusal, ne kadar güvenilir(ve/veya teyite muhtaç) veya ne kadar saldırgan profil oluşturduklarını görebilmektedir.

## Paylaşılan Modeller
Paylaştığımız modeller Türkçe sosyal medya verisi kullanarak oluşturduğumuz 56 milyonluk veri iletisinin 5 milyonluk kısmından eğitilmiştir. Ayrıca BERT metodu yerine onun gibi ancak bazı istisnalar yaparak daha hızlı çalışan RoBERTa metodunu kullandık. 
1 ve 5 milyon Türkçe sosyal medya iletisi kullanılarak oluşturduğumuz RoBERTa modellerimiz:
```
https://huggingface.co/YSKartal/berturk-social-5m
https://huggingface.co/ibahadiraltun/berturk-social
```

5 milyon ileti ile eğitilen modeli 3 task için ayrı ayrı fine tune ederek oluşturduğumuz modeller `/models/fine-tune/` klasörü içerisinde:
```
ft5m_sa: duygu analizi
ft5m_off: saldırganlık analizi
ft5m_cw: teyide muhtaçlık analizi
```

Fine tune etmek için kullandığımız referans veri kaynakları:
```
ft5m_sa: https://github.com/sercankulcu/twitterdata
ft5m_off: https://coltekin.github.io/offensive-turkish/
ft5m_cw: Kendimiz oluşturduk
```

Fine tune işlemini nasıl yaptığımız örneği:
```
https://colab.research.google.com/drive/1l7vZNJ6ZlX_A_6c3Lpkprg9G_AbJIIAI?usp=sharing
```

## Fine Tune Modeller Kullanımı

Colab notebook üzerinden doğrudan kullanım örneği:
```
https://colab.research.google.com/drive/17eyvr29TaFlJMNTcX6A_uZHySSFFDY1y?usp=sharing
```

## Web Uygulaması
Eğittiğimiz modellerin nasıl kullanılabileceğine dair örnek bir web arayüzü geliştirdik. Uygulamamızın çalışma prensibi aşağıdaki gibidir:
![Alt text](https://i.ibb.co/BZKqyxx/app-structure.png)  
Uygulamamız frontend tarafında vue.js ile çalışıp sizden mesaj veya tweet linki girmenizi beklemektedir. Girilen mesajlar sonrasında sunucu tarafında değerlendirilecektir. Bu esnada bize daha çok esneklik kazandırması açısından python ile oluşturduğumuz Predictor yapısı aslında arka tarafta tüm modelleri yükleyip Server tarafından iletilen isteklere gerekli yanıtları döndürmektedir. Burada python kullanarak uygulamaya büyük bir esneklik kazandırmaktayız. Bu sayede istendiğinde dil işleme üzerine yeni eklentiler rahatlıkla geliştirilebilir.  

## Web Kurulum
Web kurulumuna başlamadan önce bilgisayarınızda `npm` ve `node` yüklemesinin yapılmış olması gerekmektedir. Eğer daha önceden yapmadıysanız [buradaki](https://www.npmjs.com/get-npm) adımları takip ederek indirebilirsiniz.  
Öncelikle bu `git clone https://github.com/ibahadiraltun/nededimben.git` diyerek bu kodu indirin ve uygulamamızın bulunduğu dizine `cd web-app` ile gidin.
Sonrasında ise:  
1. `pip3 install -r requirements.txt` - gerekli python paketlerini indirir.  
2. `npm install` - server için gerekli olan node modüllerini indirir.  
3. `cd client && npm install` - client için gerekli olan node modüllerini indirir.  

Uygulamanın çalışması için gerekli olan tüm paketleri indirdik ve localhost'umuzda artık kullanabiliriz. `web-app/` dizininde olduğunuzdan emin olun ve aşağıdaki komutları çalıştırın:  
1. `npm run dev` - server tarafını localhost:8081 de çalıştırır.
2. yeni bir komut sekmesinde `cd client && npm run serve` client tarafını localhost:8080 de çalıştırır.

Harika! Artık localhost:8080/ üzerinden uygulamayı kullanabilirsiniz.  
**Not**: Server ilk kurulduğunda modelin yüklenmesi yaklaşık 30sn sürebilmektedir. Bu sürede istekler yanıtsız kalacaktır. 

![All Text](https://i.ibb.co/fx23XCd/app-ss.png)

## Test
Uygulama `Node v12.18.2 ve Python 3.7.0` ortamında, `macOS 10.15` sürümü ile test edilmiştir. Python tarafında kullanılan kütüphaneler versiyon olarak farklılık gösterebilmektedir. 

## Sunum ve Demo

Proje sunumuna ile colab ve web uygulaması demo videolarına [buradan](https://drive.google.com/drive/folders/1Q1AacR6vTuMMUvpSn3tIaWCDyFMBUhkH?usp=sharing) ulaşabilirsiniz. 

## Referans
https://github.com/stefan-it/turkish-bert  
https://huggingface.co/blog/how-to-train  
https://towardsdatascience.com/text-classification-with-hugging-face-transformers-in-tensorflow-2-without-tears-ee50e4f3e7ed  
https://towardsdatascience.com/bert-text-classification-in-3-lines-of-code-using-keras-264db7e7a358  
https://coltekin.github.io/offensive-turkish   
https://github.com/sercankulcu/twitterdata  


[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)


