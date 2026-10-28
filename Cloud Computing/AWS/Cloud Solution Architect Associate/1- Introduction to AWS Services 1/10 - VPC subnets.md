## Private Subnet vs Public Subnet

![[Pasted image 20260215010146.png]]

## امتي اقول علي ال subnet دي private 

اما يكون ال route table  مافيهوش 0.0.0.0/0 , و اللي معناها اي IP 
و بيرميها علي سواء علي ال IGW / VGW او Attached عامتا  

## طب الفكرة في ايه ؟؟

دلوقتي لما الـ request تطلع من الـ instance  
هي بتوصل للـ **Virtual Router بتاع الـ VPC**

الراوتر ده مش بيشوف الباكت جاية من أنهي subnet علشان يقرر  
لكن هو بيطبق **الـ Route Table المرتبطة بالـ Subnet اللي الـ instance موجودة فيها**

يبدأ يعمل عملية routing كالتالي:

- يشوف الـ Destination IP
    
- يعمل Longest Prefix Match في الـ Route Table
    

لو لقى route مطابق  
→ يعمل forward عادي للـ target المحدد (سواء كان local أو IGW أو غيره)

لو ملقاش route محدد مطابق  
→ يشوف هل فيه default route اللي هي 0.0.0.0/0

لو موجودة  
→ يبعت الباكت على الـ target المرتبط بيها (زي الـ Internet Gateway مثلاً)

لو مش موجودة  
→ ساعتها الباكت بتتعملها drop

----------------------
### طب لو ال request جاي من بره 

دلوقتي لما الـ request تيجي من الإنترنت  
أول حاجة بتوصل لـ **الـ Internet Gateway** المرتبط بالـ VPC

الـ Internet Gateway بيبص على الـ Public IP اللي الباكت رايحة له

لو الـ Public IP ده مرتبط بـ instance جوه الـ VPC  
→ الـ IGW يعمل Reverse NAT  
يعني يحول الـ Public IP للـ Private IP الداخلي للـ instance

بعد كده الباكت تدخل جوه الـ VPC  
وتوصل للـ Virtual Router

الـ Router هنا يبدأ يطبق الـ Route Table  
بس المرة دي بيبص على الـ Destination IP (اللي هو الـ Private IP بعد الـ NAT)

يعمل Longest Prefix Match

غالبًا هيلاقي إن الـ IP ده داخل نفس الـ VPC CIDR  
→ فيلاقي Route اسمه "local"  
→ يعمل forward للـ Subnet اللي فيها الـ instance

وبعد كده توصل فعليًا للـ instance نفسها  
(طبعًا بشرط إن الـ Security Group والـ NACL يسمحوا بالـ traffic)

---

لو بقى الـ Public IP مش مربوط بأي instance  
أو مفيش mapping أصلاً  
→ الـ Internet Gateway هيعمل drop للباكت  
ومش هتدخل الـ VPC أصلاً

---

فالتسلسل الكامل بيبقى كده:

Internet  
↓  
Internet Gateway  
↓  
Reverse NAT (لو فيه Public IP مربوط)  
↓  
VPC Virtual Router  
↓  
Route Table (local route)  
↓  
Subnet  
↓  
Instance