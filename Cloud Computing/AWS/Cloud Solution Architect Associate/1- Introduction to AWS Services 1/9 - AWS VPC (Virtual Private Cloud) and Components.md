
![[Pasted image 20260214165644.png]]

لو هنتكلم عن ال VPC هنقول انها ال virtual data center ليك علي ال Cloud و فيها ال data بتعاتك متأمنه و ليها حدود و متحدده بالحدود دي اللي هي بتبقي ال Region و ساعات بتبقي كمان ال AZ
يعني ال VPC اللي حاجزها في Region (north virginiana) , و روحت علي ال Paris مش هتعرف تشوف ال VPC بتاعتك حتي لو من نفس الاكونت 
, و بناء علي كدا VPC مش زي ال IAM مش Global service 
يعني ال VPC تقدر تتمدد جوه ال AZ لغاية ما تحبط في الحيط بتاع ال Region

----------

# VPC components 

![[Pasted image 20260214171135.png]]


### CIDR blocks and subnets

CIDR = Classless Inter-Domain Routing

![[Pasted image 20260214221356.png]]

هي من اوائل الحاجات تقريبا اللي بتشوفها و انت بتحرجز ال VPC و هي بتبقي عبارة عن ال IP Domain اللي بيحدد الشبكة الخاصة بيك في AWS ,  و بيبقي شكلها زي كده
 [ 192.168.1.0/24]
 
, و دا بيحدد حجم الشبكة و عدد ال IP المتاحة و تقسيمة ال Subnets هيبقي شكلها عامل ازاي و الحجم في Amazon بيبقي [ بين /16 , /28 ] مينفعش حجم الشبكة يخرج عن الاتنين دول 
,و الفكرة ان اول ما بتحجز VPC علي AWS بيتحجزلك بشكل تلقائي CIDR default 
, متقدر تمسحة هو اللي تعمل تعمله مجرد بس انك تزود فيه بمعني انك تعمله [ expand ]

, و بالنسبة لموضوع ال Subnets مش زي ما قولنا في ال VPC , ال Subnet محدوده بال AZ , 
و فكرة انها يتعملها تمديد ,"استخدام نفس ال Subnet في Data centers تانية" بيعمل مشاكل زي موضوع ال Overlap و مشاكل ال Broadcasting 

Note ---> Overlap : مشكلة ان يبقي في 2 subnets بينهم جزء مشترك جوه نفس ال  AZ

و بالنسبة لموضوع التقسيمة بتاعت ال VPC , subnets متقسمة زي ما موجوده في الصورة بالظبط

--------------
## Implied Router 

![[Pasted image 20260214224001.png]]

دلوقتي ال subnets دول كل واحد فيهم في ال AZ شكل 
و عاوزينهم يتكلمو مع بعض 
في العادي طبعا مش هيعرفو و لا هيشوفو بعض اصلا 
و هنا بيجي دول ال Implied router اللي AWS بتخلقه بشكل تلقائي عشان ال Subnets  يقدرو يتواصلو مع بعض 

طب سؤال ----> هل انا ليا بقي Access علي ال router و اقدر اعمله Configuration ,  و كدا؟ 
لأ

طب بيحدد منين ال Subnets اللي عاوز اتواصل معاها 
من خلال Route table انا اللي بحدده 

ال Subnet الواحده ممكن اعمله Attach ل Route table واحد فقط 

في مشكلة احنا ممكن نستنتجها و هي اننا لو جينا نكلم VPC ليها في  Account تاني  او مع Data center تانية (hypered) ليها نفس ال CIDR block مش هينفع يحصل اتصال او حتي في Overlapping 

الملحوظة الاخيرة في ال slide دي مهمة 

![[Pasted image 20260214230513.png]]

و بشكل اساسي جدا في Route بتبقي موجوده و هي اللي في الصورة اللي فوق دي , مقدرش اعدل فيه و لا امسحه و الفكرة انه اصلا بيتولد و انا بعمل ال VPC 

-------

## Internet Gateway & (VGW)


![[Pasted image 20260215001946.png]]

الهدف منه هو اني اقدر اطلع ال VPC و subnets عامتا علي النت , مسؤول عن حكاية ال Public IP address عشان ال instances تبقي Reachable علي النت 
ال Default VPC بيبقي ليه internet Gateway جاهز مش محتاج تخلقه و بتحتاج ساعة لما تعمل VPC تانية او مش Default 
و معلومة : ان كل VPC بتخلقها لازم يتعملها Attach ل internet gateway خاص بيها 

نفس قصة ال router بالظبط مينفعش اعمل login عليه و لا configuration
 و برضو مينفعش ال VPC الواحد يتعمله Attach مع اكتر من Internet Gateway 

بيوفر Support ل IPv4 , IPv6 , و شغال في الاتجاهين لو السيرفر عاوز حاجة من برة و لو اللي برة عاوز يتواصل مع السيرفر

##### كل دا كنا بنتكلم علي ال Internet Gateway العادي اللي يوصل علي ال public internet  , دلوقتي هنتكلم عن ال Virtual Private Gateway

![[Pasted image 20260215000034.png]]

طب انا عاوز اوصل لل On-Prem data center بتاعي 
في الحالة دي مينفعش اوصله من خلال internet gateway العادي 
لازم يبقي [ Virtual Private Gateway ] , ساعتها بيقدر يوصلي بال data center بتاعي عن طريق VPN Connection  بس مستخدمة IPsec ----> IP secure 
بحيث انها لو عدت في النت و حد عمل sniff لل data يلاقيها مشفرة 

و طريقة تانية غير ال VPN عشان ال internet مش stable يعني ساعات بيبقي الاتصال سريع و ساعات بطئ و في حالة زي كده بتروح لل internet service provider بتقوله انا عاوز اوصل من النقطة أ الي النقطة ب بسرعة فلانية و لو AWS موجوده في المكان القريب منك بتتولي هي الموضوع دا و تعمل حاجة اسمها ال direct connection , السرعة بتوصل ل 40 GB/s
فال latency قليل و Bandwidth عالي 