# Monitoramento-compliance
**Esse repositório documenta um projeto que utiliza serviços como: EC2, IAM, CloudWatch, Config, S3 entre outros. O objetivo é garantir um monitoramento preciso das máquinas virtuais utilizando agente CloudWatch e a implementação de regras através do AWS Config. Dessa forma, qualquer alteração na configuração desses recursos será monitorada e notificada.**

## **Diagrama do projeto**

   ![Diagrama_project-Página-1.drawio.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/Diagrama_project-P%C3%A1gina-1.drawio.png)

## Descrição do projeto 

### Objetivo
Implementar serviços para garantir monitoramento detalhado sobre a utilização dos componentes do hardware das instâncias, fornecendo informações essenciais aonde podem auxiliar em tomada de decisões. Utilizar também serviços de compliance, para que possa garantir a conformidade com as regras da empresa, garantindo boas práticas em sua carga de trabalho 
---
### Componentes e funcionalidades 
1. **EC2 (Elastic Compute Cloud)**
   - **Serviço de computação em nuvem que permite criar máquinas virtuais com diferentes famílias de instâncias**
   - **Cada uma é otimizada para um propósito específico, como Machine Learning, Hospedagem de sites, armazenamentos de alto desempenho HDD, SSD entre outros**

2. **CONFIG**
   - **Serviço de compliance da AWS que fornece uma maneira de monitorar nossos recursos**
   - **Além de monitorar, podemos implementar regras para nossos recursos, onde essa regra crie uma barreira ao querer alterar as configurações do recursos desejado**
   - **Qualquer alteração feita nas configurações de uma Instância, por exemplo, são gerados logs, onde serão armazenados no S3 para que possam ser acessados para fins de auditoria** 
   - **Fornece métodos de notificação e correção automática caso as configurações deste recurso tenha sido alterados**

3. **CloudWatch**
   - **Serviço de monitoramento que permite coletar logs e visualizar métricas de diversos recursos da AWS**
   - **Fornece dashboards personalizáveis, possibilita o armazenamento de logs e permite a criação de regras para automação de alertas e ações corretivas**
   - **pode ser utilizado para monitorar serviços como: EC2, RDS, Lambda, incluindo métricas como tempo de execução, utilização de CPU, Memória RAM e muito mais...**
---
# passo a passo da criação do projeto 

1. VPC (Virtual Private Cloud)
   - Serviço que nos possibilita criar um ambiente virtual, privado e seguro através de outras ferramentas
   - utilizado na criação de ambientes seguros para a criação de suas aplicações, podendo ter acesso á internet ou internal entre os recursos.
     
   ![Criação_VPC.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/Cria%C3%A7%C3%A3o_VPC.png)

   > **⚠️ Atenção:** Criei uma VPC com CIDR IPV4 de 10.0.0.0/24. Esse endereço ip é privado, e deve ser utilizado para manter a privacidade do seu ambiente virtual. Utilizei a mascára 24 pois ela me fornece uma quantidade de 256 endereços IP que posso fornecer aos meus recursos.
---
2.0 Criação das Sub-redes
   - É a segmentação de endereço ip de uma rede maior 
   - Sub-redes são muito boas para o gerenciamento, eficiência de comunicação entre dispositivos, aonde o tráfego não será direcionado para switchs desnecessários, garantindo maior velocidade da comunicação.

2.1 Criação de sub-rede pública 
   - Sub-redes públicas residem em availability zone divergentes de cada uma, pois elas são únicas 
   - Com isso, ela irá fornecer IP público para qualquer recurso que tenha uma ENI, para que consiga essa comunicação com a internet e a internet consiga comunicação com ele.
     
   ![Criação_subrede_pública.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/Cria%C3%A7%C3%A3o_subrede_p%C3%BAblica.png)

2.2 Criação de sub-rede Privada 
   - Sub-redes privadas elas não tem acesso a internet, não estão associadas a uma Route table que permite o tráfego até a internet
   - Utilizadas para proteger recursos como Banco de dados, Instâncias
     
   ![Criação_subrede_privada.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/Cria%C3%A7%C3%A3o_subrede_privada.png)

3. IGW(Internt Gateway)
   - Criado para permitir e associado na vpc para permitir o tráfego de saida e entrada de tráfego
   - Através dele, recursos internos da aws podem ter acesso a internet, conseguindo se comunicar com recursos externos
   - Permite também que usuários externos, recursos externos consigam se comunicar com a vpc, recursos públicos, que permitem acesso público
     
   ![criação_igw.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/cria%C3%A7%C3%A3o_igw.png)

4. Criação da route Table
   - A route table irá criar uma rota para algum recurso, como por exemplo: IGW
   - Ela fará com que recursos que estão em sub-redes públicas, sejam direcionados para o IGW

   ![criação_Route_Table.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/cria%C3%A7%C3%A3o_Route_Table.png)

4.1 Tabelas de rotas 
   - Para que a tabela de rota funcione corretamente, deve associa-lá em alguma subrede
   - Em alguns casos quando a sub-rede necessita de acesso a internet, precisa associar ao IGW também

   ![table_route.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/table_route.png)

   > **⚠️ Atenção:** Quando se cria uma VPC, automaticamente se cria uma Route Table default para que os recursos internos da vpc, possam se comunicar entre eles. Que é a tabela que está na imagem 10.0.0.0/24.
