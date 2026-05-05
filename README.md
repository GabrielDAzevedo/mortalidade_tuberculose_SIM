# ⚰️ Mortalidade por Tuberculose (SIM/DataSUS)

Este projeto contém um script em Python (Jupyter Notebook) desenvolvido para automatizar a extração de dados do Sistema de Informação sobre Mortalidade (SIM) e calcular o **Coeficiente de Mortalidade por Tuberculose por 100 mil habitantes**, agrupado por Unidade Federativa (UF) do Brasil.

## 🎯 Objetivo

Facilitar a obtenção e o processamento de dados de mortalidade pública, integrando os registros de óbitos por Tuberculose do DataSUS com as projeções populacionais do IBGE. O resultado final é uma planilha consolidada pronta para análises epidemiológicas e painéis de visualização.

## ✨ Funcionalidades

*   **Extração Automatizada:** Download direto dos arquivos do SIM via FTP do DATASUS utilizando a biblioteca `PySUS`.
*   **Filtro Histórico:** Processamento dos dados de mortalidade dos anos de 2019 a 2022.
*   **Filtro por Causa Básica (CID-10):** Seleção automática dos óbitos cuja Causa Básica (`CAUSABAS`) se enquadre nos códigos referentes à Tuberculose (A15 a A19).
*   **Cálculo Epidemiológico:** Cruzamento do número de óbitos com os dados populacionais do IBGE para o cálculo exato da taxa de mortalidade por 100 mil habitantes (`(Óbitos / População) * 100.000`).
*   **Exportação Organizada:** Geração de um arquivo Excel consolidado (`TUB_MORT_GERAL.xlsx`), contendo uma aba (sheet) dedicada para cada ano analisado.

## 🛠️ Tecnologias e Bibliotecas Utilizadas

*   `Python 3`
*   `pandas` (Manipulação e estruturação dos dados)
*   `PySUS` (Interface de conexão com os bancos de dados do DataSUS/SIM)
*   `openpyxl` (Criação e manipulação de planilhas Excel)

## 🚀 Como Utilizar

### 1. Pré-requisitos
O notebook foi estruturado considerando o ambiente do **Google Colab** (utilizando o diretório `/content/`). Para rodar o código localmente ou no Colab, você precisará instalar as dependências principais:

```bash
pip install pysus
pip install --upgrade openpyxl pandas
```

### 2. Arquivos Necessários
Para que o cruzamento de dados e o cálculo do coeficiente funcionem corretamente, é **obrigatório** ter o arquivo com a projeção populacional do IBGE no diretório raiz de execução (`/content/` no Colab):
*   📄 `dados_ibge_projecao_tab_net_2022.xlsx` (A planilha deve conter as colunas `CODIGO_UF` e `POP_ANT`).

### 3. Execução
Basta executar as células do notebook sequencialmente. O script realiza o download granular por estado e ano (para evitar sobrecarga de memória) e salva os arquivos `.csv` temporários.

Para rodar a função principal de consolidação (que lê os arquivos baixados, filtra o CID-10 e calcula os coeficientes), o script executa:

```python
cria_dt()
```
*(Você pode passar parâmetros como `estado="RJ"` ou `anos=[2022]` para filtrar a geração da planilha final, caso necessário).*

## 📁 Estrutura de Diretórios Gerada

Durante a execução, o script criará a seguinte estrutura no seu ambiente:

```text
/content/
│
├── dados_ibge_projecao_tab_net_2022.xlsx  (Input manual necessário)
│
├── 2019/
│   ├── TUB_MORT_AC.csv
│   ├── TUB_MORT_AL.csv
│   └── ...
├── 2020/
├── 2021/
├── 2022/
│
└── TUB_MORT_GERAL.xlsx                    (Output Final Consolidado)
```

## ⚠️ Dicionário de Dados e Observações

*   **CID-10:** Classificação Estatística Internacional de Doenças e Problemas Relacionados à Saúde. Este script filtra as doenças infecciosas do bloco **A15 ao A19** (Tuberculose).
*   **CAUSABAS:** Causa básica da morte, conforme a CID-10, sendo o principal campo utilizado para a filtragem no SIM.
*   **Performance:** Os arquivos do SIM podem ser extensos. A rotina `baixa_DBSIM()` foi desenhada para baixar os estados iterativamente e salvá-los localmente em `.csv`, garantindo que você não precise refazer o download caso o kernel reinicie.
