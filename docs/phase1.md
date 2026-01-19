# Arquitetura de Alto Nível — Busca em Áudios do WhatsApp (MVP)

## Visão Geral da Arquitetura

O sistema será uma aplicação web com backend responsável por orquestrar um pipeline de processamento de arquivos, cujo objetivo é transformar um histórico exportado do WhatsApp em uma base pesquisável de áudios transcritos.

A arquitetura do MVP prioriza:

- Clareza
- Separação de responsabilidades
- Simplicidade operacional
- Facilidade de evolução futura

Não há necessidade de alta disponibilidade ou escala neste estágio.

## Componentes Principais

### Frontend (Web Client)

Responsabilidades:

- Upload do arquivo .zip
- Coleta do texto de busca em linguagem natural
- Exibição do estado do processamento
- Apresentação dos resultados

Limites:

- Não processa arquivos
- Não executa transcrição
- Não realiza busca local

### Backend (API + Orquestração)

Responsável por:

- Receber o arquivo .zip
- Validar formato e tamanho
- Coordenar o pipeline de processamento

Expor endpoints para:

- Upload
- Consulta de status
- Busca de resultados

Atua como orquestrador, não como executor pesado sempre que possível.

### Pipeline de Processamento

O pipeline é o núcleo do sistema.
Ele deve ser composto por etapas bem definidas e desacopladas.

Etapas conceituais:

- Extração do .zip
- Leitura do histórico de mensagens
- Identificação de mensagens de áudio
- Conversão de áudio para texto (Speech-to-Text)
- Normalização e enriquecimento dos dados
- Indexação para busca
- Consulta e ranqueamento de resultados

Cada etapa deve poder:

- Falhar isoladamente
- Ser observável
- Evoluir sem impactar as demais

### Armazenamento (Temporário no MVP)

Tipos de dados manipulados:

- Arquivos de áudio
- Transcrições
- Metadados (autor, data, origem)

No MVP:

- Armazenamento temporário
- Vida curta dos dados
- Sem necessidade de versionamento

A arquitetura deve permitir decidir posteriormente:

- Persistência em banco
- Processamento 100% em memória
- Limpeza automática (TTL)

## Pipeline de Processamento — Detalhamento

### Etapa 1 — Upload e Validação

Responsabilidades:

- Receber o .zip

Validar:

- Tipo do arquivo
- Tamanho máximo permitido

Rejeitar inputs inválidos o mais cedo possível

Decisão:

- Falhar rápido (fail fast)

### Etapa 2 — Extração e Leitura

Responsabilidades:

- Extrair os arquivos do .zip

Identificar:

- Arquivo de histórico textual
- Arquivos de áudio associados

Risco:

Dependência do formato de exportação do WhatsApp

Mitigação:

- Isolar parsing em um módulo específico

### Etapa 3 — Identificação de Áudios

Responsabilidades:

Mapear:

- Arquivo de áudio
- Autor
- Data
- Referência no histórico

Resultado esperado:

Lista estruturada de áudios prontos para transcrição

### Etapa 4 — Speech-to-Text

Responsabilidades:

- Converter áudio em texto

Retornar:

- Texto bruto
- Confiança (se disponível)

Características:

- Operação lenta
- Potencialmente custosa
- Sujeita a falhas externas

Decisão:

- Tratar como etapa isolada e observável

### Etapa 5 — Normalização e Enriquecimento

Responsabilidades:

- Limpar texto
- Padronizar datas
- Unificar formato dos dados
- Preparar para indexação

Exemplos:

- Remover ruídos
- Ajustar capitalização
- Associar metadados

### Etapa 6 — Indexação

Responsabilidades:

- Tornar os textos pesquisáveis
- Preparar estrutura de busca

Observação:

A decisão entre busca por palavra-chave ou semântica não é feita aqui, apenas suportada.

### Etapa 7 — Busca e Ranqueamento

Responsabilidades:

- Receber consulta em linguagem natural
- Comparar com os textos indexados
- Retornar resultados ordenados por relevância

Saída:

Lista de áudios relevantes com metadados

## Sincronia vs Assincronia

### Abordagem Inicial (MVP)

Pipeline executado de forma sincrona controlada

Usuário aguarda o processamento

Feedback visual claro

Justificativa:

- Menor complexidade
- Menos infraestrutura
- Mais foco no aprendizado

### Evolução Natural (Futuro)

Processamento assíncrono

Filas

Workers

Status polling ou eventos

Esses conceitos devem estar previstos na arquitetura, mas não implementados agora.

## Pontos Críticos de Decisão Arquitetural

Ainda não decididos (intencionalmente):

- Busca semântica vs keyword
- Persistência vs processamento em memória
- Limites exatos de tamanho
- Política de retenção de dados
- Granularidade de logs e métricas

Esses pontos serão resolvidos na próxima etapa.

## O Que Essa Arquitetura É

- Modular
- Orientada a pipeline
- Focada em clareza
- Adequada para MVP educacional

## O Que Essa Arquitetura NÃO É

- Altamente escalável
- Event-driven completa
- Multi-tenant
- Otimizada para custo extremo

## Critérios de Qualidade da Arquitetura (para revisão)

- Cada etapa tem responsabilidade clara
- Mudanças em uma etapa não quebram as outras
- O fluxo completo é compreensível por leitura
- A arquitetura pode ser explicada em 5 minutos