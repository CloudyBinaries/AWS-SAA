
![[Pasted image 20260220075448.png]]

في سينايوهات بيكون في ال Application علي ال EC2 instance 
بيبقي محتاج انه يوصل ل Resource  و يبدأ يشتغل عليه 
الفكرة هنا اني مينفعش ادي ال EC2 , Application اي Programmatic Access علي ال Resource اللي هو في الحالة دي S3 , لان في الحالة دي انا بديله Long Term Credentials i هو مش محتاجها اصلا و اللي بدورها ممكن تبقي خطر لانه هياخد صلحيات من خلالها ممكن يخرب في ال Resources المختلفة 
فا في الحالة دي بديله ال IAM Roles بصلاحيان مؤقته يقدر يشتغل بيها ل Session مدتها اقدر اتحكم فيها 

ينفع ال EC2 تاخد ال IAM Role دي سواء كانت لسه معمولها Creation h, بعد ما تكون اشتغلت بفترة زي في الصورة اللي تحت

![[Pasted image 20260220190327.png]]


و الEC2 instance غير ال User , ال User ممكن اعمله Attach لاكتر من Security policies , انما ال EC2 instance بتاخد IAM Roles واحده بس و اقدر اعملها هي كمان Attach لاكتر من Security Policy


في نقطة اتقالت عن ال IAM عامتا بس ممكن نقولها تاني و هي انها Global Service يعني لو اديت ل User في Region ممكن استخدمه في Region تانية لو انا محتاجة 

------


![[Pasted image 20260220200121.png]]

زي ما في الصورة حاولت اعمل اي تصرف علي Resource زي ال S3 , رفض و عاوز Credentials ادخل بيها 
, و طبعا مينفعش اعمل AWS Config زي ما هو بيقول هنا 
لانها هتخليني اعمل Access keys مش مفروض ال Instance تاخدها 

فا في الخطوات الجاية هخلق IAM role 

![[Pasted image 20260220200430.png]]

![[Pasted image 20260220200534.png]]

![[Pasted image 20260220200605.png]]

![[Pasted image 20260220200709.png]]
![[Pasted image 20260220200746.png]]
![[Pasted image 20260220200809.png]]

اللي فات ده بيقول ان ال Role اتخلتق و بقت موجوده بس لسه مش معمول ليها Attach في ال Instance 

![[Pasted image 20260220200932.png]]

فا برجه تاني و اعمل Attach 

![[Pasted image 20260220201039.png]]

لو نلاحظ في ال مكان بتاع ال IAM Role في دلوقتي ال IAM Role اللي لسه عاملينها 

![[Pasted image 20260220201132.png]]

دلوقتي ال IAM Role سمعت في ال Shell 

معلومة اضافية : aws  s3 ls 
هنا بيعمل list لل Buckets اللي عندي مش بي List محتويات Bucket معينة 

---------

### الجزء دا كله بأختصار ناقش فكرة ازاي اعمل Create ل STS لل EC2 instance عشان يبقي ليها IAM Role علي S3 

