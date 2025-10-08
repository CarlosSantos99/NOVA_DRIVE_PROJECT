# Data Warehouse para Montadora de Veículos 🚗

Este projeto apresenta a construção completa de um **Data Warehouse** para uma **montadora de veículos fictícia**, com foco em aplicar práticas reais de **engenharia de dados**.  
A empresa simulada possui um **site, sistema e banco de dados “vivo”**, onde novas vendas são geradas em tempo real — o que permite criar pipelines dinâmicos e próximos de um cenário real de produção.

O pipeline integra **PostgreSQL**, **Airflow (na AWS EC2)**, **Snowflake**, **dbt** e **Power BI**, formando uma arquitetura moderna e escalável de dados.

---

## 📚 Objetivos do projeto

- Construir um **Data Warehouse na nuvem** para consolidar dados de vendas automotivas.  
- Demonstrar uma arquitetura completa de **ETL/ELT**, desde o banco transacional até o dashboard analítico.  
- Aplicar ferramentas amplamente utilizadas no mercado: **PostgreSQL, Airflow, dbt, Snowflake e Power BI**.  
- Implementar o pipeline em um ambiente **virtualizado na AWS EC2**, simulando uma infraestrutura corporativa real.  
- Criar dashboards interativos para análise de vendas no **Power BI**.

---

## 🧱 Arquitetura do projeto

A arquitetura segue o fluxo:

**PostgreSQL → Airflow (EC2) → Snowflake → dbt → Looker Studio**

- O **PostgreSQL** armazena os dados de origem (vendas, clientes, veículos etc).  
- O **Airflow**, hospedado em uma **instância AWS EC2**, orquestra a extração, transformação e carga dos dados.  
- O **Snowflake** atua como **Data Warehouse**, armazenando os dados tratados e estruturados.  
- O **dbt** executa as transformações dentro do Snowflake, aplicando boas práticas de modelagem e versionamento.  
- O **Power BI** apresenta os resultados em **dashboards interativos**.


## UML

- Dados em tabela unica Postgres:

id_estados
estado
sigla
id_cidades
nome_cidade
id_concessionarias
nome_concessionaria
id_clientes
cliente
endereco
id_vendedores
nome_vendedor
id_veiculos
nome
tipo
valor
id_vendas
valor_venda
data_inclusao
data_atualizacao
data_venda

- Dados em Dimenções e Fatos Snowflake:

DIM_ESTADOS:
id_estados (PK)
estado
sigla
data_inclusao
data_atualizacao

DIM_CIDADES:
id_cidades (PK)
nome_cidade
id_estados (FK para DIM_ESTADOS)
data_inclusao
data_atualizacao

DIM_CONCESSIONARIAS:
id_concessionarias (PK)
nome_concessionaria
id_cidades (FK para DIM_CIDADES)
data_inclusao
data_atualizacao

DIM_CLIENTES:
id_clientes (PK)
cliente
endereco
id_concessionarias (FK para DIM_CONCESSIONARIAS)
data_inclusao
data_atualizacao

DIM_VENDEDORES:
id_vendedores (PK)
nome_vendedor
id_concessionarias (FK para DIM_CONCESSIONARIAS)
data_inclusao
data_atualizacao

DIM_VEICULOS:
id_veiculos (PK)
nome
tipo
valor
data_inclusao
data_atualizacao

FCT_VENDAS:
id_vendas (PK)
id_veiculos (FK para DIM_VEICULOS)
id_concessionarias (FK para DIM_CONCESSIONARIAS)
id_vendedores (FK para DIM_VENDEDORES)
id_clientes (FK para DIM_CLIENTES)
valor_venda
data_venda
data_inclusao
data_atualizacao

- UML do projeto:

+-----------------+
|   DIM_ESTADOS   |
+-----------------+
| id_estados (PK) |
| estado          |
| sigla           |
| data_inclusao   |
| data_atualizacao|
+-----------------+

           1
           |
           | 
           * 
+-----------------+
|   DIM_CIDADES   |
+-----------------+
| id_cidades (PK) |
| nome_cidade     |
| id_estados (FK) |
| data_inclusao   |
| data_atualizacao|
+-----------------+

           1
           |
           |
           *
+-------------------------+
|   DIM_CONCESSIONARIAS   |
+-------------------------+
| id_concessionarias (PK) |
| nome_concessionaria     |
| id_cidades (FK)         |
| data_inclusao           |
| data_atualizacao        |
+-------------------------+

        1          1
        |          |
        |          |
        *          *
