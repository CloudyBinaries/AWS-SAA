
![[Pasted image 20260218204508.png]]

ال NACLs هو الحماية علي مستوي ال Subnet
او بيشتغل علي ال Implied Routers

المشكلة اللي هنا ان ال rule اللي بتحطه مينفعش تعمله استثناء ل instance معينة بالسماح او بالرفض او علي حسب اللي محطوط 
يعني لو انا سمحت بان ال IP معين ميدخلش علي ال Subnet دي 
يبقي مينفش احدد انه عادي انه يدخل علي ال Instance دي بس 

الميزة انه بيعمل خط دفاع مسبق قبل ما توصل ال Request علي ال Instance 

ال NACLs مش زي ال Security Groups : أقصد هنا انه معني انه مش زيه لو سمح ان في IP معاه Port معين يعدي "Inbound" انه هيسمحله برضو انه يخرج "Outbound" , لا الكلام دا مرفوض في ال NACLs مش حاجة يتقال عليها Stateful

برضو مش زية في حته اني اقدر علي NACLs اعرف Permit , Deny Rules مش permit بس زي في ال Security groups

كل Rule في ال NACLs ليها Sequence number , و ال Rules هتتقيم من الرقم الاقل و انت طالع لغاية لغاية ما يبقي في Match مع Rule معينه مش بتكمل , و في حالة ان ال NACLs كملت و ملقتش لا Permit او Deny  بتععتبرها Deny by default : اسمها "Explicit deny"

![[Pasted image 20260218212324.png]]


![[Pasted image 20260218210237.png]]

في الصورة ال NACLs هنا بيبقي هي الحارس اللي واقف من بره ال Subnet بس الفكرة هنا ان لو الاجهزة من نفس ال Subnet بتتكلم مع بعض ال NACLs هنا ملهاش اي دول في العمليه دي و هنا بيبقي دور ال Security Groups 

#### Security Groups vs NACLs

![[Pasted image 20260218210518.png]]

#### Whitelisting vs Blacklisting

![[Pasted image 20260218211153.png]]

الاصل ان الحاجة دي موجود في ال List ايا كان ايه هو بس لاسباب معينه بقي مستثني منها 

و هنا نبدأ في حاجة زي ال Whitelisting , و هي ان الاصل فيها انها المنع و لكن لاسباب بحط Rule بقول ان الشئ دا مسموحله و عادي يدخل 

-> و دا بيطبق فيال [ NACLs , Security Groups ]

بالنسبة لل Blacklisting هي كل حاجة كانت مسموحة ليك و انه مفيش مشكلة بالنسبالك بس جه استثناء و منعها عنك 

-> و دا بيطبق في ال [ NACLs ] بس 

---------------

## و اخيرا اهم حاجة في ال NACLs الترتيب لانها هيفرق في طريقة شغلها 

### حاجة تانية و هي و انا بعرف ال port بتاع ال inbound في ال NACLs هنا انا اقصد ال Client و هو بيبعت لل Server , هيبعتله علي Port كام و عشان كده بحط بورت واحد , مثال " لو Client هيكلم ال Webserver هيبعتله علي port 80 " .                   و في ال Outbound بعرف Port Range لان دا اللي قول فيه لما السيرفر هيبعت لل Client هيبقي Port كام و و اللي هو " 49512 - 65535 "

## لما اجي لعمل Association لل NACL علي Subnet بطبق واحده بس مش اكتر لكن العكس صحيح