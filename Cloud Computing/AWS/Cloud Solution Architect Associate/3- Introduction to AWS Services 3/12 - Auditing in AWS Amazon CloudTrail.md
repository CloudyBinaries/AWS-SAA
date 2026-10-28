
![[Pasted image 20260221195605.png]]

اي service بيتم استخدامها او مثلا S3 بتتخلق او بعمل EC2 instance endpoint دي عبارة عن API calls في ال Background ل AWS عشان ينفذ ال Function اللي مسؤولة عن ال Service دي 

ال service اللي زي CloudTrail دي بتاخد بقي Log من كل API call اللي حصل ل AWS Services في حاجة اسمها Event history , و يخليهم موجودين 90 يوم 

و logs زي كده شايلة جواها كميه معلومات رهيبة زي انه 
مين اللي عمل ال call دا و لاني Services و  الساعة كام و من IP كام و هكذا 

بس لو في Application عندي علي ال EC2 هل و الناس بتعمل عليه Requests هل ال CloudTrail بيشيل ال Calls بتاعته برضو 
الاجابة لأ 
هو بس بيشيل ال API calls علي مستوي ال AWS Services 

و طبعا بنستفيد من الخدمة دي في ال Auditing , Gradience 

"في خدمة قدام اسمها Gard Duty 
الخدمة دي بتاخد ال logs بتاعت CloudTrail , ال Logs بتاعت CloudWatch , مع بتاعت ال DNS , مع ال Application logs 
, و بعد دا كله بتعمل عليهم Processing ب Machine Learning Model و الي جانب Database هي عارفها بتتعلم منها ال Malicious
Attacks و تترجم دا الي معرفة اذاكان في محاولة هجوم و لا لأ
"

--------
### CloudTrail Trail

![[Pasted image 20260223060507.png]]

في بعض الاحيان حكاية ال 90 يوم لل Logs مينفعوش مستخدمين معينين 
بالذات الشركات الشركان ممكن تبقي محتاج ال Logs لفترة اكبر بكتير و تعملها Auditing 
فا AWS وفرت حاجة اسمها CloudTrail Trail و دي حاجة مفروض اعملها Enable , و احددلها ال scope 
و ساعتها ال Trails دي بتتخزن في S3 Bucket تابعه ليا , اقدر اخلي ال Data اللي في ال Bucket لمده 3 سنين مثلا , و اقدر اعمل عليها Query ب SQL و بعد كده تمسحهم  

ال scope بتاعت ال trail ممكن تبقي Single Region , و ممكن نبقي multi-region 
لو انا عاوز Single-Region لازم اعملها من ال CLI , اما ال All Regions دي بتبقي by default و من ال Console عادي جدا 
بس الفكرة ان ال all regions دي اسهل و اريح لان لو عملت Single Region دي هضطر اعمل كده في كل region كل مرة و بعد كده اعمل Central S3 Bucket و اجمع فيها كل ال Trails اللي من كل Region 

##### في مزايا مهمة في CloudTrail 

- ممكن اخلي logs معينه لو حصلت يبعتلي علي SNS 
- ممكن اخليها تبعت ال logs اللي اتكونت علي CloudWatch Logs Group
- ال Logs اللي بتتبعت من CloudTrail ل S3 
  encrypted by default at rest


---------------
### Logs Type 

![[Pasted image 20260221200431.png]]

##### management events

الحاجات اللي من فوق خالص اللي زي - ببلاش
- EC2 instance creation
- Editing Security policy
- deleting S3 bucket
- modifying IAM Roles

##### data events

الحاجات اللي بتحصل علي ال Resource نفسه - مدفوعة

- Downloading or uploading object in S3 Bucket 
- Deleting Object 

##### insight events

دي اذكي لانها بتشوف النشاطات الغير معتاده في ال API calls  - مدفوعة 

- 100 unusual API calls
- deleting lots of object in S3
- many user creations

![[Pasted image 20260221200609.png]]

لو في واحد مؤذي حاول انه يخرب و يعدل بشكل مؤدي في Data معينة محوظع علي حاجة مثلا زي S3 Bucket و علي اساس ان محدش هيعرف راح ماسك ان ال objects حصل عليه deleting, Modifying من ال API calls اللي علي CloudTrail 

ال CloudTrail برضو بتضمنلك ان ال integrity برضو حصلل فيها حاجة ولا لا لانه بيحس الهاش بتاع ال file الاصلي و يقارن بين ال hash بتاع اخر تعديل "دا في حالة انه حصل" , و تقول لو حصل تعديل و لا لا 

 و كمان بيجيب مين اللي عمل الحركة دي 

-----

### CloudTrail vs. CloudWatch Logs 

![[Pasted image 20260223062921.png]]

# مثال يوضحلك الصورة

لو حد اخترق حسابك:

## السيناريو 1:

هاكر شغّل EC2 جديدة

→ ده يتسجل في CloudTrail  
لأنه API Call

---

## السيناريو 2:

الـ EC2 شغالة وبتطلع Error Logs

→ ده يروح CloudWatch Logs  
لأنه System/Application Log

---

# طب ليه الاتنين موجودين؟

لأنهم بيحلوا مشكلتين مختلفتين:

- CloudTrail = "مين لمس الحساب؟"
    
- CloudWatch = "السيستم بيعمل إيه؟"
    

---

# تخيلها كده 👇

CloudTrail = كاميرات مراقبة إدارية  
CloudWatch = أجهزة قياس أداء داخل المصنع

---

# نقطة مهمة جدًا ليك كـ Security

بما إنك مهتم بالـ Pentesting و AD:

CloudTrail مهم جدًا في:

- Incident Response
    
- Detect Privilege Escalation
    
- Track Suspicious API usage
    
- Detect Persistence via IAM
    

CloudWatch Logs مهم في:

- Log analysis
    
- Detect command execution
    
- Analyze app vulnerabilities
---------
