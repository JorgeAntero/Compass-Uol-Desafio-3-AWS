# - 🔒 Projeto de Bolsas DevSecOps/AWS,  Compass UOL, abril 2025 🔒 -

## 📦 Aplicação Wordpress na AWS 📦

## 📜 0 - Breve resumo >
Para o terceiro projeto, nos foi instruído a executar, seguindo a topologia proposta _(imagens abaixo)_, uma aplicação Wordpress na AWS utilizando:  

        - 🌐 VPC (2 AZ's, cada uma com uma rede pública e uma privada);  
        - 📨 RDS;  
        - 🗃️ EFS;  
        - 👥 ALB;  
        - 🤖 Auto Scaling;  
        
![Print zero](/Prints/0.0.png)
![Print zero dois](/Prints/0.1.png)

---
## 🌐 1 - Criação da VPC >
O começo de tudo foi estabelecer nossa VPC:  

![Primeiro print](/Prints/1.1.png)  
>- Criei manualmente, para melhor aprendizado e evitar erros simples;  
>- No CIDR IPv4, coloquei o range de `/16`;
>- De resto, mantive a configuração padrão;  

E então criei e configurei as sub-redes da minha VPC:  

![Segundo print](/Prints/1.2.png)  
![Terceiro print](/Prints/1.3.png) 

>- Apliquei o range de CIDR IPv4 `/24` para todas as sub-redes;  
>- As duas acima estão na primeira AZ;  

![Quarto print](/Prints/1.4.png)  
![Quinto print](/Prints/1.5.png)  

>- Essas são da segunda AZ;  





---
