
دا شكل ال logs جوا ال Event History 

![[Pasted image 20260226063624.png]]

![[Pasted image 20260226063648.png]]

--------

دي بقي ال log نفسها 

![[Pasted image 20260226063750.png]]
![[Pasted image 20260226063733.png]]

-------
Trail Creation - Put logs in S3 Bucket

- زي ما هو باين من الصورة هو هنا بيقول ان ال Trail دي لكل ال Regions
- ال management event هي اللي هبقي شغال تحتيها by default 
- ال multi-region مش ظاهرة هنا عشان هيا Enabled by default 
- ال Single Region بيتعمل من ال CLI
- 
![[Pasted image 20260226065019.png]]


- التشفير للملفات هنا Enabled by default 
- ال log file validation هنا برضو لو انا معملتش ليه enable 
  ممكن ساعتها لو حد يعمل اي حاجة زي انه مثلا يمسح ال S3 و يغير في ال logs و يمسح فيها برضو عادي من غير اي validation اللي كانت بتحصل علي ال hash بتاعت ال File 
- 
![[Pasted image 20260226065503.png]]

- هنا في option اني ابعت ال logs دي CloudWatch

![[Pasted image 20260226065904.png]]

بعد ما بنخلص اللي فات دا بندخل علي ال logs event 


![[Pasted image 20260226070025.png]]

- هنا بيقولي اسجل ال API log في حاله ال Read - Write و لا ايه 

![[Pasted image 20260226070230.png]]

------

![[Pasted image 20260226070427.png]]

---
## في Note ملهاش علاقة بالموضوع بس ممكن اقولها و هي ان و انا بمسح ال S3 لازم افضيها الاول و بعد كده امسح ال Bucket عادي

