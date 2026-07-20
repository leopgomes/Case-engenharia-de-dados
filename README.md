# Engenharia de Dados com Databricks e PySpark

Pipeline completo utilizando arquitetura Medalhão (Raw → Bronze → Silver → Gold), modelagem Star Schema e geração de JSON analítico com PySpark e Spark SQL.

# Arquitetura

```mermaid
graph TD
    CSV[CSV] --> RAW[RAW]
    RAW --> BRONZE[BRONZE]
    BRONZE --> SILVER[SILVER]
    SILVER --> GOLD[GOLD]
    GOLD --> STAR[STAR SCHEMA]
    STAR --> JSON[JSON]
```

## Tecnologias

- Databricks
- Apache Spark
- PySpark
- Spark SQL
- Delta Lake
- Python

## Objetivo

Desenvolver um pipeline de dados utilizando arquitetura Medalhão.

O pipeline realiza:

- ingestão de dados CSV
- tratamento e padronização
- validação de qualidade
- modelagem dimensional
- geração de JSON analítico

## Arquitetura Medalhão

### Raw

Armazena o arquivo original.

### Bronze

Estrutura os dados mantendo fidelidade ao arquivo de origem.

### Silver

Realiza limpeza, tipagem, padronização e validações.

### Gold

Disponibiliza modelo Star Schema para consumo analítico.

## Modelagem 

```mermaid
erDiagram
    dim_customer ||--o{ fact_sales : "faz"
    dim_product  ||--o{ fact_sales : "contém"
    dim_status   ||--o{ fact_sales : "possui"
    dim_date     ||--o{ fact_sales : "ocorre_em"

    dim_customer {
        int customer_key PK
        string customer_name
        string email
        string city
    }

    dim_product {
        int product_key PK
        string product_name
        string category
        decimal unit_price
    }

    dim_status {
        string status_name
        boolean is_completed
    }

    dim_date {
        int date_key PK
        date full_date
        int month
        int year
    }

    fact_sales {
        int sales_id PK
        int customer_key FK
        int product_key FK
        string status_name FK
        int date_key FK
        int quantity
        decimal unit_price
        decimal total_amount
    }
```
## Data Quality

Foram implementadas validações para:

- valores nulos
- quantidade negativa
- normalização de países
- normalização de datas
- remoção de duplicidades

## Consulta Final 

A solução foi desenvolvida utilizando:

- Spark SQL
- PySpark

## Resultado
```json
{
  "data": [
    {
      "Country": "USA",
      "Sales": [
        {
          "ProductLine": "Classic Cars",
          "QuantityOrdered": "120",
          "Sales": "14500.50"
        }
      ]
    }
  ]
}
```





















