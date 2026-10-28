
![[Pasted image 20260227224055.png]]

### Solution

![[Pasted image 20260227230408.png]]

افكرة هنا في الطلب 3 
انه عاوز يسمح بس بال Bastion host , application انهم بس اللي يخشو لل Database instance 
ساعتها مش هقولو اني هسمح بالترافيك يجي من ال Subnet كامله عشان هتدخل اجهزة تانية معاهم
و لا هقول ال IP بتاعت الأجهزة دي عشان لو حصل Auto Scaling لل Application مش هعرف اني ادخل ال hosts التانية 
لاكن هسمح لل Security Group بتاعت ال Application , Bastion host بس انه هو اللي يسمح بالترافيك تاعتهم 
و تبقي هي ال Source 