# Otimização de Telemetria com IA Generativa (Google Gemini)

> **Projeto de TCC:** Pipeline de engenharia de dados para classificação automatizada de feedbacks operacionais em máquinas de construção.

## O Desafio de Negócio
No setor de máquinas de construção, o encerramento de alertas técnicos (falhas) advindos da telemetria gera milhares de **comentários em texto livre** (dados não estruturados) preenchidos pelos concessionários no momento do encerramento do alerta. 
A análise manual de +5.000 registros era inviável, resultando em "Dark Data" — dados coletados mas não utilizados para decisão estratégica.

**Objetivo:** Criar um classificador automatizado para transformar texto em KPIs de gestão (ex: identificar falsos positivos e falhas de atendimento).

## Arquitetura da Solução

A solução foi desenvolvida em Python utilizando a API do **Google Gemini 2.5-flash**. O grande diferencial técnico foi a **estratégia de ingestão em lotes** para contornar limitações de API.

### 1. Descoberta de Padrões (Fase 1)
Utilizei técnicas de *Prompt Engineering* (Few-Shot Learning) em uma amostra pequena para que a IA identificasse autonomamente as 5 categorias principais do negócio.

### 2. Engenharia de Batching (Fase 2)
A API gratuita possui um *Rate Limit* (limite de requisições). Processar linha por linha (`row-by-row`) gerava erro `429 Resource Exhausted`.

**Solução Implementada:**
- Desenvolvi um loop que agrupa os comentários em **lotes de 50 itens**.
- Envia um único payload JSON para a IA contendo os 50 textos.
- A IA processa e retorna um array JSON estruturado.
- **Resultado:** Redução de **98%** no número de chamadas HTTP e aumento de 10x na velocidade de processamento.
