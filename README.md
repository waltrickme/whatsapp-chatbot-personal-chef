# 🍽️ Thamy Personal Chef — WhatsApp Automation

> Automação completa de atendimento via WhatsApp para marca de personal chef em Lisboa, integrando **n8n + Evolution API + Google Gemini AI**.

---

## 📌 Visão Geral do Projeto

Este projeto implementa um sistema de atendimento automatizado 24/7 para uma marca de personal chef, substituindo o atendimento manual por um agente de IA multilingue capaz de qualificar clientes, recolher informações de pedidos e gerar resumos automáticos para o chef.

---

## 🏗️ Arquitetura

```
Cliente (WhatsApp)
        │
        ▼
Evolution API (self-hosted · Railway)
        │
        ▼
n8n (orquestração de workflows)
        │
        ├──► Google Gemini AI (agente conversacional multilingue)
        │
        ├──► Google Sheets (registo de pedidos e clientes)
        │
        └──► Make.com (resumos automáticos de pedidos)
```

---

## ⚙️ Funcionalidades

- ✅ Atendimento automático 24/7 via WhatsApp
- ✅ Suporte multilingue (Português, Inglês, Espanhol)
- ✅ Qualificação automática de clientes (intake)
- ✅ Routing por tipo de serviço (eventos, refeições semanais, catering)
- ✅ Geração automática de resumo de pedidos para o chef
- ✅ Registo de clientes em Google Sheets
- ✅ Integração com Google Forms para pedidos estruturados
- ✅ System prompt de IA calibrado com tom de marca

---

## 🛠️ Stack Tecnológica

| Componente | Tecnologia |
|---|---|
| Mensagens | WhatsApp via Evolution API |
| Orquestração | n8n (self-hosted no Railway) |
| Inteligência Artificial | Google Gemini 1.5 Flash |
| Automações secundárias | Make.com |
| Base de dados | Google Sheets |
| Formulários | Google Forms |
| Hosting | Railway |

---

## 🚀 Como Configurar

### Pré-requisitos

- Conta no [Railway](https://railway.app)
- Conta Google (para Sheets, Forms e Gemini API)
- Instância n8n (self-hosted ou cloud)
- Número WhatsApp Business

### 1. Variáveis de Ambiente

Copia o ficheiro de exemplo e preenche com as tuas credenciais:

```bash
cp .env.example .env
```

Edita o `.env` com os teus dados reais (ver secção abaixo).

### 2. Evolution API

```bash
# Deploy no Railway usando o template oficial
# https://railway.app/template/evolution-api
```

Configura a instância e liga ao número WhatsApp Business.

### 3. n8n

Importa o workflow disponível em `/workflows/whatsapp-chatbot.json`:

```
n8n > Workflows > Import from file
```

Configura as credenciais nas variáveis de ambiente do n8n.

### 4. Make.com

Importa o blueprint em `/workflows/order-summary-blueprint.json` e liga ao Google Forms e Google Sheets.

---

## 🔐 Variáveis de Ambiente

Cria um ficheiro `.env` baseado no `.env.example`:

```env
# Evolution API
EVOLUTION_API_URL=https://your-server.railway.app
EVOLUTION_API_KEY=your_api_key_here
WHATSAPP_INSTANCE=your_instance_name

# Google Gemini
GEMINI_API_KEY=your_gemini_key_here

# Google Sheets
GOOGLE_SHEET_ID=your_sheet_id_here
GOOGLE_SERVICE_ACCOUNT_EMAIL=your_service_account@project.iam.gserviceaccount.com

# Make.com
MAKE_WEBHOOK_URL=your_webhook_url_here
```

> ⚠️ **Nunca commites o ficheiro `.env` com dados reais.** Está incluído no `.gitignore`.

---

## 📁 Estrutura do Repositório

```
thamy-personal-chef-automation/
├── .env.example                        # Template de variáveis de ambiente
├── .gitignore                          # Ficheiros ignorados pelo Git
├── README.md                           # Este ficheiro
├── workflows/
│   ├── whatsapp-chatbot.json           # Workflow principal n8n (sanitizado)
│   └── order-summary-blueprint.json    # Blueprint Make.com (sanitizado)
├── prompts/
│   └── ai-agent-system-prompt.md      # System prompt do agente Gemini
└── docs/
    ├── architecture.md                 # Diagrama detalhado da arquitetura
    └── setup-guide.md                  # Guia de instalação passo a passo
```

---

## 📊 Resultados

- Redução do tempo de resposta de horas para segundos
- Atendimento 24/7 sem intervenção manual
- Qualificação automática de leads antes de chegarem ao chef
- Resumos estruturados de pedidos enviados diretamente ao chef

---

## 👤 Autor

**Gustavo de Oliveira Waltrick**
Data Analyst & Digital Project Manager | Lisboa, Portugal

- LinkedIn: [linkedin.com/in/waltrick](https://www.linkedin.com/in/waltrick/)
- GitHub: [github.com/waltrickme](https://github.com/waltrickme)
- Email: waltrickme@gmail.com

---

## 📄 Licença

Este projeto é um portfólio pessoal. Os dados do cliente foram anonimizados. Uso educacional e demonstrativo.
