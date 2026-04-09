# 🏗️ Arquitetura do Sistema — Thamy Personal Chef Automation

## Diagrama de Fluxo

```
┌─────────────────────────────────────────────────────────────┐
│                     CLIENTE (WhatsApp)                      │
└─────────────────────────┬───────────────────────────────────┘
                          │ Mensagem recebida
                          ▼
┌─────────────────────────────────────────────────────────────┐
│              EVOLUTION API (self-hosted · Railway)          │
│  • Gere a instância WhatsApp Business                       │
│  • Recebe e envia mensagens                                 │
│  • Webhook para n8n                                         │
└─────────────────────────┬───────────────────────────────────┘
                          │ Webhook POST
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                    N8N (self-hosted · Railway)               │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Workflow: WhatsApp Chatbot                         │   │
│  │                                                     │   │
│  │  1. Trigger (Webhook)                               │   │
│  │  2. Filtro de mensagens (texto/media/status)        │   │
│  │  3. Gestão de sessão (memória de conversa)          │   │
│  │  4. Agente IA (Google Gemini)                       │   │
│  │  5. Router (tipo de serviço identificado)           │   │
│  │  6. Registo em Google Sheets                        │   │
│  │  7. Resposta via Evolution API                      │   │
│  └─────────────────────────────────────────────────────┘   │
└──────┬──────────────────┬──────────────────┬───────────────┘
       │                  │                  │
       ▼                  ▼                  ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐
│ GOOGLE GEMINI│  │GOOGLE SHEETS │  │    MAKE.COM           │
│              │  │              │  │                       │
│ • Processa   │  │ • Regista    │  │ • Recebe dados do     │
│   mensagens  │  │   clientes   │  │   Google Forms        │
│ • Gere       │  │ • Histórico  │  │ • Gera resumo de      │
│   contexto   │  │   de pedidos │  │   pedido formatado    │
│ • Responde   │  │ • KPIs       │  │ • Envia ao chef       │
│   em PT/EN/ES│  │              │  │   via WhatsApp/Email  │
└──────────────┘  └──────────────┘  └──────────────────────┘
```

---

## Componentes Detalhados

### 1. Evolution API
- **Função:** Gateway WhatsApp Business
- **Hosting:** Railway (Docker container)
- **Versão:** v2.x self-hosted
- **Funcionalidades usadas:** Webhook de mensagens recebidas, envio de mensagens de texto

### 2. n8n
- **Função:** Orquestrador central de workflows
- **Hosting:** Railway (self-hosted após limite do n8n Cloud)
- **Workflows ativos:**
  - `whatsapp-chatbot` — fluxo principal de atendimento
  - `order-summary` — geração de resumos de pedidos

### 3. Google Gemini
- **Modelo:** gemini-1.5-flash
- **Função:** Agente conversacional multilingue
- **Integração:** Nó de IA no n8n com system prompt personalizado
- **Idiomas:** Português, Inglês, Espanhol (deteção automática)

### 4. Google Sheets
- **Função:** Base de dados de clientes e pedidos
- **Integração:** Via Google Sheets API no n8n
- **Dados registados:** Nome, contacto, tipo de serviço, data, notas

### 5. Make.com
- **Função:** Automação secundária para resumos de pedidos
- **Trigger:** Google Forms submetido pelo cliente
- **Output:** Mensagem formatada enviada ao chef

---

## Decisões de Arquitetura

| Decisão | Motivo |
|---|---|
| n8n self-hosted | Limite de execuções atingido no n8n Cloud |
| Railway para hosting | Simples, económico e suporta Docker |
| Gemini vs GPT | Melhor integração com ecossistema Google e custo |
| Evolution API | Alternativa gratuita ao WhatsApp Business API oficial |
| Google Sheets como DB | Familiaridade do cliente e zero custo |

---

## Limitações Conhecidas

- A Evolution API não é oficial WhatsApp — pode ter instabilidade ocasional
- O histórico de conversa é mantido em memória (perde-se se o n8n reiniciar)
- Sem painel de administração dedicado (gestão via Google Sheets)
