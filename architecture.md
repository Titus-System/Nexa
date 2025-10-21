# Arquitetura do Sistema Nexa

## Visão Geral

O sistema Nexa é composto por quatro grandes módulos principais:

- **Nexa-frontend**: Interface web moderna, desenvolvida em React + TypeScript, responsável pela interação com o usuário.
- **Nexa-api**: Backend robusto em Python (Flask), que orquestra requisições, processamento assíncrono, persistência de dados e comunicação em tempo real.
- **Nexa-AI-Agents**: Serviço especializado para execução de tarefas de classificação com IA, também em Python, que utiliza uma base vetorial para enriquecimento de contexto.
- **Banco de Dados**: O sistema utiliza duas soluções de persistência:
  - **Banco Relacional (PostgreSQL)**: Armazena os resultados finais das classificações, servindo como a fonte da verdade e cache de resultados.
  - **Banco Vetorial (ChromaDB)**: Dedicado à implementação de RAG, armazenando embeddings para busca semântica.

A comunicação entre os módulos é feita por HTTP REST, WebSocket (Socket.IO) e Redis Pub/Sub, garantindo flexibilidade, baixa latência e desacoplamento.

-----

## Diagrama

```uml
@startuml
title Diagrama de Arquitetura do Sistema Nexa (Layout Melhorado)

top to bottom direction

actor Usuário

package "Nexa-frontend" {
  component "React + TypeScript" as frontend
}

    package "Nexa-api" {
        component "API Server (Flask)" as api_server
        component "Workers (Celery)" as celery_workers
        database "PostgreSQL" as db_postgres #LightSkyBlue
    }

    queue "Fila de Tarefas\n(Celery Queue)" as celery_queue #LightSalmon
    database "Redis\n(Broker & Pub/Sub)" as redis #LightSkyBlue

    package "Nexa-AI-Agents" {
        component "Serviço de IA (Flask)" as ai_service
        database "ChromaDB" as db_chroma #LightSkyBlue
    }



' Conexões e Fluxo de Dados

' 1. Fluxo Principal do Usuário
Usuário --> frontend : "1. Interage com a UI"
frontend --> api_server : "2. Inicia Classificação\n(HTTP REST)"
api_server --> celery_queue : "3. Enfileira Tarefa Assíncrona"
celery_workers <-- celery_queue : "4. Consome a Tarefa"
celery_workers --> ai_service : "5. Delega Processamento Pesado\n(HTTP REST)"

' 6. Fluxo de Progresso via Pub/Sub (assíncrono)
ai_service .> redis : "6a. Publica Progresso"
redis .> celery_workers : "6b. Escuta o Progresso"

' 7. Atualização para o Usuário
api_server --> frontend : "7. Envia Atualizações\n(WebSocket)"


' Conexões com os Bancos de Dados
api_server ..> db_postgres : "Lê/Escreve dados"
celery_workers ..> db_postgres : "Persiste resultados"
ai_service ..> db_chroma : "Salva/Busca embeddings"

@enduml
```

![Diagrama de Arquitetura](/docs/arquitetura_sprint_2.svg)
-----

## Nexa-frontend

- **Tecnologias**: React, TypeScript, Vite, Socket.IO-client.
- **Responsabilidades**:
  - **Interface de Upload**: Permite ao usuário submeter um ou mais documentos PDF.
  - **Entrada Manual**: Mantém o formulário para submissão manual de partnumbers.
  - **Comunicação com API**: Consome a API REST para iniciar tanto o processamento de PDFs (`/upload-pdf`) quanto classificações individuais.
  - **Feedback em Tempo Real**: Conecta-se via WebSocket para receber atualizações de progresso e o resultado final.
  - **Exibição de Resultados**: Apresenta os dados extraídos e classificados em uma tabela organizada.

-----

## Nexa-api (Backend)

- **Tecnologias**: Python 3.11+, Flask, Flask-RESTful, Flask-SocketIO, Celery, Redis, Pydantic, Docker, **SQLAlchemy**, **pdfplumber**.
- **Responsabilidades**:
  - **Gestão de Endpoints**:
    - `/upload-pdf`: Recebe arquivos PDF e inicia tarefas assíncronas de processamento.
    - `/classify-partnumber`: Recebe requisições para classificação de um item individual.
  - **Orquestração de Tarefas**: Utiliza Celery para delegar tarefas pesadas, mantendo a API responsiva.
  - **Persistência de Dados**: Gerencia a conexão com o banco de dados relacional. Salva todos os resultados de classificação (PN, descrição, NCM, alíquota) e os consulta para evitar reprocessamento.
  - **Comunicação em Tempo Real**: Orquestra a comunicação com o frontend via WebSocket, retransmitindo o progresso recebido dos workers via Redis Pub/Sub.
  - **Módulo de Extração**: Contém a lógica para processar o texto extraído de PDFs.

