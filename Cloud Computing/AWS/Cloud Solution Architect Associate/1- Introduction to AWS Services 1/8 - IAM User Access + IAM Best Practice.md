
## Access to AWS 

![[Pasted image 20260213213218.png]]

###### في طرقتين عشان اوصل لل AWS و هي 
###### Access keys  ---  username/password  ---  Certificates 

###### Access key for [ programmatic access - CLI - SDK - APIs ]

###### username/password for [ Console Access ]

لو عندنا root users / مستخدمين مهمين ساعتها من الحاجات الكويسة اللي ممكن اعملها اني اعمل multi-factor authentication

-------------

### من الحاجات اللي AWS بتنصحك بيها و انت بشتغل علي ال IAM 

![[Pasted image 20260214165411.png]]
![[Pasted image 20260214165429.png]]


##### 1 - انت تقفل علي مفاتيح ال root users في مكان أمن 

##### 2 - اعمل يوزرز مختلفين و ابعتلم علي الميل بشكل امن و خلق ال password policy 

##### 3 - بتنصحك AWS انك تستخدم ال policies الجاهزة للتعامل مع ال group , و غيره مع ذلك برضو ممكن تخلق ال policies الخاصة بيك برضو

##### 4 - بدل ما تحط permissions علي كل user حط ال users في group و طبق عليهم مجموعة ال permissions الواحده الخاصة بيهم

##### 5 - لازم تبقي الصلاحيات تبقي فيها الحد الكافي اللي تمكن ال user من اداء مهمته و صلاحياتة تكون برضو محدده في النظام

##### 6 - خلق strong policy للمستخدمين 

##### 7 - طبق ال Multi-Factor Authentication علي اليوزر لزياده الحماية خاصة لليوزرز اللي عندهم صلاحيات عالية 

##### 8 - لما يكون عندك Application علي EC2 instance متخليش ال Access ليه من خلال [ username / password ] او [ access keys ] لكن استخدم [ IAM role ] 

##### 9 - لما تدي Access لحد علي الاكونت معين او حتي الاكونت بتاعك , استخدم ال IAM Role و باعتبار انها صلاحيات مؤقتة و برضو بقدر اتحكم في مده ال session بتاعته -----> [ STS ]

##### 10 - واحده من الحاجات اللي بتزود الامان هي انك بتعمل حاجة اسمها [ rotate credentials ] و هي ان اليوزر مضطر انه يغير ال username / password بتوعه كل مثلا 90 يوم او حتي ياخد Access keys جديده 

##### 11 - امسح ال credentials اللي ماعدتش مستخدمه او مبقتش ليها اهمية زي مثلا ان في موظف كان ليه Access ل resources معينة و بعد كده اتنقل لقسم تاني ساعتها بشيل منه ال permissions اللي تخليه يدخل علي ال resources اللي كان بيقدر يخشها 

##### 12 - استخدم policies للامان الزايد في حاجات مثلا زي انك تخلي ال IP اللي تقدر تعمل Access ل resources معينه تكون ال IP بتاع اللي جوه الشركة بس 

##### 13 - خلي في مراقبه دايما للنشاط بتاع الموظفين جوه ال cloud environment 

##### 14 - تقدر تعرف Access levels مختلفة   