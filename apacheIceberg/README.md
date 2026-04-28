# 🧊 Apache Iceberg com Apache Spark

Este módulo do projeto demonstra a utilização do **Apache Iceberg** com **Apache Spark (PySpark)** para manipulação de dados em Data Lakes com suporte a versionamento, evolução de schema e operações ACID.

O objetivo é apresentar, de forma prática, a criação e manipulação de tabelas Iceberg utilizando operações comuns em engenharia de dados, como INSERT, UPDATE e DELETE, permitindo comparação direta com o Delta Lake.

## ⚙️ Configuração do ambiente Spark

A configuração do Spark foi realizada adicionando as extensões e dependências necessárias para suporte ao Apache Iceberg, além da configuração de um catálogo do tipo Hadoop:

```python
from pyspark.sql import SparkSession

spark = (
    SparkSession
    .builder
    .master("local[*]")
    .config(
        "spark.jars.packages",
        "org.apache.iceberg:iceberg-spark-runtime-3.5_2.12:1.5.0"
    )
    .config("spark.sql.extensions", "org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions")
    .config("spark.sql.catalog.local", "org.apache.iceberg.spark.SparkCatalog")
    .config("spark.sql.catalog.local.type", "hadoop")
    .config("spark.sql.catalog.local.warehouse", "spark-warehouse-iceberg")
    .getOrCreate()
)
```

## 📊 Estrutura da tabela

Foi criada a tabela `local.db.ar_condicionado_iceberg` com os seguintes campos:

- id (INT)
- nm_modelo (STRING)
- nm_marca (STRING)
- valor (DECIMAL)
- ano (INT)
- potencia (STRING)

A tabela foi criada utilizando o formato Iceberg:

```sql
CREATE TABLE local.db.ar_condicionado_iceberg (
    id INT,
    nm_modelo STRING,
    nm_marca STRING,
    valor DECIMAL(10,2),
    ano INT,
    potencia STRING
) USING iceberg
```

## 🧪 Operações realizadas

### ➕ INSERT

Foram inseridos registros na tabela para simular dados reais de equipamentos de ar condicionado:

```sql
INSERT INTO local.db.ar_condicionado_iceberg 
VALUES 
(1, 'KOHI 09QC 1HV', 'KOMECO', 1798.90, 2023, '9000 BTU/h'),
(2, 'B0DSLQP69S', 'SAMSUNG', 2089.10, 2024, '12000 BTUs'),
(3, '42AGVCC30M5', 'SAMSUNG', 6590.00, 2025, '30000 BTUs')
```

### ✏️ UPDATE

Foram adicionadas novas colunas e posteriormente atualizados seus valores:

```sql
ALTER TABLE local.db.ar_condicionado_iceberg ADD COLUMN fl_inverter BOOLEAN;
ALTER TABLE local.db.ar_condicionado_iceberg ADD COLUMN tensao STRING;
```

```sql
UPDATE local.db.ar_condicionado_iceberg
SET fl_inverter = true, tensao = '220v'
WHERE id IN (2, 3);
```

### ❌ DELETE

Foi realizada a remoção de registros da tabela:

```sql
DELETE FROM local.db.ar_condicionado_iceberg
WHERE id = 1;
```

## 📜 Histórico de alterações (Snapshots)

Uma das principais características do Apache Iceberg é o controle de versões baseado em snapshots, permitindo auditoria e rastreamento das alterações realizadas na tabela.

Os snapshots podem ser consultados com:

```sql
SELECT * 
FROM local.db.ar_condicionado_iceberg.snapshots
```

## 🔍 Características do Apache Iceberg

- Controle de versão baseado em snapshots
- Suporte a transações ACID
- Evolução de schema eficiente
- Uso de catálogo (Hadoop, Hive, etc.)
- Integração com múltiplos motores de processamento
- Foco em ambientes distribuídos e Data Lakes modernos

## ⚠️ Diferenças em relação ao Delta Lake

- Não possui API Python equivalente ao `DeltaTable`
- A manipulação é feita principalmente via SQL
- Utiliza catálogo para gerenciamento das tabelas
- Não trabalha diretamente com paths físicos no código

## 📁 Notebook relacionado

O código completo da implementação está disponível no notebook:

```text
spark-iceberg.ipynb
```

## 🎯 Conclusão

O Apache Iceberg é uma solução robusta para gerenciamento de grandes volumes de dados em Data Lakes, oferecendo versionamento, consistência e flexibilidade, sendo especialmente indicado para ambientes distribuídos e arquiteturas modernas de dados.