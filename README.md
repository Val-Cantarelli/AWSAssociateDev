# amazonAWSCertification
Introdução:
Iremos usar uma web aplication que registra novo empregados. Iremos usar todos os serviços abaixo: 
 - VPC para o buid;
 - EC2(essencialmente oferece máquinas virtuais) onde hospedaremos o backend;
 - RDS: usaremos o banco de dados não relacional para guardar os dados dos employee;
 - S3(Simple Storage Service): onde ficarão imagens;
 - Amazon CloudWatch: irá fazer a parte de monitoramento da aplicação;
 - Elastic Load Balancing e EC2 auto Scaling farão a aplicação ser scalable and fault tolerant;
 - IMA: segurança e identidade;

E, para isso, usaremos o AWS Management Console.

![image](https://github.com/Val-Cantarelli/amazonAWSCertification/assets/78885340/2fa362a3-c4c0-494c-921c-c52ab58bea96)

Redundancy: se eu guardo dados no meu laptop e ele queima, perco os dados. Então os guardo no cloud, mas se ocorre um desastre natural no data center onde estavam meus dados? Pra isso temos o conceito de redundância. Meus dados estão replicados em outro data center. E esse cluster de data center é camado Availability Zone: onde temos redundância de energia, networking e conectividade. e além disso, eles fazem cluster de Availability Zone.

![image](https://github.com/Val-Cantarelli/amazonAWSCertification/assets/78885340/b72435a3-1c16-435e-963c-8626cb585f32)




