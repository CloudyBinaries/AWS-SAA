
#### outlines

![[Pasted image 20260213150254.png]]

##### AWS Global Infrastructure

![[Pasted image 20260213150439.png]]

الفكرة ان كل شوية الشكل دا هيتغير بسبب التوسعات اللي بتعملها Amazon
في ال infrastructure بتاعتها في ال regions المختلفة 


طب التقسيمة بتبقي عاملة ازاي
-> الفكرة ان region بتبقي هي النطاق الاوسع جغرافيا 
-> ال region بيبقي متقسم من جوة ل AZ (Availability zones)
-> و ال  AZ دي المنطقة اللي موجود فيها فعلا ال Data centers , و فيها اكتر من  data center
-> في بقي حاجة تانية متقدمة اسمها Points of presence , و دي Edge location هنتكلم عليها بعدين 
لانها مرتبطة بمصطلحات جاية زي ال cloud front , Route53 .......
, و هدفها دايمة تقريب المحتوي و تقليل ال latency 