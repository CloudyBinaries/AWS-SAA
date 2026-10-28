
![[Pasted image 20260221042226.png]]

في نوعين من اشكال استخدامات ال Database و هما 
[ Transactional - Analytical ]
ال Transactional يبقي لالاستخدامات الاساسية او العادية لل Database  في العمليات اللي ليها علاقة بال  CRUD 
, زي ما انا عاوز مثلا اشوف المنتجات اللي بتتباع علي ال Amazon , او عاوز اضيف تفاصيل عني زياده في ال Profile و هكذا 
و للعلم الفكرة في ال Database اللي زي كده هي بتشتغل علي data معينه و tables معينه , و برضو بتدور علي Rows معينة لغاية ماتلاقي match 

ال Analytical يبقي الاستخدامات التحليلية لل Data اللي في ال Database 
, و بيظهر في الحاجات اللي شبه اني اعرف اعلي منتج في المبيعات و مبيعات الشهور اللي فاتت و عاوز اعمل Reports ,BI , و هكذا  
الفكرة ان ال Analytical بيركز علي ال data كلها و يدخل عليها يعرف تفاصيلها

----------

### AWS RDS 

![[Pasted image 20260221070628.png]]

 ال RDS في حد ذاتها مش Database واحده 
 دي عبارده عن Services موفره انواع كتير من Database من مختلف ال vendors , حوالي 6
 و منهم Amazon Aurora و دي يعتبر افضلهم 
 و دول كلهم OLTP 

ال RDS  عبارة عن AWS Fully Managed Service 
انت بس بتحددلهم مواصفات ال Instance اللي بيتحط عليها ال database 
و هما اللي بيعملوها و ينلو كل حاجة خاصة بال database 
و هما  اللي بيعملولما Batch و حتي هما مسؤولين عن ال OS updates
و انت كل اللي عليك انت تبدا تشتغل علي ال Database , و تنفذ تبقي عليها العمليات و تخزن ال data و هكذا 
لكن انت ملكش Root Access علي ال Instance h, او حتي ال OS نفسه
, و للعلم طلا انت سمعت كلمه Fully Managed من AWS يبقي by default انت ملكش Root Access علي ال Instance اللي فيها ال Services دي 

خدمة ال RDS بتشتغل في instance جوة ال VPC بتاعتك مش زي ال S3 مثلا في [ في نفس ال Region و لكن مش نفس ال VPC ]

من ال Best Practice بتاع AWS في ال RDS هي انك تخلي ال Instance في ال private subnet مش Public 

في حالة انت عاوز تاخد Root Access علي ال machine اللي فيها ال RDS يبقي في الحالة دي انت اللي هتعمل Create لل Machine اللي فيها ال Database بالمواصفات و ال size اللي انت عاوزه , و انت هتاخد ال database engine من AWS و انت اللي هتبقي مسؤول عن كل حاجة و ساعتها هتبقي اسمها [Self-Managed] و الفكرة هنا ان لو حصل مشكلة في اي حاجة AWS مش هتبقي مسؤولة عن الاعطال و انت بس اللي هتصلح 

![[Pasted image 20260222223337.png]]

ال RDS instance هتعرف عليها security group , و دي بتبقي انت مسؤول عن تأمينها و انت اللي بتعرف مين اللي يعدي و مين لا 
و ممكن تقول مثلا ان الوحيد اللي مسموح ليه انت يدخل علي ال instance  هو ال Security Group بتاعت ال application نفسة و علي ال port الفلاني , و ال port بيفرق علي حسب ال Database Vendor اللي انت مشغله

في 2 moods  في ال deploy بتاع ال RDS instances و هي اما انك تخلق RDS instance واحد عليه كل ال database بتاعتك و دا هيبقي اسمه [standalone mode]
 و في حالة تانية و هي انك تعمل [Multi-AZ mode] و في الحالة دي بيبقي في Primary Instance و واحده تانية في AZ تانية اسمها Standby instance و دي بيبقي في بينهم Syncing و انت في العادي مش هتبقي عاوز توصل لل standby instance و لا تنفذ عليهم حاجة و تخزن حاجة 
  , و لكن لو ال Primary وقعت ال standby  في الحالة دي بتقلب ل Primary ,و اللي بيعمل الترقية او القلبه دي هي AWS برضو 
  و الطريقة اللي ممكن تكون بتحصل هي ان ال URL بتاع ال Primary Instance بتحوله لل IP بتاعت ال Standby instance من خلال Route53

