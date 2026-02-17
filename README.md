#  Breast Cancer Detection: SVM Analysis on Databricks

> **Estudo de Caso:** Comparativo entre Engenharia de Features (Spark ML) e Kernel Methods (Scikit-Learn) para classificação de dados médicos.

## Sobre o Projeto

Este projeto explora a implementação do algoritmo **Support Vector Machines (SVM)** em um ambiente de nuvem (**Databricks**). 

Embora o dataset utilizado (*Breast Cancer Wisconsin*) seja pequeno o suficiente para processamento local, optou-se intencionalmente pelo uso do **Apache Spark** para simular um pipeline de Engenharia de Machine Learning escalável. O objetivo foi validar como técnicas de *Large Scale Machine Learning* se comparam a métodos tradicionais *in-memory* em cenários de alta complexidade teórica (não-linearidade).

##  Objetivos de Negócio e Técnica

1.  **Infraestrutura:** Configurar um fluxo completo (End-to-End) no Databricks (Ingestão, Tratamento, Modelagem).
2.  **Teoria SVM:** Implementar os conceitos de *Large Margin Classification* e *Kernel Trick*.
3.  **Métrica Crítica:** Em diagnóstico de câncer, a **Acurácia** é secundária. O foco total deste estudo foi maximizar o **Recall (Sensibilidade)** para minimizar Falsos Negativos (pacientes doentes diagnosticados como saudáveis).

##  Tech Stack

* **Plataforma:** Databricks Community Edition
* **Processamento Distribuído:** PySpark (MLlib)
* **Modelagem Comparativa:** Scikit-Learn
* **Linguagem:** Python 3.x
* **Conceitos:** SVM, Pipelines, Polynomial Expansion, RBF Kernel.

##  A Batalha dos Modelos

Implementamos três abordagens distintas para resolver o problema de classificação:

### 1. Spark Linear SVC
* **Abordagem:** Modelo linear padrão escalável.
* **Resultado:** Sofreu *underfitting* devido à natureza não-linear das fronteiras de decisão do câncer.

### 2. Spark Polynomial SVC (Feature Engineering)
* **Abordagem:** Como o Spark não possui Kernel RBF nativo (devido ao custo computacional em Big Data), simulamos a não-linearidade expandindo as features matematicamente (`PolynomialExpansion` de grau 2).
* **Resultado:** Aumento significativo na acurácia, provando que a engenharia de dados pode compensar limitações algorítmicas.

### 3. Scikit-Learn SVC (RBF Kernel)
* **Abordagem:** Uso do "Kernel Trick" (Radial Basis Function) processando os dados em memória (Driver Node).
* **Resultado:** Capaz de desenhar fronteiras de decisão complexas e orgânicas ("ilhas" de decisão).

## 📊 Resultados Finais

| Modelo | Acurácia Global | Recall (Maligno) | Análise |
| :--- | :--- | :--- | :--- |
| **Spark Linear** | ~95.1% | 87.93% | Incapaz de capturar complexidade geométrica. |
| **Spark Polinomial** | ~97.9% | 94.83% | Ótima generalização, mas computacionalmente custoso (muitas features). |
| **Scikit-Learn RBF** | **~98.2%** | 96.83% | O melhor equilíbrio. O Kernel RBF conseguiu isolar os casos difíceis, maximizando a detecção de doentes. |

> **Conclusão:** Para este volume de dados, a abordagem matemática refinada do Scikit-Learn (RBF) superou a força bruta do Spark. No entanto, o pipeline do Spark está pronto para ser plugado em bases de Terabytes, onde o Scikit-Learn falharia.

## Aprendizados Chave

* **Databricks & Spark:** O uso de `VectorAssembler` e `Pipelines` garante que o pré-processamento (como `StandardScaler`, crucial para SVM) seja reproduzível em produção.
* **O Dilema do Kernel:** Em Big Data, muitas vezes trocamos algoritmos complexos (Kernels) por mais dados ou mais features (Expansão Polinomial).
* **Medicina vs. Matemática:** O ajuste de *Threshold* (limiar de decisão) foi essencial para priorizar a vida (Recall) em detrimento da precisão pura.

---
Desenvolvido por **Leonardo Bento Maria**
