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

<h1>AWS Identity and Access Management</h1>

Como os usuários irão acessar a nossa aplicação directory? 
Podemos requerer que a pessoa tenho um login em senha: isso é acess management on the app level; E então tem o fAato de que nós conhecemos o código que está rodando na VM hosted by EC2 e esse código vai precisar fazer chamadas de API para o serviço de armazenamento de objetos Amazon S3 para ler e gravar os dados para os funcionários, por exemplo as fotos deles. Só pq EC2 e S3 são serviços de uma conta em questão não significa que as chamadas de API entre eles se conectam automaticamente. Esses acessos precisam ser authenticados e autorizados.

<h2>IAM - Identity and Access Management:</h2> 

Nos dá acesso ao AWS account level(login na AWS) e também permite a conexão para chamadas de API entra EC2 e S3. Isso faz todo sentido porque eu quero que os gerentes da aplicação de Acaí apenas façam chamadas de API e não mexam no código fonte e vice-verso: se eu chamo um consultor para analisar o software não qero que ele tenha acesso ao dados dos meus clientes/funcionários.

Nesse IAM: 
 - eu crio logins para que as pessoas possam acessar os recursos dessa conta Amazon AWS(autenticação);
 - e crio as permissões de ação que cada login terá dentro da plataforma(autorização): "Este usuário tem autorização para criar um instância de EC2?"

Essa "ação" se refere à chamadas API. Dentro da AWS tudo é chamada API.

<h2>IAM Policies</h2>

![image](https://github.com/Val-Cantarelli/amazonAWSCertification/assets/78885340/80825d03-a343-4509-81aa-2b4d7303fc6d)


São documentos baseados em JSON que contém as permissões relacionadas a algum recurso.
"*": permite todas as ações(chamadas API) relacionadas ao EC2;

Action: Define quais operações específicas podem ser realizadas. Por exemplo, s3:GetObject permite obter um objeto do Amazon S3.

Resource: Define os recursos sobre os quais a ação pode ser executada. Por exemplo, um bucket específico do Amazon S3 (arn:aws:s3:::nome-do-bucket/*).

Boas práticas: 

- organize usuários em em grupos e passe permissões. Todos os usuários de um grupo herdarão as permissões;
- crie a conta e configure o root com MFA e em seguida crie um user com permissões de administrador(não use o root no dia a dia)e faça o login com ele para começar a criar usuários, políticas IAM, etc.;

<h1>IAM Roles</h1>

Quando algo/alguém precisa de acesso temporarário a AWS, é bem comum para permitir chamadas de API entre os serviços da conta da AWS e também entre pq qualquer chamada http para a conta AWS vai precisar de uma "assinatura". 

Para criar um Role, é só acessar  o IAM,buscar o app(S3, EC2,etc) que deseja fornecer acesso e o tipo de permissão(read only, reandandwrite,etc). Esse acesso será temporário, programático e rotacional.

Outra entidade que pode assumir uma role de IAM são os <i>External identity provider.</i>








