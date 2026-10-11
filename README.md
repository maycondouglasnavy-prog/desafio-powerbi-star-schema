# 📊 Processamento de Dados com Power BI: Construção de Modelo Star Schema

Este projeto consiste na transformação e modelagem dimensional de uma base única de dados financeiros (`Financial Sample`) em um modelo baseado na arquitetura **Star Schema** (Esquema em Estrela) no Power BI.

---

## 🎯 Objetivo do Projeto

O objetivo principal foi reestruturar a tabela única de origem em um modelo relacional otimizado para análise de inteligência de negócios (BI), separando as informações em **tabelas fato** (que contêm métricas e transações) e **tabelas dimensão** (que fornecem contexto às métricas).

---

## 📐 Arquitetura do Modelo Relacional

O esquema em estrela construído é composto pelas seguintes tabelas:

![Modelo Star Schema](imagesmodelo_star_schema.png)

### 1. Tabela Origem e Backup
* **`financials_origem`**: Tabela original mantida como backup em modo oculto, garantindo a integridade dos dados primários.

### 2. Tabela Fato
* **`F_Vendas`**: Concentra os registros transacionais de vendas
  * **Campos**: `SK_ID`, `ID_Produto`, `Product`, `Units Sold`, `Gross Sales`, `Discounts`, `Profit`, `Discount Band`, `Country`, `Date`, etc.

### 3. Tabelas Dimensão
* **`D_Produtos`**: Tabela agregada com estatísticas descritivas por produto.
  * **Métricas**: Média de unidades vendidas, média do valor de venda, mediana do valor de venda, preço máximo e mínimo de venda.
* **`D_Produtos_Detalhes`**: Detalhes adicionais e atributos dos produtos (como `Manufacturing Price`, `Sale Price`, `Discount Band`).
* **`D_Descontos`**: Mapeamento dos descontos por produto e faixa.
* **`D_Detalhes`**: Informações adicionais de vendas e contexto operacional.
* **`D_Calendario`**: Tabela temporal para suporte a análises baseadas em inteligência temporal (*Time Intelligence*).

---

## ⚙️ Etapas de Construção e Funcionalidades Utilizadas

### 1. Transformação no Power Query
* **Duplicação e Seleção de Colunas**: A partir da tabela `Financials_origem`, foram criadas cópias e referências para filtrar e reorganizar as colunas pertencentes a cada dimensão.
* **Agrupamento de Dados (`Group By`)**: Utilizado na tabela `D_Produtos` para calcular métricas como Média, Mediana, Mínimo e Máximo dos preços de venda e manufatura.
* **Coluna Condicional (Índice de Produtos)**: Criação do campo `ID_Produto` atribuindo identificadores numéricos aos produtos (`Carretera = 0`, `Montana = 1`, `Paseo = 2`, `Velo = 3`, `VTT = 4`, `Amarilla = 5`).
* **Reorganização de Colunas e Chaves Surrogate**: Reordenação de campos (`SK_ID` e `ID_Produto`) para estruturar os relacionamentos no modelo.

### 2. Criação da Tabela Calendário com DAX
A tabela `D_Calendario` foi gerada via código DAX utilizando a função `CALENDARAUTO()`, criando dinamicamente colunas de apoio temporal:

```dax
D_Calendario = 
ADDCOLUMNS(
    CALENDARAUTO(),
    "Ano", YEAR([Date]),
    "Mês Num", MONTH([Date]),
    "Mês", FORMAT([Date], "MMMM", "pt-BR"),
    "Trimestre", "T" & FORMAT([Date], "Q"),
    "Ano-Mês", FORMAT([Date], "YYYY-MM")
)
```

---

## 🚀 Como Executar o Projeto

1. Baixe o arquivo `.pbix` na pasta `src/`.
2. Abra no **Power BI Desktop**.
3. Verifique o modelo de dados na aba **Exibição de Modelo**

---

## 👤 Autor

* **Maycon Douglas da Silva**
* GitHub: (https://github.com/maycondouglasnavy-prog)
* LinkedIn:(www.linkedin.com/in/maycon-silva-5721a2186)