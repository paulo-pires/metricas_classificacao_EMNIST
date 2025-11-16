🚀 Meu Estudo sobre Métricas de Classificação com MNIST

🎯 O Desafio Original

Esta foi a descrição do desafio que motivou este projeto:

Descrição do Desafio: Cálculo de Métricas de Avaliação de Aprendizado

Neste projeto, vamos calcular as principais métricas para avaliação de modelos de classificação de dados, como acurácia, sensibilidade (recall), especificidade, precisão e F-score. Para que seja possível implementar estas funções, você deve utilizar os métodos e suas fórmulas correspondentes (Tabela 1).

Para a leitura dos valores de VP, VN, FP e FN, será necessário escolher uma matriz de confusão para a base dos cálculos.

Tabela 1: Fórmulas Implementadas

Com base no desafio, implementei as seguintes fórmulas no script Python:

Acurácia: (VP + VN) / (VP + VN + FP + FN)

Precisão (P): VP / (VP + FP)

Sensibilidade (S / Recall): VP / (VP + FN)

Especificidade: VN / (VN + FP)

F-Score: 2 * (Precisão * Sensibilidade) / (Precisão + Sensibilidade)

📖 Descrição do Estudo

Neste projeto. Em vez de apenas usar model.evaluate() ou classification_report(), assim foi necessario construir as métricas mais importantes (Acurácia, Precisão, Sensibilidade, etc.) a partir das suas fórmulas básicas.

O principal desafio foi que as métricas que eu queria calcular (VP, VN, FP, FN) são para classification binária (sim/não), mas o MNIST é multi-classe (dígitos 0-9).

💡 Conceitos que Apliquei

Este projeto foi centrado em dois conceitos fundamentais:

1. Métricas de Avaliação Fundamentais

Eu queria entender a fundo o que cada métrica realmente significa e como ela é calculada:

VP (Verdadeiro Positivo): Acertei, era positivo.

VN (Verdadeiro Negativo): Acertei, era negativo.

FP (Falso Positivo): Errei, chamei de positivo mas era negativo (Erro Tipo I).

FN (Falso Negativo): Errei, chamei de negativo mas era positivo (Erro Tipo II).

A partir disso, construí as funções para:

Acurácia: De todas as previsões, quantas eu acertei no total? (Bom, mas enganoso).

Precisão: De todos que eu disse serem positivos, quantos realmente eram? (Mede a "confiança" da minha previsão positiva).

Sensibilidade (Recall): De todos que realmente eram positivos, quantos eu consegui encontrar? (Mede a "capacidade de busca" do modelo).

Especificidade: De todos que realmente eram negativos, quantos eu acertei? (Mede o quão bem o modelo ignora os negativos).

F-Score: Uma média harmônica entre Precisão e Sensibilidade, ótima para quando as classes estão desbalanceadas.

2. Adaptação "Um-contra-Todos" (One-vs-Rest)

Este foi o "pulo do gato" do projeto. Como aplicar as métricas acima no MNIST?

O que eu fiz: Eu escolhi arbitrariamente uma classe para ser a minha "Classe Positiva". No meu script, escolhi o dígito '5'.

Positivo: O dígito é '5'.

Negativo: O dígito é qualquer outra coisa (0, 1, 2, 3, 4, 6, 7, 8, 9).

Vantagem: Isso transformou o problema multi-classe em um problema binário ("É 5 ou não é 5?"), permitindo-me extrair VP, VN, FP e FN da matriz de confusão 10x10.

💾 O Conjunto de Dados: MNIST

Fonte: tensorflow.keras.datasets.mnist.

Minha Solução: Tratar o problema como binário, focando em uma única classe (o dígito '5') como "Positivo" e todas as outras 9 classes como "Negativo".

🔬 Minha Metodologia (Pipeline do Código)

Eu estruturei meu script em 8 etapas claras:

1. Funções de Métricas

Defini funções Python (calcular_acuracia, calcular_precisao, etc.) que recebem VP, VN, FP, FN e retornam o resultado da fórmula.

2. Carregamento dos Dados

Carreguei o MNIST usando keras.datasets.mnist.load_data().

3. Pré-processamento

Normalizei as imagens (dividindo por 255.0) para otimizar o treinamento.

4. Criação do Modelo

Criei uma Rede Neural sequencial com Keras (Flatten, Dense, Dropout, Dense).

5. Treinamento

Compilei o modelo e treinei por 5 épocas. O suficiente para ter um modelo que acerta e erra.

6. Avaliação e Previsão

Usei model.predict() no conjunto de teste para obter as previsões do modelo.

7. Geração da Matriz de Confusão

Usei tf.math.confusion_matrix() para gerar a matriz 10x10.

8. Extração e Cálculo

A parte crucial: Eu usei a matriz 10x10 para extrair os valores para a minha classe-alvo (dígito '5'):

VP = cm[5, 5]

FN = Soma da linha 5 - VP

FP = Soma da coluna 5 - VP

VN = Soma total da matriz - (VP + FP + FN)

Finalmente, alimentei esses 4 valores nas minhas funções da Etapa 1 e imprimi os resultados.

