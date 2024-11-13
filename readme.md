# Projeto: Ambiente de Dados com Dremio, MinIO, Nessie e OpenTelemetry

Este projeto utiliza o Docker Compose para criar um ambiente de dados com as seguintes tecnologias:
- **Dremio**: plataforma de virtualização e análise de dados.
- **MinIO**: servidor de armazenamento de objetos compatível com S3.
- **Nessie**: sistema de controle de versões para lakes de dados.
- **OpenTelemetry Collector**: coletor de dados para monitoramento e rastreamento.

## Configuração de Rede

Todos os serviços estão conectados a uma rede Docker chamada `iceberg_env`.

## Pré-requisitos

- Docker (versão 20.10+)
- Docker Compose (versão 1.29+)

## Estrutura de Serviços

### 1. Dremio
- **Imagem**: `dremio/dremio-oss:latest`
- **Portas**:
  - `9047`: interface de administração
  - `31010` e `32010`: portas de execução
- **Volume**:
  - `./data:/opt/dremio/data`: permite persistência de dados.
- **Depende de**:
  - `minioserver` e `nessie`

### 2. MinIO
- **Imagem**: `minio/minio`
- **Portas**:
  - `9000`: API principal
  - `9001`: console web
- **Ambiente**:
  - `MINIO_ROOT_USER`: nome de usuário (definido como `minio`)
  - `MINIO_ROOT_PASSWORD`: senha do usuário root
- **Comando**:
  - Inicia o MinIO com o console na porta `9001` e o diretório de dados em `/data`

### 3. Nessie
- **Imagem**: `projectnessie/nessie`
- **Porta**:
  - `19120`: interface para versionamento de dados.

### 4. OpenTelemetry Collector (OTLP Collector)
- **Imagem**: `otel/opentelemetry-collector:latest`
- **Portas**:
  - `4317` e `55681`: portas para coleta de dados
- **Ambiente**:
  - `OTEL_COLLECTOR_ZIPKIN_HTTP_PORT`: porta Zipkin
  - `OTEL_COLLECTOR_ZIPKIN_HTTP_ENDPOINT`: endpoint Zipkin

## Executando o Projeto

1. Clone o repositório para sua máquina local.
2. No diretório do projeto, execute o comando:
   ```bash
   docker-compose up -d
3. Acesse cada serviço por meio das portas mapeadas:
- Dremio: http://localhost:9047
- MinIO: http://localhost:9001
- Nessie: http://localhost:19120

## Notas

O diretório ./data é montado no contêiner do Dremio para persistir os dados.

A senha 2'zrFR51EgT[ é utilizada para o usuário root do MinIO. Lembre-se de alterá-la para um valor seguro em ambientes de produção.