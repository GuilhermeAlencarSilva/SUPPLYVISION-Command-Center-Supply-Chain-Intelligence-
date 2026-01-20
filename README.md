# SUPPLYVISION  
Command Center & Supply Chain Intelligence

SUPPLYVISION é um projeto de *Inteligência de Cadeia de Suprimentos e Logística, desenvolvido como case executivo em Power BI, com foco em monitoramento operacional, risco logístico e suporte à tomada de decisão.

O projeto simula um ambiente corporativo realista, com alto volume de dados, múltiplas dimensões e métricas estratégicas utilizadas por lideranças de Supply Chain.

---

## 🎯 Objetivo do Projeto

Responder rapidamente a perguntas executivas como:

- A operação logística está saudável?
- Onde estão os principais riscos de atraso?
- Quais regiões concentram maior valor financeiro?
- Quais fornecedores impactam negativamente o nível de serviço?
- Onde estão os gargalos de entrega?

---

## 🧠 Abordagem

O projeto foi construído seguindo princípios de:
- Modelagem dimensional (Esquema Estrela)
- Storytelling analítico
- Design executivo orientado à decisão
- Boas práticas de Power BI para ambientes corporativos

---

## 🛠️ Tecnologias Utilizadas

- Power BI
- Python (Google Colab) — geração de dados simulados
- DAX — métricas e indicadores
- GitHub — versionamento e portfólio

---

## 📊 Estrutura do Dashboard

O dashboard é composto por 4 páginas executivas:

1. Visão Executiva
2. Estoque & Produtos
3. Fornecedores & Logística
4. Entregas & Performance

Cada página responde a um conjunto específico de decisões de negócio.


---

# Dicionário de Dados — SUPPLYVISION

## 📌 Tabela Fato

### Fato_Pedidos_Logistica

| Campo | Descrição |
|------|----------|
| ID_Pedido | Identificador único do pedido |
| ID_Tempo | Chave para dimensão tempo |
| ID_Produto | Chave para dimensão produto |
| ID_Fornecedor | Chave para dimensão fornecedor |
| ID_Armazem | Chave para dimensão armazém |
| ID_Regiao | Chave para dimensão região |
| ID_Status | Chave para status do pedido |
| Quantidade | Quantidade de itens no pedido |
| Valor_Pedido | Valor total do pedido |
| Custo_Logistico | Custo logístico associado |
| Prazo_Entrega_Dias | Prazo total de entrega |
| Atraso_Dias | Dias de atraso |
| Pedido_Entregue | Indicador Sim / Não |

---

## 📌 Dimensões

### Dim_Tempo
- Data
- Ano
- Mês
- Nome_Mês
- Trimestre

### Dim_Produto
- Produto
- Categoria
- Subcategoria

### Dim_Fornecedor
- Fornecedor
- País
- Nível do Fornecedor (A/B/C)

### Dim_Armazem
- Armazém
- Cidade
- Capacidade Máxima

### Dim_Regiao
- Região
- Estado

### Dim_Status_Pedido
- Status do Pedido

- # Modelo de Dados — Esquema Estrela

O SUPPLYVISION utiliza um modelo estrela clássico, garantindo:

- Performance
- Clareza analítica
- Facilidade de manutenção
- Escalabilidade

## ⭐ Estrutura

- Tabela Fato: Fato_Pedidos_Logistica
- Dimensões:
  - Dim_Tempo
  - Dim_Produto
  - Dim_Fornecedor
  - Dim_Armazem
  - Dim_Regiao
  - Dim_Status_Pedido

## 🔗 Regras de Relacionamento

- Cardinalidade: 1 : *
- Direção de filtro: Dimensão → Fato
- Nenhuma relação bidirecional
- Conexão por IDs substitutos

Esse modelo reflete padrões usados em ambientes corporativos reais.

📐 MEDIDAS DAX — SUPPLYVISION

Tabela base: Fato_Pedidos_Logistica
Modelo: Estrela (dimensões → fato)

🟦 MÉTRICAS BASE (FUNDAMENTAIS)
🔹 Total de Pedidos
Total de Pedidos =
COUNT ( Fato_Pedidos_Logistica[ID_Pedido] )

🔹 Quantidade Total
Quantidade Total =
SUM ( Fato_Pedidos_Logistica[Quantidade] )

🔹 Valor Total de Pedidos
Valor Total de Pedidos =
SUM ( Fato_Pedidos_Logistica[Valor_Pedido] )

🔹 Valor Médio por Pedido
Valor Médio por Pedido =
DIVIDE (
    [Valor Total de Pedidos],
    [Total de Pedidos]
)

🟦 CUSTOS LOGÍSTICOS
🔹 Custo Logístico Total
Custo Logístico Total =
SUM ( Fato_Pedidos_Logistica[Custo_Logistico] )

🔹 Custo Logístico Médio por Pedido
Custo Logístico Médio por Pedido =
DIVIDE (
    [Custo Logístico Total],
    [Total de Pedidos]
)

🟦 PRAZOS E ATRASOS
🔹 Prazo Médio de Entrega
Prazo Médio de Entrega =
AVERAGE ( Fato_Pedidos_Logistica[Prazo_Entrega_Dias] )

🔹 Atraso Médio (dias)
Atraso Médio (dias) =
AVERAGE ( Fato_Pedidos_Logistica[Atraso_Dias] )

🔹 Total de Pedidos Atrasados
Total de Pedidos Atrasados =
CALCULATE (
    [Total de Pedidos],
    Fato_Pedidos_Logistica[Atraso_Dias] > 0
)

