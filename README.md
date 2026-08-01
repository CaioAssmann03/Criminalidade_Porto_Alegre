# Análise Exploratória da Criminalidade no Rio Grande do Sul — 2025

Análise exploratória de dados (EDA) sobre ocorrências criminais registradas no Rio Grande do Sul em 2025, com tratamento de dados, cruzamento de indicadores e visualização de padrões de criminalidade por município, bairro (Porto Alegre), tipo de crime, horário e perfil de vítimas.

## 🧭 Estrutura deste projeto

O projeto foi desenvolvido em duas etapas, com ferramentas diferentes:

| Etapa | Ferramenta | Onde está |
|---|---|---|
| 1. Tratamento e análise exploratória do dataset de ocorrências | Python (Pandas) no Google Colab | `analise_criminalidade_rs_2025.ipynb` |
| 2. Cruzamento com dados populacionais do IBGE e construção do painel | Power BI (Power Query + DAX) | `medidas_dax.md` (documentação das medidas e do modelo de relacionamento) |

**Importante:** o notebook Python cobre apenas a etapa 1 (dataset de ocorrências). O cruzamento com a base de população do IBGE, o relacionamento entre as duas tabelas e o cálculo da taxa por 100 mil habitantes foram feitos diretamente no Power BI — essa parte está documentada em `medidas_dax.md`, não no notebook. Se você esperava encontrar esse cruzamento em Python, ele não existe neste repositório; foi um processo feito na ferramenta de BI.

## 🎯 Objetivo

Identificar padrões espaciais e temporais da criminalidade no RS a partir de dados oficiais, respondendo perguntas como:
- Quais crimes são mais frequentes no estado?
- Quais cidades concentram mais mortes violentas?
- Qual o bairro mais afetado em Porto Alegre?
- Existe um horário com maior incidência de golpes (estelionato)?
- O tipo e o local do crime mudam ao longo do dia?
- Há diferença de perfil de vítima (idade, sexo) entre os tipos de crime?
- Como a criminalidade se compara entre municípios de tamanhos diferentes, proporcionalmente à população?

## 🗂️ Fonte dos dados

- **Ocorrências criminais:** Secretaria de Segurança Pública do Rio Grande do Sul (SSP-RS), registros de 2025.
- **População por município:** IBGE, Estimativas de População 2025.

> **Nota:** o dataset bruto de ocorrências não está incluído neste repositório por conter informações sensíveis (dados de vítimas). Apenas o notebook com o pipeline de tratamento e análise está disponível.

## 🛠️ O que o notebook faz (etapa 1)

1. **Exploração inicial** — dimensão da base, tipos de dados, valores nulos
2. **Tratamento de dados** — padronização de nomes de bairros, preenchimento de valores ausentes, conversão de datas e criação da coluna "Período do Dia"
3. **Análise exploratória** — cruzamentos entre tipo de crime, município, bairro, horário, sexo e idade da vítima
4. **Visualizações** — gráficos de barras, boxplots e crosstabs para cada pergunta de análise

## 📊 Principais achados

- **Ameaça** e **Estelionato** são os crimes mais registrados no estado.
- Os **estelionatos** concentram-se principalmente entre manhã e tarde (mais de 83% dos casos), com idade mediana da vítima de **46 anos** — o golpe atinge majoritariamente adultos economicamente ativos, não apenas idosos.
- O crime muda de perfil ao longo do dia: durante o horário comercial predominam ocorrências em vias públicas e estabelecimentos; à noite, os registros migram para dentro de residências.
- Furtos superam roubos com folga tanto para celulares (11.904 vs. 2.408) quanto para veículos (13.488 vs. 1.790) — o criminoso tende a evitar contato direto com a vítima.
- Mulheres são maioria entre as vítimas de ameaça e lesão corporal; em estelionato, a distribuição por sexo é praticamente equilibrada.

## 📦 Como rodar

**No Google Colab (recomendado):**
1. Abra o notebook no Colab
2. Monte seu Google Drive com o arquivo `SPJ_OCORRENCIAS_2025.xlsx` na pasta indicada
3. Execute as células em ordem

**Localmente:**
1. Coloque o arquivo `SPJ_OCORRENCIAS_2025.xlsx` na mesma pasta do notebook
2. Instale as dependências: `pip install pandas numpy seaborn matplotlib openpyxl`
3. Execute o notebook célula a célula

## 📈 Painel complementar (etapa 2)

Este projeto também gerou um dashboard interativo em Power BI, com visão geral do estado, detalhamento por bairro de Porto Alegre e uma página de perfil das vítimas. O dashboard cruza os dados de ocorrências com estimativas populacionais do IBGE para calcular a taxa de criminalidade por 100 mil habitantes.

Todo o modelo de relacionamento entre as tabelas e as medidas DAX utilizadas estão documentados em [`medidas_dax.md`](./medidas_dax.md).

## 👤 Autor

Caio Cristiano Antunes Assmann