, و طالما احنا قلنا انها في Private Subnet يبقي هي لا محتاجة Public / Elastic IP Address 

-----

-  في مرحلة ال create بتاعت ال database انت بتختار في ال Templates , Dev/Test عشان فيها free tier instances لان ال Free tier بتخليك تختار ال T2.micro  و مش بتعرف تعمل ال Multi-AZ 
  , و لو روحت لل Production بيبقي الموضوع دا بيخليك تختار paid instances بس 

- بتحدد انهي VPC هيبقي فيها ال instance بتاعتك 

- نظام الحساب في ال RDS علي ال [ DB instance - Storage - Multi-AZ Standby storage - Provisioned IPOS  او سرعة الوصول لل storage  ]

- عشان التعريف بتاع ال Database في ال application اللي بيبقي في ال connection with backend , هتلاقي ان AWS عامله سايبالك حاجاة اسمها Endpoint - port اللي هو ال URL بتاع ال Database و الport اللي بتوصل عليه ----- الي جانب انه بيبقي في ال username/password اللي بيبقي في ال creation و انت بتحددهم عشان يكمل ال Connection بنجاح 
  و ال URL مش ال IP عشان زي ما قولنا فوق في مشكلة اللي ممكن تحصل ال Primary instance و امكانية الوصول ل Standby instance من ال Route53

![[Pasted image 20260222232359.png]]


في ال Security في Certificate Authority و هي RDS بتنزلها عشان تعرف تتكلم من ال Application , و بيبقي ليها مده صلاحية زي ما هو باين 

![[Pasted image 20260222233146.png]]

زي ما هو باين في جزء ال KMS Keys , هو محدد انه AWS/RDS و احنا مكناش محددين حاجة 
و من هنا نستنتج ان ال RDS is Encrypted by default 


- ممكن اعلي الامكانيات بتاعت ال Instance من Modify , 
  , ممكن اعمل Snapshot لل database 

![[Pasted image 20260222234006.png]]

![[Pasted image 20260222234247.png]]


و انت بتمسح ال Database بيخيرك هل تمسح ال Backups ,و لا لأ 
و في بعد الاحيان انت بتمسحهم عشان ال Storage دي عليها فلوس  

----

##### اسئلة :- 

1- لو انا عندي On-Prem Application ازاي اربطه مع RDS instance ؟ 
هو في الحالة دي لو انا واصل مع ال private Subnet ب IPsec over VPN عن طريق ال VGW , اما انك تخلي ال RDS instance Public accessible

2 - ازاي نقدر نعمل migration بين ال on-prem database , و ال RDS ؟ 
الفكرة هنا ان هتستخدم Service من AWS اسمها database migration service و ليها , و هتعمل ال Migration بس في هنا حالتين في ال database اللي هتتنقل 
اما هتكون نفس نوع ال database و اللي هي مثلا من MySQL ل MySQL و اللي ساعتها الموضوع بيبقي عادي , و الحالة التانية ان يكون الاتنين مش نفس النوع اني مثلا بنقلمن MySQL ل MongoDB "NoSQL" و اللي في الحالة دي بستخدم حاجة اسمها Schema Converter 

- في حالات معينة بستخدم ال standby database ك read-only و في الحالة دي اسمها Replica 

- و ال RDS بتدعم ال auto-scaling لل size بس ال Resources هي نفسها 

- ال CDN او ال Cloud front مش بيشتغل علي انه يوصل لل RDS لانه مش من ال Origins اللي موجوده في ال Cloud Front, لكن في احد الحلول للتسريع هو ان في حاجة اسمها Global RDS و اللي بتخلي في Standby Database instances و اللي بتكون ساعتها عشان تقرا منها من اي مكان فيها "Replication" 

- بالنسبة لل Backup بتاع ال database و مش بتكلم هنا هن ال Standby database ,  بيتخزن في S3 مش بتاعتك /  و برضو الفكرة دي ممكن انت تعملها لو عملت Snapshot و حطيتها في S3 Bucket بتاعتك "تعملها export هناك"

-------
## عاوز أكد علي مصطلح ال Replication 

