<img src="https://raw.githubusercontent.com/rhatiro/Curso_EBAC-Profissao_Cientista_de_Dados/main/ebac-course-utils/media/logo/newebac_logo_black_half.png" alt="ebac-logo">

---

# **Módulo 7** | SQL Avançado

Professora [Mariane Neiva](https://www.linkedin.com/in/mariane-neiva/)<br>
Aluno [Roberto Hatiro Nishiyama](https://www.linkedin.com/in/rhatiro/)<br>

Data: 17 de maio de 2023.

---

## Atividades

### **1. Criação da tabela**

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS default.cliente (
	`id_cliente` int,
	`nome` string,
	`valor_compra` double,
	`loja_cadastro` string
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
	'serialization.format' = ',',
	'field.delim' = ','
)
LOCATION 's3://modulo7-robertohatiro-ebac/cliente/'
TBLPROPERTIES ('has_encrypted_data' = 'false');
```

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS default.transacoes (
	`id_cliente` int,
	`id_transacao` bigint,
	`valor_compra` double,
	`id_loja` string
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
	'serialization.format' = ',',
	'field.delim' = ','
)
LOCATION 's3://modulo7-robertohatiro-ebac/transacoes/'
TBLPROPERTIES ('has_encrypted_data' = 'false');
```

### **2. Subqueries**

#### [**2.1. Query 1**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%207%20-%20SQL%20Avanc%CC%A7ado/query1.csv)

```sql
SELECT id_loja,
	id_cliente,
	id_transacao
FROM transacoes
WHERE id_loja IN (
		SELECT cliente.loja_cadastro
		FROM cliente
		WHERE cliente.valor_compra > 160
	);
```

### **3. Particionamento**

```sql
CREATE EXTERNAL TABLE transacoes_part(
	id_cliente BIGINT,
	id_transacoes BIGINT,
	valor DOUBLE
)
PARTITIONED BY (id_loja string)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
	'serialization.format' = ',',
	'field.delim' = ','
)
LOCATION 's3://transacoes-partition-robertohatiro/';
```

```sql
MSCK REPAIR TABLE transacoes_part;
```

```sql
select count(*)
from transacoes;
```

```sql
select count(*)
from transacoes_part;
```

#### [**3.1. Query 2**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%207%20-%20SQL%20Avanc%CC%A7ado/query2.csv)

```sql
SELECT *
FROM transacoes_part
WHERE id_loja = 'magalu';
```

```sql
SELECT *
FROM transacoes
WHERE id_loja = 'magalu';
```

### **4. Junções: left / right**

#### [**4.1. Query 3**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%207%20-%20SQL%20Avanc%CC%A7ado/query3.csv)

```sql
CREATE VIEW transacoesV100 AS
SELECT id_cliente,
	valor_compra,
	id_loja AS nome_loja
FROM transacoes
WHERE valor_compra > 100;
```

```sql
SELECT *
FROM transacoesv100;
```

#### [**4.2. Query 4**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%207%20-%20SQL%20Avanc%CC%A7ado/query4.csv)

```sql
CREATE VIEW clientevalor AS
SELECT id_cliente,
	valor_compra
FROM transacoes
ORDER BY valor_compra DESC
LIMIT 2;
```

```sql
SELECT *
FROM clientevalor;
```

### **5. Bônus**

```sql
DESCRIBE clientevalor;
```

```sql
SHOW COLUMNS IN clientevalor;
```

```sql
SHOW VIEWS;
```

```sql
SHOW CREATE VIEW clientevalor;
```

```sql
DROP VIEW clientevalor;
```
