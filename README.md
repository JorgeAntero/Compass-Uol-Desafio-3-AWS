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
>LADFQAF

       Depois disso, criei um servidor no Discord e fiz um Webhook personalizado no chat principal.
![Segundo print](/Prints/1.1%20-%202.png)

        Então, já dentro da VM, fiz a instalação dos pacotes que eu precisaria para a realização do projeto:
>`apt-get install nginx` - Para conseguir levantar o servidor da página web;  
>`apt-get install samba` - Para conseguir compartilhar arquivos da minha máquina windows com a VM;  
>`apt-get install curl` - Para verificar a conectividade da URL;
---
