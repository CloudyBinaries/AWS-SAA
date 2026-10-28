
![[Pasted image 20260227111318.png]]

سؤال ممتاز 👌 وده موضوع مهم جدًا في تصميم الشبكات في AWS.

خليني أرتبهولك بشكل مذاكرة واضحة 👇

---

# 📌 أولًا: يعني إيه VPC Endpoint؟

الـ **VPC Endpoint** هو طريقة تخلي الـ EC2 أو أي Resource جوه الـ VPC يوصل لخدمات AWS:

- من غير ما يطلع على الإنترنت
    
- من غير ما يستخدم NAT
    
- من غير ما يعدي على Internet Gateway
    

يعني الاتصال يفضل جوه شبكة AWS الداخلية (Private Network).

---

# ليه محتاجينه؟

لو عندك:

```
Private EC2 (No Public IP)
```

وعايز توصل لـ:

- S3
    
- DynamoDB
    
- SSM
    
- SNS
    
- KMS
    

قدامك 3 حلول:

1. تديله Public IP ❌ (خطر)
    
2. تستخدم NAT Gateway 💰
    
3. تستخدم VPC Endpoint ✅ (أأمن وأرخص غالبًا)
    

---

# 📌 أنواع VPC Endpoint

في نوعين أساسيين:

1️⃣ Gateway Endpoint  
2️⃣ Interface Endpoint

---

# أولًا: Gateway Endpoint

## بيشتغل إزاي؟

- بيتضاف في Route Table
    
- ملوش ENI
    
- ملوش Security Group
    
- الترافيك بيتحول مباشرة من خلال AWS backbone
    

## بيوصل لإيه؟

فقط خدمتين:

- Amazon S3
    
- Amazon DynamoDB
    

(الاتنين خدمات Data كبيرة وموزعة جدًا)

---

## مثال عملي

```
Private EC2 → Route Table → Gateway Endpoint → S3
```

من غير:

- NAT
    
- Public IP
    
- Internet Gateway
    

---

# ليه S3 و DynamoDB بس؟

لأنهم:

- لانها Highly distributed services
    
- بيتعاملوا بآلية Routing ضخمة
    
- مناسب ليهم Route-based redirection
    

يعني AWS قدرت تعملهم integration على مستوى الـ Route Table مباشرة.

---

# ثانيًا: Interface Endpoint

## بيشتغل إزاي؟

- ا AWS بتنشيء ENI (Network Interface) جوه الـ Subnet
    
- ليه Private IP
    
- ليه Security Group
    
- بيتعامل كأنه Network Interface عادي
    

يعني فعليًا:

```
Private EC2 → ENI (Interface Endpoint) → AWS Service
```

---

## بيوصل لإيه؟

تقريبًا معظم خدمات AWS:

- EC2 API
    
- SSM
    
- KMS
    
- SNS
    
- SQS
    
- CloudWatch
    
- Secrets Manager
    
- ECR
    
- Lambda
    
- وغيرها
    

---

# الفرق الأساسي بين الاتنين

|Gateway Endpoint|Interface Endpoint|
|---|---|
|Route Table based|ENI based|
|لا يستخدم SG|يستخدم SG|
|فقط S3 + DynamoDB|أغلب خدمات AWS|
|أرخص|أغلى شوية|
|مفيش Private IP خاص|له Private IP|

---

# طيب ليه في نوعين أصلاً؟

السؤال الذكي 👌

السبب معماري:

## Gateway Endpoint

- معمولة خصيصًا لخدمات ضخمة جدًا (S3/DynamoDB)
    
- optimized routing
    
- أقل تكلفة
    
- scalable جدًا
    

---

## Interface Endpoint

- مناسب لأي Service API-based
    
- لأن أغلب خدمات AWS عبارة عن Endpoints API
    
- محتاجة ENI علشان:
    
    - تطبق Security Groups
        
    - تعمل Private DNS
        
    - تبقى مرنة
        

---

# ملحوظة مهمة جدًا 👀

Interface Endpoint هو تطبيق لـ:

AWS PrivateLink

وهو نظام يسمح بعمل Private access بين VPC وخدمة.

---

# مثال تصميم احترافي

عندك:

```
Private Subnet
    ↓
   EC2
    ↓
Interface Endpoint (SSM)
    ↓
AWS SSM Service
```

كده تقدر:

- تدخل على السيرفر
    
- من غير Bastion
    
- من غير NAT
    
- من غير Public IP
    

وده Design أأمن بكتير.

---

# إمتى أستخدم كل نوع؟

## استخدم Gateway Endpoint لو:

- عايز توصل لـ S3 أو DynamoDB
    
- عايز أقل تكلفة
    
- مش محتاج Security Group
    

---

## استخدم Interface Endpoint لو:

- بتوصل لأي خدمة AWS تانية
    
- عايز تحكم في Security Group
    
- محتاج Private DNS
    

---

# الجملة اللي تتحفظ 🔥

