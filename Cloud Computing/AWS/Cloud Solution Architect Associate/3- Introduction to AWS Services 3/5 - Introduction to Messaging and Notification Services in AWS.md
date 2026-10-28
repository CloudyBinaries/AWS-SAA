
### SQS (Simple Queue Service)

![[Pasted image 20260220205555.png]]

نفترض سينايرو اننا بنشتغل علي Multi-tier Application 
, و انا الطبيعي ان Back-End بيستقبلال Request و بيعمل عليه ال Process و بيبعته لل Database 
بس الفكرة ان ال Back-End بيقدر بيستحمل لحد مثلا 500 Request , غير طاقة تحمل ال Database اللي بتستحمل لحد 50 Request بس.
فالمشكلة بتبدأ تحصل علي ال Application لما يحصل Load تقيل في اوقات زي ال Black Friday , ال backend بيستقبل ال  500 Request و ال Database مش ملاحق علي كده فابيحصل latency , ال backend مش عارف ياخد requests تانية عشان ال Database ملحقش يخلص اللي جاله في الاول 

الحل بقي كان في حاجة من AWS اسمها SQS و هي عبارة عن Queue
بتشيل ال Requests , او ال Orders بتخزنها و تبعتها لل Database 
, و بدروها بتشيل الحمل عن ال Database , و تخلي ال Back-End تكمل شغلها عادي , و  كمان High Available , Redundant يعني مش يتضيل ال Requests اللي شايلاها   

و الحوار دا بيظهر في مثلا E-Commerce Application 
لما اكون هطلب Order ,  و عاوز ال Application يبعت ان الطلب خلاص اتضاف و هينتظر خلال كام يوم عشان تسلم Order 
الفكرة هنا ان لولا ال SQS كان زمان الابليكشن اما انه تبعت Check-out Request , و السيرفر يقع او يرد عليك بعد كام ساعة.
لما ضيفنا ال SQS هنا هيدي فرصة ان ال Database تهندل ال Request , و بيظهر دا في سلوك البرنامج انه مثلا بيقولك انتظر في خلال ساعة مثلا و يتبعتلك Email Confirmation يأكد عليك الطلب


طريقة الربط بقي بتبقي مختلفه عن ال Application العادي مع ال Database المتعارف عليه في كود ال Connection 
الفكرة هنا انك بدل ماتبعت ال Request ل Database بشكل مباشر هتبعت الطلب علي SQS و تخلي في حاجة برضو زي ال Lambda function تقرا ال Request , data اللي شايلاها ال SQS و تديها لل Database 

خدمة SQS مناسبة اكتر لل native cloud applications او يعني اللي اتبنك من Scratch in AWS مش ال Application اللي جاية من برة 

الوحده اللي بنقيس بيها ال SQS هي ال Queue

ال Requests بتتخزن في SQS لفترة طويلة ممكن توصل 16 يوم 

اللي بيبعت ال Message  لل Queue اسمه [message producer]

اللي بيستقبل ال Message من ال Queue اسمه [message consumer] ----> اللي في حالة دي كان ال Database 

, و من الكلمات الداله عليه في المتاحان هي ال [Decoupling]
 او [Broker] ----> قاصد هنا يفكر الارتباط بين ال Backend , database 


---------
### SNS (Simple Notification Service)

![[Pasted image 20260220205050.png]]

في اللي فات كانت ال SQS بتشتغل علي ال Messages بشكل بين و واحد بيبعت و واحد بيستقل بس مش Real time يعني Asynchronies

عشان نبدأ نشرح فكرة ال Service دي لازم نشرح السيناريو دا 
لو نتخيل ان ال AZ اللي حاطط عليها ال Application بتاعتي و كل ال Resources كلها حصل فيها مشكلة 
و انا مش عارف طبعا و مش فاتح ال Console 
, في مستخدمين ميعرفوش ايه اللي حصل برضو و فاكرين ان ال Application مش بيفتح , الاجمال ايه اللي يعرفني ايه المشكلة اللي حصلت و ينبهني ليها عشان اعرف اتصرف بدري 
و اكيد مش هيبقي حاجة من جوة الا  Application عشان كل حاجة فعلا راحت 
هنا بيجي دور ال SNS و اللي مسؤوله عن عملية ال Notify لكل المشتركين او مؤول ليهم انهم يوصلو علي ال Architecture اللي معمول فعلا 

, و الموضوع كله ما هو الا Process عبارة عن Topic بتكتب فيه ناس ليها permission و ال notification مش بتتبعت غير للناس اللي مشتركه علي ال Topic دا 

خلال ما الرسالة بتتبعت بيحصل حاجة اسمها http in transit يعني الرسالة و محتواها مشفرين بين ال publisher , subscribers 

بس الفكرة ان الموضوع بتاع ال Notification دا ممكن يتبعت باكتر من طريقة منها ال lambda function 


