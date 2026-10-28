
![[Pasted image 20260220031044.png]]

#### IAM Elements
- ال principle هو عامتا الكيان اللي هيتعامل مع ال AWS , و من امثلته هي ال IAM user , IAM role و اللي يعتبر زي مستخدم بصلاحيات مؤقته , Application , Federated user اللي بيجي من Trusted Sites زي ال Facebook , Google , Apple 
- ال Authentication هنا بتنقسم الي Console Access , Programmatic Access
- ال Request و اللي مينفعش تتعمل الا اذا كان ال user عامل Authentication و بيحصل استثناء في حالة ان ال Anonymous User , و عاوز يعمل Request علي S3 Bucket عامل ال Public Access عليها 
- ال Authorization , و اللي بتنقسم برضو لحاجتين [ IAM Identity Based Policies - Resource Based Policies ] و اللي بيشكلها و يتحكم فيها حاجتين ----> [ Policy - Permissions ] ,  و دول IAM نفسها بتعمل Check عليهم عشان هشوفو ممكن يقبلو ال Request و لا مكانش ينفع انك تعمله من الاول اصلا 
- ال Actions و دي بتظهر كتير في حاجات زي ال Policies علي ال Users اللي في ال Account
- ال Resources 

كل الكلام اللي فات دا كنا بتكلم في نفس ال Account 
طب هل ينفع User من Account B يدخل علي user من Account A

و اللي في بيحصل في Scenarios اللي بيكون في Account مثلا مخصص لكل قسم في الشركة دي , و حد من اكونت ال Administration عاوز يصل حاجة في Account بتاع قسم تاني 
ساعتها بخلق حاجة اسمها Cross Account و بدورها بدي Permissions للاكونت دا علي Resources عندي و حتي ال permission دي ل Users معينين او مش كل ال Users اللي في الاكونت 

--------
#### IAM Policies

![[Pasted image 20260220070105.png]]

في البداية لما نيجي نتكلم عن ال Policies 
ال User او ال Principle عامتا لو مفيش ليه security policy سواء بشكل مباشر او غير مباشر مطبقة عليه يبقي كل حاجة بالنسبالة Denied by default 
انواع ال security policies في AWS :-
- ممكن تبقي AWS Managed اللي بتبقي Security Policies جاهزة ممكن اطبقها علي ال User , Groups 
- ال Customer Managed دي بتبقي من تحكم انا من خلال تجميع شوية Policies و اطبقهم برضو علي ال Group , Users 
- ال Inline Policies و دي بتطبق علي مستوي ال User الواحد بس و مش زي ال Policies اللي قبلها مش بتبقي Shared مع Users تانين 

-------


![[Pasted image 20260220032557.png]]

الكلام اللي فات دا كله كنا بتكلهم علي IAM Identity Based Policies 

طب اعمل ايه عشان اطبق برضو تحكم في الوصول لل Resources نفسها 
, ساعتها بعمل حاجة اسمها ال Resource Based Policies , و ساعتها بتضاف تحكم جديد من ناحية ال Resource نفسها 
و حاجة زي كده بتظهر في مثلا امكانية الوصول لل resources بعد ساعات العمل الرسمية و هكذا

- اول حاجة ال requests علي ال resources المختلفة Denied by default 
- و برضو لو مفيش حاجة متعرفه انها Allow يبقي هي By Default Denied
- بالنسبة لل Explicit deny اللي يتماتش في ال policy بتلغي كل ال Allow اللي قبلها 
- ال Polices عامتا بتتخزن علي ال JSON Format 

ال Security Policies مش شرط تبقي كبيرة او في مكان واحد بس 
ممكن اخليها مقسمه و عملها Attach for group , users 

--------


شكل ال security policies اللي ممكن تبقي Attached لل user الواحد 

![[Pasted image 20260220074542.png]]


شكل ال Policy 

![[Pasted image 20260220074957.png]]

![[Pasted image 20260220074626.png]]

و الصلاحيات اللي في الصورة دي ممكن تبقي خطيرة جدا فا بيبقي ترشيح انك تعمل يوزر زي دا MFA لضمان ان الحساب دا لا يخترق


شكل AWS managed policies 

![[Pasted image 20260220074909.png]]

شكل ال inline policy 

![[Pasted image 20260220075150.png]]

-------
