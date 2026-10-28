
![[Pasted image 20260221174739.png]]

ال CloudWatch هي عينك اللي موجوده في ال Account 
و بتراقب كل حاجة تقريبا في AWS 

هي جزء ال Monitor , visibility  في AWS , و عبارة عن Metric data repository 
و الفكرة بتاعت ال Metric دا انه بيقيس قيمة معينة بمرور الوقت 

![[Pasted image 20260223041530.png]]

ال CloudWatch بيقيس حاجات زي:

- ال CPU Utilization = 45%
    
- ال Network In = 2MB
    
- ال Disk Read Ops = 120

شبه فكرة عداد العربية 
Time-stamped numerical value describing system performance


كمان ال CloudWatch في منه جزء اسمه CloudWatch Logs بتجمع ال Logs و من خلال ال logs اللي بتتبعت و بتتراقب, بيبقي في حاجة اسمها CloudWatch Alarm & Events و اللي بدولها بتنبهك عن ان في حاجة بتحصل 
زي مثلا ان بينبهني ان ال CPU Utilization علي عن 70% فايخليني ازود instances زياده لل Auto-scaling
مثلا في محاولات دخول علي ال EC2 من IP address معينة يبقي اكيد في حاولة Attack 
, و برضو ال Events بتاعت ال CloudWatch دا انها ممكن بقي اخد اكشن نتيجة للتحزيرات دي زي اني اطفي ال instances لو قل ال Utilization عن 10% 

, برضو في احصائيات من ال logs , metrics اللي بتتجمع دي كلها 

و الكلام دا كووووووله محطوط علي AWS Dashboard 

من المزايا اللي موجوده اني اعمل Custom metrics , يعني Metrics مش موجوده اساسا, زي مثلا ان في 5 محاولات للدخول علي EC2 instance من خلال ال SSH يبقي دي ممكن تبقي محاوله hacking 