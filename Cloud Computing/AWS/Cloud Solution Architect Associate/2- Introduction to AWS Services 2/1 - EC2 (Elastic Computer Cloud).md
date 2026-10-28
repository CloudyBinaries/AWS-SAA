
--------
# outlines 

![[Pasted image 20260215065449.png]]

----

# EC2 Fundamentals


### before we kick start learning about EC2 , we have to know first what is [ visualization ] 

![[Pasted image 20260215070008.png]]

نفترض اني واحد عاوز يحجز 10 سيرفرات علي AWS 
هيبقي صعب جدا لو هو ادوني 10 سيرفرات physical و استخدمهم "الا لو استدعي"
فا هنا AWS فكرت في حاجة ان ليه ميبقاش اليوزر واخد سيرفر واحد و السيرفر متقسم داخليا ل 10 سيرفر
و هنا السيرفر هيتقسم بشكل وهمي انه 10 سيرفرات , بأننا نخلي كل سيرفر وهمي يحجز جزء من قدرات المعالجة بتاعت السيرفر الحقيقي 
و اللي بيدعم الفكرة دي هي القدرات العالية لموارد السيرفر ---> "ال RAM بيوصل احيانا في بعض السيرفرات ل 1TB  "
يعني لو كل سيرفر خد حوالي 2-4 GB RAM هيكفي و زيادة 
و طبعا بيحصل تقسيم في كل الموارد اللي علي السيرفر - عملية التقسيم الافتراضي دا اسمه "visualization"

كل سيرفر من السيرفرات الوهمية دي "VM" بيبقي نازل عليه OS  كامل زي ال windows , Linux 
او ايا كان , و دي "ميزة بيتم فقدها في ال Containers بعد كده"
و دي الفكرةاللي بتميز الVirtualization بقي عامتا 
اني اقدر يبقي عندي جهاز واحده بس هو يقدر يشغل 10 اجهزة وهمية كل واحد فيهم شغال بشكل مستقل و مش شايف اللي جنبه و كل واحد شايل حاجة مختلفة تماما عن التاني من OS, services 

VM اللي خارجة من ناتج التقسيمة دي بيبقي اسمها 
EC2 instance 

----

## EC2 (Elastic Compute Cloud)

![[Pasted image 20260215183744.png]]

ال EC2 عبارة عن VM بتتحكم في القدرات بتاعته من مساحة و قدرات معالجة و دا بيرجع لفكرة اني محتاجها ليه ---- هل انها ل host ل Website عادي او اني محتاجها عشان انفذ عليه ML models , و قدرات معالجة كبيرة ولا محتاجها عشان المساحة التخزينية و ال Database

ال EC2 بتقدر تتحكم في انها اما تشتغل او تتوقف او يتم مسحها بشكل كامل 

ال EC2 ممكن تبقي علي physical server مع VMs تانية (shared host) , او انها تبقي لوحدها علي , و غالب بتبقي للحاجات بتاعت البنوك عشان السياسات بتاعتهphysical server (dedicated host)

ال limit محدد لكل Account بشكل default انك تقدر تعمل حوالي 20 EC2 instances , و ممكن تغيرها

---------

## EC2 storage 

#### في نوعين من ال Storage لل EC2 instances , و هما EBS Volume , instance store


##### EBS Volume

![[Pasted image 20260215220258.png]]

دي باختصار عبارة عن ال hard disk علي ال Cloud في نفس ال AZ بتاعت ال instance و مش معاك في نفس ال Physical server و انت بتوصلها من خلال ال Elastic Network Interface ENI
ال OS بتاعت ال instance عليها By default

مزايايها انها :- 
- ال Persistent (البيانات بتفضل موجودة حتى لو الـ instance اتقفلت)
- ممكن تعمله Detach وتوصله بـ instance تانية
- تقدر تعمل Snapshot ليه
- تقدر تكبر حجمه
- بيتخزن في نفس الـ Availability Zone

و بنستخدمها لو عاوزين نخزن Database , Application , Root disk or Boot volume


##### Instance Store

![[Pasted image 20260215220902.png]]

الميزة اللي فيه انه علي نفس ال Physical server و الوصول ليها سريع جدا 
, ممكن انزل عليها ال OS بس الفكرة انها Volatile memory يعني لو فكرت انك تعمل stop لل instance بتاعتك و تشغلها تاني هتلاقي ال data راحت 

و اغلب استخدامتها في حاجات اللي عاوزه سرعة في المعالجة زي ال 
Cache - Temporary files - Buffers - Data processing

-------

### SSH (Secure Shell)


![[Pasted image 20260215225827.png]]
![[Pasted image 20260215225912.png]]

Shell بيوصل لل  instance عن طريق ال Public IP Address


---------

![[Pasted image 20260215230246.png]]
AMI ---> Amazon machine image "VM" جاهزة


