# **Sprint 3: Autenticação e consultas**

## Duração
>
>**Início**: 03/11/2025  
>**Fim**: 23/11/2025  
> **Sprint Review**: 24/11/2025  
> **Status**: Encerrada

## Foco

A terceira sprint foi concluída com foco em implementar um sistema de autenticação seguro e fornecer aos usuários autenticados as ferramentas para acessar, filtrar, e exportar seu histórico de operações. Também foram implementadas melhorias de performance dos agentes, como refino da implementação da arquitetura **Retrieval-Augmented Generation (RAG)**, busca mais eficiente na web por informações do partnumber e diferentes modelos de pdf suportados.

## Backlog

### **Resumo do Backlog**

| Rank | User Story | Prioridade | Critérios de Aceite |
| :--- | :--- | :--- | :--- |
| **1** | Como usuário, quero que o sistema suporte invoices de compra de diferentes fornecedores e extraia informações dos partnumbers | **▲** Alta | Extração de PN, Fabricante e Modelo de diferentes layouts de invoices (Testado em dois fornecedores). Integração do novo *scraper* com o fluxo de classificação. |
| **2** | Como usuário, quero autenticar no sistema para acessar minhas operações com segurança. | **▲** Alta | Registro e Login com **JWT**. Middleware de validação de token aplicado. Rotas de histórico e exportação protegidas por autenticação. |
| **3** | Como usuário autenticado, quero exportar minhas operações para Excel, para usá-las no registro oficial de importação. | **▲** Alta | Arquivo gerado com PN, descrição ERP, descrição técnica, NCM, fabricante, endereço e país. |
| **4** | Como usuário autenticado, quero acessar o histórico de minhas operações para reaproveitar informações em importações futuras. | **=** Média | Listagem por usuário autenticado. Filtros por PN, fabricante e data. |
| **5** | Como usuário, quero consultar informações por partnumber, NCM e fabricante para visualizar informações já existentes | **=** Média | Clique no NCM abre modal com descrição completa, alíquotas e exceções da TIPI (dados detalhados). |

-----

### **DoD Específica | US-1: Suporte a Invoices e Extração de Dados**

| Módulo | Critério de Pronto |
|:---|:---|
| **Backend** | O fluxo principal de classificação foi atualizado para considerar o tratamento de diferentes tipos de pdf de acordo com o fornecedor. |
| **Frontend** | O componente de upload de PDF inclui um campo de fornecedor no formulário de upload. |
| **Integração** | O Agente de IA recebe os dados já existentes extraídos do PDF. |

-----

### **DoD Específica | US-2: Autenticação de Usuário (JWT)**

| Módulo | Critério de Pronto |
|:---|:---|
| **Backend** | Endpoint `POST /register` e `POST /login` funcionais e testados. O **Middleware de validação de JWT** está aplicado em todas as rotas restritas. |
| **Frontend** | Telas de Login e Registro criadas. O **token JWT** é salvo no navegador (`localStorage` ou similar) em caso de login bem-sucedido. |
| **Integração** | Tentar acessar rotas como `/historico` sem autenticação resulta em consulta genérica do sistema a classificações sem usuário atribuído. Novas operações de classificação são persistidas no banco com o **`user_id` correto** (chave estrangeira). |

-----

### **DoD Específica | US-3: Exportação de Operações**

| Módulo | Critério de Pronto |
|:---|:---|
| **Backend** | O endpoint `GET /exportar-excel` está protegido, aceita filtros e retorna um arquivo `.xlsx` com o `Content-Type` e `filename` corretos. |
| **Frontend** | Botão "Exportar para Excel" está visível na tela de Histórico. O botão passa a classificação selecionada para a api exportar o arquivo e o download inicia automaticamente. |
| **Integração** | O arquivo exportado (`.xlsx`) pode ser aberto sem erros em um software de planilhas. |

### **DoD Específica | US-4: Consulta de operações**

| Módulo | Critério de Pronto |
|:---|:---|
| **Backend** | Filtragem de operações realizadas por usuário `user_id` |
| **Frontend** | Implementação de login realizada e envio da requisição de histórico com token de acesso. |

### **DoD Específica | US-4: Filtragem por critérios**

| Módulo | Critério de Pronto |
|:---|:---|
| **Backend** | Filtragem de operações realizadas por atributos de classificação, como NCM, Partnumber, fabricante, etc |
| **Frontend** | Implementação de lógica e componentes de UI para filtragem de classificações existentes. |

## Entregas Realizadas

### **Funcionalidades Principais**

* **Sistema de Autenticação JWT**

  * **Descrição:** Implementação completa dos fluxos de registro, login e logout. Todas as rotas sensíveis do backend estão protegidas por um middleware de validação de JWT, garantindo que as operações sejam associadas e restritas ao usuário logado.

* **Histórico de Operações Personalizado**

  * **Descrição:** Criado o endpoint `/historico` que retorna as classificações de forma paginada e exclusiva para o usuário autenticado. A funcionalidade é complementada por filtros avançados de **Part Number**, **Fabricante** e **Intervalo de Data**, permitindo consultas eficientes.

* **Exportação para Excel**

  * **Descrição:** Adicionada a capacidade de exportar o histórico de operações para o formato Excel (`.xlsx`). O arquivo gerado segue o layout de colunas necessário para registro oficial de importação e é nomeado automaticamente (`PO_usuario_data.xlsx`).

* **Refino da IA e Busca de Dados**

  * **Descrição:** O sistema agora realiza **Web Scrapping** automatizado para enriquecer os dados do Part Number antes da classificação. O motor **RAG** foi aprimorado para incluir e priorizar o campo **"Exceção da TIPI"** na busca vetorial, aumentando a precisão das sugestões de NCM.

* **Consulta Detalhada de NCM**

  * **Descrição:** Os códigos NCM nas tabelas são interativos. Ao clicar, um modal é exibido com detalhes completos do item na TIPI (descrição, alíquotas, exceções), permitindo ao analista validar a informação sem sair do sistema.

### Demonstração

* **YouTube:**

  * Um vídeo demonstrando o novo fluxo de trabalho, desde o upload do PDF até a exibição dos itens classificados.
  * [Vídeo Demonstração da Entrega da Segunda Sprint](https://youtu.be/kPAlwLgL88o)
