# EC2 instances storages

## EBS - Elastic Block Store

- It's a virtual HD (volume) you can attach to your instances while they run;
- Network drive: it's a network drive that connects via network to the instance;
- The data persists even after terminating the instance;
- Created for a **specific AZ**. If you want to move to another AZ, you need to create a snapshot;
- Generally used with EC2 for boot volumes and data volumes.

**Types of usage with EC2:**


┌─── Root Volume (Boot) ───┐    ┌─── Data Volumes ───┐
│ • Operating System       │    │ • Applications      │
│ • Basic configurations   │    │ • Databases         │
│ • DeleteOnTermination    │    │ • User files        │
└─────────────────────────┘    └─────────────────────┘









EBS Multi-Attach = only when the application is cluster-aware and knows how to handle block concurrency.

EFS/FSx = when you need ready and easy file sharing between instances. (Knows how to handle concurrency and blocking)

---

Quiz:

1. EBS Volumes are created for specific AZs. If you want to migrate to another, it will require a snapshot.

2. An instance has the Root Volume (AMI and instance configs) and the Volume with the data we put.

Root volume = disposable infrastructure (`DeleteOnTermination = true`)

Data volume = persistent data (`DeleteOnTermination = false`)

When we terminate an instance, the root volume is deleted. The data volume is not.

3. Which of the following EBS volume types can be used as boot volumes when you create EC2 instances?

SSD = can boot (gp2, gp3, io1, io2).

HDD = only data (st1, sc1, standard).


4. A company wants to reduce costs and use st1 for their root volume. What will happen?

st1 cannot be a root volume, only data volume.

5. "Which EBS type is best for boot volumes requiring high IOPS?"
io1 or io2

Note: st1 (cheap, but only for throughput, not boot)

6. What is EBS Multi-Attach?

It's only for data volumes (not root/boot). Up to 16 instances can share the same data volume as long as they are in the same AZ. Attention: unlike EFS or FSx, EBS is not a "managed shared file system". Consistency is the application's responsibility.

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