+-----------------+        +-----------------+
|   DIM_CLIENTES  |        |  DIM_VENDEDORES |
+-----------------+        +-----------------+
| id_clientes(PK) |        | id_vendedores(PK)|
| cliente         |        | nome_vendedor    |
| endereco        |        | id_concessionarias(FK)|
| id_concessionarias(FK)|   | data_inclusao   |
| data_inclusao   |        | data_atualizacao|
| data_atualizacao|        +-----------------+
+-----------------+

+-----------------+
|   DIM_VEICULOS  |
+-----------------+
| id_veiculos(PK) |
| nome            |
| tipo            |
| valor           |
| data_inclusao   |
| data_atualizacao|
+-----------------+

                *
                |
                1
+-----------------+
|   FCT_VENDAS    |
+-----------------+
| id_vendas (PK)  |
| id_veiculos(FK) |
| id_concessionarias(FK) |
| id_vendedores(FK) |
| id_clientes(FK) |
| valor_venda     |
| data_venda      |
| data_inclusao   |
| data_atualizacao|
+-----------------+


## 📝 Seções do projeto

### 1️⃣ Introdução
- Apresenta os conceitos fundamentais de **Engenharia de Dados**.
- Explica a importância de um **Data Warehouse** para consolidar informações e apoiar decisões estratégicas.
- Mostra como o uso de dados pode **impulsionar o desempenho e a inovação** na indústria automotiva.

---

### 2️⃣ Explorando o banco de dados da empresa
- Navegação e exploração do banco **PostgreSQL** da empresa fictícia.
- Compreensão da **estrutura de tabelas, relacionamentos e dados de vendas**.
- Base para o **planejamento da modelagem e migração de dados** para o Data Warehouse.

---

### 3️⃣ Configurando o ambiente na AWS EC2
- Criação e configuração de uma instância **AWS EC2 (Ubuntu Server)** para hospedar o Airflow.
- Instalação e inicialização dos serviços do Airflow (Webserver, Scheduler e Metadatabase).
- Configuração de variáveis de ambiente e conexões seguras (PostgreSQL → Snowflake).
- Simulação de um ambiente **de produção corporativo na nuvem**, com agendamento e monitoramento contínuo.

---

### 4️⃣ Configurando o Airflow
- Criação de **DAGs (Directed Acyclic Graphs)** para orquestração de pipelines de dados.
- Implementação de operadores para **extração, transformação e carga (ETL/ELT)**.
- Aprendizado sobre **automação, dependências e logs de execução** no Airflow.

---

### 5️⃣ Criando conta e configurando o Snowflake
- Criação e configuração de conta no **Snowflake**.
- Entendimento da arquitetura de **Data Warehouse na nuvem** (Virtual Warehouses, Databases, Schemas, Stages).
- Integração do Snowflake com o Airflow para **receber os dados processados** a partir do PostgreSQL.

---

### 6️⃣ Criando a DAG e carregando os dados
- Desenvolvimento da DAG `postgres_to_snowflake_dag.py` para **extrair dados do PostgreSQL e carregar no Snowflake**.
- Demonstração de **pipelines incrementais e full load**.
- Registro e acompanhamento da execução via **Airflow UI** hospedado na **AWS EC2**.

---

### 7️⃣ Criando projeto e configurando o dbt
- Criação de um projeto **dbt** conectado ao **Snowflake**.
- Aplicação de **transformações, modelagem de tabelas fato e dimensão**.
- Execução de testes de qualidade (`dbt test`) e geração de documentação (`dbt docs generate`).
- Estrutura modular para **garantir rastreabilidade e governança dos dados**.

---

### 8️⃣ Produção de dashboard e análise no Power BI
- Conexão do **Power BI** ao **Snowflake** para visualização dos dados tratados.
- Criação de **dashboards dinâmicos e interativos** com KPIs como:
  - Volume de vendas por região e modelo de veículo.
  - Ticket médio e evolução temporal.
  - Comparativos de desempenho e indicadores de produtividade.
- Discussão de **alternativas e boas práticas** de visualização e storytelling de dados.

---

## 📊 Ferramentas utilizadas

| Ferramenta       | Função no projeto |
|-----------------|-----------------|
| **PostgreSQL**       | Banco de dados de origem com dados transacionais da montadora |
| **AWS EC2 (Ubuntu)** | Hospedagem do Airflow em ambiente virtualizado na nuvem |
| **Airflow**          | Orquestração e automação dos pipelines de dados (ETL/ELT) |
| **Snowflake**        | Data Warehouse na nuvem para armazenamento e modelagem de dados |
| **dbt**              | Transformações, modelagem, testes e documentação dos dados no Snowflake |
| **Power BI**         | Visualização e análise interativa dos dados modelados |





