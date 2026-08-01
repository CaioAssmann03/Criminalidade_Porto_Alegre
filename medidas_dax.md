# Medidas DAX — Painel Criminalidade RS 2025

## 1. Modelo de dados (antes de criar as medidas)

Importe os dois arquivos:
- `ocorrencias_rs_2025_tratado.csv` → tabela principal (fatos)
- `populacao_rs_2025_tratado.xlsx` → tabela de dimensão (população)

**Relacionamento:**
- `Ocorrencias[Municipio_Chave]` → `Populacao[Municipio_Chave]`
- Cardinalidade: **Muitos-para-um** (Ocorrências → População)
- Direção do filtro: **Único** (de População pra Ocorrências)

⚠️ **Atenção ao tipo de dado na importação:** ao carregar o CSV, confirme que "Quantidade Vítimas", "Idade Vítima" e "Hora" usam **Alterar Tipo → Usando Localidade → Inglês (Estados Unidos)**, senão o Power BI pode interpretar "1.0" como "10" (foi o bug que resolvemos antes).

---

## 2. Medidas — Visão Geral (RS)

```dax
Total_Ocorrencias = COUNTROWS(Ocorrencias)
```

```dax
Total_Vitimas = SUM(Ocorrencias[Quantidade Vítimas])
```

```dax
Populacao_Total_RS = SUM(Populacao[Populacao_2025])
```

```dax
Taxa_100mil =
DIVIDE(
    [Total_Ocorrencias],
    [Populacao_Total_RS]
) * 100000
```

```dax
Media_Diaria =
DIVIDE([Total_Ocorrencias], 365)
```

```dax
Crime_Mais_Frequente =
CALCULATE(
    SELECTEDVALUE(Ocorrencias[Tipo Enquadramento]),
    TOPN(1, VALUES(Ocorrencias[Tipo Enquadramento]), CALCULATE([Total_Ocorrencias]), DESC)
)
```

```dax
Municipio_Maior_Taxa =
CALCULATE(
    SELECTEDVALUE(Ocorrencias[Municipio Fato]),
    TOPN(
        1,
        SUMMARIZE(Ocorrencias, Ocorrencias[Municipio Fato]),
        CALCULATE([Taxa_100mil]),
        DESC
    )
)
```

---

## 3. Medidas — Página Porto Alegre

```dax
Total_Ocorrencias_POA =
CALCULATE(
    [Total_Ocorrencias],
    Ocorrencias[Municipio Fato] = "PORTO ALEGRE"
)
```

```dax
Taxa_100mil_POA =
CALCULATE(
    [Taxa_100mil],
    Ocorrencias[Municipio Fato] = "PORTO ALEGRE"
)
```

```dax
Bairro_Mais_Ocorrencias_POA =
CALCULATE(
    SELECTEDVALUE(Ocorrencias[Bairro]),
    TOPN(
        1,
        SUMMARIZE(
            FILTER(Ocorrencias, Ocorrencias[Municipio Fato] = "PORTO ALEGRE"),
            Ocorrencias[Bairro]
        ),
        CALCULATE([Total_Ocorrencias]),
        DESC
    )
)
```

```dax
Crime_Mais_Frequente_POA =
CALCULATE(
    [Crime_Mais_Frequente],
    Ocorrencias[Municipio Fato] = "PORTO ALEGRE"
)
```

---

## 3.1 Medidas — Página 3 (Curiosidades / Perfil das Vítimas)

```dax
Horario_Pico_Estelionato =
CALCULATE(
    SELECTEDVALUE(Ocorrencias[Hora]),
    FILTER(
        Ocorrencias,
        Ocorrencias[Tipo Enquadramento] = "ESTELIONATO"
    ),
    TOPN(
        1,
        SUMMARIZE(
            FILTER(Ocorrencias, Ocorrencias[Tipo Enquadramento] = "ESTELIONATO"),
            Ocorrencias[Hora]
        ),
        CALCULATE(COUNTROWS(Ocorrencias)),
        DESC
    )
)
```

```dax
Idade_Media_Estelionato =
CALCULATE(
    AVERAGE(Ocorrencias[Idade Vítima]),
    Ocorrencias[Tipo Enquadramento] = "ESTELIONATO",
    Ocorrencias[Idade Vítima] > 0
)
```

