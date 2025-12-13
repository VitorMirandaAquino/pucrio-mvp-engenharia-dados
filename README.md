# 📄 **MVP COMPLETO – Projeto de Engenharia de Dados**

## **Transparência do Uso de Emendas Parlamentares no Brasil**

**Autor:** Vitor

**Curso:** Pós-Graduação em Ciência de Dados

**Sprint:** Engenharia de Dados – Bancos, ETL, DW, Governança e Qualidade

**Plataforma:** Databricks Community Edition 

---

# SUMÁRIO

1. **Objetivo do Trabalho**
2. **Perguntas de Negócio**
3. **Busca e Seleção da Base de Dados**
4. **Modelagem de Dados (DW / Data Lake)**
5. **Pipeline de ETL (Bronze → Silver → Gold)**
6. **Análise – Qualidade de Dados**
7. **Análise – Respostas ao Problema**
8. **Dashboard Final**

---

# 1. **OBJETIVO DO TRABALHO**

O objetivo deste MVP é construir um pipeline completo de engenharia de dados — desde a coleta até a análise — utilizando dados públicos de **emendas parlamentares**, com o intuito de:

- possibilitar que cidadãos visualizem como parlamentares direcionam suas emendas;
- permitir análises por **ano**, **partido**, **parlamentar**, **localidade** e **área temática** (saúde, educação etc.);
- promover transparência no uso do dinheiro público;
- criar um dashboard dinâmico para exploração aberta dos dados.

Esse MVP abrange **coleta**, **modelagem dimensional**, **ETL**, **governança**, **qualidade de dados** e **análise final**.

---

# 2. **PERGUNTAS DE NEGÓCIO (ANTES DE QUALQUER DADO)**

### Perguntas centrais:

1. Qual o volume total de emendas por ano?
2. Quais parlamentares movimentam mais recursos?
3. Quais partidos destinam mais verbas por área (saúde, educação etc.)?
4. Para onde (UF/município) cada parlamentar direciona sua verba?
5. Qual a razão entre verba empenhada vs. paga? Há diferenças significativas?
6. Quais áreas recebem mais recursos ao longo do tempo?

---

# 3. **BUSCA E ESCOLHA DA BASE DE DADOS**

### Fonte oficial

**Portal da Transparência – Emendas Parlamentares**

**Formato utilizado:**

- Downloads CSV

**Câmaro dos deputados**

**Formato utilizado:**

- Requisições por API

---

# 4. **MODELO DE DADOS – DATA WAREHOUSE (ESQUEMA ESTRELA)**

*A modelagem completa está na pasta de documentações*

### **Fato Principal**

**fato_emendas**

### **Dimensões**

- **dim_parlamentar**
- **dim_partido**
- **dim_municipio**
- **dim_tempo**
- **dim_funcao_governamental**



---

# 5. **PIPELINE ETL (STAGING → BRONZE → SILVER → GOLD)**

### **Staging Layer – Upload de arquivos**
- Upload de arquivos de emendas parlamentares

### **Bronze Layer – Dados brutos**

- Criação de uma tabela de emendas parlamentares com os CSVs da camada staging
- Criação de uma tabela de parlamentares com os dados obtidos via api

### **Silver Layer – Limpeza e normalização**

Transformações principais:

- Padronização de tipos
- Remoção de duplicatas
- Normalização de UF e municípios
- Conversão de valores para `Decimal`
- Explosão de colunas aninhadas

### **Gold Layer – Pronto para análises**

- Tabelas fato & dimensões
- Chaves substitutas (`surrogate keys`)
- Tabelas otimizadas em Delta Lake
- Integridade referencial garantida

---

# 6. **ANÁLISE – QUALIDADE DE DADOS**

Checklist executado:

✔ Valores ausentes (missing) — encontrados principalmente em subfunções menores

✔ Problemas de padronização de nomes de municípios

✔ Tipos incorretos vindos da API (strings com números)

✔ Categorias desconhecidas de função → normalizadas

Cada problema foi documentado e tratado na Silver Layer.

---

# 7. **ANÁLISE – RESPOSTAS AO PROBLEMA**

Cada insight é acompanhado de:

- Query SQL
- Print do resultado
- Discussão interpretativa

---

# 8. **DASHBOARD FINAL**

### Ferramenta:

Power BI / Tableau / Databricks SQL

### Elementos principais:

**Filtros**

- Ano
- Partido
- Parlamentar
- Função/Subfunção
- Estado UF


