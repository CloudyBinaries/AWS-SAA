
##### بأختصار شديد ال IAM (identity and access management) مسؤوله عن ال Authentication , Authorization علي مستوي ال Organizations اللي مسؤولة عنها


 ![[Pasted image 20260213164842.png]]

الشغل علي ال IAM حساس جدا لان اي حاجة بتعملها عليها AWS مش بتسألك عنها او بتحددلك الموضوع تتحرك قد ايه لا بالعكس كل شئ محدده في ال IAM لا غبار عليه 


![[Pasted image 20260213165145.png]]

##### ايه الخصائص بتاعت ال IAM ؟

-> اول حاجة و هي ال shared access to AWS accounts 
, و هنا احنا لازم نفتكر اننا في ال public cloud , و دا معناه ان في شركة X موجوده و شركة Y موجوده و امازون بتضمن ان الاكونتين ميعرفوش يشوفو ال resources بتاعت كل واحد فيهم 
الا اذا 
حد منهم حدد ان حد من شركة X قالت في ال IAM ان شركة Y بقدر تشوف resources بتعاتهم 

-> ال Granular permissions و هي تحديد الصلحيات بشكل صارم 
لو حد ليه انه يعمل view بس يبقي مفيش اي احتيال انه يشوف حاجة غير ال view 
ميقدرش يعلي صلحياته من نفسه

-> ال secure access to AWS resources for applications that run on AWS
, يعني لكل ال resources اللي وجوده بتقدر تتحكم في وصول المستخدمين ليها من خلال ال IAM عن طريق تحديد ال permissions 

-> ال Multi-factor Authentication , و دي الفكرة اللي بتامن عملية ال Authentication بالتحديد , و الفكرة و انا داخل لاول مرة بيسجل بالاميل و الباسورد 
و هنا ال AWS بشك و يقول ما يمكن يكون اللي داخل بيهم دول كان سارقهم اصلا مش بتوعو فايقول انا هبعت علي الايميل دا او الرقم المعين بتاعك code تحقق متزامن مع AWS ممكن تكون من خلال Authenticator بتاع Google , و الامان هنا بيتخلص ان السارق لازم يكون معاه ال email , password و الجهاز بتاعك كمان عشان يبقي عاده اماكنية الدخول للاكونت بتاعك علي AWS , و دي كمان حاجة بتقدر تحددها من خلال ال IAM 

-> ال Identity federation و  هي اني انا معديش IAM user و AWS متعاملتش معايا قبل كده و انا ممكن اعمل حاجة زي ال Login with google , او  login with Facebook 
, من خلال كده بقدر اسجل في AWS , بس الفكرة ان الجهة دي لازن تكون trusted من AWS

-> ال Identity information logs for audit and compliance purposes , هنا الموضوع من مزايا فكرة ال Cloud عامتا و ال IAM بالتحديد و هي ان كل حاجة متسجلة و كل الاكشنز معروف مين اللي عملها و امتي و دا مفيد طبعا لفرق ال security بالتحديد و هناك طبعا بيشوفو هل في user استغل وجود misconfiguration , و عمل حاجة مش مصرح ليه بيها  

-> ال PCI Compliance و دي خاصة بال Credit cards 

-> ال Integrated with AWS services و هي ان كل الصلاحيات و التعليمات داخله في كل حاجة خاصة بال services و بتتحكم فيها 

-> ال Eventually Consistent , و هي انه بيتكلم علي ان ال IAM دي Global Service و دا بيخدم علي حاجة زي اني لو ليا اكونت ب permissions معينه و روحت Region تانية هلاقي ان ال permission بتاعتي زي ما هي 

-> و ان ال IAM ببلاش AWS مش بتحاسبنا عليها 

-> ال AWS STS , و دي زاحده من الحاجات الحساسة علي AWS و هي ان لو في مهندس هيجي يدخل علي الاكونت بتاع الشركة بشكل مؤقت و عاوزه ياخد  Access علي Resources معينة لوقت معين و كمان اقدر اتحكم في الوقت اللي بقعده ال user دا , طبعا مش بياخد IAM user لان دي اسمها long term credentials 


-------------------------------

## IAM Identities [ users - groups - roles ]

![[Pasted image 20260213185707.png]]

--------

## Note that
ال [ AWS IAM Roles ] فكرتها انك مش شخص هنا و لا جهة الفكرة فيها اني دي زي [ temp credentials ] هي ليها اكسس بشكل مستقل عن المستخدمين او اي حاجة تانية و دي اللي منها الحاجات اللي زي [ STS ] و هي عاملة زي اني لو كنت في فندق و نسيت الكارت بتاع غرفتي و عاوز افتح الغرفة , بخلي حد من الريسبشن معاه كارت يقدر يفتح بيه الغرف و يفتح غرفتك 
###### و دي بتنفع في حالات اني عامل Application ما لوش انه يتكتب فيه username , password و انا مش عاوز اخليه يخرن الحاجات دي او حتي Access keys فيه او اي حاجة long term credentials , فاساعتها ب Attach معاها role 

###### بس في مصطلحات مهمة لازم تتزكر و هي ال [ Policy - Permissions - Principal ], ال principal هي عبارة عن الكيان نفسه اللي بيعمل ال Action , request 


