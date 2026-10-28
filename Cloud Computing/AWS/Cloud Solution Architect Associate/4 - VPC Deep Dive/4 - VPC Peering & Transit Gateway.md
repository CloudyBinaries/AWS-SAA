
### VPC Peering

![[Pasted image 20260227100922.png]]

من المعروف ان مينفعش ان اتنين VPC يتواصلو مع بعض عادي 
طب انا و انا عندي شركة و محتاج اعمل اتنين VPC و اعمل ما بينهم اتصال , ايه الحل ؟

الحل "قديم" اني اعمل VPC Peering , و دا الفكرة منه اني اعمل زي Routing ما بين ال VPC بيحيث ان الشبكتين يبقو شايفين بعض 
بس شرط ان ميبقاش في اي Overlapping ما بين الاتنين VPC "تعارض بين ال CIDRs" 

الاتنين VPCs ممكن يبقوا :- 
- في نفس الاكونت
- في اتنين اكونتس في نفس ال Region
- في Regions مختلفة في نفس الاكونت
- اتنين اكونتس في اتنين Regions 

ال Peering Connection مش بيحصل فجاة اللي هو الاقي في VPC بيعرف بيدخل علي ال VPC بتاعتي و يتواصل معايا 
لا دا لما ببيقي في VPC بيبعت Peering Connection Request بيفضل bending لغايه ال VPC تقبل ال Connect او يرفض 

بعد كده محتاج اظبط ال Routing Table بتات ال Router اللي في كل واحد عشان يقبل ال Connection , implied router , NACLs , Security Groups 
اللي هو زي الشبكتين اللي في الصورة لو عاوزين يتواصلو مع بعض يظبط في ال Implied router انه لو عاوز يروح علي 10.0.0.0/0 يروح علي ال Peering connection "router "

----------------

#### Transitive Peering

![[Pasted image 20260227102614.png]]

ال VPC Peering مش بيسمح بال VPC 1 و 3 يتواصلو مع بعض من خلال VPC 2 
لو عاوزين يتواصلو مع بعض لازم يبقي في Peering  ما بينهم هما كمان 

![[Pasted image 20260227103133.png]]

و برضو نفس الكلام في Edge 2 Edge Routing لو انا عاوز تواصل ما بين On-Prem و بين VPC 1 
مش هينفع من خلال VPC 2 علي اساس ان في ما بينهم Peering 
لازم ساعتها اعمل اما بقي VPN او Direct Connection 

من الاخر مش مسموح يبقي في Jumping Stone VPC ما بين اتنين VPCs 

-----

### AWS Transit Gateway 

![[Pasted image 20260227103354.png]]
![[Pasted image 20260227103445.png]]

في شوية مصاعب في ال VPC Peering  بتخليه زي ما قلت حل قديم 
ان لو انا عندي VPCs كتير و اللي هو اصلا بشكل واقعي بيبقو كتير جدا و علي اكتر من اكونت ساعات 
فا عشان كل VPC يوصل علي التانية لازم اوصلهم كلهم ببعض و الموضو بيبقي صعب جدا 
خاصتا لو انا عاوز اعمله كمان Hybrid و لا كمان عاوز اعمل Multi فالموضوع بيصعب اكتر 

![[Pasted image 20260227104215.png]]

ظهر حل احسن و هو ال AWS Transit Gateway الفكرة بتاعته بس انه Regional peering يعني هي تقدر توصل بين ال VPCs اللي علي نفس ال Region بس 
لو عاوز اوصل ل Region تانية (TG) تاني في ال Region اللي هناك و اخلي الاتنين يكلمو بعض 

طريقة التوصيل بيبقي من خلال اني بعض حاجة زي Attachment لل TG --- سواء عاوز اوصل مع VPC او VPN مع on-prem 
بس الفكرة اللي هنا انها Per-Subnet يعني هي لو ال VPC عليها اكتر من  Subnet عارفين ال Attachment 

يبقي ال TG حول االدنيا من Full mesh ل Hub and spokes 

-----
