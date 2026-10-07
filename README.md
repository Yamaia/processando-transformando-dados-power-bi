# 🔄 Processando e Transformando Dados com Power BI

Projeto de ETL no Power BI: importação de dados brutos, limpeza e transformação no **Power Query** e carga em um modelo pronto para análise.

Desenvolvido como desafio de projeto da [DIO](https://www.dio.me/).

## 🎯 Objetivo

Pegar uma base de dados "suja" e deixá-la pronta para análise, documentando cada etapa de transformação (Extract, Transform, Load) e mostrando o antes e o depois.

## 🗂️ Estrutura do repositório

```
├── dados/
│   ├── base-original.csv        # Dados brutos (antes)
│   └── base-tratada.csv         # Dados tratados (depois)
├── relatorio/
│   └── processando-transformando-dados.pbix
├── imagens/                     # Prints do Power Query e do relatório
│   ├── 01-importacao.png
│   ├── 02-transformacoes.png
│   └── 03-resultado.png
└── README.md
```

## 🛠️ Ferramentas

- Power BI Desktop
- Power Query (linguagem M)
- DAX
- GitHub

## 🔁 Etapas do processo

### 1. Extract (Importação)
- **Obter dados → Texto/CSV** (ou Excel) e carregar a base em **Transformar dados**
- Conferir a origem, o delimitador e a codificação (UTF-8)

### 2. Transform (Transformação no Power Query)

| # | Etapa | Como fazer |
|---|---|---|
| 1 | Promover cabeçalhos | Transformar → *Usar a Primeira Linha como Cabeçalhos* |
| 2 | Ajustar tipos de dados | Texto, Número Inteiro, Decimal, Data |
| 3 | Remover colunas desnecessárias | *Escolher Colunas* / *Remover Colunas* |
| 4 | Tratar valores nulos | *Substituir Valores* ou *Remover Linhas Vazias* |
| 5 | Remover duplicatas | *Remover Duplicatas* na coluna-chave |
| 6 | Remover erros | *Remover Erros* |
| 7 | Limpar textos | *Cortar*, *Limpar* e *Capitalizar Cada Palavra* |
| 8 | Dividir colunas | *Dividir Coluna → Por Delimitador* |
| 9 | Colunas condicionais | *Adicionar Coluna → Coluna Condicional* |
| 10 | Colunas personalizadas | *Adicionar Coluna → Coluna Personalizada* |
| 11 | Mesclar / Acrescentar consultas | Combinar tabelas relacionadas |
| 12 | Renomear colunas e consultas | Nomes claros e padronizados |

Exemplo do código M gerado:

```m
let
    Fonte = Csv.Document(File.Contents("dados/base-original.csv"), [Delimiter=",", Encoding=65001]),
    Cabecalhos = Table.PromoteHeaders(Fonte, [PromoteAllScalars=true]),
    Tipos = Table.TransformColumnTypes(Cabecalhos, {{"Data", type date}, {"Valor", type number}}),
    SemNulos = Table.SelectRows(Tipos, each [Valor] <> null),
    SemDuplicatas = Table.Distinct(SemNulos, {"ID"}),
    TextoLimpo = Table.TransformColumns(SemDuplicatas, {{"Nome", each Text.Proper(Text.Trim(_)), type text}})
in
    TextoLimpo
```

> Ajuste os nomes das colunas (`Data`, `Valor`, `ID`, `Nome`) para os da base usada no projeto.

### 3. Load (Carga e modelagem)
- **Fechar e Aplicar** para carregar as consultas no modelo
- Criar relacionamentos entre as tabelas (chaves primária e estrangeira)
- Criar uma tabela Calendário, se houver datas:

```DAX
Calendário = CALENDARAUTO()
```

### 4. Visualização
- Páginas com indicadores (cartões), gráficos e segmentadores sobre os dados tratados
- Publicação no Power BI Service

## 📊 Antes e depois

| Antes | Depois |
|---|---|
| Cabeçalhos na primeira linha | Cabeçalhos corretos |
| Tipos todos como texto | Tipos ajustados |
| Nulos, duplicatas e erros | Dados limpos e únicos |
| Textos inconsistentes | Textos padronizados |

Prints em [`imagens/`](imagens/).

## 🌐 Relatório publicado

🔗 [Acessar no Power BI Service](COLE_AQUI_O_LINK)

## 💡 Aprendizados

- Fluxo completo de ETL dentro do Power BI
- Uso do Power Query para limpar e padronizar dados sem código
- Leitura e edição de código M gerado nas etapas aplicadas
- Importância de tratar a base antes de montar visuais

## 👤 Autor

**Yamaia** — desenvolvedor focado em backend, cloud e arquitetura de soluções.

[GitHub](https://github.com/SEU_USUARIO) · [LinkedIn](https://linkedin.com/in/SEU_PERFIL)
