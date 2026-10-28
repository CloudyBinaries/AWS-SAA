
---

# أولًا: الفكرة المعمارية (زي ما المحاضر شرح)

المحاضر ركّز على حاجتين أساسيتين داخل الـ VPC:

## 1️⃣ Reverse Proxy = المدخل الوحيد من الخارج

هو:

- نقطة الدخول الوحيدة من الإنترنت
    
- المكان اللي بيتطبق فيه الـ Security
    
- اللي بيقرر مين يدخل ومين لأ
    
- اللي بيمرر خدمات معينة فقط (HTTP / RDP / API...)
    

الصورة المعمارية المعتادة:

![Image](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/vpc-example-private-subnets.png)

![Image](https://miro.medium.com/1%2A-ft05Aq5c-rlFL7buOjCtA.png)

![Image](https://docs.microfocus.com/file/images/a/a4/if_you_have_a_reverse_proxy_server%2C_how_to_set_up.png)

![Image](https://homenetworkguy.com/images/how-to/deploy-nginx-proxy-manager-in-dmz-with-opnsense/reverse-proxy-network-diagram_hu27d8091b03003ade4006a9fa74f29185_60658_812x451_resize_q90_h2_lanczos_3.webp)

```
Internet
   ↓
Reverse Proxy (Public Subnet)
   ↓
Private Servers (No Public IP)
```

✔ السيرفرات الخلفية مش مكشوفة  
✔ كل الترافيك لازم يعدي من نقطة واحدة  
✔ الأمن بيتركز في المدخل ده

وده يحقق:

- تقليل Attack Surface
    
- Centralized Security Control
    
- منع الوصول المباشر للسيرفرات
    

---

## 2️⃣ Proxy = نقطة الخروج للإنترنت

المحاضر أشار لفكرة:

> الأجهزة داخل الشبكة تخرج من نقطة موحدة  
> بدون ما تكون مكشوفة مباشرة

ده غالبًا بيكون:

- NAT Gateway
    
- أو Forward Proxy
    

المعماريًا:

```
Private Server
     ↓
Proxy / NAT
     ↓
Internet
```

✔ السيرفر ملوش Public IP  
✔ لكنه يقدر يعمل Update / API Calls  
✔ الخروج يتم عبر نقطة واحدة فقط

---

# ثانيًا: الفهم العملي (إزاي بيخبّي فعليًا؟)

هنا بقى الجزء اللي شرحناه Packet-Level 👇

---

# 🔹 Forward Proxy — بيخبّي العميل إزاي؟

بدون Proxy:

```
Client (1.1.1.1) → Server (8.8.8.8)
```

السيرفر يشوف:

```
Source IP = 1.1.1.1
```

---

مع Proxy:

```
Client (1.1.1.1)
        ↓
Proxy (5.5.5.5)
        ↓
Server (8.8.8.8)
```

اللي بيحصل فعليًا:

1. العميل يعمل TCP Session مع البروكسي
    
2. البروكسي يعمل TCP Session جديد مع السيرفر
    

السيرفر يشوف:

```
Source IP = 5.5.5.5
```

📌 السبب:  
الـ Proxy **أنهى الاتصال الأول وفتح اتصال جديد باسمه هو**

ممكن يضيف:

```
X-Forwarded-For
```

لكن ده اختياري.

---

# 🔹 Reverse Proxy — بيخبّي السيرفر إزاي؟

```
Client (1.1.1.1)
        ↓
Reverse Proxy (5.5.5.5)
        ↓
Backend Server (10.0.0.7)
```

1. العميل يعمل TCP مع الـ Reverse Proxy
    
2. البروكسي يعمل TCP داخلي مع السيرفر
    

العميل يشوف:

```
Destination IP = 5.5.5.5
```

ولا يعرف إن السيرفر الحقيقي:

```
10.0.0.7 (Private)
```

📌 السيرفر الخلفي ممكن:

- ميكونش عنده Public IP
    
- يكون في Private Subnet
    
- غير قابل للوصول من الإنترنت مباشرة
    

---

# الربط بين كلامك وكلام المحاضر 🔥

|كلام المحاضر|التفسير الشبكي العملي|
|---|---|
|مدخل وحيد للأمن|Reverse Proxy هو اللي يستقبل كل الترافيك|
|منع الوصول المباشر|السيرفر Private IP فقط|
|التحكم في الرد|البروكسي هو اللي يقرر يمرر لمين|
|نقطة خروج موحدة|Proxy / NAT هو اللي يعمل Outbound|

---

# أهم فرق تحفظه

|Forward Proxy|Reverse Proxy|
|---|---|
|يخبي العميل|يخبي السيرفر|
|يستخدم للخروج|يستخدم للدخول|
|يحمي Users|يحمي Infrastructure|

---

# في AWS تحديدًا

التصميم الشائع:

```
Internet
   ↓
Application Load Balancer
   ↓
Private EC2
   ↓
NAT (لو محتاجين خروج)
   ↓
Internet
```

- الـ Load Balancer = Reverse Proxy
    
- الـ NAT = Outbound Proxy-like behavior
    
- السيرفرات الخلفية لا تحتاج Public IP
    

---

# الخلاصة الذهبية 🔥

- Reverse Proxy = Secure Entry Point
    
- Proxy / NAT = Controlled Exit Point
    
- الاتنين بيكسروا الاتصال لجزئين
    
- الاتنين بيعملوا New TCP Session
    
- الفرق في **مين مستخبي عن مين**
    

---
