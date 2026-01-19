# Etapa 2 — Decisões de Stack Tecnológica (MVP)

## Princípios de Decisão

Antes de falar de tecnologias específicas, os critérios que guiam todas as escolhas neste projeto são:

- Clareza > Sofisticação
- Aprendizado técnico real > Hype
- Baixo overhead operacional
- Facilidade de iteração
- Stack comum de mercado (evitar exotismos)

Este não é um projeto para otimizar custo ou escala extrema.

## Visão Geral da Stack (Resumo)

| Camada | Escolha para o MVP |
|--------|---------------------|
| Frontend | React + Web |
| Backend | Node.js |
| API | HTTP (REST simples) |
| Processamento | Backend monolítico (MVP) |
| Speech-to-Text | API externa |
| Busca | Busca semântica |
| Armazenamento | Temporário (in-memory ou file-based) |
| Infraestrutura | Simples (1 serviço backend) |

Cada item é detalhado abaixo.

## Frontend

### Escolha

Aplicação Web em React

### Justificativa

Interface centrada em:

- Upload de arquivos
- Estados de processamento
- Lista de resultados

React é:

- Amplamente usado no mercado
- Adequado para fluxos assíncronos
- Familiar para evolução futura

Não há necessidade de:

- Mobile
- Offline
- Alta interatividade gráfica

### O que NÃO usar no MVP

- Frameworks mobile (React Native, Flutter)
- SSR avançado como requisito inicial
- Complexidade de autenticação

## Backend

### Escolha

Node.js (JavaScript ou TypeScript)

### Justificativa

Forte ecossistema para:

- Manipulação de arquivos
- Integrações com APIs externas

Excelente para:

- Orquestrar pipelines
- Lidar com I/O

Facilita:

- Iteração rápida
- Debug
- Leitura do código como material de estudo

### O que foi considerado e descartado

| Alternativa | Motivo do descarte |
|-------------|---------------------|
| Go | Overhead maior para MVP educacional |
| Python | Menor familiaridade com pipeline web completo |
| Microserviços | Complexidade desnecessária |

## Comunicação Frontend ↔ Backend

### Escolha

HTTP REST simples

### Justificativa

- Upload de arquivos
- Polling de status
- Busca síncrona

Não há necessidade de:

- WebSockets
- GraphQL
- Streaming em tempo real

## Speech-to-Text

### Escolha

Serviço externo de Speech-to-Text (API pronta)

### Justificativa

O foco do projeto não é IA de baixo nível

Resolver o problema real mais rápido

Permite concentrar aprendizado em:

- Pipeline
- Busca
- Arquitetura

Premissas

- Qualidade da transcrição é suficiente para busca
- Custo será limitado por:
  - Tamanho do MVP
  - Limite de uploads

### O que NÃO fazer

- Treinar modelos próprios
- Ajustes finos de áudio
- Processamento offline complexo

## Busca: Keyword vs Semântica

### Decisão para o MVP

Busca semântica

### Justificativa

O input do usuário é descritivo, por exemplo:

“Estou procurando um áudio onde eu disse que iria casar”

Esse tipo de busca:

- Não funciona bem com keyword pura
- Exige interpretação de contexto

Busca semântica:

- Tolera variação de linguagem
- É mais alinhada ao problema real
- Reduz frustração do usuário

Risco aceito

- Resultados podem ser menos previsíveis
- Necessidade de explicar o comportamento ao usuário

## Armazenamento

### Decisão para o MVP

Armazenamento temporário

Opções aceitáveis:

- Em memória
- Em arquivos locais
- Com limpeza automática

### Justificativa

- Não há login
- Não há histórico entre sessões
- Privacidade é prioridade
- Menor complexidade operacional

Persistência definitiva não agrega valor nesta fase.

## Infraestrutura

### Escolha

Backend único (monolítico)

### Justificativa

- Pipeline sequencial
- Baixa concorrência
- Facilidade de debug
- Melhor para aprendizado

Arquitetura já preparada conceitualmente para:

- Workers
- Filas
- Processamento assíncrono

Mas não agora.

## Observabilidade (mínimo necessário)

### O que é necessário no MVP

- Logs por etapa do pipeline
- Mensagens claras de erro
- Identificação de falhas por etapa

### O que NÃO é necessário

- Tracing distribuído
- Dashboards avançados
- Métricas de produção

## Trade-offs Aceitos Explicitamente

| Trade-off | Por que aceitar |
|-----------|-----------------|
| Processamento lento | Clareza > performance |
| Uso de APIs externas | Foco no problema real |
| Backend monolítico | Menor complexidade |
| Busca semântica | Melhor UX para o caso |

Esses trade-offs são intencionais e documentados.

## Decisões Abertas (para próximas etapas)

- Qual API específica de Speech-to-Text?
- Como representar embeddings e similaridade?
- Limites exatos de tamanho e duração dos áudios
- Estratégia de fallback se STT falhar
- UX de feedback durante processamento

## Critério de Qualidade da Stack

A stack é considerada adequada se:

- Um engenheiro pleno conseguir entender o sistema em 1 leitura
- O pipeline puder ser explicado sem diagramas complexos
- Mudanças futuras não exigirem reescrita completa
- O foco permanecer no aprendizado, não na infraestrutura