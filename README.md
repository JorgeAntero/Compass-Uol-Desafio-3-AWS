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
### O começo de tudo foi estabelecer nossa VPC:  

![Primeiro print](/Prints/1.1.png)  
>- Criei manualmente, para melhor aprendizado e evitar erros simples;  
>- No CIDR IPv4, coloquei o range de `/16`;
>- De resto, mantive a configuração padrão;  

### E então criei e configurei as sub-redes da minha VPC:  

![Segundo print](/Prints/1.2.png)  
![Terceiro print](/Prints/1.3.png) 

>- Apliquei o range de CIDR IPv4 `/24` para todas as sub-redes;  
>- As duas acima estão na primeira AZ;  

![Quarto print](/Prints/1.4.png)  
![Quinto print](/Prints/1.5.png)  

>- Essas são da segunda AZ;  

### O próximo passo foi criar o Internet Gateway, para possibilitar nossas sub-redes públicas a se comunicar com a internet:  

![Sexto print](/Prints/1.6.png)  

>- Ele só necessita de um nome para ser criado;  

### Para ele funcionar corretamente, precisa ser associado a uma VPC:  

![Sétimo print](/Prints/1.8.png)  

>- Bastou selecionar a VPC que criei;  

### Em seguida, criei tabelas de rotas para nossas sub-redes. Comecei pela pública:  

![Oitavo print](/Prints/1.7.png)  
![Nono print](/Prints/1.9.png)  
![Décimo print](/Prints/1.10.png)

>- Só precisei nomeá-la e associar a nossa Vpc para criar;  
>- Em suas rotas, adicionei para o destino `0.0.0.0/0` (que significa todos os destinos IPv4) o alvo do nosso Internet Gateway;  
>- E por último, associei as sub-redes públicas que eu criei antes à ela;  

### Antes de criar as tabelas privadas, precisei criar os NAT Gateways.  
![Print Onze](/Prints/1.11.png)  
>- Primeiro de tudo, fiz os IPs elásticos;  

![Print Doze](/Prints/1.12.png)  
>- Os NAT Gateways precisam estar associados à redes públicas, por isso, para esse primeiro, coloquei a `Publica-1`;  
>- Também aloquei um dos IPs elásticos a ele;  

![Print Treze](/Prints/1.13.png)  

>- Mesmas etapas da última print;  

![Print Quatorze](/Prints/1.14.png)  
>- E então, assim como fiz com as públicas, atribui cada Sub-rede a sua tabela de rotas;  

![Print Quinze](/Prints/1.15.png)  
![Print Dezesseis](/Prints/1.16.png)  

>- E por último, em suas rotas adicionei o destino `0.0.0.0/0` com o alvo de cada NAT Gateway;  

---
## 🚔 2 - Configurando os Grupos de Segurança >  
### Para o próximo passo, decidi já deixar todos os grupos de segurança que eu precisaria configurados:  

![Print Dezessete](/Prints/2.1.png)  
>- Primeiro configurei o das EC2, habilitando a entrada de qualquer IP dos tipos `SSH` e `HTTP`;  
>- Nas regras de saída deixei o padrão;  

![Print Dezoito](/Prints/2.2.png)  
>- Para o RDS, habilitei a entrada de IP's associados ao grupo de segurança da EC2, do tipo `MYSQL/Aurora`;  
>- Saída padrão;  

![Print Vinte](/Prints/2.3.png)  
>- No grupo do EFS, permiti entrada de IP's associados também ao SG do EC2, do tipo `NFS`;  
>- Saída padrão novamente;  

![Print Vinte e um](/Prints/2.4.png)  
>- Por último, a entrada do tipo `HTTP`para qualquer IP;  
>- Saída padrão novamente;  

## 📨 3 - RDS configurado >  
### Então, segui para configurar o Banco de Dados  
### Primeiramente, configurei o grupo de segurança dele:

![Print Vinte e dois](/Prints/3.1.png)  

>- Associei a minha VPC e adicionei as sub-redes privadas de cada AZ;  

### Com ele pronto, configurei o banco de dados RDS:

![Print Vinte e três](/Prints/3.2.png)  

>- Selecionei a criação padrão, e escolhi o banco MySQL, por conta do Wordpress;  

![Print Vinte e quatro](/Prints/3.3.png)  

>- Como modelo, selecionei o gratuito;  

![Print Vinte e cinco](/Prints/3.4.png) 

>- Deixei o usuário principal como admin e criei sua senha;  

![Print Vinte e seis](/Prints/3.5.png)  

>- Sua classe foi `db.t3.micro`, e o armazenamento mantive o padrão;  

![Print Vinte e sete](/Prints/3.6.png)  

>- Neguei o acesso público, e selecionei o SG que criei mais cedo;  
>- Deixei sem preferência de AZ;  
>- A partir dessa etapa até a condifuração adicional, mantive tudo padrão;  

![Print Vinte e oito](/Prints/3.7.png)  

>- A única configuração adicionais feita foi nomear o banco de dados inicial como `Projeto`, para facilitar a integração futura no User_Data;  

