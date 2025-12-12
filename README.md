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
4. **Coleta e Armazenamento na Nuvem**
5. **Modelagem de Dados (DW / Data Lake)**
6. **Catálogo de Dados (Data Dictionary)**
7. **Pipeline de ETL (Bronze → Silver → Gold)**
8. **Análise – Qualidade de Dados**
9. **Análise – Respostas ao Problema**
10. **Dashboard Final**
11. **Autoavaliação**

---

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
7. Para um parlamentar específico:
    - Para quais áreas ele destina recursos?
    - Quais municípios são mais beneficiados?

---

# 3. **BUSCA E ESCOLHA DA BASE DE DADOS**

### Fonte oficial

**Portal da Transparência – Emendas Parlamentares**

Website oficial do Governo Federal, base já pública por lei.

**Formatos disponíveis:**

- Downloads CSV

---

# 4. **COLETA E ARMAZENAMENTO**

## Como foi feita a coleta:

- A API é paginada → necessário iterar `offset` e `limit`.
- Criado script de coleta em Python (notebook Databricks).
- Cada requisição salva um arquivo JSON no **Data Lake (Bronze Layer)**.

## Estrutura de pastas (recomendada):

```
/bronze/emendas/ano=2020/*.json
/bronze/emendas/ano=2021/*.json
/bronze/emendas/ano=2022/*.json
/parlamentares/*.json
/partidos/*.json

```

## Evidências armazenadas:

- Pasta *bronze* no DBFS
- Arquivos JSON organizados por ano
- Trecho da chamada da API

---

# 5. **MODELO DE DADOS – DATA WAREHOUSE (ESQUEMA ESTRELA)**

*(versão completa está no Anexo A)*

### **Fato Principal**

**fato_emendas**

### **Dimensões**

- **dim_parlamentar**
- **dim_partido**
- **dim_municipio**
- **dim_tempo**
- **dim_funcao_governamental**

### Justificativa:

- Simplifica consultas exploratórias
- Suporta dashboards
- Alinha-se a boas práticas de BI

---

---

# 7. **PIPELINE ETL (BRONZE → SILVER → GOLD)**

*(versão completa no Anexo C)*

### **Bronze Layer – Dados brutos**

- Estrutura original JSON
- Sem limpeza
- Apenas padronização de armazenamento

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

# 8. **ANÁLISE – QUALIDADE DE DADOS**

Checklist executado:

✔ Valores ausentes (missing) — encontrados principalmente em subfunções menores

✔ Problemas de padronização de nomes de municípios

✔ Tipos incorretos vindos da API (strings com números)

✔ Categorias desconhecidas de função → normalizadas

✔ Possíveis outliers em valores > R$ 50 milhões

Cada problema foi documentado e tratado na Silver Layer.

---

# 9. **ANÁLISE – RESPOSTAS AO PROBLEMA**

*(exemplo de interpretações que você colocará com base no SQL real)*

### Exemplos de descobertas:

- **O partido X** é o que mais destinou verbas à saúde em 2021.
- **O parlamentar Y** concentrou mais de 60% de suas emendas em apenas **2 municípios**.
- **Educação e saúde** representam **74% do gasto total** das emendas de 2020–2023.
- Alguns parlamentares apresentam **baixa taxa de pagamento** mesmo com alto empenho → alerta de eficiência.
- Municípios de pequeno porte aparecem como maiores beneficiados em algumas regiões — padrão incomum.

Cada insight é acompanhado de:

- Query SQL
- Print do resultado
- Discussão interpretativa

---

# 10. **DASHBOARD FINAL**

### Ferramenta:

Power BI / Tableau / Databricks SQL

### Elementos principais:

**Filtros**

- Ano
- Partido
- Parlamentar
- Função/Subfunção
- Estado UF

**Visuais sugeridos**:

- Ranking de parlamentares por valor total
- Mapa de calor dos municípios beneficiados
- Barras: verba por área
- Linha: evolução anual por partido
- Indicadores: % pago vs % empenhado

---

# 11. **AUTOAVALIAÇÃO**

Inclua tópicos como:

- Quais perguntas foram respondidas
- Quais não foram e por quê
- Dificuldades técnicas (API lenta, municípios duplicados etc.)
- Melhorias futuras:
    - Incluir liquidação de pagamentos
    - Dashboard mais avançado
    - Machine Learning para detectar padrões suspeitos
