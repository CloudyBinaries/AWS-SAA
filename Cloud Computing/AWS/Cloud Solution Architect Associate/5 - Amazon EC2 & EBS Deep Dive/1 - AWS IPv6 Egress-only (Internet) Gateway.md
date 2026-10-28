
![[Pasted image 20260910064858.png]]

the default IP version that works in AWS is IPv4 


the core difference between IPv4 and V6 is [ IPv6 doesn't have the ranges of private and public IPs. All are public ] so the IPv6 must be allocated only from AWS 

لو كنت عاوز اعمل  private server اخليه يطلع علي ال internet عشان يعمل updates or batches
بس مفيش حد يدخله من علي ال internet. انا كنت بخليه يستخدم ال NAT Gateway or NAT instance و دا في حاله اني بستخدم ال IPv4 بس 

اما في حالة ال IPv6 مفيش فيه NAT اصلا 
و في نفس الوقت مش هينفع يتعمله internet gateway زي بتاع ال IPv4 العادي 
فاقامو عمملوله [ IPv6 Internet Gateway ] Egress only  و اللي معناه ان ال Requests من جوة لبرة بس و هو بيجيله ال reply مش العكس 

==و مقدرش احط علي ال [ IPv6 Internet Gateway ] ال Security Group زيه زي ال NAT Gateway , Internet Gateway==   [ AWS Fully Managed ]

##### NOTE ! that is Stateful 

, و كمان بيتعرف في ال Route Table بتاع ال private subnet
انه لو عاوز يطلع علي ال internet لغرض ما يخرج من ال [ IPv6 Internet Gateway ] : 

![[Pasted image 20260910065745.png]]

----
#### AWS IPv6 Egress-only [ Hands on Lab ] 

مراعاة اني بعمله create و بضيفه في ال route table

------

![[Pasted image 20260910070649.png]]

![[Pasted image 20260910070716.png]]

![[Pasted image 20260910070906.png]]

![[Pasted image 20260910070945.png]]

