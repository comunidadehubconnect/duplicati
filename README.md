<p align="center">
<img src="https://cwmkt.com.br/wp-content/uploads/2024/04/logo_github.png" width="240" />
<p align="center">Seja bem-vindo ao Guia de Instalação Duplicati 🚀</p>
</p>

<p align="center"> 
<a href="https://hubconnect.top" target="_blank">👉 Participe da Comunidade HubConnect 👈</a>
</p>

<hr />
<hr />

## Comando para Instalar o Docker e Docker Compose

Antes de instalar o Duplicati, é necessário que o Docker e o Docker Compose estejam instalados em sua máquina. Caso ainda não tenha essas ferramentas, siga os passos abaixo:

### Instalar o Docker

1. Atualize os pacotes existentes:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
   
2. Instale pacotes de dependências:

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```

Adicione a chave GPG oficial do Docker:

   ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

3. Adicione o repositório Docker:

   ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

## Instale o Docker:

   ```bash
    sudo apt update
    sudo apt install docker-ce -y
   ```

## Verifique a instalação:

   ```bash
    docker --version
   ```

## Instalar o Docker Compose
1. Baixe a versão estável mais recente do Docker Compose:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. Dê permissão de execução ao Docker Compose:

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

3. Verifique a instalação:

```bash
docker-compose --version
```

## Configuração do Duplicati com Docker Compose
Após a instalação do Docker e Docker Compose, você pode usar o seguinte arquivo docker-compose.yml para configurar o Duplicati:

```bash
services:
  duplicati:
    image: lscr.io/linuxserver/duplicati:latest
    environment:
      - PUID=0
      - PGID=0
      - TZ=America/Sao_Paulo
    volumes:
      - /mnt/storage/docker/duplicati/appdata/config:/config
      - /mnt/storage/docker/duplicati/backups:/backups
      - /mnt/storage/docker/duplicati/source:/source
    ports:
      - 8200:8200
    restart: unless-stopped
```

### Passos para Instalação:
1. Crie um diretório para armazenar o docker-compose.yml:

```bash
mkdir -p /mnt/storage/docker/duplicati
```
```bash
cd /mnt/storage/docker/duplicati
```

2.Crie o arquivo docker-compose.yml e cole o conteúdo acima.
Execute o comando para iniciar o serviço:

```bash
docker-compose up -d
```

Acesse o Duplicati no navegador através do endereço:

```bash
http://<seu-ip>:8200
```
![image](https://github.com/user-attachments/assets/ed603444-8f6a-4da1-ac76-6336560e6b93)

Ao acessar o duplicati pela primeira vez, será solicitado que você crie uma senha, ou deixe sem, caso você esteja numa rede interna sem está exposta para a internet.
![image](https://github.com/user-attachments/assets/c59084ae-bf0e-452a-9f15-7de646c33632)

## Configuração de Proxy Reverso com Nginx
Se você deseja configurar um proxy reverso para o Duplicati usando o Nginx, pode utilizar a seguinte configuração:

```bash
server {
    listen 80;
    server_name duplicati.seudominio.com;

    location / {
        proxy_pass http://localhost:8200;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Certifique-se de ter configurado corretamente seu DNS para apontar para o servidor que está rodando o Duplicati.

**Pronto tudo Funcionando** ✅😎
