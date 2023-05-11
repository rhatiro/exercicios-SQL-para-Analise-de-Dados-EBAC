<img src="https://raw.githubusercontent.com/rhatiro/Curso_EBAC-Profissao_Cientista_de_Dados/main/ebac-course-utils/media/logo/newebac_logo_black_half.png" alt="ebac-logo">

---

# **Módulo 3** | SQL: Selecionando & Ordenando

Professora [Mariane Neiva](https://www.linkedin.com/in/mariane-neiva/)<br>
Aluno [Roberto Hatiro Nishiyama](https://www.linkedin.com/in/rhatiro/)<br>

Data: 11 de maio de 2023.

---

## Atividades

### **2. Selecionando dados**

```sql
CREATE EXTERNAL TABLE transacoes (
	id_cliente BIGINT,
	id_transacao BIGINT,
	valor FLOAT,
	id_loja STRING
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
	'separatorChar' = ',',
	'quoteChar' = '"',
	'escapeChar' = '\\'
)
STORED AS TEXTFILE
LOCATION 's3://bucket-robertohatiro-transacoes/'
```

#### [**2.1. Query 1**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%203%20-%20Selecionando%20%26%20Ordenando/query1.csv)
```sql
SELECT *
FROM transacoes
```

#### [**2.2. Query 2**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%203%20-%20Selecionando%20%26%20Ordenando/query2.csv)
```sql
SELECT id_cliente,
	valor,
	id_loja AS nome_loja
FROM transacoes;
```

#### [**2.3. Query 3**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%203%20-%20Selecionando%20%26%20Ordenando/query3.csv)
```sql
SELECT DISTINCT id_loja AS nome_loja
FROM transacoes;
```

### **3. Ordenando e limitando dados**

#### [**3.1. Query 4**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%203%20-%20Selecionando%20%26%20Ordenando/query4.csv)
```sql
SELECT id_cliente,
	valor
FROM transacoes
ORDER BY valor DESC
LIMIT 2;
```
