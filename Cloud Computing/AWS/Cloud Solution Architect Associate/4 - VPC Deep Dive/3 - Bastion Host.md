
![[Pasted image 20260227094212.png]]

فكرة ال Bastion host هو اني ازاي ادخل علي ال Private instance , و اعمل عليها Administration و هي بطبيعتها مش Accessible 

 ظهرت فكرة ال Bastion Host هو عبارة عن EC2 instance عادية جدا و فاضية اصلا بس عليها Security guards عاليه جدا و شغل Anti-Virus و IPS , IDS , DDOS , و هكذا و السبب هو انه ان دا المدخل الوحيد من بره لل Private Instances 
 و الفكرة اني ادخل علي ال host دا و بعدها اعمل SSH علي ال Private instance و او حتي اي جهاز تاني جوة الشبكة (VPC) زي مثلا ال NAT Instance  

في الصورة باين اني في اكتر من bastion host مع اني كان ممكن احط واحد بس 
بس هي الفكرة هنا عبارة عن حاجة زي ال fault tolerance , High Availability 

المفروض طلما دا المكان الوحيد للوصل علي ال Private Instance يبقي لازم احدد مين ال IPs اللي مفروض توصل عليه اصلا و دا من خلال ال NACLs , Security Groups 

