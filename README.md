# Análise Exploratória da Criminalidade no Rio Grande do Sul — 2025

Análise exploratória de dados (EDA) sobre ocorrências criminais registradas no Rio Grande do Sul em 2025, com tratamento de dados, cruzamento de indicadores e visualização de padrões de criminalidade por município, bairro (Porto Alegre), tipo de crime, horário e perfil de vítimas.

## 🎯 Objetivo

Identificar padrões espaciais e temporais da criminalidade no RS a partir de dados oficiais, respondendo perguntas como:
- Quais crimes são mais frequentes no estado?
- Quais cidades concentram mais mortes violentas?
- Qual o bairro mais afetado em Porto Alegre?
- Existe um horário com maior incidência de golpes (estelionato)?
- O tipo e o local do crime mudam ao longo do dia?
- Há diferença de perfil de vítima (idade, sexo) entre os tipos de crime?

## 🗂️ Fonte dos dados

Secretaria de Segurança Pública do Rio Grande do Sul (SSP-RS) — Ocorrências registradas em 2025.

> **Nota:** o dataset bruto de ocorrências não está incluído neste repositório por conter informações sensíveis (dados de vítimas). Apenas o notebook com o pipeline de tratamento e análise está disponível.

## 🛠️ O que o notebook faz

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

## 📈 Painel complementar

Este projeto também gerou um dashboard interativo em Power BI com visão geral do estado e detalhamento por bairro de Porto Alegre. O dashboard cruza esses dados com estimativas populacionais do IBGE para calcular a taxa de criminalidade por 100 mil habitantes — esse cruzamento é feito só no Power BI, não neste notebook.

## 👤 Autor

Caio Cristiano Antunes Assmann
