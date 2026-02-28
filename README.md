# 🤖 Chatbot Telegram - Previsão do Tempo

Um chatbot inteligente para Telegram que fornece informações meteorológicas de cidades brasileiras em tempo real, utilizando n8n, OpenWeatherMap API e Google Gemini para respostas naturais e amigáveis.

## Bot Live to test

- Bot: [FTR_BR_Weather_Bot](https://t.me/ftr_br_weather_bot)

> [!WARNING]
> **Importante:** Este projeto foi desenvolvido utilizando o n8n versão **2.9.4**.

## 📋 Descrição

Este projeto implementa um chatbot completo no Telegram que:

- ✅ Recebe mensagens com o nome de cidades brasileiras
- ✅ Consulta dados meteorológicos em tempo real via OpenWeatherMap API
- ✅ Processa e formata as informações de clima
- ✅ Usa Google Gemini AI para gerar respostas naturais e amigáveis com emojis
- ✅ Retorna informações detalhadas de temperatura, sensação térmica, umidade e vento
- ✅ Fornece dicas úteis baseadas nas condições climáticas

### Tecnologias Utilizadas

- **n8n**: Plataforma de automação workflow (versão 2.9.4)
- **PostgreSQL**: Banco de dados para persistência (versão 16)
- **Redis**: Gerenciamento de filas e cache (versão 6)
- **Docker & Docker Compose**: Containerização e orquestração
- **OpenWeatherMap API**: Dados meteorológicos
- **Telegram Bot API**: Interface do chatbot
- **Google Gemini AI**: Processamento de linguagem natural
- **ngrok**: Exposição de webhooks para desenvolvimento

## 📁 Estrutura do Projeto

```
chatbot-telegram/
├── docker-compose.yml           # Configuração dos serviços Docker
├── .env                         # Variáveis de ambiente (não versionado)
├── .env.example                 # Exemplo de configuração
├── .gitignore                   # Arquivos ignorados pelo Git
├── workflow-chatbot-telegram.json  # Workflow exportado do n8n
└── README.md                    # Este arquivo
```

## 🔑 Pré-requisitos

Antes de começar, você precisará:

1. **Docker** e **Docker Compose** instalados
2. **Telegram Bot Token**: Crie um bot via [@BotFather](https://t.me/botfather) no Telegram
3. **OpenWeatherMap API Key**: Registre-se gratuitamente em [openweathermap.org](https://openweathermap.org/api)
4. **Google Gemini API Key** (opcional): Para respostas com IA via [Google AI Studio](https://ai.google.dev/)
5. **ngrok Auth Token** (opcional): Para expor webhooks localmente via [ngrok.com](https://ngrok.com/)

---

## ⚙️ Configuração do Ambiente

### 1. Clone ou baixe o projeto

```bash
cd chatbot-telegram
```

### 2. Configure as variáveis de ambiente

Crie o arquivo `.env` baseado no `.env.example`:

```bash
cp .env.example .env
```

### 3. Edite o arquivo `.env` com suas credenciais

Abra o arquivo `.env` e configure as seguintes variáveis:

```bash
# ========================================
# PostgreSQL Database
# ========================================
POSTGRES_USER=n8n_user
POSTGRES_PASSWORD=SuaSenhaSegura123!
POSTGRES_DB=n8n
POSTGRES_PORT=5432

# ========================================
# Redis Cache
# ========================================
REDIS_HOST=redis
REDIS_PORT=6379

# ========================================
# n8n Configuration
# ========================================
N8N_ENCRYPTION_KEY=SuaChaveCriptografiaUnica32Chars!
N8N_PORT=5678
N8N_PROTOCOL=http
N8N_HOST=localhost
WEBHOOK_URL=http://localhost:5678/
GENERIC_TIMEZONE=America/Sao_Paulo

# ========================================
# ngrok (para desenvolvimento/webhooks)
# ========================================
NGROK_AUTHTOKEN=seu-token-ngrok-aqui

# ========================================
# TELEGRAM BOT CREDENTIALS
# ========================================
# Obtenha via @BotFather no Telegram
TELEGRAM_BOT_TOKEN=123456789:ABCdefGHIjklMNOpqrsTUVwxyz1234567890

# ========================================
# OPENWEATHERMAP API CREDENTIALS
# ========================================
# Obtenha em: https://openweathermap.org/api
OPENWEATHER_API_KEY=abc123def456ghi789jkl012mno345pqr
```

### 📌 Variáveis Obrigatórias

| Variável | Descrição | Como Obter |
|----------|-----------|------------|
| `TELEGRAM_BOT_TOKEN` | Token de autenticação do bot Telegram | 1. Abra o Telegram e fale com [@BotFather](https://t.me/botfather)<br>2. Digite `/newbot` e siga as instruções<br>3. Copie o token fornecido |
| `OPENWEATHER_API_KEY` | Chave de API do OpenWeatherMap | 1. Acesse [openweathermap.org](https://openweathermap.org/api)<br>2. Crie uma conta gratuita<br>3. Vá em "API Keys" e copie sua chave |

---

## 🚀 Executando o Projeto

### Iniciar os serviços

```bash
docker-compose up -d
```

### Verificar status dos containers

```bash
docker-compose ps
```

### Parar os serviços

```bash
docker-compose down
```

### Parar e remover volumes (limpar dados)

```bash
docker-compose down -v
```

---

## 📥 Importando o Workflow no n8n

### 1. Acesse a interface do n8n

Após iniciar os serviços, acesse:

```
http://localhost:5678
```

### 2. Crie uma conta

Na primeira vez, você precisará criar um usuário administrador.

### 3. Importe o workflow

1. No n8n, clique em **"+"** no menu superior
2. Selecione **"Import from File"** ou **"Import from URL"**
3. Selecione o arquivo `workflow-chatbot-telegram.json` deste repositório
4. Clique em **"Import"**

### 4. Configure as credenciais no n8n

O workflow importado **NÃO contém credenciais embutidas** (por segurança). Você precisará configurá-las manualmente dentro do n8n:

#### a) Configurar credencial do Telegram

1. Abra o nó **"Telegram Trigger - FTR BR Weather Bot"**
2. Clique em **"Create New Credential"** ou selecione uma existente
3. Insira o `TELEGRAM_BOT_TOKEN` obtido via @BotFather
4. Salve a credencial

Repita para os nós:
- **"Send Weather"**
- **"Send Error"**

#### b) Configurar credencial do OpenWeatherMap

1. Abra o nó **"OpenWeatherMap"**
2. Clique em **"Create New Credential"**
3. Escolha o tipo: **"OpenWeatherMap API"**
4. Insira o `OPENWEATHER_API_KEY`
5. Salve a credencial

#### c) Configurar credencial do Google Gemini (opcional)

1. Abra o nó **"Message a model"**
2. Clique em **"Create New Credential"**
3. Obtenha uma API Key em [Google AI Studio](https://ai.google.dev/)
4. Insira a chave e salve

### 5. Ative o workflow

1. No canto superior direito, clique no botão **"Active"** para ativar o workflow
2. O webhook do Telegram será registrado automaticamente

---

## 🧪 Testando o Chatbot

1. Abra o Telegram e procure pelo seu bot usando o `@username` que você criou
2. Inicie uma conversa com `/start`
3. Envie o nome de uma cidade brasileira no formato:
   ```
   São Paulo, SP
   ```
   ou
   ```
   Curitiba, PR
   ```

4. O bot responderá com informações detalhadas do clima! ☀️🌧️

---

## 📊 Como Funciona o Workflow

O workflow possui os seguintes nós:

1. **Telegram Trigger**: Recebe mensagens do Telegram
2. **Formatar Message**: Normaliza o nome da cidade (remove acentos, etc.)
3. **OpenWeatherMap**: Consulta a API de clima
4. **Code in JavaScript**: Processa e estrutura os dados meteorológicos
5. **Check Status**: Verifica se a consulta foi bem-sucedida
6. **Message a model (Gemini)**: Gera resposta amigável com IA
7. **Send Weather**: Envia a resposta ao usuário
8. **Send Error**: Envia mensagem de erro caso a cidade não seja encontrada

---

## 📝 Logs e Monitoramento

### Visualizar logs dos serviços

```bash
# Logs do n8n principal
docker-compose logs -f n8n

# Logs do worker n8n
docker-compose logs -f n8n-worker

# Logs do PostgreSQL
docker-compose logs -f postgres

# Logs do Redis
docker-compose logs -f redis

# Logs do ngrok
docker-compose logs -f ngrok
```

### Acessar logs de execução no n8n

1. Acesse a interface web do n8n
2. Vá em **"Executions"** no menu lateral
3. Visualize o histórico e detalhes de cada execução

---

## 🔐 Segurança

### ✅ Credenciais Seguras

- ✅ O arquivo `workflow-chatbot-telegram.json` **NÃO contém** tokens ou credenciais embutidos
- ✅ Todas as credenciais são referenciadas por IDs internos do n8n
- ✅ As credenciais reais são armazenadas criptografadas no banco PostgreSQL
- ✅ O arquivo `.env` está no `.gitignore` e **NÃO deve ser versionado**
- ✅ Use o `.env.example` como referência para configuração

### 🚨 Importante

- **NUNCA** versione o arquivo `.env` com credenciais reais
- **NUNCA** compartilhe seus tokens publicamente
- Use `N8N_ENCRYPTION_KEY` forte e única
- Em produção, use HTTPS e configure adequadamente as variáveis de ambiente

---

## 🔧 Solução de Problemas

### n8n não inicia

- Verifique se as variáveis no `.env` estão corretas
- Confirme que PostgreSQL e Redis estão rodando: `docker-compose ps`
- Veja logs: `docker-compose logs n8n`

### Webhook não funciona

- Certifique-se de que o ngrok está rodando e o `WEBHOOK_URL` está correto
- No n8n, vá em Settings > General > Webhook URL e configure corretamente
- Verifique se o workflow está **Ativo**

### Bot não responde

- Verifique se o `TELEGRAM_BOT_TOKEN` está correto
- Confirme que todas as credenciais foram configuradas no n8n
- Veja as execuções no n8n para identificar erros

### Erro "City not found"

- Certifique-se de usar o formato: `Cidade, UF` (ex: `Rio de Janeiro, RJ`)
- Verifique se o `OPENWEATHER_API_KEY` está válido
- Confirme que não ultrapassou o limite da API gratuita

---

## 🌐 Webhooks e ngrok

Para desenvolvimento local, o ngrok expõe os webhooks do n8n para a internet:

```
http://<seu-subdominio>.ngrok-free.dev/webhook/...
```

Configure o `WEBHOOK_URL` no `.env` para apontar para o ngrok em desenvolvimento.