```dax
Total_Furto_Celular =
CALCULATE(
    COUNTROWS(Ocorrencias),
    Ocorrencias[Tipo Enquadramento] = "FURTO DE CELULAR"
)
```

```dax
Total_Roubo_Celular =
CALCULATE(
    COUNTROWS(Ocorrencias),
    Ocorrencias[Tipo Enquadramento] = "ROUBO DE CELULAR"
)
```

```dax
Proporcao_Furto_Roubo_Celular =
DIVIDE([Total_Furto_Celular], [Total_Roubo_Celular])
```

> Card de KPI: use uma medida de texto pra exibir tipo "5,0x mais furtos que roubos":
```dax
Texto_Proporcao_Celular =
FORMAT([Proporcao_Furto_Roubo_Celular], "0.0") & "x mais furtos que roubos"
```

```dax
Local_Mais_Comum =
CALCULATE(
    SELECTEDVALUE(Ocorrencias[Local Fato]),
    TOPN(
        1,
        SUMMARIZE(
            FILTER(Ocorrencias, Ocorrencias[Local Fato] <> "OUTROS"),
            Ocorrencias[Local Fato]
        ),
        CALCULATE(COUNTROWS(Ocorrencias)),
        DESC
    )
)
```

> Nota: essa medida exclui a categoria "OUTROS" de propósito — ela é a mais frequente (330.939 registros) mas não diz nada de específico. Sem ela, o resultado correto é "RESIDÊNCIA" (217.713 registros), que é o dado que realmente vale destacar no card.

**Medidas de apoio pros gráficos dessa página:**

```dax
Total_Vitimas_Femininas =
CALCULATE(
    SUM(Ocorrencias[Quantidade Vítimas]),
    Ocorrencias[Sexo Vítima] = "Feminino"
)
```

```dax
Total_Vitimas_Masculinas =
CALCULATE(
    SUM(Ocorrencias[Quantidade Vítimas]),
    Ocorrencias[Sexo Vítima] = "Masculino"
)
```

```dax
Total_Lesao_Corporal_Residencia_Feminino =
CALCULATE(
    COUNTROWS(Ocorrencias),
    Ocorrencias[Tipo Enquadramento] = "LESAO CORPORAL",
    Ocorrencias[Local Fato] = "RESIDENCIA",
    Ocorrencias[Sexo Vítima] = "Feminino"
)
```

⚠️ **Atenção:** confira o texto exato usado na coluna `Tipo Enquadramento` pra "Furto de Celular" e "Roubo de Celular" — no seu dataset original os nomes podem vir como "FURTO DE CELULAR" e "ROUBO DE CELULAR" (com acento removido/maiúsculo). Ajuste as strings nas medidas acima se o nome exato na sua base for diferente. Use `DISTINCT(Ocorrencias[Tipo Enquadramento])` numa tabela auxiliar pra conferir a grafia exata antes de aplicar.

---

## 4. Medidas auxiliares (variação % — opcional, se tiver mais de um ano de dados)

```dax
Ocorrencias_Mes_Anterior =
CALCULATE(
    [Total_Ocorrencias],
    DATEADD(Ocorrencias[Data Fato], -1, MONTH)
)
```

```dax
Variacao_Percentual =
DIVIDE(
    [Total_Ocorrencias] - [Ocorrencias_Mes_Anterior],
    [Ocorrencias_Mes_Anterior]
)
```

> Nota: como sua base é só de 2025, a variação mês a mês funciona (compara janeiro com dezembro do ano anterior não vai ter dado). Para variação "ano vs ano anterior" seria necessário importar 2024 também.

---

## 5. Checklist antes de publicar

- [ ] Conferir se `Total_Vitimas` bate com ~674 mil (não 6,7 milhões)
- [ ] Conferir se `Taxa_100mil` geral bate com ~6.799 (não 67.990 nem 679)
- [ ] Conferir se `Taxa_100mil_POA` bate com ~9.432
- [ ] Testar clique em uma barra do gráfico "Top Municípios" e ver se todos os visuais da página reagem (Editar Interações)
- [ ] Ocultar/remover o painel de filtros na versão final antes de exportar como imagem para o LinkedIn
