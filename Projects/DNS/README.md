# DNS Nasıl Çalışıyor?

İnternet üzerinde sıklıkla tarayıcı kullanıyoruz. Peki hangi websitenin hangi sunucuyu belirttiğini internet nasıl anlıyor? Tarayıcı örneğini kullanarak hadi bunu özetleyelim.

# 1. Tarayıcıdan "patika.dev" isteğini başlattık

**Tarayıcı:** Sahip "patika.dev" sitesine ulaşmak istiyor. Bakalım "patika.dev" için kaydettiğim bir IP adresi var mı...

**Tarayıcı:** Hiç kaydetmemişim bunu. İşletim sisteminden bu sitenin IP adresini öğreneyim.

**Tarayıcı:** Orada mısın _OS_ ? "patika.dev" için IP adresi var mı sende?

**OS:** Bu kadar işin arasında bir de bunu mu istiyorsun? İyi tamam önbellekte bakayım var mı.

**OS:** Bende de yok ama hemen öğrenirim.

# 2. DNS isteğinin Resolver'a ulaşması

**Resolver/ISP:** Bir istemciden "patika.dev"'a ait DNS bilgisi istenmiş. Bir bakayım önbelleğime...

**Resolver/ISP:** Hay Allah... Hiçbir istemci istememiş şimdiye kadar. "patika.dev" için bu bilgiye ulaşamıyorum ama _Root_ sunucuya bunu sorabilirim.

**Resolver/ISP:** Hey _Root_ bana "patika.dev" için DNS bilgisini verebilir misin?

**Root Server:** "patika.dev" için bu bilgiyi veremem ama sana bunu bilgiyi verebilecek .DEV gTLD sunucusunun bilgisini verebilirim.

**Resolver/ISP:** Çok iyi olur. İki de bir buraya gelip sana sormam. .DEV gTLD sunucusunun bilgisini de kaydediyorum.

```terminal3
bash -c "dig a.root-server.net | grep ^a.root-server.net."
```

# 3. İsteğin TLD Sunucusuna Ulaşması

**Resolver/ISP:** Hey _DEV gTLD Sunucusu_ çok uzun yoldan geldim. Bana "patika.dev" 'un adresini verebilir misin?
**DEV gTLD:** Baya yorulmuşsun ama maalesef bunu sana ben söyleyemem. Fakat sana bu bilgiyi verecek olan isim sunucusunun adresini verebilirim. 

```terminal5
bash -c "dig patika.dev NS | grep ^patika.dev."
```

**Resolver/ISP:** Ohoo bir de buraya mı sorucam? Yapacak bir şey yok gidicez artık sahip bekliyor. Ben bu bilgiyi de kaydedeyim de tekrar bu kadar uzak yollara gelmeyeyim. 

# 4. İsteğin Name Server'a Ulaşması

**Resolver/ISP:** Selam Google Domains. Bana `patika.dev` için A kaydını verebilir misin?

**Google Domains:** Doğru yere geldin! `patika.dev` ile alakalı tüm güncel DNS kayıtları bende. Aradığın adres: `75.2.70.75`

```terminal5
bash -c "dig patika.dev A | grep ^patika.dev."
```

**Resolver/ISP:** Mükemmel! Sonunda! Bu adresi kaydediyorum, tekrar bu kadar yola kalbim dayanmaz.

# 5. Dönüş

**Resolver/ISP:** Çok ilginç bir yolculuktu. *Root* sunucu bana *.DEV gTLD*'nin yerini söyledi. O da bana *name server* ın yerini söyledi. Son olarak da *name server* bana istediğim bilgiyi verdi.

**Resolver/ISP:** Hey _OS_, benden istediğin bilgiyi getirdim. 

**OS:** Teşekkür ederim, bu bilgiyi kaydettim. Tekrar rahatsız etmem bu konuda seni. Hey _Tarayıcı_, HTTP isteğini göndermek için al bu bilgiyi kullan.

**Tarayıcı:** Tamamdır `75.2.70.75` 'e `patika.dev` websitesi için `/` adresini göstermesini söyleyeceğim. Arkadaşı çok beklettim, hemen söyleyeyim de diğer derse başlasın.

