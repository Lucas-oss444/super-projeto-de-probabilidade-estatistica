# Plano de Tratamento, Variável Alvo e Metodologia

Este documento registra as decisões metodológicas do projeto. Ele alimenta
diretamente a seção "Tratamentos aplicados" do relatório técnico, em que cada
decisão precisa de justificativa técnica e impacto esperado.

> Observação importante: confira os nomes exatos das colunas no arquivo bruto
> antes de rodar o código. Os nomes abaixo seguem a estrutura típica do dataset
> e podem variar (maiúsculas, espaços, etc.).

## 1. Definição do problema e da variável alvo

- Pergunta de investigação: dados os atributos de uma reserva, qual a
  probabilidade de ela ser cancelada?
- Variável alvo (categórica): `Booking Status`.
- Como o `Booking Status` original tem várias categorias (ex.: Completed,
  Cancelled by Customer, Cancelled by Driver, No Driver Found, Incomplete),
  vamos derivar uma variável binária:
  - `Concluida`  -> status "Completed"
  - `Cancelada`  -> demais status (cancelamentos e não atendidas)

Justificativa: a versão binária deixa o Teorema de Bayes mais didático
(probabilidades a priori e verossimilhanças mais interpretáveis) e o problema
de classificação mais direto. A versão multiclasse pode ser explorada como
extensão.

## 2. Variáveis preditoras candidatas

Quantitativas: `Booking Value`, `Ride Distance`, `Avg VTAT`, `Avg CTAT`.
Qualitativas: `Vehicle Type`, `Payment Method`, mais variáveis derivadas de
data/hora (faixa de horário, dia da semana, mês).

### Alerta de vazamento de dados (data leakage)

Algumas colunas só existem DEPOIS que o desfecho aconteceu e não podem ser
usadas para prever o cancelamento, pois "entregam" a resposta:

- `Reason for cancelling by Customer`, `Driver Cancellation Reason`,
  `Incomplete Rides Reason`, `Cancelled Rides by Customer`,
  `Cancelled Rides by Driver`, `Incomplete Rides`;
- `Driver Ratings` e `Customer Rating` (normalmente só existem em corridas
  concluídas).

Essas colunas devem ser removidas do conjunto de preditoras (mas podem ser
usadas na EDA para entender os motivos de cancelamento).

## 3. Plano de tratamento

| # | Problema | Ação | Justificativa técnica | Impacto esperado |
|---|----------|------|-----------------------|------------------|
| 1 | Valores ausentes condicionais (valor, distância e avaliações nulos em corridas canceladas) | Entender o motivo do nulo; tratar como ausência estrutural (não aleatória) | A ausência é informativa (depende do desfecho); imputar cegamente distorceria a análise | Preserva o sinal de cancelamento sem introduzir viés |
| 2 | Vazamento de dados (colunas de motivo/avaliação) | Remover do conjunto de preditoras | Essas colunas só existem após o desfecho; usá-las inflaria artificialmente o desempenho | Modelo honesto e resultados realistas |
| 3 | Tipos incorretos (`Date`, `Time` como texto) | Converter para datetime e extrair hora, dia da semana e mês | Permite engenharia de atributos temporais e análise de sazonalidade | Habilita preditoras temporais relevantes |
| 4 | Valores numéricos como texto (`Booking Value`, `Ride Distance`) | Converter para numérico, tratando símbolos/separadores | Necessário para estatísticas, gráficos e modelos | Viabiliza cálculos e padronização |
| 5 | Duplicatas (mesmo `Booking ID`) | Remover registros duplicados | Duplicatas enviesam contagens e probabilidades | Estatísticas e prioris corretas |
| 6 | Outliers em valor e distância | Detectar por IQR/boxplot; tratar valores implausíveis (ex.: distância 0 ou negativa) | Outliers distorcem médias, correlações e a verossimilhança | Distribuições mais representativas |
| 7 | Categorias inconsistentes (grafia de `Vehicle Type`, `Payment Method`) | Padronizar rótulos (minúsculas, remover espaços) | Evita que a mesma categoria seja contada como duas | Contagens e codificações corretas |
| 8 | Alta cardinalidade em localização (`Pickup`, `Drop`) | Agrupar categorias raras ou usar top-N | Reduz dimensionalidade e ruído | Modelos mais estáveis |
| 9 | Desbalanceamento da classe alvo | Documentar a proporção; usar divisão estratificada e/ou pesos de classe | Classe rara é mal aprendida e enviesa a priori | Avaliação mais justa entre classes |
| 10 | Escalas diferentes entre variáveis numéricas | Padronizar/normalizar para KNN e Regressão Logística | Algoritmos baseados em distância/gradiente são sensíveis à escala | Convergência e desempenho melhores |
| 11 | Codificação de categóricas para os modelos | One-hot ou label encoding para os classificadores | Modelos exigem entrada numérica | Permite treinar os algoritmos |

Observação: para o Teorema de Bayes manual, mantenha também as variáveis
categóricas e numéricas binadas (em faixas), pois o cálculo de verossimilhança
por contagem precisa de categorias discretas.

## 4. Teorema de Bayes (esboço da implementação manual)

Objetivo: calcular P(Classe | Atributos) para uma nova reserva.

1. Probabilidade a priori: P(C) = numero de registros da classe C / total.
2. Verossimilhança por atributo: P(X_i | C) = contagem(X_i e C) / contagem(C).
   - Use suavização de Laplace para evitar probabilidade zero quando uma
     combinação não aparece no treino.
   - Variáveis contínuas (valor, distância): ou binar em faixas, ou usar a
     distribuição normal (Naive Bayes Gaussiano).
3. Posteriori (hipótese Naive Bayes - independência condicional):
   P(C | X) proporcional a P(C) * produto de P(X_i | C).
   - Normalize dividindo pela soma das duas classes.
4. Interpretação: explicar o que cada probabilidade significa no contexto.

Importante: implementar os passos 1 a 3 com pandas (groupby/value_counts), sem
depender só de biblioteca, e só depois comparar com `CategoricalNB`/`GaussianNB`
do scikit-learn como validação. A rubrica exige compreensão genuína das etapas.

## 5. Algoritmos de classificação

- Sugestão: Árvore de Decisão e Regressão Logística (ou KNN / Random Forest).
- Métricas: acurácia, precisão, recall, F1-score e matriz de confusão.
- Comparar os dois algoritmos entre si e com a abordagem bayesiana.
- Usar divisão treino/teste estratificada e a mesma divisão para todos os
  métodos, garantindo comparação justa.
