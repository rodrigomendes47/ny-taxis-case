# 🚕 NY Taxi Data Engineering Case Study

Esta aplicação de engenharia de dados tem como objetivo principal a **análise e transformação de dados históricos de corridas de táxis de Nova York**, seguindo as melhores práticas e arquitetura de dados moderna.

## 📝 Visão Geral do Projeto

O projeto consiste em um notebook de processamento de dados que realiza a **ingestão, limpeza, padronização e análise** dos dados brutos, facilitando a geração de **insights valiosos** para o negócio e na disponibilização de dados tratados para consumo.

Os dados brutos são provenientes do repositório oficial de dados de táxi de Nova York, acessível através do seguinte link:
https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

### ⚙️ Arquitetura e Tecnologia

A solução foi desenvolvida em ambiente **Databricks Community**, utilizando um **Amazon S3 bucket** como Data Lake para armazenamento de dados persistente.

Adotou-se a **Arquitetura Medalhão (Medallion Architecture)**, que organiza os dados em camadas Bronze (dados brutos e imutaveis), Silver (dados limpos e padronizados) e Gold (dados agregados, transformados e específicos para as regras de negócio, servindo como a fonte principal para relatórios e BI), garantindo a qualidade e o refinamento progressivo dos dados.
<img width="1927" height="1021" alt="image" src="https://github.com/user-attachments/assets/96269180-ff0c-4b48-ab86-8086af7d35e7" />
fonte: Statplace

## 🚀 Execução

Para executar o notebook e o pipeline de processamento de dados, os seguintes requisitos são mandatórios:

### Pré-requisitos

1.  Um **AWS S3 Bucket** criado para servir como Data Lake.
2.  Um **usuário AWS IAM** com as permissões necessárias para acesso ao S3 e um **Token de Acesso (Access Key e Secret Key)** válido para autenticação.

### Dependências

As bibliotecas e pacotes necessários para a execução do notebook podem ser instalados de duas maneiras:

1.  **Execução da primeira célula** do notebook principal, que contém os comandos de instalação necessários.
2.  Utilizando o arquivo `requirements.txt` via **pip**:
    ```bash
    pip install -r requirements.txt
    ```

## 📂 Estrutura de Saída (Data Lake Layers)

Ao final da execução do pipeline, três diretórios principais são criados no S3 Bucket, correspondendo às camadas da Arquitetura Medalhão:

### 1. `raw-data` (Camada Bronze)

* Cópia fiel dos dados brutos extraídos da fonte original.
* Preservar a integridade do dado original sem qualquer transformação ou limpeza.

### 2. `cleaned-data` (Camada Silver)

* Dados tratados, padronizados e limpos (anormalidades e valores nulos removidos).
* **Particionamento:** Os dados desta camada são otimizados para consulta através do seguinte esquema de particionamento:
    ```
    cleaned_data/
    ├── [yellow/green]/  <- Tipo de táxi
    │   ├── Ano/
    │   │   ├── Mês/
    │   │   │   └── arquivo.parquet
    ```

### 3. `metrics-data` (Camada Gold)

* Dados consolidados com a aplicação da **Regra de Negócio (Business Logic)**.
* Geração de insights e métricas prontas para consumo pelo Business Intelligence (BI) ou aplicações de Data Science.
* **Métrica levantada:** Análise da **distribuição de corridas por hora**, visando compreender padrões de comportamento e tendências ao longo do tempo.
