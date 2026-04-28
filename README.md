# 📊 Projeto Apache Spark com Delta Lake e Apache Iceberg

Projeto desenvolvido para demonstrar o uso do **Apache Spark (PySpark)** com duas tecnologias de Data Lake:

- 🧊 Delta Lake
- 🧊 Apache Iceberg

O objetivo é comparar as duas abordagens utilizando o mesmo cenário de dados e operações.


## Membros
- João Miguel Fortunato Rita
- Gustavo de Freitas Cardoso


## 🖥️ Ambiente utilizado

- Sistema Operacional: Ubuntu 24.04 (WSL recomendado)
- Python: 3.10.x
- Gerenciador de pacotes: UV
- Apache Spark (PySpark): 3.5.3


## 📦 Dependências

```bash
pyspark==3.5.3
delta-spark==3.2.0
jupyterlab
ipykernel
```


## ⚙️ Setup do ambiente

```bash
uv init
uv venv
source .venv/bin/activate
uv add pyspark==3.5.3 delta-spark==3.2.0 jupyterlab ipykernel
```


## 🚀 Execução

- Selecione o kernel |.venv| e execute os notebooks
```jupyter lab```
- Ou através da extensão Jupyter no vscode


## 📁 Estrutura do projeto

```
├── .venv/
├───── deltaLake
|   │   └─ spark-warehouse/ar_condicionado_delta
│   │   └─ spark-delta-lake.ipynb
│   │   └─ README.md
│   └── apacheIceberg
|   │   └─ spark-warehouse/db/ar_condicionado_iceberg
|   │       └─ data
|   │       └─ metadata
│   │   └─ spark-delta-iceberg.ipynb
│   │   └─ README.md
├── uv.lock
├── .python-version
├── pyproject.toml
├── README.md
```


## ⚠️ Observações

* O projeto foi executado utilizando VS Code com a extensão Jupyter
* É necessário selecionar o kernel correto antes de executar os notebooks
* Recomenda-se ambiente Linux ou WSL para evitar problemas de compatibilidade


## 🔗 Referências

* [https://github.com/jlsilva01/spark-delta](https://github.com/jlsilva01/spark-delta)
* [https://github.com/jlsilva01/spark-iceberg/tree/main](https://github.com/jlsilva01/spark-iceberg/tree/main)
* [https://youtu.be/eOrWEsZIfKU](Como Sair do Zero no Delta Lake e PySpark)

---