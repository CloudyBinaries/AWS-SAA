
![[Pasted image 20260215030023.png]]

---

### طرق الربط بين الـ On-Prem و الـ AWS بتكون غالبًا واحدة من اتنين:

- **VPN (Site-to-Site VPN)**
    
- **Direct Connect (DX)**
    

وفي التصميم البسيط، الاتنين بيخلصوا على **Virtual Private Gateway (VGW)** المرتبط بالـ VPC  
(وفي تصميمات أكبر ممكن نستخدم Transit Gateway بدل VGW).

---

### بالنسبة للـ VPN:

- بيشتغل فوق الإنترنت العام
    
- بيستخدم IPsec Encryption
    
- سريع في الإعداد
    
- تكلفته أقل
    
- لكن الأداء والـ latency بيعتمدوا على جودة الإنترنت، فمش ثابت دايمًا
    

---

### بالنسبة للـ Direct Connect:

- عبارة عن خط خاص Dedicated بين الـ Data Center و AWS
    
- أسرع
    
- ال latency أقل
    
- أداء ثابت ومستقر
    
- مش بيكون مشفر افتراضيًا
    
- لكنه معزول عن الإنترنت العام
    

---

### لو عايزين نجمع بين الاستقرار والتشفير:

ممكن نعمل VPN فوق Direct Connect  
يعني ناخد:

- ثبات الخط الخاص
    
- - تشفير IPsec
        

وده تصميم شائع في الشركات الكبيرة.

---

### من ناحية الـ Routing داخل الـ VPC:

لو عايزين نوصل لشبكة الـ Data Center  
بنضيف Route في الـ Route Table زي:

```
شبكة الـ Data Center → VGW
```

فالـ Virtual Router جوه الـ VPC  
لما يشوف إن الـ Destination IP تابع للـ On-Prem  
هيبعت الترافيك على الـ Virtual Private Gateway  
ومن هناك يطلع عبر الـ VPN أو Direct Connect حسب التصميم.

---
