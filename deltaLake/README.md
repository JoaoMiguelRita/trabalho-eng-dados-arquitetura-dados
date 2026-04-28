# 🧊 Delta Lake com Apache Spark

Este módulo do projeto demonstra a utilização do **Delta Lake** com **Apache Spark (PySpark)** para manipulação de dados em formato de Data Lake com suporte a transações ACID.

O objetivo é apresentar, de forma prática, a criação e manipulação de tabelas Delta utilizando operações comuns em engenharia de dados, como INSERT, UPDATE e DELETE.

## ⚙️ Configuração do ambiente Spark

A configuração do Spark foi realizada adicionando as extensões e dependências necessárias para suporte ao Delta Lake:

```python
from pyspark.sql import SparkSession

spark = (
    SparkSession
    .builder
    .master("local[*]")
    .config("spark.jars.packages", "io.delta:delta-spark_2.12:3.2.0")
    .config("spark.sql.extensions", "io.delta.sql.DeltaSparkSessionExtension")
    .config("spark.sql.catalog.spark_catalog", "org.apache.spark.sql.delta.catalog.DeltaCatalog")
    .getOrCreate()
)
```

## 📊 Estrutura da tabela

Foi criada a tabela `ar_condicionado_delta` com os seguintes campos:

- id (INT)
- nm_modelo (STRING)
- nm_marca (STRING)
- valor (DECIMAL)
- ano (INT)
- potencia (STRING)

A tabela foi criada utilizando o formato Delta:

```sql
CREATE TABLE ar_condicionado_delta (
    id INT,
    nm_modelo STRING,
    nm_marca STRING,
    valor DECIMAL(10,2),
    ano INT,
    potencia STRING
) USING delta
```

## 🧪 Operações realizadas

### ➕ INSERT

Foram inseridos registros na tabela para simular dados reais de equipamentos de ar condicionado:

```sql
INSERT INTO ar_condicionado_delta 
VALUES 
(1, 'KOHI 09QC 1HV', 'KOMECO', 1798.90, 2023, '9000 BTU/h'),
(2, 'B0DSLQP69S', 'SAMSUNG', 2089.10, 2024, '12000 BTUs'),
(3, '42AGVCC30M5', 'SAMSUNG', 6590.00, 2025, '30000 BTUs')
```

### ✏️ UPDATE

Foram adicionadas novas colunas e posteriormente atualizados seus valores:

```sql
ALTER TABLE ar_condicionado_delta ADD COLUMN fl_inverter BOOLEAN;
ALTER TABLE ar_condicionado_delta ADD COLUMN tensao STRING;
```

```sql
UPDATE ar_condicionado_delta
SET fl_inverter = true, tensao = '220v'
WHERE id IN (2, 3);
```

### ❌ DELETE

Foi realizada a remoção de registros da tabela:

```sql
DELETE FROM ar_condicionado_delta
WHERE id = 1;
```

## 📜 Histórico de alterações

Uma das principais vantagens do Delta Lake é o controle de versões da tabela, permitindo auditoria e rastreamento de mudanças ao longo do tempo.

O histórico pode ser consultado com:

```python
from delta.tables import DeltaTable

delta_table = DeltaTable.forPath(spark, "./spark-warehouse/ar_condicionado_delta")
delta_table.history().show(truncate=False)
```

## 🔍 Características do Delta Lake

- Armazenamento baseado em arquivos Parquet com controle via `_delta_log`
- Suporte a transações ACID
- Controle de versão e histórico de alterações
- Integração nativa com Apache Spark
- API Python para manipulação avançada (`DeltaTable`)

## 📁 Notebook relacionado

O código completo da implementação está disponível no notebook:

```text
spark-delta-lake.ipynb
```

## 🎯 Conclusão

O Delta Lake simplifica a construção de pipelines de dados confiáveis ao adicionar recursos como versionamento, consistência e operações transacionais sobre arquivos em Data Lakes, sendo uma solução eficiente tanto para ambientes locais quanto distribuídos.