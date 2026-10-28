
![[Pasted image 20260219173025.png]]

و انا بتعامل مع AWS انا محتاج حاجة من الاتنين و هما 
- ال Console 
- ال Programmatic interfaces : CLI - SDK - APIs

لل Authentication علي ال interfaces المختلفين دول انا محتاج نوعين مختلفين برضو من ال Authentication methods 

- Console Access : Username/password
- Programmatic Access : Access Keys

اتكلمانا سابقا عن فكرة ال Username/password 
و دلوقتي هنتكلم عن ال Access keys و ازاي بطبقهم و ازاي ال Users بيدخلوهم في ال Application و ازاي بتعرفهم في ال Application اصلا

اولا : 
اليوزر هنا مش بيدخل ال Access key بايده علي ال Keyboard

ثانيا : 
انا بحتاج ال Access keys دول في حالة اني عاوز اوصل لل AWS Resources من اي حاجة غير ال Console 

![[Pasted image 20260220025323.png]]

ال Keys بتتون من جزئين و هما ال Key ID , Secret Access Key و الاتنين مع بعض اسمهم ال Key pair 

ممكن تشوف و تقراء و تعدل و تغير ال Access keys 

و الوقت الوحيد اللي ممكن تاخد ال Access keys هو الوقت اللي بعمل في ال Add User و تقريبا بيبقي اخر خطوة و بيقولك فيه Download CSV و قصده هنا انك تنزل المعلومات اللي عن ال user و منها ال Key pair بتوعه

![[Pasted image 20260220030242.png]]


معلومة اضافية : 
و انت ب Add user انت بيبقي في قدامك اختيارين و هما انك تخلقله Programmatic Access Keys , Console Access , و ممكن تعمله واحده من الاتنين او تعمله الاتنين و الاختيار هنا بيرجع لمبدأ ال Least Privilege Access , و اللي بيقول انت تدي ال User اقل صلاحيات ممكنه تمكنه من اداء وظيفته 