---
5.0 Criação do SG( Security Group)
   - O SG é um firewall a nível de instância que cria regras para filtras quem ou oque pode acessar a instância, aquele recurso
   - Essas regras podemos criar regras como: portas de acesso (http, https), endereços IP específicos (escolher um enedereço específico aonde só ele pode acessar aquela máquina)

   ![Security_group_ssh.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/Security_group_ssh.png)
   
   > **⚠️ Atenção:** Criei em SG para associa-lo a instância privada, pois ele não terá acesso a internet, mas caso eu queira ter um acesso de
alguma forma, irei me comunicar com ele através do SSH. Não está de acordo com as boas práticas de segurança pois, com essa regra, qualquer pessoa que tenha as informações necessarias dessa máquina, irá conseguir acessa-lá, porém, como estou fazendo um teste, permiti que o acesso SSH seja de qualquer lugar. Repetindo, Não é aconselhavel utilizar esse meio porque seu ambiente, sua máquina não está segura. 

5.1 Criação SG Público
   - Esse SG será difente pois essa máquina terá acesso a internt, e tenho que implmentar regras aonde permitem que hosts externos condigam ter acesso a essa máquina

   ![security_group_public_Acess.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/security_group_public_Acess.png)
   
6. Criação de instâncias

   ![instancia_publica.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/instancia_publica.png)
   
   - Criei duas intâncias EC2 T2. micro, Amazon linux 2 ami
   - Tive que criar com essas configurações em especifíco pois ela compõe de um pacote que irei instalar futuramente
   - Fiz uma única alteração nessas instância, que seria qual availability zone ele iria ficar, e com essa alteração, eu irei determinar se essas máquinas virtuais teriam endereço público ou não.
  
   ![Criação_ec2.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/Cria%C3%A7%C3%A3o_ec2.png)

7.Test ping da Instância privada 
   1. - Por ter criado duas instância em sub-redes diferentes, e em Availability zone distintas também, eu quis testar o tempo de resposta que uma instância tem da outra.
   2. - Fiz uma pequena alteração no SG da Máquina virtual que estava na sub-rede privada aonde eu permitia "Custom ICMP Rule" para qualquer lugar.
   3. - Acessei o terminal da minha instância pública, escrevi " ping 10.0.0.97"  

   ![ping_server_privado.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/ping_server_privado.png)

   ---
 8. Criação da role e associando ela à instância
   - Está Role será criada e associada em uma máquina pois irei instalar um Agente do Cloudwatch
   - Com está Regra o Agente irá conseguir acesso das informações daquela instância, como por exemplo: Volume de armazenamento, identidade da máquina e entre outros
   - Além de ler, irá fornecer essas informações visualmente através do CloudWatch, garantindo uma visualização amigável para quem terá acesso aos dados  
  
   ![Criação_Role_Policy.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/Cria%C3%A7%C3%A3o_Role_Policy.png)   
 
 9. Instalação do Agente na máquina pública
   - Um Agente é utilizado para monitorar uma gama maior de métricas, fornecendo detalhes maiores sobre elas
   - Monitora Memória RAM, udo detalhado do disco (I/O) entre outros
   - A coleta das métricas do CloudWatch é de 5 minutos, podendo aprimorar para 1 min, mas isso gerar custos
   - O Agente permite ter uma coleta de métricas de 10, 20 ou 30 segundos, tornando menor o tempo de coleta e ajudando a encontrar problemas mais rápido
 
   ![instalando_agente.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/instalando_agente.png) 
   
   > **⚠️ Atenção:** Como já havia dito antes, por ter escolhido a Instância Amazon Linux 2, facilitaria pois com ela eu não seria necessario baixar o Agente. Este pacote se encontra na VM, com isso, só seria necessário escrever **sudo yum install amazon-cloudwatch-agent** e a instalação do Agente seria feita

 10. Iniciando o assistente
   - O assistente irá ajudar na configuração do seu Agente
   - Fará diversas perguntas sobre qual seria as métricas que queremos monitorar
   - tempo de coleta dessas métricas, Qual seria o sistema operacional da máquina que se está utilizando

   ![assistente_config_agent.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/assistente_config_agent.png)
   
   - Fiz todo o processo para instalar, aparetemente a isntalação havia sido feita com sucesso, porém, verificar se o Agente já estava sendo executado, ele estava inativo
   - Tive que forçar a inicialização dele manualmente, tive que escrever **sudo systemctl start amazon-cloudwatch-agent** e após **sudo systemctl status amazon-cloudwatch-agent**
   - Eu inicei o Agente do CloudWatch e iria verificar se ele já estava em execução
   
   ![iniciliazação_sucesso_agent.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/iniciliaza%C3%A7%C3%A3o_sucesso_agent.png) 

 11. Monitoramento das métrcias no console do CloudWatch
   - O teste final seria conseguir visualizar a coleta das métricas que selecionei
   - Fui até o Console do CloudWatch para verificar se tudo estava funcionando corretamente

   ![coleta_métricas_1.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/coleta_m%C3%A9tricas_1.png)
     
