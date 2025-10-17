# 🌐 Projeto: AWS S3 Static Website Lab

Este projeto demonstra passo a passo como criar e implantar um **site estático na AWS** usando **Amazon S3**, **IAM** e **AWS CLI**, com automação de atualização via script bash.  
O conteúdo foi desenvolvido a partir de um laboratório prático da AWS Training.

---

## 🧩 Objetivos do Projeto

- Criar um bucket no **Amazon S3**.
- Criar um usuário **IAM** com permissões de acesso total ao S3.
- Fazer **upload** dos arquivos de um site estático.
- Habilitar **hospedagem de site estático** diretamente no S3.
- Criar um **script automatizado** para atualizar o site de forma rápida e repetível.
- Comparar o uso de `aws s3 cp` com `aws s3 sync`.

---

## ⚙️ Tecnologias Utilizadas

- **AWS CLI** (linha de comando)
- **Amazon S3**
- **IAM (Identity and Access Management)**
- **Amazon EC2 (para execução dos comandos)**
- **Linux / Bash**

---



🚀 Passos Principais

1️⃣ Conectar à instância EC2
```bash
sudo su -l ec2-user
pwd
2️⃣ Configurar o AWS CLI
bash
Copiar código
aws configure
Insira suas chaves de acesso, região us-west-2 e formato json.

3️⃣ Criar o bucket S3
bash
Copiar código
aws s3api create-bucket --bucket <nome-unico-do-bucket> --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2
4️⃣ Criar usuário IAM com acesso total ao S3
bash
Copiar código
aws iam create-user --user-name awsS3user
aws iam create-login-profile --user-name awsS3user --password Training123!
aws iam list-policies --query "Policies[?contains(PolicyName,'S3')]"
aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess --user-name awsS3user
5️⃣ Extrair e preparar os arquivos do site
bash
Copiar código
cd ~/sysops-activity-files
tar xvzf static-website-v2.tar.gz
cd static-website
6️⃣ Enviar arquivos para o S3
bash
Copiar código
aws s3 website s3://<bucket>/ --index-document index.html
aws s3 cp /home/ec2-user/sysops-activity-files/static-website/ s3://<bucket>/ --recursive --acl public-read
7️⃣ Criar script para atualização automática
bash
Copiar código
cd ~
touch update-website.sh
vi update-website.sh
Conteúdo do script:

bash
Copiar código
#!/bin/bash
aws s3 cp /home/ec2-user/sysops-activity-files/static-website/ s3://<bucket>/ --recursive --acl public-read
Permitir execução:

bash
Copiar código
chmod +x update-website.sh
Atualizar o site:

bash
Copiar código
./update-website.sh
💡 Otimização opcional (com sync)
bash
Copiar código
aws s3 sync /home/ec2-user/sysops-activity-files/static-website/ s3://<bucket>/ --acl public-read
🌎 Resultado Final
Ao final, o site estará disponível publicamente em um endpoint semelhante a:

cpp
Copiar código
http://<bucket>.s3-website-us-west-2.amazonaws.com
🧠 Aprendizados
Criação e configuração de buckets S3 via AWS CLI.

Controle de acesso com IAM.

Publicação e atualização de conteúdo estático.

Automação com scripts Bash.

Diferença entre cp (copia tudo) e sync (atualiza apenas o que mudou).