> Gateway Endpoint = Route-based لسيرفيسز ضخمة (S3/DynamoDB)  
> Interface Endpoint = ENI-based لباقي خدمات AWS --- DNS Resolution

---
![[Pasted image 20260227214558.png]]

في واحد من السناريوهات اللي بتيجي في الامتحان انها كانت لو انا عاوز اوصل من ال On-Prem Datacenter  علي ال S3 Bucket Privately --- و الفكرة اني لو حبيت اوصل بال Gateway Endpoint لازم اوصل عليه من جوه ال VPC مش بشكل مباشر من برة علطول ب VPN مثلا 

 اني اعمل اتصال بال VPN مع Private Subnet و اوصل علي Proxy Server يبعت الطلب علي Gateway Endpoint لل S3 في الاخر بس لازم اكون مظبط ال DNS علي انه لو عاوز اروح ل S3 لازم اوصل علي ال VPN لل Proxy Server اللي جوه --- و ال Proxy هيعمل ال NATing لل implied router يبعت ال Request دا لل Gateway Endpoint 

--------
![[Pasted image 20260227215645.png]]


الصورة بتتكلم عن إن في حاجة اسمها **VPC Endpoint Policy** سواء للـ Gateway Endpoint أو للـ Interface Endpoint (ENI).

بشكل افتراضي، أي Endpoint بيتعمل بيبقى سامح Full Access على الخدمة اللي هو مربوط بيها.

لكن نقدر نستخدم الـ Endpoint Policy عشان نتحكم في الصلاحيات جوه الخدمة نفسها.

يعني إيه الكلام ده؟

يعني أقدر أقول إن أي Request هيعدي من خلال الـ Endpoint ده:

- يقدر يعمل عمليات معينة بس (زي Get Object بس)
    
- أو يوصل لـ S3 Bucket معين
    
- أو Resources معينة جوه الخدمة
    
- أو حتى Account معين
    

وده بيتم عن طريق إننا بنحط Policy Attached للـ Endpoint.

---

لكن هنا لازم نفهم نقطة مهمة عشان ميبقاش في لخبطة 

لما المحاضر قال:

> أتحكم مين يستخدم الـ Endpoint من IPs أو Subnets أو من VPC معينة

الكلام ده ممكن يتحقق بطريقتين مختلفتين، وكل واحدة ليها طبقة مختلفة من التحكم.

---

## أول طبقة: Network Access Control 

ده بيتحدد حسب نوع الـ Endpoint:

### 🔹 لو Gateway Endpoint

- مفيش Security Group
    
- التحكم بيبقى عن طريق Route Table
    
- أي Subnet فيها Route للـ Endpoint تقدر توصله
    
- وكونه Regional معناه إنه متاح لأي AZ جوه نفس الـ Region طالما متضاف في الـ Route Table
    

يعني هنا تحديد مين يوصل بيتم شبكيًا عن طريق الـ Routing.

---

### 🔹 لو Interface Endpoint (ENI)

- عليه Security Group
    
- وهنا فعلاً أقدر أحدد:
    
    - IP معينة
        
    - CIDR معينة
        
    - Port معين
        

وده تحكم شبكي صريح.

---

## ثاني طبقة: Endpoint Policy (IAM Level)

دي بقى اللي الصورة بتتكلم عنها.

وظيفتها مش تمنع الوصول الشبكي  
لكن تتحكم في قبول أو رفض الـ Request منطقيًا.

يعني أقدر أقول:

- اسمح لو الطلب جاي من VPC معينة (باستخدام Condition زي aws:SourceVpc)
    
- أو من Endpoint معين (aws:SourceVpce)
    
- أو من Principal معين
    

فهنا الطلب ممكن يوصل فعليًا للـ Endpoint  
لكن لو الشرط مش متحقق → ياخد Access Denied.

---

1️⃣ هل يقدر يوصل للـ Endpoint أصلاً؟  
→ Route Table (Gateway) أو Security Group (Interface)

2️⃣ بعد ما يوصل، هل مسموح له يعمل العملية؟  
→ Endpoint Policy

3️⃣ هل الخدمة نفسها سامحة؟ (زي Bucket Policy في S3)  
→ Resource Policy

---

فالكلام مش متعارض مع المحاضر،  
بس لازم نفهم إن:

التحكم في IP/Subnet ممكن يكون:

- يا إما Network Layer (Security Group / Route Table)
    
- يا إما Logical Restriction داخل Endpoint Policy باستخدام Conditions
    

لكن الاتنين مش نفس الحاجة.

---

والفكرة الأساسية اللي تحفظها:

ال Network Access Control ≠ Service Permission Control

الأولى (Route Table / Security Group)  
والتانية (Endpoint Policy)

-------
و انا بخلق ال Gateway بيقولي اختار ايه ال Route Tables اللي عاوز تعملها Update عشان توصل ليا "Gateway Endpoint"

![[Pasted image 20260227223359.png]]

![[Pasted image 20260227223708.png]]
