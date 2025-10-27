# **Sprint 2: Extração de Dados de PDF e Classificação Fiscal Automática**

## Duração
>
>**Início**: 06/10/2025  
>**Fim**: 26/10/20245  
> **Sprint Review**: 31/10/2025  
> **Status**: Encerrada
## Foco

A segunda sprint do projeto NEXA foi concluída com foco na automação da entrada de dados e classificação fiscal. Em conformidade com as especificações alinhadas ao cliente, foi desenvolvida a funcionalidade de extração automática de Part Numbers (PNs) a partir de documentos PDF. Adicionalmente, implementou-se a lógica de classificação automática, que atribui a NCM (Nomenclatura Comum do Mercosul) e a respectiva alíquota de IPI (Imposto sobre Produtos Industrializados) para cada item extraído.

## Backlog

### **Resumo do Backlog**

| Rank | User Story | Prioridade  | Critérios de Aceite
| :--- | :--- | :--- | :--- |
| **1** | Como analista fiscal, quero enviar um PDF com pedidos de compras para que o sistema extraia os Part Numbers automaticamente. | ▲ Alta | - O sistema deve expor um endpoint que aceite o upload de arquivos PDF.<br>- Deve ser capaz de processar o PDF e extrair Part Number (PN), fabricante e modelo.<br>- A resposta da API deve retornar um JSON com a lista de itens extraídos. |
| **2** | Como analista fiscal, quero que o sistema atribua automaticamente NCM e alíquota aos itens extraídos, para acelerar o processo. | ▲ Alta | - O sistema deve identificar e atribuir o código NCM correto para cada item.<br>- A alíquota de imposto correspondente ao NCM deve ser associada ao item.<br>- O resultado da classificação deve ser salvo no banco de dados.<br>- O sistema deve retornar a lista de itens com a classificação fiscal. |
| **3** | Como usuário, quero acessar o status da minha requisição de classificação para saber quando ela foi concluída. | **=** Média | - Ao iniciar um processo assíncrono, a API deve retornar um `task_id`.<br>- O usuário deve poder usar o `task_id` para verificar o andamento da tarefa.<br>- Deve existir um endpoint `GET /api/status/{task_id}` para consultar o status. |

-----

Excelente ideia\! Transformar as listas de tarefas em uma **Definição de Pronto (Definition of Done - DoD)** é uma prática ágil fundamental para garantir que todos tenham o mesmo entendimento do que significa "completo".

A DoD funciona como um checklist de qualidade. Em vez de apenas listar o que fazer, ela descreve o estado em que a funcionalidade deve estar para ser considerada entregue.

Abaixo, transformei suas tarefas em critérios de pronto para cada User Story, apresentados em formato de tabela.

-----

### **DoD Específica | US-1: Extração Automática de Part Numbers de PDF**

| Módulo | Critério de Pronto |
|:---|:---|
| **Backend** | - O endpoint `POST /api/upload-pdf` está funcional, aceitando arquivos PDF e retornando os devidos status HTTP. <br> - A lógica de parsing extrai com sucesso os dados esperados (PN, fabricante, modelo) de um PDF de exemplo. <br> - Os dados extraídos são limpos e normalizados conforme as regras de negócio. <br> - A resposta da API segue o formato JSON definido, contendo os dados extraídos. |
| **Frontend** | - O componente de UI permite que o usuário selecione e envie um arquivo PDF. <br> - A interface exibe feedback visual claro para o usuário (sucesso, erro, carregando) durante e após o upload. |
| **Integração** | - O fluxo completo (upload no frontend -\> processamento no backend -\> resposta) foi testado e está funcionando ponta a ponta. |

-----

### **DoD Específica | US-2: Classificação Fiscal Automática de Itens**

| Módulo | Critério de Pronto |
|:---|:---|
| **Backend** | - O endpoint `POST /api/classificar` está funcional e inicia corretamente o processo de classificação. <br> - A integração com a base de dados TIPI (ou similar) está implementada e retornando os dados fiscais. <br> - O modelo de dados no PostgreSQL foi criado, e a migração foi aplicada com sucesso. <br> - Os resultados da classificação são persistidos corretamente no banco de dados após o processamento. |
| **Frontend** | - A interface exibe uma tabela com os itens processados, mostrando suas descrições e os NCMs atribuídos. |
| **Integração** | - O fluxo completo de receber dados, classificar e salvar no banco foi testado e validado. |

