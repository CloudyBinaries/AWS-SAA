
![[Pasted image 20260221170941.png]]

اتكلمنا قبل كده علي ال Monolithic و قلنا انه بيحط ال Application كلها في حته واحده , و قلنا انها ممكن تسبب مشاكل في حالة ان ال Instance اللي عليه ال Application وقع او حصل في ال Application مشكلة في ال Runtime 

و هنا جينا ب Architecture جديد اسمه ال Micro-services و هي اني مش بس بقسم ال Application و اقول ان ال frontend هنا و ال Backend هنا و database في حتة تانية 
لا دا انا بمسك ال code نفسه بقسمه ل Sperate services مستقلة و احط كل واحده منهم في حاجة اسمها Containers و الميزة هنا ان الموضوع هنا هيبقي سريع و خفيف و لو حصل حاجة هلاقي ان مفيش تاثر علي ال باقي الكود 

و الفكرة هنا ان ليه ال Containers مش VMs 
عشان ال VMs ال Boot time بتاعها بياخد وقت , و الفكرة عندي System كامل كل ما احب اعمل Scaling  باخد وقت كبير و Process تقيل فا هو Heavyweight مش خفيف زي ال Containers  
الي جانب ان ال Containers ان مافيهاش OS تقدر ت Boot بسرعة , و تقدر تعمل scaling بسهولة و تقدر تهندل كميات كبيرة من ال Traffic 


![[Pasted image 20260221171202.png]]