🔹 % Pedidos Atrasados
% Pedidos Atrasados =
DIVIDE (
    [Total de Pedidos Atrasados],
    [Total de Pedidos]
)

🔹 Taxa de Entrega no Prazo
Taxa de Entrega no Prazo =
1 - [% Pedidos Atrasados]

🟦 ANÁLISE TEMPORAL (YoY / MoM)

Pré-requisito:
A dimensão Dim_Tempo deve estar marcada como Tabela de Data no Power BI.

🔹 Valor Total — Ano Anterior (YoY)
Valor Total YoY =
CALCULATE (
    [Valor Total de Pedidos],
    SAMEPERIODLASTYEAR ( Dim_Tempo[Data] )
)

🔹 Variação YoY — Valor
Variação YoY Valor (%) =
DIVIDE (
    [Valor Total de Pedidos] - [Valor Total YoY],
    [Valor Total YoY]
)

🔹 Quantidade Total — Ano Anterior
Quantidade Total YoY =
CALCULATE (
    [Quantidade Total],
    SAMEPERIODLASTYEAR ( Dim_Tempo[Data] )
)

🔹 Variação YoY — Quantidade
Variação YoY Quantidade (%) =
DIVIDE (
    [Quantidade Total] - [Quantidade Total YoY],
    [Quantidade Total YoY]
)

🔹 Valor Total — Mês Anterior (MoM)
Valor Total MoM =
CALCULATE (
    [Valor Total de Pedidos],
    DATEADD ( Dim_Tempo[Data], -1, MONTH )
)

🔹 Variação MoM — Valor
Variação MoM Valor (%) =
DIVIDE (
    [Valor Total de Pedidos] - [Valor Total MoM],
    [Valor Total MoM]
)

🔹 Quantidade Total — Mês Anterior
Quantidade Total MoM =
CALCULATE (
    [Quantidade Total],
    DATEADD ( Dim_Tempo[Data], -1, MONTH )
)

🔹 Variação MoM — Quantidade
Variação MoM Quantidade (%) =
DIVIDE (
    [Quantidade Total] - [Quantidade Total MoM],
    [Quantidade Total MoM]
)

🟦 FORNECEDORES & PERFORMANCE
🔹 Prazo Médio por Fornecedor
Prazo Médio por Fornecedor =
AVERAGE ( Fato_Pedidos_Logistica[Prazo_Entrega_Dias] )

🔹 Atraso Médio por Fornecedor
Atraso Médio por Fornecedor =
AVERAGE ( Fato_Pedidos_Logistica[Atraso_Dias] )

🟦 PEDIDOS CRÍTICOS (Página 4)
🔹 Pedido Crítico (Sim/Não)

Usado como filtro da tabela de pedidos críticos

Pedido Crítico =
IF (
    Fato_Pedidos_Logistica[Atraso_Dias] > 10
        && Fato_Pedidos_Logistica[Custo_Logistico] > [Custo Logístico Médio por Pedido],
    "Sim",
    "Não"
)

🟦 MEDIDAS DE APOIO (RECOMENDADAS)
🔹 Pedidos Entregues
Pedidos Entregues =
CALCULATE (
    [Total de Pedidos],
    Fato_Pedidos_Logistica[Pedido_Entregue] = "Sim"
)

🔹 Pedidos Não Entregues
Pedidos Não Entregues =
CALCULATE (
    [Total de Pedidos],
    Fato_Pedidos_Logistica[Pedido_Entregue] = "Não"
)

# Guia do Dashboard — SUPPLYVISION

## 🟦 Página 1 — Visão Executiva
Pergunta-chave:  
A operação logística está saudável?

KPIs:
- Total de Pedidos
- Valor Total de Pedidos
- % Pedidos Atrasados

Gráficos:
- Pedidos por Status
- Valor por Região
- Evolução de Pedidos
- Insights Automáticos

---

## 🟦 Página 2 — Estoque & Produtos
Pergunta-chave:  
Quais produtos concentram volume e valor?

- Quantidade por Categoria
- Top 10 Produtos por Valor
- Produto × Armazém
- Filtro por Categoria

---

## 🟦 Página 3 — Fornecedores & Logística
Pergunta-chave:
Quem está causando atraso e custo?

- Ranking de Fornecedores por Atraso
- Prazo vs Custo Logístico
- Pedidos por Nível de Fornecedor

---

## 🟦 Página 4 — Entregas & Performance
Pergunta-chave:  
Onde estão os gargalos de entrega?

- Prazo Médio ao Longo do Tempo
- Heatmap Região × Mês
- Tabela de Pedidos Críticos

# Principais Análises — SUPPLYVISION

## 🔍 Insights Esperados

- Concentração de valor em poucas regiões
- Fornecedores nível C com maior atraso médio
- Correlação entre prazo elevado e aumento de custo logístico
- Sazonalidade clara nos atrasos
- Pequeno percentual de pedidos gerando grande impacto financeiro

---

## 🎯 Uso Executivo

O dashboard permite:
- Priorização de fornecedores críticos
- Redirecionamento de estoque
- Negociação logística baseada em dados
- Monitoramento contínuo de SLAs

---

## ⚠️ Observações

Os dados são simulados, porém construídos para refletir:
- Distribuições assimétricas
- Variabilidade realista
- Cenários de risco plausíveis


👤 Autor

Projeto desenvolvido por Guilherme Alencar
Especialista em Análise de Dados, Negócios e Visualização Executiva

📫 LinkedIn: https://www.linkedin.com/in/guilherme-alencar-327413213/
📊 Portfólio: https://github.com/GuilhermeAlencarSilva
