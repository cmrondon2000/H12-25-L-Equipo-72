# H12-25-L – Equipe 72

## Projeto 1: SentimentAPI — Análise de Sentimentos de Feedbacks ONE

## 1. Análise do Projeto

### 1.1 Problema de Negócio
As empresas recebem grandes volumes de comentários (avaliações, pesquisas, redes sociais) e não conseguem analisá-los manualmente. Isso gera:

- Lentidão no atendimento ao cliente  
- Perda de oportunidades de melhoria  
- Dificuldade em medir a satisfação ao longo do tempo  

### 1.2 Solução Proposta
Uma API de análise de sentimentos que:

- Recebe um texto como entrada  
- Classifica o sentimento (Positivo / Neutro / Negativo ou binário)  
- Retorna a predição com sua probabilidade  
- Pode ser facilmente integrada a outros sistemas  

### 1.3 Valor para o Cliente
Mesmo com um modelo simples, permite:

- Priorização de comentários negativos  
- Monitoramento de campanhas de marketing  
- Detecção de tendências de satisfação  
- Redução de esforço manual  

### 1.4 Escopo Técnico Adequado ao Hackathon
- Modelo clássico de NLP: TF-IDF + Regressão Logística ou Naive Bayes  
- API REST simples  
- Integração clara entre Data Science (DS) e Back-End (BE)  
- MVP funcional > sofisticação excessiva  

---

## FASE 1 – Definição e Alinhamento (DS + BE)

### 1.1 Definir Escopo Funcional (Conjunto)
**Responsáveis:** DS + BE  
**Dependências:** Nenhuma  

Decidir:

- Classificação binária ou ternária  
- Idioma (Espanhol / Português / ambos)  
- Definir o contrato de integração:
  - Entrada  
  - Saída  

Este contrato libera o trabalho em paralelo.

---

## FASE 2 – Data Science: Preparação do Modelo

### 2.1 Coleta do Dataset
**Responsável:** Data Science  
**Dependências:** Fase 1  

- Reviews públicas, tweets, pesquisas, etc.  
- Dataset contendo:
  - Texto  
  - Rótulo de sentimento  

### 2.2 EDA e Limpeza dos Dados
**Responsável:** Data Science  
**Dependências:** 2.1  

- Remover duplicados  
- Normalizar texto (minúsculas, símbolos)  
- Análise da distribuição das classes  
- Visualizações básicas  

### 2.3 Feature Engineering (TF-IDF)
**Responsável:** Data Science  
**Dependências:** 2.2  

- Vetorização de texto com `TfidfVectorizer`  
- Ajuste de:
  - n-grams  
  - stopwords  
  - max_features (se aplicável)  

### 2.4 Treinamento do Modelo
**Responsável:** Data Science  
**Dependências:** 2.3  

Treinar:
- Regressão Logística ou  
- Naive Bayes  

- Separação treino/teste  

### 2.5 Avaliação do Modelo
**Responsável:** Data Science  
**Dependências:** 2.4  

Métricas:
- Accuracy  
- Precision  
- Recall  
- F1-score  

Decidir se o modelo é suficiente para um MVP.

### 2.6 Serialização do Modelo
**Responsável:** Data Science  
**Dependências:** 2.5  

- Salvar o pipeline completo  
- Entregar ao time de BE:
  - Arquivo `.joblib`  
  - Exemplo de inferência em Python  
  - Mapeamento de rótulos (ex.: `0 = Negativo`)  

Esta etapa é um **bloqueio crítico** para o Back-End.

---

## FASE 3 – Back-End: Construção da API

### 3.1 Criar Projeto Spring Boot
**Responsável:** Back-End  
**Dependências:** Fase 1  

Configuração básica:
- Spring Web  
- Validation  
- Logging  
- Estrutura limpa (controller, service, dto)  

### 3.2 Implementar Endpoint `/sentiment`
**Responsável:** Back-End  
**Dependências:** 3.1  

- `POST /sentiment`  
- DTO de entrada  

### 3.3 Validação e Tratamento de Erros
**Responsável:** Back-End  
**Dependências:** 3.2  

- Texto vazio ou muito curto → `400 Bad Request`  
- Mensagens claras e consistentes em JSON  

### 3.4 Integração com o Modelo de DS
**Responsável:** Back-End  
**Dependências:** 2.6  

**Opção A (mais simples):**
- Microsserviço em Python (FastAPI) com o modelo  
- BE consome via HTTP  

**Opção B (avançada):**
- Carregar modelo exportado (ONNX)  

A integração depende diretamente do entregável de DS.

---

## FASE 4 – Testes e Validação

### 4.1 Testes Manuais (Postman / cURL)
**Responsáveis:** BE + DS  
**Dependências:** Fase 3 concluída  

Três exemplos reais:
- Positivo  
- Neutro  
- Negativo  

### 4.2 Logs e Estabilidade
**Responsável:** Back-End  
**Dependências:** 4.1  

- Logs de requisições  
- Tratamento de exceções inesperadas  

---

## FASE 5 – Documentação e Entrega

### 5.1 Notebook Final (DS)
**Responsável:** Data Science  
**Dependências:** Fase 2 concluída  

- Código limpo  
- Comentários claros  
- Resultados e conclusões  

### 5.2 README do Projeto
**Responsáveis:** DS + BE  
**Dependências:** Fases 2–4  

Incluir:
- Como executar o modelo  
- Como subir a API  
- Exemplos de request/response  
- Dependências e versões  

### 5.3 Preparação da Demo
**Responsáveis:** DS + BE  
**Dependências:** Todas as fases anteriores  

Mostrar:
- Chamada ao endpoint  
- Resposta do modelo  
- Explicação simples do fluxo  
