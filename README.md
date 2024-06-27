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

![image](https://github.com/Val-Cantarelli/amazonAWSCertification/assets/78885340/7d6178dd-9671-40d5-b1b7-27a27e22273e)

Como saber qual região eu devo escolheer?

Considerações:
- Compliance:see existe algum regulamento que faça o seu DB e/ou resources rodarem dentro de um país /localidade específica;
- Latency: quão perto meus resources estão dos meus users; Você pode escolher usar no Brazil, mas os usuários de Oregan irão ser afetados
- Pricing: em SP, por exemplo, é mais caro por conta a estrutra de impostos
- Service availability: assim que um servico é lançados você não aplica ele para todo o mundo e sim, primeiramente, na sua zona

Os quatro campos acima são básicop. Precisamos pensar em Edge locations and regional Edge caches: replicar cópias de conteúdo para que cada solicitação não faça o download de novo.

Como interagir com a AWS API: 
- AWS Management Console
- AWS Comand Line Iterface
- AWS Software Development Kits
  
<h1>Security and the AWS Shared Responsibility Model</h1>

A aplicação é composta por partes e assim também é a responsabilidade. Algumas partes são  de responsabilidade da AWS e outras do cliente.

![image](https://github.com/Val-Cantarelli/amazonAWSCertification/assets/78885340/372288ae-a30d-461e-a79e-430de6ab5672)

![image](https://github.com/Val-Cantarelli/amazonAWSCertification/assets/78885340/8414615d-d16f-4055-a15e-875add35a16c)

![image](https://github.com/Val-Cantarelli/amazonAWSCertification/assets/78885340/edc3abc5-807e-4335-a5b0-a248e115e38b)

![image](https://github.com/Val-Cantarelli/amazonAWSCertification/assets/78885340/e882620c-d6f7-4d9c-b6e9-c75dec3fbdfb)

Apicar a MFA ao root, mas evitaremos de usá-lo pois há poucas ações que vc precisa do root. 

Authentication: login
Authorizations: qual recurso e/ou serviço você pode usar.


