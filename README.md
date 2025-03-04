# Monitoramento-compliance
**Esse repositório documenta um projeto que utiliza serviços como: EC2, IAM, CloudWatch, Config, S3 entre outros. O objetivo é garantir um monitoramento preciso das máquinas virtuais utilizando o CloudWatch e a implementação de regras através do AWS Config. Dessa forma, qualquer alteração na configuração desses recursos será monitorada e notificada."

## **Diagrama do projeto**

   ![Diagrama_project-Página-1.drawio.png](https://github.com/Jeff01875/Monitoramento_compliance-/blob/main/Diagrama_project-P%C3%A1gina-1.drawio.png)

## Descrição do projeto 

### Objetivo
Implementar serviços para garantir monitoramento detalhado sobre a utilização dos componentes do hardware das instâncias, fornecendo informações essenciais aonde podem auxiliar em tomada de decisões. Utilizar também serviços de compliance, para que possa garantir a conformidade com as regras da empresa, garantindo boas práticas em sua carga de trabalho 
---
### Componentes e funcionalidades 
1. **EC2 (Elastic Compute Cloud)
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

5. 



   
