## Objetivo
Construir um pipeline de dados containerizado, orquestrado pelo Airflow, com validações de qualidade na fronteira de ingestão, transformação analítica utilizando dbt e PostgreSQL como data warehouse.

## Visão Geral

                   ┌───────────────┐
                   │  Data Source  │
                   └───────┬───────┘
                           │
                           ▼
                    ┌────────────┐
                    │   Python   │
                    │ Extract/   │
                    │    Load    │
                    └─────┬──────┘
                          │
                          ▼
                    ┌────────────┐
                    │ PostgreSQL │
                    │    RAW     │
                    └─────┬──────┘
                          │
                          ▼
                ┌────────────────────┐
                │ Great Expectations │
                │   Data Quality     │
                └─────────┬──────────┘
                          │
                          ▼
                    ┌────────────┐
                    │ PostgreSQL │
                    │  STAGING   │
                    └─────┬──────┘
                          │
                          ▼
                       ┌─────┐
                       │ dbt │
                       └──┬──┘
                          │
                          ▼
                    ┌────────────┐
                    │ PostgreSQL │
                    │  ANALYTICS │
                    └─────┬──────┘
                          │
                          ▼
                         SQL

### Responsabilidades
- Orchestration: Apache Airflow
- Containerization: Docker
- Data Quality: Great Expectations
- Transformation: dbt
- Database: PostgreSQL
- Language: Python / SQL

## Arquitetura Geral

                 AIRFLOW
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
      Python ETL          Data Quality
          │                   │
          ▼                   ▼
       raw/staging ──→ Great Expectations
                              │
                              ▼
                         PostgreSQL
                              │
                              ▼
                             dbt
                              │
                 ┌────────────┴───────────┐
                 ▼                        ▼
             dimensions                 facts
                 │                        │
                 └────────────┬───────────┘
                              ▼
                           marts
                              │
                              ▼
                             SQL
                             
## Estrutura do projeto

data-engineering-pipeline/

│

├── docker/

│   ├── airflow/

│   │   └── Dockerfile

│   │

│   ├── python/

│   │   └── Dockerfile

│   │

│   └── postgres/

│       └── init.sql

│

├── dags/

│   └── etl_pipeline.py

│

├── src/

│   ├── extract/

│   │   └── extract.py

│   │

│   ├── transform/

│   │   └── transform.py

│   │

│   └── load/

│       └── load.py

│

├── great_expectations/

│   ├── expectations/

│   └── checkpoints/

│

├── dbt/

│   ├── models/

│   │   ├── staging/

│   │   ├── intermediate/

│   │   └── marts/

│   │

│   ├── tests/

│   ├── macros/

│   ├── seeds/

│   ├── snapshots/

│   └── dbt_project.yml

│

├── sql/

│   └── analysis/

│

├── tests/

│   └── test_pipeline.py

│

├── docker-compose.yml

├── requirements.txt

## Estrutura PostgreSQL

PostgreSQL

│

├── raw

│   ├── users

│   ├── courses

│   ├── episodes

│   └── ...

│

├── staging

│   ├── stg_usesrs

│   ├── stg_courses

│   └── stg_episodes

│

├── intermediate

│   └── ...

│

└── analytics

    ├── dim_users
    
    ├── dim_courses
    
    ├── fact_history
    
    └── ...
    
├── .env

├── .gitignore

└── README.md


## Arquitetura da orquestração
A DAG representa as dependências do pipeline e impede que transformações analíticas sejam executadas caso a validação da camada de ingestão falhe.

                    START
                      │
                      ▼
                 extract_data
                      │
                      ▼
                  load_data
                      │
                      ▼
             validate_data
                      │
                ┌─────┴─────┐
                │           │
              FAIL         PASS
                │           │
                ▼           ▼
             STOP        run_dbt
                            │
                            ▼
                     data_quality_dbt
                            │
                            ▼
                           END

## Fonte de dados
CSV / API / Kaggle
        │
        ▼
      Python
        │
        ▼
   PostgreSQL raw


## Fases do projeto
### Fase 1, ambiente
☐ Criar repositório
☐ Criar estrutura de pastas
☐ Criar Dockerfile
☐ Criar docker-compose
☐ Subir PostgreSQL
☐ Conectar pelo VS Code

### Fase 2, Python
☐ Criar ambiente Python
☐ Criar conexão PostgreSQL
☐ Criar extract.py
☐ Criar load.py
☐ Testar manualmente

### Fase 3, Great Expectations
☐ Instalar/configurar GE
☐ Criar datasource
☐ Criar expectations
☐ Criar checkpoint
☐ Testar dados inválidos

### Fase 4, Airflow
☐ Subir Airflow
☐ Criar DAG
☐ PythonOperator / TaskFlow
☐ Dependências
☐ Retries
☐ Logs

### Fase 5, dbt
☐ Inicializar projeto dbt
☐ Configurar PostgreSQL
☐ Criar staging
☐ Criar intermediate
☐ Criar marts
☐ Criar dbt tests

### Fase 6, camada analítica
☐ Criar perguntas de negócio
☐ Criar SQL
☐ Documentar métricas
☐ Criar dashboard opcional

### Fase 7, projeto de portfólio
☐ README
☐ Arquitetura
☐ Data model
☐ DAG
☐ Data quality report
☐ dbt lineage
☐ Exemplos SQL
☐ Screenshots
☐ Como reproduzir
