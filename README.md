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
├── relatorio/       # relatório técnico e declaração de uso de IA
├── PLANO_DE_TRATAMENTO.md
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
streamlit run dashboard/app.py
```

## Equipe e divisão de tarefas

| Integrante | Responsabilidade principal | Entregas |
|------------|----------------------------|----------|
| Integrante 1 | Tratamento e limpeza dos dados | Notebook 01 + seção de tratamentos do relatório |
| Integrante 2 | Análise exploratória e Seção 1 do dashboard | Notebook 02 + visualizações |
| Integrante 3 | Teorema de Bayes, classificadores e Seção 2 do dashboard | Notebooks 03 e 04 + comparação dos métodos |

Observação: a arguição é individual. Cada integrante deve dominar o projeto
inteiro, não apenas a própria parte. Recomenda-se que cada um faça os próprios
commits, para que o histórico do GitHub reflita a contribuição de todos.

## Entregas (conforme o enunciado)

- [ ] Nome completo de todos os integrantes
- [ ] Link do repositório GitHub com README
- [ ] Link do dataset (fonte original)
- [ ] Relatório técnico (`relatorio/`)
- [ ] Declaração de uso de IA generativa (`relatorio/DECLARACAO_IA.md`)

## Declaração de uso de IA generativa

A declaração obrigatória está em `relatorio/DECLARACAO_IA.md` e deve ser
preenchida de forma honesta pela equipe.
