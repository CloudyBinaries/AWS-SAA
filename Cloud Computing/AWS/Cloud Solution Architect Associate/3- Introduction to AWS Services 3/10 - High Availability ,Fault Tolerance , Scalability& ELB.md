
### High Availability vs. Fault Tolerance


![[Pasted image 20260223012541.png]]
![[Pasted image 20260223011829.png]]


![[Pasted image 20260223012610.png]]
![[Pasted image 20260223012629.png]]


اول حاجة ايه ال Load Balancer هنا ؟
هي عبارة عن موزع للحمل علي ال Instances اللي محطوط عليهم ال Application بطريقة التوزيع المناسبة (بالتساوي - علي حسب قدرات كل Instance)

في اتنين Concepts بنحتاج نفاضل ما بينهم لما بنيجي نبني ال Architecture , و هما ال High-Availability , و بين ال Fault Tolerance 

  الفكرة هنا ان ال High Availability انه يبقي ال Resources مش موجوده كلها في مكان واحد و دا اللي بتخليها موجوده في اكتر من مكان , و ال Fault tolerance انه يعرف يعمل handle لاي تعطيل او مشكلة في ال Architecture حتي لو كانت Highly Available 

- اول Design هنا High Available بس مش Fault Tolerant 
  و الفكرة انه الحمل متوزع ما بينهم 60 , 60 من قدرات ال Compute , يعني ال Application متاح في اتنين Instance لو حصل في واحده فيهم مشكلة بشكل نظري  
  لكن بشكل عملي التحميل دا اللي هو 60 , 60 هيخلي لو واحده وقعت فعلا التاني هيشيل بقدرات تقريبا 120% من قدرات ال Computing و دا مستحيل و اللي هيسبب اما delay - timeout او ان ال instance تقع بقي خالص 

- ال Design التاني هو حل مقترح لل Design الاول و هو حل مشكلة ال Fault Tolerance 
  بانه زود instance جديدة و وزع الحمل ما بينهم تاني و وصل ل  40 , 40 , 40 من قدرات ال Computing و الفكرة هنا ان لما instance وقعت هتلاقي في اتنين بيشيلو الحمل و ال load لوصل  60 , 60 من ال Computing 

, و الاستنتاج من ال Designs دول ان لو عاوز Fault Tolerance هتحتاج Extra cost 

-------------

### Scalability 

![[Pasted image 20260223021710.png]]

انا دلوقتي في سناريو من ال سيناروهات المشهورة اللي هي 
عامل تطبيق e-commerce و التطبيق دا شغال في الطبيعي علي 2 instances 
, و فجأة في مناسبة زي ال Black Friday ال load علي الموقع بتاعي زاد و الاتنين instances دلوقتي مش كفاية 
انا محتاج ان ال Resources تزيد و توصل ل 20 instances او اعلي ال resources بتاعت ال 2 instances نفسهم 
بس لما ال black Friday بيخلص بكتشف اني مش هحتاح ال Resources اللي زودتها دي تاني 

هنا AWS خلت الموضوع شكله Automated و قالتلك اني ممكن اعمل Scale up في الوقت اللي انت عاوزه و و توصل للمواصفات اللي انت عاوزها , الموضوع يرجع للشكل الطبيعي او اقل كمان لو تحب 

ال Scalability عندي متقسم لنوعين و جوه النوعين في نوعين تانين 

[Horizontal scaling] -----> [ In / Out ] زياده عدد الموارد او بزود مثلا عدد السيرفرات اللي شايلة ال Application و تكون ال servers من نفس الفئة او من فئة تانية

[Vertical scaling] -----> [ Up / Down ] زياده قدرات الموارد اللي موجوده بالفعل 

-------------

### Elasticity 

![[Pasted image 20260223023353.png]]

ال Elasticity عبارده عن مرونة ال Instance , Application انه يستحمل ال Load بزياده ال Resources او تقليلها حسب الحاجة بشكل Automatic من خلال ال Horizontal scaling 
و الفكرة هنا اني بستخدم service اسمها Auto-scaling 
و بديله Auto-scaling Group و من خلال اني بوريله Template فيها كل المواصفات بتاعت ال Instance اللي هبقي عاوز ازود ليها 
عاوز اشتغل علي ال OS الفلاني 
علي AMI الفلانية 
علي Security group معين 
و بعدها اقرله ان هيبقي ليها مثلا  Public IP Address
في الحتة الفالنية في ال AZ الفلاني 

و اديلة ال Cases اللي هيوزد عندها "مثلا ال CPU وصل ل 70% من قدراته" ساعتها زود
و اللي هينفص برصو عندها "CPU -----> 55%" , كل مدي بيقل عند level معين بينقص عدد معين من ال instances 


Auto-Scaling فيه منه نوعين ----> [ EC2 Auto scaling - Application Auto scaling ]

, و اللي كنت بتكلم عنه دا اسمه EC2 auto scaling 

--------

### Load Balancer 

![[Pasted image 20260223024557.png]]

![[Pasted image 20260223024613.png]]

ال load Balancer دا عبارة عن توزيع لل Load علي instances 
و بيعرف ان ال instance موجوده و لا وقفت من خلال health check 
اللي هو لو ال instance بعتلها زي ping و قعدت 10 ثواني و لسه ما ردتش يبقي ال instance دي Down و مش هبعتلها لغايه ما تبقي UP و يبدا يوزع ال load من تاني علي ال instances الباقيين

ال Load Balancer محدود بال Region الواحده و يعرف يوزع علي AZ مختلفة 
في Exception في الموضوع دا لو انا بتعامل مع ال instance بال IP Address  