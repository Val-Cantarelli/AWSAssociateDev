## EBS - Elastic Block Store

- É um HD Virtual (volume) you can attach to your instances while they run;
- Network drive: é um drive de rede que se conecta via rede à instância;
- The data it will persists even after terminate the instance;
- É criado para um **AZ específica**. Se quiser mover pra outra AZ, tem que fazer snapshot;
- Geralmente usado com EC2 para o boot volumes e data volumes.

**Tipos de uso com EC2:**


┌-── Root Volume (Boot) ───┐    ┌─── Data Volumes ───┐
│ • Sistema Operacional    │    │ • Aplicações       │
│ • Configurações básicas  │    │ • Bancos de dados  │
│ • DeleteOnTermination    │    │ • Arquivos usuário │
└─────────────────────────-┘    └───────────────────-┘








EBS Multi-Attach = só quando a aplicação é cluster-aware e sabe lidar com concorrência de blocos.

EFS/FSx = quando você precisa de compartilhamento de arquivos pronto e fácil entre instâncias. (Sabe lidar com concorrência e blocking)

---

Quiz:

1. EBS Volumes are created for specific AZs. If you want to migrate to another, it will require a snapshot.

2. Uma instancia possui o Volume Root(AMI e configs da instancia) e o Volume com os dados que colocamos. 

Root volume = infraestrutura descartável(`DeleteOnTermination = false`)

Data volume = dados persistentes.(`DeleteOnTermination = true`)

Quando terminamos uma instância o volume root é apagado. O volume com dados não.

3. Which of the following EBS volume types can be used as boot volumes when you create EC2 instances?

SSD = pode bootar (gp2, gp3, io1, io2).

HDD = só dados (st1, sc1, standard).


4. A company wants to reduce costs and use st1 for their root volume. What will happen?

st1 não pode ser root volume, só data volume.

5. "Which EBS type is best for boot volumes requiring high IOPS?"
io1 ou io2

Ps.: st1(barato, mas só para thorughput, nao boot)

6. What is EBS Multi-Attach?

É apenas para volumes de dados(não root/boot). Até 16 instancias podem compartilhar o mesmo volume data desde que na mesma AZ. Atenção: diferente de EFS ou FSx, o EBS não é um “file system compartilhado gerenciado”. A consistência fica por conta da aplicação.



## AMI

1. You can use an AMI in N.Virginia Region us-east-1 to launch an EC2 instance in any AWS Region?

You cannot use an AMI in N. Virginia (us-east-1) to launch an EC2 instance in another AWS Region directly. An AMI only points to the EBS snapshots that are used to recreate volumes, and those snapshots exist only in the region where they were created (stored in S3 regionally). Therefore, if you want to launch the same AMI in another region, you must copy the AMI to the destination region, which copies the snapshots as well.

2. You have provisioned an 8TB gp2 EBS volume and you are running out of IOPS. What is NOT a way to increase performance?

