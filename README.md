# Zammad com Docker + Nginx + SSL (Renovação Automática)
![Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)
![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white)

Implementação do Zammad usando Docker Compose com proxy reverso Nginx e certificado SSL Let's Encrypt automatica.

## Pré-requisitos

- Ubuntu 22.04 LTS
- Docker e Docker Compose instalados
- Domínio apontando para o IP do servidor

## Configuração

1. **Clone o repositório**

```bash
   git clone https://github.com/Paulohco47/Deploy-do-Zammad-via-Docker.git
   cd Deploy-do-Zammad-via-docker

```

2. **Configure as variáveis de ambiente**

```bash
   cp .env.example .env
   nano .env
```

3. **Gere uma nova SECRET_KEY_BASE com:**

```bash
   openssl rand -hex 64
```

4. **Crie os diretórios necessários**

```bash
   mkdir -p data/{letsencrypt,webroot}
```

5. **Inicie os containers**

```bash
   docker-compose up -d
```

6. **Gere o certificado SSL**

```bash
   docker-compose run --rm certbot
```

7. **Reinicie o Nginx**

```bash
   docker-compose restart nginx
```

8. **Acesse o Zammad**

   Abra o navegador e acesse: [https://atendimento.seudominio.com.br](https://atendimento.seudominio.com.br)

   Siga o setup inicial do Zammad conforme as instruções na interface.

## Manutenção

Renovar certificado

1. **Adicione ao seu crontab**

```bash
  crontab -e
```

2. **Renovação Automática às 00h**

```bash
  0 0 * * * docker-compose run --rm certbot renew && docker-compose restart nginx
```

3. **Atualizar containers**

```bash
   docker-compose pull
   docker-compose up -d
```

## Backup

1. **Backup do diretório data e volumes Docker**

```bash
   tar -czvf backup-zammad.tar.gz data
   docker save $(docker images -q) -o docker-images-backup.tar
```

## Troubleshooting

1. **Verifique os logs**

```bash
docker-compose logs -f nginx
docker-compose logs -f zammad
