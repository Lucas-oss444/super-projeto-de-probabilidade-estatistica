# Super Projeto - Estatística e Probabilidade

Análise exploratória, tratamento de dados, aplicação do Teorema de Bayes,
algoritmos de classificação e dashboard interativo sobre um conjunto de dados
de operação de ride-sharing (corridas tipo Uber) referente a 2024.

## Sobre o projeto

O objetivo é, a partir de um conjunto de dados de reservas de corridas, prever a
situação de uma reserva (concluída ou cancelada) e comparar três abordagens:

1. Teorema de Bayes (implementado manualmente);
2. Dois algoritmos de classificação;
3. Comparação entre os três métodos em um dashboard interativo.

## Dataset

- Fonte: Kaggle - "Uber Ride Analytics Dashboard"
  (https://www.kaggle.com/datasets/yashdevladdha/uber-ride-analytics-dashboard)
- Volume: aproximadamente 148.770 reservas (ano de 2024).
- Variável alvo: `Booking Status`, reduzida a duas classes (Concluída x Cancelada).
- O arquivo bruto deve ser colocado em `data/` (não versionar arquivos muito grandes;
  ver `.gitignore`).

## Pipeline do projeto

1. Tratamento e limpeza dos dados (`notebooks/01_limpeza_tratamento.ipynb`)
2. Análise Exploratória de Dados (`notebooks/02_eda.ipynb`)
3. Teorema de Bayes manual (`notebooks/03_teorema_bayes.ipynb`)
4. Algoritmos de classificação (`notebooks/04_classificacao.ipynb`)
5. Dashboard interativo (`dashboard/app.py`)

## Estrutura do repositório

```
projeto-uber/
├── data/            # dataset bruto e versão tratada
├── notebooks/       # notebooks de limpeza, EDA, Bayes e classificação
├── src/             # funções reutilizáveis (.py)
├── dashboard/       # aplicação do dashboard interativo
├── requirements.txt
├── .gitignore
└── README.md
```

## Requisitos e instalação

Requer Python 3.10 ou superior.

```
pip install -r requirements.txt
```

No Google Colab, instale o que faltar na primeira célula:

```python
!pip install -r requirements.txt
```

## Como executar

Notebooks (ordem recomendada):

1. `01_limpeza_tratamento.ipynb` - gera `data/dataset_tratado.csv`
2. `02_eda.ipynb`
3. `03_teorema_bayes.ipynb`
4. `04_classificacao.ipynb`

Dashboard (local):

```
go live app.html
```

## Equipe e divisão de tarefas

| Integrante      | Github                          |
|-----------------|---------------------------------|
| Lucas Jatene    | https://github.com/Lucas-oss444 |
| Gustavo alencar |https://github.com/Galencar14    |
| Cauê Milhomem   |https://github.com/caueroc       |