-----

## Nexa-AI-Agents

- **Tecnologias**: Python 3.10+, Flask, Pydantic, Redis, Ollama, smolagents, **ChromaDB**.
- **Responsabilidades**:
  - Recebe requisições de classificação do backend via REST.
  - **Enriquecimento com RAG (Retrieval-Augmented Generation)**: Utiliza um banco de dados vetorial, **ChromaDB**, para a implementação de RAG. Antes de acionar o modelo de linguagem, ele busca semanticamente por classificações similares no ChromaDB para enriquecer o contexto do prompt, aumentando drasticamente a precisão.
  - Executa o processamento de IA para definir a descrição técnica, NCM e alíquota.
  - Publica mensagens de progresso e o resultado final no canal Redis informado na requisição.

-----

## Fluxos de Requisição Detalhados

### Fluxo 1: Processamento de PDF

1. O **Usuário** faz o upload de um PDF pelo **Frontend**.
2. O **Frontend** envia o arquivo via POST para o endpoint `/upload-pdf` da **API**.
3. A **API** valida, inicia uma tarefa no **Celery** para o processamento do PDF e responde ao Frontend, que se conecta via **WebSocket**.
4. Um **Worker** consome a tarefa, extrai o texto e identifica os Part Numbers.
5. Para cada Part Number, o worker cria **sub-tarefas de classificação** no Celery.
6. Outros **Workers** consomem as sub-tarefas e chamam o serviço de **IA (Agents)** via REST.
7. Os **Agents** utilizam **RAG** (consultando o **ChromaDB** para buscar informações contextuais) e publicam o progresso no **Redis (Pub/Sub)**.
8. A **API**, inscrita no canal, retransmite o progresso em tempo real para o **Frontend** via **WebSocket**.
9. Ao final de cada classificação, o worker salva o resultado no **Banco de Dados Relacional**.

### Fluxo 2: Classificação Manual (Original)

1. O **Usuário** envia um Part Number pelo formulário no **Frontend**.
2. O fluxo segue a partir do **passo 6** do processamento de PDF, focando em um único item.

-----

## Tecnologias e Justificativas

- **Flask + Flask-RESTful + Flask-SocketIO**: Permite API REST e WebSocket no mesmo backend, simplificando a infraestrutura.
- **Celery + Redis**: Gerencia tarefas assíncronas e Pub/Sub, desacoplando o processamento pesado do fluxo principal.
- **SQLAlchemy**: ORM que abstrai o acesso ao banco de dados relacional.
- **ChromaDB**: Banco de dados vetorial open-source utilizado para armazenar embeddings e permitir buscas semânticas de baixa latência. É a peça central da implementação de RAG.
- **pdfplumber / PyPDF2**: Bibliotecas especializadas para extração de dados de arquivos PDF.
- **Pydantic**: Garante validação e tipagem robusta dos dados em trânsito.
- **React + Vite**: Interface moderna, responsiva e de fácil manutenção.
- **Docker**: Facilita o deploy e a padronização de ambientes.

-----

## Pontos Fortes da Arquitetura

- **Desacoplamento**: Módulos de IA, parsing de PDF e API são independentes.
- **Baixa latência para o usuário**: Feedback em tempo real via WebSocket.
- **Resiliência**: Falhas no processamento não bloqueiam a API.
- **Construção de Conhecimento**: O banco relacional armazena os fatos (resultados), enquanto o **ChromaDB** armazena o conhecimento semântico para a IA (RAG), tornando o sistema mais preciso e inteligente com o uso.
- **Evolução Facilitada**: Componentes bem definidos permitem a adição de novas funcionalidades com menor impacto.

-----

## Resumo

O sistema Nexa adota uma arquitetura moderna, orientada a eventos e altamente desacoplada. A utilização de bancos de dados distintos para dados factuais (Relacional) e conhecimento semântico (ChromaDB) representa uma abordagem sofisticada e eficiente. Isso permite automatizar a extração de dados e construir uma base de conhecimento robusta, mantendo o foco em experiência do usuário, confiabilidade e integração transparente com IA de ponta.
