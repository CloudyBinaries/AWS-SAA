
### Section Outlines

![[Pasted image 20260224105959.png]]

---
### Why do we need NAT (instance or gateway)?

![[Pasted image 20260224110229.png]]

اللي مخلي ال Public Instance ----> Public هو انها لما كانت في الاصل فيها Private IP عشان تعرف تتكلم مع ال Resources اللي جوة الشبكة بتاعتها , قمت مديها عنوان Public عشان اعرف اوصلها علي ال Internet و الفكرة اني ربط ما بين العنوانين دول باني عرفت علي ال Router table حاجة بتقول لو علوز تتطلع علي النت اللي هو 0.0.0.0/0 هوتروح علي IGW و هو من هناك رابط العنوانين ببعض بال NAT
المشكلة بقي هي في اني لو عاوز اكلم ال Private Instance او اعمل عليها patch او Update ساعتها هعمل كده ازاي ما هو حتي اني لو اديتها عنوان Public مش هيعرف يتربط بيها لان السكة اللي اللي في النص مقطوعة لان ال Row اللي في ال Route Table اللي بيقول "لو علوز تتطلع علي النت اللي هو 0.0.0.0/0 هوتروح علي IGW" مش موجود اصلا .
فالحل في حاجة زي كده اني اعمل لل Private instance حاجة من الاتنين , اما NAT Instance h, NAT Gateway 

--------

#### NAT Instance 

![[Pasted image 20260224111825.png]]

الحل هنا للمشكلة دي "ان ال Private Instance" تتطلع علي النت من غير ما يبقي ليها Public / Elastic IP
و هو هيبقي يبقي في وسيط معاها في نفس ال VPC يبقي هو عارف يتكلم معاها و عارف يطلع علي ال internet 
, و الوسيط  دا هيبقي اسمه NAT Instance ---- الفكرة في ال Row اللي هيتعرف في ال Route Table هو ان دلوقتي هيخلي ال Target بتاع ال 0.0.0.0/0 هو ال NAT Instance مش IGW ,
و لما ال NAT Instance هنا بتبني ال Table بتاعها اللي بيحدد ال Source IP , Port اللي جاي من ال Packet او هتروحله ال Packet و اللي هيكون متحدد فيه ال Private IP بتاع ال Instance و اللي Port اللي بتتكلم عليه و بعد كده يبدلهم مع ال Public IP , Port اللي هتطلع بيه ال Packet علي ال internet 
, و ال Packet راجع بال Response بيحصل العكس 

بتاخد اما Public , Elastic IP Address

--------
#### NAT Gateway

![[Pasted image 20260224112851.png]]

ال NAT Gateway هو كل اللي احنا قولناه في ال NAT Instance و لكن الموضوع هنا بيبقي مسؤول من AWS او زي ما بيتقال Fully Managed , و Recommended من AWS 
الفكرة ان التميز اللي هنا عن ال NAT Instance هو ان ال Single point of failure يعني لو حصل مشكل فيه هيبقي كل حاجة اتعطلت و لو جالي Load تقيل عليه ممكن يحصلي Time out او Delay و دي كمان مشكلة , و لو في مشكلة في ال AZ اللي عليها ال NAT Instance هيحصل برضو مشكلة و ال interruption of service

شغالة بس علي Elastic IP Address 

مينفعش احط عليها Security Group 

![[Pasted image 20260224113909.png]]

هنا سؤال خاص بالامتحان علي ال High Availability Architecture
و هو اني لو حصل مشكلة في ال AZ اللي عليها ال NAT Gateway او هي نفسها اتعطلت ايه اللي ممكن اعمله 
ممكن زي ما هو في الصورة اعمل NAT Gateway  في public subnet في ال AZ اللي فيها ال Private Subnet  و ال private instances يشتغلو عليه 
و الفكرة ان لو حصل مشكلة في واحد منهك يغير ال Route Table بتاع ال Subnet يخلي يرمي علي ال NAT Gateway اللي في ال Subnet التانية و العملية دي بتحصل بال Lambda Function 

------

![[Pasted image 20260226121301.png]]
