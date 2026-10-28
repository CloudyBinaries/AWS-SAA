
ال NAT Gateway , Bastion Host لازم يكونو في Public Subnet حتي لو لوحدهم 

يستحسن ان ال bastion host يكون ليه elastic IP

ال Endpoints دي بتتعمل لل Communication مع Services برة ال VPC --- يعني ال RDS ملهاش شغل Endpoint

ال Gateway endpoint ملهاش انها يتحط عليها سواء NACLs او Security Groups لكن ال Endpoint interface بيتحط ليها 

انا بخلق Gateway Endpoint لكل واحده مش لكله "S3 - DynamoDB" , يعني يتعمل واحد ل S3 و واحد ل DynamoDB --- و دا بالناسبة لل  VPC --- محدود بال Region يعني مينفعش استخدم واحد في Region تانية 

الاتصال مع ال Services من ال ENI لازم يكون ال Initiation من ال Instance نفسها او من ال ENI نفسها مش من ال Service