## 🗃️ 4 - Criando EFS >  
### Criar a EFS foi o próximo passo:  

![Print Vinte e Nove](/Prints/4.1.png)  

>- Mantive o padrão do tipo regional, porém, como é um caso acadêmico, desabilitei o backup automático, o gerenciamento do ciclo de vida e a criptografia, para evitar custos adicionais desnecessários;  
>- De resto, mantive padrão;  

![Print Trinta](/Prints/4.2.png)  

>- Em acesso à rede, associei a minha VPC;  
>- Por último, em Destinos de montagem, apontei as minhas duas AZ's, nas sub-redes privadas e as duas com o grupo de segurança que criei para a EFS;

## 👥 5 - Configuração do Load Balancer >  
### Para o penúltimo passo, criei o Load Balancer:  

![Print Trinta e um ](/Prints/5.1.png)  

>- Configurei no esquema voltado para a internet, e com o tipo IPv4;  

![Print Trinta e um ](/Prints/5.2.png)  

>- No mapeamento de rede, associei minha VPC e minhas sub-redes privadas das duas AZ'S;  

![Print Trinta e um ](/Prints/5.3.png)  

>- Então, precisei criar um grupo de destino. Selecionei o tipo como Instâncias, e de resto mantive o padrão;  

![Print Trinta e um ](/Prints/5.4.png)  

>- Selecionei o grupo de segurança do LB criado mais cedo;  
>- E por último, em Listeners e roteamento, mantive o padrão e associei o meu Grupo de Destino;  

## 🤖 7 - Criando o Auto Scaling >  
### O último passo necessitou de diversas etapas, a primeira sendo configurar um modelo de execução para as instâncias:  

![Print Trinta e um ](/Prints/6.1.png)   
![Print Trinta e um ](/Prints/6.2.png)  

>- Dei a ele um nome, e escolhi Amazon Linux como sua imagem;  

![Print Trinta e um ](/Prints/6.3.png)  

>- O tipo escolhido foi `t2.micro`, por ser gratuito;  
>- Não associei um par de chaves;  
>- Não associei também sub-redes no modelo, e escolhi o grupo de segurança da EC2;  
>- **OBS: Na imagem não aparece, mas também associei o grupo de segurança do RDS para evitar erros**   

### Também adicionei no modelo meu user data:

```
#!/bin/bash


exec > /var/log/user-data.log 2>&1                            #Toda saída vai para o arquivo de log;
set -euxo pipefail                                            #Serve para evitar erros durante a execução do script;

dnf update -y
dnf install -y docker
systemctl enable --now docker
usermod -aG docker ec2-user

mkdir -p /usr/libexec/docker/cli-plugins
curl -SL "https://github.com/docker/compose/releases/download/v2.23.0/docker-compose-linux-x86_64" -o /usr/libexec/docker/cli-plugins/docker-compose
chmod +x /usr/libexec/docker/cli-plugins/docker-compose  

#As linhas acima baixam o docker e o docker compose, além de atualizar o sistema

mkdir -p /mnt/efs                                            #Cria um diretório de montagem para o EFS;
sudo mount -t nfs4 -o nfsvers=4.1 *MEU ID DA EFS*:/ /mnt/efs #Monta o EFS no diretório;

while ! mountpoint -q /mnt/efs; do
  sleep 2
done                                                         #Esse While garante que o EFS seja montado antes de continuar;

mkdir -p /mnt/efs/wordpress                                  #Cria o diretório para o Wordpress;
chown -R ec2-user:ec2-user /mnt/efs                          #Dá permição ao ec2-user para usar os arquivos EFS;

#A partir daqui, o comando cria o compose;
sudo -u ec2-user bash -c 'cat > /home/ec2-user/docker-compose.yaml <<EOF
version: "3.8"
services:
  wordpress:
    image: wordpress
    restart: always
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST:*MEU ENDPOINT*
      WORDPRESS_DB_USER: admin
      WORDPRESS_DB_PASSWORD: *MINHA SENHA*
      WORDPRESS_DB_NAME: Projeto
    volumes:
      - /mnt/efs/wordpress:/var/www/html
EOF'

#Abaixo, apenas executa o compose;
sudo -u ec2-user bash -c "cd /home/ec2-user && docker compose up -d"
```


### Só após isso criei o Auto Scaling em si: 

![Print Trinta e um ](/Prints/6.4.png)  

>- Escolhi o template do modelo de execução;  

![Print Trinta e um ](/Prints/6.5.png)  

>- Associei minha VPC, com as duas sub-redes privadas;  

![Print Trinta e um ](/Prints/6.6.png)  

>- Anexei o balanceador de carga que criei anteriormente;  

![Print Trinta e um ](/Prints/6.7.png)  

>- Não habilitei a mudança de zonas, e ativei a verificação de integridade do LB;  

![Print Trinta e um ](/Prints/6.8.png)  

>- Escolhi a capacidade desejada, assim como a mínima desejada como 2, e a máxima como 4;  
>- Nao escolhi nenhuma política de escalabilidade;    

![Print Trinta e um ](/Prints/6.9.png)  

>- Nessa etapa deixei tudo padrão;  
