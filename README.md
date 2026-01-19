# RFC / PRD — Busca em Áudios Exportados do WhatsApp

## 1. Visão Geral
### Problema
Usuários frequentemente trocam informações relevantes por meio de áudios no WhatsApp (decisões, combinações, lembretes, relatos pessoais).
Com o passar do tempo, essas informações se tornam praticamente irrecuperáveis, pois:

- Áudios não são pesquisáveis
- Não possuem indexação textual
- A busca manual exige ouvir múltiplos áudios longos

O WhatsApp permite exportar o histórico de conversas, mas não oferece ferramentas para extrair valor desses dados, especialmente no caso de mensagens de voz.

### Para quem é

Usuários que:
- Possuem históricos exportados do WhatsApp
- Enviaram ou receberam muitos áudios
- Precisam reencontrar informações específicas ditas em algum momento no passado

### Relevância do Problema

- O uso de mensagens de voz é crescente
- A perda de informação é um problema real e recorrente
- Existe uma lacuna clara entre “ter os dados” e “conseguir usá-los”
- É um problema bem delimitado, ideal para exploração de:
  - Processamento de arquivos
  - Speech-to-Text
  - Busca textual e semântica
  - Arquitetura de pipelines simples

## 2. Objetivos
### Objetivos Principais

1. Permitir que um usuário encontre informações específicas dentro de áudios antigos do WhatsApp
2. Transformar mensagens de voz em texto indexável
3. Retornar os áudios mais relevantes com base em uma descrição em linguagem natural

### Objetivos Secundários

- Criar um pipeline claro e compreensível de processamento de dados
- Servir como estudo prático de:
  - Processamento assíncrono
  - Busca por similaridade
  - Arquitetura orientada a dados
- Produzir documentação técnica de qualidade (RFC, decisões, trade-offs)

## 3. Não-Objetivos (Out of Scope)

Explicitamente fora do escopo inicial:

- Integração direta com a API do WhatsApp
- Processamento em tempo real de mensagens
- Suporte a mensagens de texto (foco exclusivo em áudios)
- Compartilhamento de resultados entre usuários
- Criação de contas, autenticação ou perfis
- Treinamento de modelos próprios de IA
- Aplicativo mobile

## 5. Casos de Uso Principais
### Caso de Uso 1 — Buscar uma informação em áudios antigos
1. O usuário exporta uma conversa do WhatsApp em formato .zip
2. O usuário acessa a aplicação web
3. Faz upload do arquivo .zip
4. Informa, em linguagem natural, o que está procurando
  - Exemplo:
    - “Um áudio onde eu comento que iria casar no final do ano”

5. O sistema processa os dados
6. O usuário recebe uma lista de áudios relevantes, contendo:
  - Texto transcrito
  - Data
  - Autor da mensagem

## 6. Requisitos Funcionais

- O sistema deve permitir upload de um arquivo .zip exportado do WhatsApp
- O sistema deve extrair o conteúdo do arquivo enviado
- O sistema deve identificar mensagens de áudio dentro do histórico
- O sistema deve converter áudios em texto (Speech-to-Text)
- O sistema deve armazenar as transcrições de forma estruturada
- O sistema deve permitir que o usuário descreva o que está procurando em linguagem natural
- O sistema deve comparar a busca do usuário com os textos transcritos
- O sistema deve retornar os áudios mais relevantes
- Cada resultado deve exibir:
  - Texto transcrito
  - Data do áudio
  - Autor

## 7. Requisitos Não Funcionais
### Performance
- O MVP pode aceitar tempos de processamento maiores (ex: dezenas de segundos ou minutos)
- A interface deve comunicar claramente que o processamento está em andamento

### Segurança e Privacidade
- Os arquivos enviados devem ser processados apenas para o objetivo da busca
- Não deve haver compartilhamento de dados entre usuários
- Dados não devem ser utilizados para treinamento de modelos
- Idealmente, os dados devem ser descartados após o uso (ou isso deve ser explicitado)

### Escalabilidade
- O sistema não precisa escalar para muitos usuários simultâneos no MVP
- A arquitetura deve permitir evolução futura para processamento assíncrono e filas

### Manutenibilidade
- Pipeline de processamento deve ser claramente separado em etapas
- Componentes devem ter responsabilidades bem definidas
- Decisões técnicas devem ser documentadas

## 8. Premissas e Restrições
### Premissas
- O usuário consegue exportar corretamente o histórico do WhatsApp
- O formato do .zip segue o padrão atual do WhatsApp
- O usuário aceita aguardar o processamento
- A qualidade da transcrição automática é “boa o suficiente” para busca

### Restrições
- Projeto desenvolvido como side project
- Tempo limitado de desenvolvimento
- Infraestrutura simples
- Custos de APIs de Speech-to-Text devem ser controlados

## 9. Riscos e Pontos em Aberto
### Riscos Técnicos
- Variação no formato de exportação do WhatsApp
- Áudios de baixa qualidade prejudicando a transcrição
- Custo e latência de serviços de Speech-to-Text
- Busca semântica retornar resultados pouco relevantes

### Pontos em Aberto / Perguntas
1. Qual o limite aceitável de tamanho do .zip no MVP?
2. Todos os áudios devem ser processados antes da busca, ou sob demanda?
3. Busca por palavra-chave seria suficiente para o MVP ou a semântica é essencial?
4. Os dados devem ser persistidos temporariamente ou apenas em memória?
5. Qual política de retenção de dados faz mais sentido para um projeto educacional?

## 10. Métricas de Sucesso (para estudo)
- O pipeline completo funciona de ponta a ponta
- O usuário consegue encontrar ao menos um áudio relevante
- O escopo permanece controlado e bem definido
- O projeto gera aprendizados claros sobre:
- Processamento de dados
- Trade-offs de arquitetura
- Busca textual vs semântica
- O RFC serve como base sólida para evolução técnica do projeto