-----

### **DoD Específica | US-3: Acompanhamento do Status da Requisição**

| Módulo | Critério de Pronto |
|:---|:---|
| **Backend** | - O endpoint que inicia a classificação retorna o `task_id` da tarefa assíncrona gerada pelo Celery. <br> - O endpoint `GET /api/status/{task_id}` retorna o estado atual da tarefa (ex: "Processando", "Concluído"). <br> - Quando a tarefa é concluída com sucesso, o endpoint de status também retorna o resultado final da classificação. |
| **Frontend** | - A interface armazena o `task_id` recebido após iniciar a classificação. <br> - A interface consulta periodicamente (polling) o endpoint de status para buscar atualizações. <br> - O progresso da tarefa é exibido visualmente para o usuário. <br> - A tabela de resultados é preenchida e exibida na tela assim que a tarefa é concluída. |
| **Integração** | - O ciclo completo de acompanhamento de status (iniciar tarefa -\> consultar progresso -\> exibir resultado final) foi testado e funciona como esperado. |

## Tarefas



## Entregas Realizadas

Nesta sprint, o foco foi automatizar a entrada de dados e enriquecer as informações para o analista fiscal, reduzindo o trabalho manual e acelerando a tomada de decisão.

### **Funcionalidades Principais**

* **Extração Automática de Dados via PDF:**

  * Implementada a capacidade de o usuário submeter um pedido de compra em formato PDF.
  * O sistema agora processa os documentos e extrai automaticamente as informações essenciais: **Part Number (PN)**, **Fabricante** e **Modelo**. Esta funcionalidade elimina a necessidade de digitação manual, reduzindo erros e otimizando o tempo do analista.

* **Classificação Fiscal Automática:**

  * Para cada Part Number extraído, o sistema agora consulta uma base de dados fiscal (TIPI) para atribuir a **Nomenclatura Comum do Mercosul (NCM)** e a **alíquota de IPI** correspondente.
  * Os resultados são salvos para consultas futuras, evitando reprocessamento e garantindo agilidade.

### **Melhorias na Arquitetura e Backend**

* **Módulo de Processamento de PDF:**

  * Integração de bibliotecas de parsing de PDF (como `pdfplumber`/`PyPDF2`) para a extração robusta de texto e dados estruturados dos documentos.

* **Persistência e Cache de Classificações:**

  * Modelagem e implementação do banco de dados para armazenar os itens processados, incluindo PN, descrição enriquecida, NCM, alíquota e data da classificação.
  * Esta estrutura serve como uma base de conhecimento que agiliza futuras classificações para itens já processados.

* **Evolução do Agente de IA (RAG):**

  * Iniciada a implementação da arquitetura **Retrieval-Augmented Generation (RAG)**. O agente de IA agora pode consultar a base de dados de classificações já realizadas para melhorar a precisão e o contexto ao gerar descrições e sugerir classificações fiscais.

* **Novos Endpoints da API:**

  * `/upload-pdf`: Endpoint REST para o recebimento e processamento inicial dos arquivos PDF.
  * `/classificacao`: Endpoint que, a partir de um Part Number, aciona o fluxo de enriquecimento e classificação fiscal.

### **Frontend**

* **Tela de Upload de Documentos:**

  * Desenvolvimento de uma nova interface que permite ao usuário arrastar e soltar (`drag-and-drop`) ou selecionar arquivos PDF para envio.

* **Visualização de Resultados:**

  * Criação de uma tela que exibe os resultados da extração e classificação em um formato  claro e organizado, apresentando o Part Number, a descrição, a NCM e a alíquota de cada item.

### Demonstração

* **Figma:**

  * O protótipo visual foi atualizado para incluir as novas telas de upload de PDF e a tabela de resultados da classificação.
  * [Protótipo Visual com Telas de Upload e Classificação](https://www.figma.com/design/vgEGyLsrUEILCN7W8UYE6t/Nexa-by-Titus-Systems?node-id=0-1&p=f&t=LcNUCiWVZ2RGiUmG-0)

* **YouTube:**

  * Um vídeo demonstrando o novo fluxo de trabalho, desde o upload do PDF até a exibição dos itens classificados.
  * [Vídeo Demonstração da Entrega da Segunda Sprint](https://youtu.be/kPAlwLgL88o)
