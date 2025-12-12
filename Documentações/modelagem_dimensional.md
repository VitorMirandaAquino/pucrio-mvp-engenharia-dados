# 📂 Modelo Dimensional de Emendas Parlamentares

Este documento descreve o modelo dimensional (Esquema Estrela) utilizado para a análise dos dados de emendas parlamentares e informações dos deputados federais.

O objetivo é otimizar a estrutura de dados para responder às **Perguntas Centrais** do projeto, focando em análises de volume financeiro, destinação de recursos (quem, para onde, em quê) e comparativos entre valores empenhados e pagos.

---

## 🌟 Esquema Estrela: `Emendas_e_Parlamentares`

O esquema é composto por uma Tabela Fato central (`Fato_Emenda`) e quatro Tabelas de Dimensão que fornecem o contexto analítico.

### 1. Tabela Fato: `Fato_Emenda` (Métricas Financeiras)

Registra as transações financeiras das emendas e conecta-se a todas as dimensões através de chaves estrangeiras (`sk_...`).

| Coluna | Descrição | Tipo de Dado | Chave | Origem |
| :--- | :--- | :--- | :--- | :--- |
| `id_emenda` | Identificador único do registro de emenda. | `INT` | **PK** | Gerada/Sequencial |
| `sk_parlamentar` | Chave de conexão com o autor da emenda. | `INT` | **FK** | `Dim_Parlamentar` |
| `sk_tempo` | Chave de conexão com o ano de registro. | `INT` | **FK** | `Dim_Tempo` |
| `sk_localidade` | Chave de conexão com o destino do gasto (UF/Município). | `INT` | **FK** | `Dim_Localidade` |
| `sk_orcamento` | Chave de conexão com a destinação orçamentária (Área/Função). | `INT` | **FK** | `Dim_Orcamento` |
| **`valor_empenhado`** | Volume de recursos empenhado. | `NUMERIC(18, 2)` | **MÉTRICA** | Coluna Original |
| **`valor_liquidado`** | Volume de recursos liquidado. | `NUMERIC(18, 2)` | **MÉTRICA** | Coluna Original |
| **`valor_pago`** | Volume de recursos efetivamente pago. | `NUMERIC(18, 2)` | **MÉTRICA** | Coluna Original |

---

### 2. Tabelas de Dimensão (Contexto Analítico)

#### 2.1. 👤 `Dim_Parlamentar` (Quem)

Informações sobre o autor da emenda e seu contexto político.

| Coluna | Descrição | Tipo de Dado | Chave |
| :--- | :--- | :--- | :--- |
| `sk_parlamentar` | Chave Primária da Dimensão (Surrogate Key). | `INT` | **PK** |
| `nome_parlamentar` | Nome do autor da emenda. | `STRING` | |
| `sigla_partido` | Sigla do partido político. | `STRING` | |
| `sigla_uf` | Unidade da Federação que o deputado representa. | `STRING` | |

#### 2.2. 📅 `Dim_Tempo` (Quando)

Contexto temporal para análise de séries históricas.

| Coluna | Descrição | Tipo de Dado | Chave |
| :--- | :--- | :--- | :--- |
| `sk_tempo` | Chave Primária da Dimensão. | `INT` | **PK** |
| `ano` | Ano da Emenda (2023, 2024, 2025). | `INT` | |

#### 2.3. 💰 `Dim_Orcamento` (O Quê / Área)

Detalhes sobre a destinação orçamentária. A coluna `funcao` é utilizada como a categoria de alto nível para análise.

| Coluna | Descrição | Tipo de Dado | Chave |
| :--- | :--- | :--- | :--- |
| `sk_orcamento` | Chave Primária da Dimensão. | `INT` | **PK** |
| `funcao` | **COLUNA CHAVE:** Função orçamentária que representa a área temática principal (e.g., Saúde, Educação). | `STRING` | |

#### 2.4. 🗺️ `Dim_Localidade` (Para Onde)

Informações sobre o município e UF de destino dos recursos.

| Coluna | Descrição | Tipo de Dado | Chave |
| :--- | :--- | :--- | :--- |
| `sk_localidade` | Chave Primária da Dimensão. | `INT` | **PK** |
| `localidade_gasto` | Nome original da Localidade/Regionalização. | `STRING` | |
| `municipio_destino` | **COLUNA DERIVADA:** Nome padronizado do Município. | `STRING` | |
| `uf_destino` | **COLUNA DERIVADA:** Sigla da UF do destino. | `STRING` | |

---
![MODELAGEM](../imagens/Modelagem Dimensional - PUCRIO.drawio.png)