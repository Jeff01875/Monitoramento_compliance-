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
   - **Qualquer alteração feita nas configurações de uma Instância, por exemplo, são gerados logs, onde serão armazenados no S3 para que possam ser acessados para fins de auditoria 
   - **Fornece métodos de notificação e correção automática caso as configurações deste recurso tenha sido alterados**

3. **CloudWatch**
   - **Serviço de monitoramento que permite coletar logs e visualizar métricas de diversos recursos da AWS**
   - **Fornece dashboards personalizáveis, possibilita o armazenamento de logs e permite a criação de regras para automação de alertas e ações corretivas**
   - **pode ser utilizado para monitorar serviços como: EC2, RDS, Lambda, incluindo métricas como tempo de execução, utilização de CPU, Memória RAM e muito mais...**
