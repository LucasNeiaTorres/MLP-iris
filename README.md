# Estudo e Otimização de uma Rede Neural MLP "From Scratch"

Este repositório documenta um estudo aprofundado e uma série de experimentos realizados sobre uma implementação de rede neural MLP (Perceptron de Múltiplas Camadas) para a classificação do clássico dataset Iris.

O objetivo principal não foi criar a rede do zero, mas sim analisar, depurar e otimizar uma implementação base existente para alcançar um desempenho robusto e generalizável.

* **Código Base:** A implementação do MLP foi adaptada do [notebook original de Vítor Gama Lemos](https://www.kaggle.com/code/vitorgamalemos/multilayer-perceptron-from-scratch). Todo o crédito pela arquitetura inicial do código vai para ele.

---

## A Jornada de Experimentação

O projeto seguiu uma jornada iterativa de análise e melhoria, passando por diversas fases:

### 1. O Sucesso Inicial e a Necessidade de um Teste Robusto
O primeiro modelo treinado alcançou uma acurácia de 100%. No entanto, uma análise crítica revelou que o conjunto de teste era pequeno e desbalanceado (contendo apenas 1 amostra da classe 'Iris-Virginica'), tornando o resultado pouco confiável.

### 2. O Colapso do Modelo
Para validar o modelo de forma adequada, um novo conjunto de teste foi criado, utilizando 30% dos dados e estratificação para manter a proporção das classes. Com esta nova divisão, o mesmo modelo falhou catastroficamente, atingindo apenas **44.44% de acurácia**.

A análise via **Matriz de Confusão** foi crucial para o diagnóstico. Ela revelou que o modelo havia desenvolvido um viés extremo, classificando incorretamente todas as amostras da classe 'Iris-Setosa'.

*Aqui você pode colar a imagem da matriz de confusão do modelo problemático:*
`![Matriz de Confusão do Modelo com Colapso de Classe](caminho/para/sua/matriz_ruim.png)`

### 3. Diagnóstico e Solução
A causa do problema foi uma combinação de hiperparâmetros inadequados para a nova e mais desafiadora divisão de dados. Problemas como a "ReLU Morrendo" (Dying ReLU) e a instabilidade causada por biases negativos foram investigados.

A solução foi construir um **modelo mais robusto**, com os seguintes princípios:
* **Maior Capacidade:** Aumentar o número de neurônios na camada oculta para permitir o aprendizado de fronteiras de decisão mais complexas.
* **Aprendizado Cauteloso:** Diminuir a taxa de aprendizado para evitar "saltos" para mínimos locais ruins.
* **Estabilidade:** Utilizar a função de ativação Sigmoid, que se provou mais estável para este problema, e inicializar todos os biases em 0.

---

## Resultado Final

Após a aplicação das melhorias, o modelo final alcançou uma performance excelente e realista no conjunto de teste robusto.

**Parâmetros do Melhor Modelo:**
```python
params_robusto = {
    'InputLayer': 4,
    'HiddenLayer': 60,
    'OutputLayer': 3,
    'Epocas': 4000,
    'LearningRate': 0.005,
    'BiasHiddenValue': 0,
    'BiasOutputValue': 0,
    'ActivationFunction': 'sigmoid'
}
```

**Performance Final:**
* **Acurácia:** **97.78%**
* **Análise dos Erros:** O único erro ocorreu na distinção entre 'Iris-Versicolour' e 'Iris-Virginica', que é a parte mais complexa do dataset.

**Matriz de Confusão do Modelo Final:**

*Aqui você pode colar a imagem da sua melhor matriz de confusão:*
`![Matriz de Confusão do Modelo Final Otimizado](caminho/para/sua/matriz_boa.png)`

---

## Principais Aprendizados
* A acurácia pode ser uma métrica enganosa se o conjunto de teste não for robusto e bem distribuído.
* A Matriz de Confusão é uma ferramenta indispensável para diagnosticar *como* um modelo está errando.
* Hiperparâmetros ótimos para uma divisão de dados podem não ser ótimos para outra.
* Estabilidade no treinamento (através de funções de ativação e taxas de aprendizado adequadas) é crucial para evitar colapsos e resultados ruins.

## Como Executar
1.  Abra o arquivo `.ipynb` no Google Colaboratory ou em um ambiente Jupyter.
2.  Garanta que as bibliotecas listadas abaixo estejam instaladas.
3.  Execute as células em ordem.

## Ferramentas Utilizadas
* Python
* NumPy
* Pandas
* Scikit-learn (para `train_test_split` e métricas de avaliação)
* Matplotlib & Seaborn (para visualização de dados)
