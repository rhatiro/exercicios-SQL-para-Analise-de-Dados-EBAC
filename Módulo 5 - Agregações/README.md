<img src="https://raw.githubusercontent.com/rhatiro/Curso_EBAC-Profissao_Cientista_de_Dados/main/ebac-course-utils/media/logo/newebac_logo_black_half.png" alt="ebac-logo">

---

# **Módulo 5** | SQL: Agregações

Professora [Mariane Neiva](https://www.linkedin.com/in/mariane-neiva/)<br>
Aluno [Roberto Hatiro Nishiyama](https://www.linkedin.com/in/rhatiro/)<br>

Data: 12 de maio de 2023.

---

## Atividades

### **1. Criação da tabela**

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS default.heartattack (
	`age` int,
	`sex` int,
	`cp` int,
	`trtbps` int,
	`chol` int,
	`fbs` int,
	`restecg` int,
	`thalachh` int,
	`exng` int,
	`oldpeak` double,
	`slp` int,
	`caa` int,
	`thall` int,
	`output` int
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
	'serialization.format' = ',',
	'field.delim' = ','
)
LOCATION 's3://heart-attack-robertohatiro-ebac/'
TBLPROPERTIES ('has_encrypted_data' = 'false');
```

### **2. Função COUNT e GROUP BY**

#### [**2.1. Query 1**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%205%20-%20Agregac%CC%A7o%CC%83es/query_1.csv)
```sql
SELECT *
FROM heartattack
limit 10;
```

#### [**2.2. Query 2**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%205%20-%20Agregac%CC%A7o%CC%83es/query_2.csv)
```sql
SELECT COUNT(age) AS QUANTIDADE_LINHAS
FROM heartattack;
```

#### [**2.3. Query 3**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%205%20-%20Agregac%CC%A7o%CC%83es/query_3.csv)
```sql
SELECT COUNT(age) AS QUANTIDADE,
	CASE
		WHEN output = 1 THEN 'more chance of heart attack' ELSE 'less chance of heart attack'
	END AS output
FROM heartattack
GROUP BY output;
```

### **3. Funções MIN/MAX/SUM/AVG**

#### [**3.1. Query 4**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%205%20-%20Agregac%CC%A7o%CC%83es/query_4.csv)
```sql
SELECT MAX(age),
	MIN(age),
	AVG(age),
	output
FROM heartattack
GROUP BY output;
```

#### [**3.2. Query 5**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%205%20-%20Agregac%CC%A7o%CC%83es/query_5.csv)
```sql
SELECT MAX(age),
	MIN(age),
	AVG(age),
	output,
	sex
FROM heartattack
GROUP BY output,
	sex;
```

### **4. Função HAVING**

#### [**4.1. Query 6**](https://raw.githubusercontent.com/rhatiro/exercicios-SQL-para-Analise-de-Dados-EBAC/main/Mo%CC%81dulo%205%20-%20Agregac%CC%A7o%CC%83es/query_6.csv)
```sql
SELECT COUNT(output),
	output,
	sex
FROM heartattack
GROUP BY output,
	sex
HAVING COUNT(output) > 25;
```
