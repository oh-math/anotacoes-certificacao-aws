
### What is cloud computing  

Existem **3 tipos de computação em nuvem**
1. Infrastructure as a Service:
	- Amazon EC2 (on AWS)  
	- GCP, Azure, Rackspace, Digital Ocean, Linode  
2. Platform as a Service:  
	- Elastic Beanstalk (on AWS)  
	- Heroku, Google App Engine (GCP), Windows Azure  
	 (Microsoft)  
3. Software as a Service:  
	- Many AWS services (ex: Rekognition for Machine Learning)  
	- Google Apps (Gmail), Dropbox, Zoom

Sistema de precificação na AWS (simplificado)
1. Computação:  
	- Paga pelo tempo em que você computa algo
1. Armazenamento:  
	- Paga pelo armazenamento na cloud  
2. Dados transferidos para FORA da cloud:  
	- Dados transferidos para DENTRO da cloud é grátis

A AWS lançou com um serviço ao público em 2014 (foi o **SQS**)

Uma **region** é um conjunto de de data centers da AWS 
- Cada region tem uma AZ (Zona de disponibilidade, como sa-east-a ou sa-east-b)
- Cada zona é um data center com conectividades, poder e rede redundantes
- As zonas são separadas uma das outras

Tudo na cloud é _self service_

Alguns recursos da cloud são de escopo global, como o IAM e Route 53(DNS)

Existe um modelo que distribui a responsabilidade da segurança na cloud AWS chamado de **Shared Responsibility Model**

### IAM - Identity and Access Management
Um usuário pode pertencer a vários grupos ao mesmo tempo que um usuário pode não pertencer a grupo algum 

Um usuário pode ter um JSON chamado policie associado a ele contendo suas permissões

É usado o principio de não dar mais acessos que um usuário precisa

**inline policy**: politica anexada a apenas um usuário

**MFA Options**:
1. Dispositivos MFA - (para celular por exemplo), e isso inclui o Google Auth, Authy
2. Universal 2nd factor (**U2F**) - uma security key, ou dispositivo fisico que você pluga e permite acessar a conta 
	- Dá suporte para vários usuários IAM ou root pelo mesmo dispositivo  
3. Hardware key device. São dispositivos que exibem o token em tela para autenticar

Existem três maneiras de ter acesso a AWS:
1. Console AWS
2. Via a AWS CLI
3. Por meio de uma SDK

A AWS CLI foi construída usando uma SDK da AWS usando Python

### EC2 - Elastic Compute Cloud
Uma forma de infra as a service(IAAS)

Consiste principalmente na capacidade de: 
- Alocar máquinas virtuais (EC2) 
- Armazenar dados em unidades virtuais (EBS) 
- Distribuir carga entre máquinas (ELB) 
- Dimensionar os serviços usando um grupo de dimensionamento automático (ASG)

**EBS (Elastic Block Store) e EFS (Elastic File System):**
O EFS é um tipo de armazenamento dimensionável, onde ele pode escalar auto e compartilhar seu sistema de arquivos com várias instâncias, enquanto o EBS é um bloco único e performático para uma única instância

O Bootstrap inicial de um EC2 que roda os **dados de usuário** roda com o usuário root

Convenção para nomear o hardware da instância
ex: ==m==**5**_.2xlarge_
==m==: instancia da classe
**5**: geração (a AWS melhora como tempo)
_.2xlarge_: tamanho de acordo com a instancia da classe

Às diversas instâncias são usadas em diferentes workloads

t2: usado para tarefas diversas que nã́o precisam de um workload muito pesado

**Instancias da familia _c_** são uteis para tarefas intensas de computação como processamento em batch, servidores web de alta performance e etc

Trabalha com **vCPU (virtual CPU)**, que são basicamente CPUs virtualizadas em um host fisico só. Então se eu possuo uma máquina com 4 CPUs eu poderia virtualizar dentro desse host fisico criando 1 vCPU, deixando 3 CPUs liberadas

#### Security Groups
Fundamento da segurança de rede da AWS. É ele o trafego de rede que pode entrar ou sair
![[Pasted image 20240911185603.png]]
Atua como um "firewall" em instancias EC2

Eles regulam: 
- Trafego nas portas
- range de IPs (ipv4 e ipv6)
- Conexão até a instância (conectando na instancia)
- Conexão fora da instância (saindo da instancia)

**AMI - Amazon Machine Image** 
- Uma imagem (parecida com imagens docker), porém um pouco maior e mais complexa
- Podem conter SO, aplicações pré configuradas
- São específicas a uma região 

**Opções de compra**
==On demand== - Por demanda, sem compromisso, mais caro. **Recomendável para trabalhos previsiveis**
==Reserved instances== - Reserva atributos específicos da instância (OS, região, tipo de instância). Pode reservar 1 ou 3 anos (quanto mais reservar mais desconto tem). **Boas para trabalhos que precisam estar disponíveis 24/7**
==Saving plans== - Similar ao reserved instances, mas o compromisso com um determinado uso (10$ hora por 1 ou 3 anos(além disso você paga no _on demand_)). 
==Spot instance== - A mais barata. Uso "emprestado" na capacidade de outros, aí quando eles pedem de volta você perde a instância por uns minutos. **Bom para trabalhos resilientes a falhas**
==Dedicated hosts== - Server só para ti uiui (o servidor como um todo é seu). Permite usar suas próprias licenças de software (BYOL). A opção mais cara. Bom para empresas que **necessitam do BYOL**
==Dedicated instances== -A sua instância está em um hardware dedicado só você 

**EBS** - Como se fossem USBs plugaveis de redes. 
- Possui a feature de snapshot(backup)
- A feature de snapshot possui um archive, onde ele armazena a snapshot (é mais barato esse archive e demora mais para recuperar o arquivo, entre 24 e 72 horas)
- Possui uma lixeira onde você pode configurar quanto tempo ficará lá os volumes deletados
- é possível manter o volume mesmo se a instância em que ele está for excluída

Ao criar uma **AMI a partir de uma instância**, essa AMI irá capturar o estado dessa instância e poderá ser replicada em outras instância


**EC2 Image builder** - serviço para criação automatizada de AMIs(manténs, valida e testa). É Gratuito e pode rodar de acordo com um schedule (semanalmente, quando um pacote é atualizado e etc)

**EC2 Instance Store** - um tipo de armazenamento mais performático que o EBS. Bom para operações de I/O, mas se a instância cair os dados são perdidos (bom para cache e outros dados temporários)