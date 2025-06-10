# - 🔒 Projeto de Bolsas DevSecOps/AWS,  Compass UOL, abril 2025 🔒 -

## 📦 Aplicação Wordpress na AWS 📦

## 📜 0 - Breve resumo >
Para o terceiro projeto, nos foi instruído a executar, seguindo a topologia proposta _(imagem abaixo)_ uma aplicação Wordpress na AWS utilizando:  
        - 🌐 VPC (2 AZ's, cada uma com uma rede pública e uma privada);  
        - 📨 RDS;  
        - 🗃️ EFS;  
        - 👥 ALB;  
        - 🤖 Auto Scaling;  
        
![Print zero](/Prints/0.0.png)

---
## 🐧 1 - Ambiente Linux e Discord >
        Utilizei em meu projeto uma Máquina Virtual, por meio do aplicativo Oracle VirtualBox. Criei a máquina com os requisitos mínimos para a utilização do Debian Headless, distro que possuo maior familiaridade. Além disso, habilitei a placa de rede em modo Bridge, para evitar possíveis erros de conexão.
![Primeiro print](/Prints/1.1.png)

       Depois disso, criei um servidor no Discord e fiz um Webhook personalizado no chat principal.
![Segundo print](/Prints/1.1%20-%202.png)

        Então, já dentro da VM, fiz a instalação dos pacotes que eu precisaria para a realização do projeto:
>`apt-get install nginx` - Para conseguir levantar o servidor da página web;  
>`apt-get install samba` - Para conseguir compartilhar arquivos da minha máquina windows com a VM;  
>`apt-get install curl` - Para verificar a conectividade da URL;
---
