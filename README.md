# Instalação do ambiente n8n com Docker local

Bem-vindo ao repositório! Este projeto configura os containers de serviços para automação de fluxos de trabalho e integração de APIs usando **n8n**, **Evolution API**, **Waha**, **PostgreSQL**, **PostgresVector**, **Redis** e **Adminer**. Abaixo estão as instruções para instalar e começar a usar todos os serviços.

## Pré-requisitos

Antes de começar, certifique-se de ter os seguintes itens instalados em sua máquina:

- [Docker](https://docs.docker.com/get-docker/)
- Video de instalação do Docker Desktop [Clique-aqui!](https://youtu.be/ViNJ4GOqR4M?si=Mqa4blkZzpwnZ4x5)

## Passo a Passo para Instalação

### WAHA

```bash
docker run -d --name waha -v waha_data:/temp/ -p 3000:3000  devlikeapro/waha:noweb-2025.5.6
```

### REDIS

```bash
docker run -d --name redis -v redis_data:/data -p 6380:6379 redis:8.0.1 
```

### POSTGRES VECTOR

```bash
docker run -d --name postgresvector -p 5433:5432 -e POSTGRES_DB=n8n_data -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -v postgresvector_data:/var/lib/postgresql/data pgvector/pgvector:0.8.0-pg16
```

### POSTGRES

```bash
docker run -d --name postgres -p 5432:5432 -e POSTGRES_DB=n8n_data -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -d -v postgres_data:/var/lib/postgresql/data postgres:17.5
```

### ADMINER

```bash
docker run -d --name adminer -p 8081:8080 -e ADMINER_DEFAULT_SERVER=host.docker.internal adminer
```

### N8N

```bash
docker run -d --name n8n_postgres -p 5678:5678 -e GENERIC_TIMEZONE="America/Sao_Paulo" -e TZ="America/Sao_Paulo" -e DB_TYPE=postgresdb -e DB_POSTGRESDB_DATABASE=n8n_data -e DB_POSTGRESDB_HOST=host.docker.internal -e DB_POSTGRESDB_PORT=5432 -e DB_POSTGRESDB_USER=postgres -e DB_POSTGRESDB_SCHEMA=public -e DB_POSTGRESDB_PASSWORD=postgres -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true -e N8N_RUNNERS_ENABLED=true -v n8n_data:/home/node/.n8n n8nio/n8n:1.94.1
```

### EVOLUTIONAPI

```bash
docker run -d --name evolutionapi -p 8080:8080 -v n8nevolution_instances:/evolution/instances -e AUTHENTICATION_API_KEY=cR3Oq3ePxJ0GW7X9U8gwsqXn1vKYD5xF -e DATABASE_ENABLED=true -e DATABASE_PROVIDER=postgresql -e DATABASE_CONNECTION_URI=postgresql://postgres:postgres@host.docker.internal:5432/evolutionapi_data?schema=public -e DATABASE_CONNECTION_CLIENT_NAME=evolution_exchange -e DATABASE_SAVE_DATA_INSTANCE=true -e DATABASE_SAVE_DATA_NEW_MESSAGE=true -e DATABASE_SAVE_MESSAGE_UPDATE=true -e DATABASE_SAVE_DATA_CONTACTS=true -e DATABASE_SAVE_DATA_CHATS=true -e DATABASE_SAVE_DATA_LABELS=true -e DATABASE_SAVE_DATA_HISTORIC=true -e CACHE_REDIS_ENABLED=true -e CACHE_REDIS_URI=redis://host.docker.internal:6380/6 -e CACHE_REDIS_PREFIX_KEY=evolution -e CACHE_REDIS_SAVE_INSTANCES=false -e CACHE_LOCAL_ENABLED=false atendai/evolution-api:v2.2.3
```

## Notas
- Substitua `SUA_KEY` em `AUTHENTICATION_API_KEY` por uma chave de API segura para o Evolution API.
- Site para gerar key [clique-aqui!](https://acte.ltd/utils/randomkeygen)
- Ajuste `POSTGRES_PASSWORD`, `N8N_BASIC_AUTH_USER` e `N8N_BASIC_AUTH_PASSWORD` para valores seguros de sua escolha.
- Certifique-se de que as portas (3000, 5678, 5432, 5433, 8081, 8080) estejam livres em sua máquina.


## Contribuições

Sinta-se à vontade para abrir issues ou enviar pull requests para melhorias!

## Licença

Este projeto está sob a licença [MIT](LICENSE).
