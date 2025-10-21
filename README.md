# NEXA

**Agente de Inteligência Artificial para instrução de Processo para Registro de Importação**

![logo](docs/logo.png)

> Status do Projeto: Em andamento ...

## Desafio

Desenvolvimento de um agente de Inteligência Artificial capaz de elaborar a instrução de registro aduaneiro com as informações pertinentes do material que relacione: Part-Number, classificação fiscal, fabricante, origem do fabricante com endereço completo, gerando a informação da descrição do material, de forma que permita a receita federal entender o que é o produto e não gere dúvidas sobre o item o e não acarrete penalidades e/ou multas sobre o material declarado.

## ⚡ Desenvolvimento Ágil

O projeto foi feito seguindo o método Ágil SCRUM, dividindo o trabalho em sprints de 21 dias, com reuniões diáras, revisões e retrospectivas ao final.

### 📋 Backlog do Produto

| Sprint | User Story | Status | Prioridade |
| :---  | :--- | :--- | :--- |
| **1** | Como analista fiscal, quero enviar um Partnumber ao sistema para receber informações detalhadas do produto. | CONCLUÍDA | ▲ Alta |
| **2** | Como analista fiscal, quero enviar um PDF com pedidos de compras para que o sistema extraia os Part Numbers automaticamente... | EM ANDAMENTO | ▲ Alta |
| **2** | Como analista fiscal, quero que o sistema atribua automaticamente NCM e alíquota aos itens extraídos, para acelerar o processo de importação. | EM ANDAMENTO | ▲ Alta |
| **2** | Como usuário, quero acessar o status da minha requisição de classificação | EM ANDAMENTO | **=** Média |
| **3** | Como usuário, quero autenticar no sistema para acessar minhas operações com segurança. | A FAZER | ▲ Alta |
| **3** | Como usuário autenticado, quero exportar minhas operações para Excel, para usá-las no registro oficial de importação. | A FAZER | **=** Média |
| **3** | Como usuário autenticado, quero acessar o histórico de minhas operações para reaproveitar informações em importações futuras. | A FAZER | **=** Média |

### 📅 Cronograma

| Sprint            | Prazo      | Status       | Documentação           | Entrega |
| ----------------- | ---------- | ------------ | ---------------------- | ------- |
| Kick Off          | 25/08/2025 | Concluído    | -                      | -       |
| Sprint 1          | 28/09/2025 | Concluído    | [sprint1](sprint_1.md) | [video](https://youtu.be/jFSbepQdjow)       |
| Sprint 2          | 26/10/2025 | Em andamento | [sprint2](sprint_2.md) | [video](https://youtu.be/jFSbepQdjow)       |
| Sprint 3          | 23/11/2025 | Pendente     | [sprint3](sprint_3.md) | -       |
| Feira de Soluções | 04/12/2025 | Pendente     | [feira](feira_sol.md)  | -       |

### Roadmap

![roadmap](docs/roadmap.png)

### 👥 Fatec São José dos Campos - Prof. Jessen Vidal

| Cliente          | Período/Curso                                  | Professor M2      | Professora P2     | Contato Cliente                    |
| ---------------- | ---------------------------------------------- | ----------------- | ---------------- | ---------------------------------- |
| Creonice Honório - Empresa TecSys | 4º Análise e Desenvolvimento de Sistemas | Giuliano Bertoti  | Juliana Pasquini | <creonice@tecsysbrasil.com.br> |

## Arquitetura

Arquitetura orientada a eventos e altamente desacoplada, mantendo foco em experiência do usuário, confiabilidade e integração transparente com IA.  

![arquitetura](docs/arquitetura_sprint_2.svg)
Para detalhes da implementação: [Documento da Arquitetura](architecture.md)

## 🛠️ Tecnologias Utilizadas

<p align="center">
  <img alt="Python" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg">
  <img alt="Flask" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/flask/flask-original.svg">
  <img alt="Pytest" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/pytest/pytest-original.svg" />
  <img alt="redis" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/redis/redis-original.svg" />
  <img alt="Smolagents" height="30" width="30" src="https://cdn-avatars.huggingface.co/v1/production/uploads/63d10d4e8eaa4831005e92b5/a3R8vs2eGE578q4LEpaHB.png">
  <img alt="Ollama" height="30" width="30" src="https://ollama.com/public/ollama.png">
  <img alt="docker" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original.svg" />
  <img alt="PostgreSQL" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/postgresql/postgresql-original.svg">
  <img alt="SQLAlchemy" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/sqlalchemy/sqlalchemy-original.svg" />

</p>

<p align="center">
  <img alt="socketio" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/socketio/socketio-original.svg" />
  <img alt="Node" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/nodejs/nodejs-original.svg">
  <img alt="Typescript" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/typescript/typescript-original.svg" />
  <img alt="React" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/react/react-original-wordmark.svg" />
  <img alt="Vite" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vitejs/vitejs-original.svg" />
  <img alt="HTML" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/html5/html5-original.svg">
  <img alt="Tailwind" height="30" width="40"src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/tailwindcss/tailwindcss-original.svg">
</p>

<p align="center">
  <img alt="Git" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/git/git-original.svg">
  <img alt="Git" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/github/github-original.svg">
  <img alt="Figma" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/figma/figma-original.svg">
  <img alt="Jira" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/jira/jira-original.svg">
</p>

## Manual de Instalação e Execução

### 1. Pré-requisitos

- Python 3.11+ (para backend e IA)
- Servidor Ollama rodando o modelo `qwen2.5:14b`
- Node.js 18+ e npm (para frontend)
- Redis (pode ser local ou via Docker)
- Docker e Docker Compose (opcional, para facilitar a execução)

---

### 2. Clonando o Repositório

```bash
git clone https://github.com/Titus-System/Nexa.git
cd Nexa
```

---

### 3. Backend (Nexa-api)

```bash
cd Nexa-api
python -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
cp .env.example .env
# Edite o .env conforme necessário
python run.py
```

**Ou Usando Docker Compose (opcional)**

```bash
cd Nexa-api
docker compose up -d --build
```

O backend estará disponível em [http://localhost:5000](http://localhost:5000).

---

### 4. IA/Agentes (Nexa-AI-Agents)

```bash
cd ../Nexa-AI-Agents
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
# Edite o .env conforme necessário
python run.py
```

O agente estará disponível em [http://localhost:5001](http://localhost:5000).

---

### 5. Frontend (Nexa-frontend)

```bash
cd ../Nexa-frontend
npm install
npm run dev
```

O frontend estará disponível em [http://localhost:5173](http://localhost:5173).

---

### 7. Observações

- Certifique-se de que o Redis está rodando (`localhost:6379` por padrão).
- Ajuste as variáveis de ambiente nos arquivos `.env` de cada módulo conforme necessário.
- Para produção, utilize Gunicorn no backend e configure variáveis de ambiente seguras.

## 🎓 Equipe <a id="equipe"></a>

<div>
  <table>
    <tr>
      <th>Membro</th>
      <th>Função</th>
      <th>Github</th>
      <th>Linkedin</th>
    </tr>
    <tr>
      <td>Pedro Garcia</td>
      <td>Product Owner</td>
      <td>
        <a href="https://github.com/pedro-fs-garcia">
          <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white">
        </a>
      </td>
      <td>
        <a href="https://www.linkedin.com/in/pedro-fs-garcia">
            <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white">
        </a>
      </td>
    </tr>
    <tr>
      <td>Julia Soares</td>
      <td>Scrum Master</td>
      <td>
        <a href="https://github.com/juliasoares17">
          <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white">
        </a>
      </td>
      <td>
        <a href="www.linkedin.com/in/julia-soares-pereira-9ab79830b">
            <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white">
        </a>
      </td>
    </tr>
    <tr>
      <td>Eduardo Ribeiro</td>
      <td>Dev Team</td>
      <td>
        <a href="https://github.com/eduardo-Rib">
          <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white">
        </a>
      </td>
      <td>
        <a href="https://www.linkedin.com/in/eduardo-ribeiro-4b78002b2">
            <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white">
        </a>
      </td>
    </tr>
    <tr>
      <td>Wesley Gonçalves</td>
      <td>Dev Team</td>
      <td>
        <a href="https://github.com/WesleyGoncalves">
          <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white">
        </a>
      </td>
      <td>
        <a href="https://www.linkedin.com/in/wesley-d-goncalves/">
            <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white">
        </a>
      </td>
    </tr>
  </table>
</div>
