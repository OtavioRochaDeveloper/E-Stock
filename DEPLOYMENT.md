# 🚀 Guia de Deployment e Produção

Instruções completas para colocar a aplicação em produção, incluindo configuração, otimização e monitoramento.

---

## 📋 Índice

- [Pré-requisitos](#pré-requisitos)
- [Preparação para Produção](#preparação-para-produção)
- [Deployment da API](#deployment-da-api)
- [Deployment do Frontend](#deployment-do-frontend)
- [Configuração do Banco de Dados](#configuração-do-banco-de-dados)
- [Segurança](#segurança)
- [Monitoramento](#monitoramento)
- [Troubleshooting](#troubleshooting)

---

## 📦 Pré-requisitos

### Ferramentas Necessárias

- **Node.js** v16+ ([Download](https://nodejs.org/))
- **Git** ([Download](https://git-scm.com/))
- **npm** ou **yarn**
- **Conta em um serviço de hosting**

### Serviços Recomendados

| Serviço | Tipo | Custo | Recomendação |
|---|---|---|---|
| **Vercel** | Frontend/Fullstack | Gratuito+ | ⭐ Melhor para React |
| **Railway** | Backend/Fullstack | Gratuito+ | ⭐ Fácil de usar |
| **Render** | Backend/Fullstack | Gratuito+ | ⭐ Boa alternativa |
| **Heroku** | Backend/Fullstack | Pago | Tradicional |
| **AWS** | Tudo | Pago | Mais controle |
| **DigitalOcean** | Tudo | Pago | Acessível |

---

## 🔧 Preparação para Produção

### 1. Variáveis de Ambiente

#### API - `.env.production`

```env
# Servidor
PORT=3333
NODE_ENV=production

# Banco de Dados
DATABASE_URL=postgresql://user:password@host:5432/db
DATABASE_SSL=true

# Autenticação JWT
JWT_SECRET=sua-chave-super-secreta-e-forte-aqui
JWT_EXPIRES_IN=24h

# CORS
CORS_ORIGIN=https://seu-dominio.com

# Email (opcional)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=seu-email@gmail.com
SMTP_PASS=sua-senha

# Logging
LOG_LEVEL=info
```

#### Frontend - `.env.production`

```env
VITE_API_URL=https://sua-api.com
VITE_APP_NAME=Authentication System
VITE_LOG_LEVEL=error
```

### 2. Atualizar Configurações

#### API - `src/server.js`

```javascript
require("dotenv").config(); // Adicionar no topo

const CORS_ORIGIN = process.env.CORS_ORIGIN || "http://localhost:5173";

app.use(cors({
  origin: CORS_ORIGIN.split(','),
  credentials: true
}));
```

#### Frontend - `src/services/api.js`

```javascript
const api = axios.create({
  baseURL: process.env.VITE_API_URL || 'http://localhost:3333'
});
```

### 3. Build Otimizado

#### API

```bash
cd api

# Remover devDependencies
npm install --production

# Verificar size
npm ls --all --depth=0
```

#### Frontend

```bash
cd web

# Gerar build otimizado
npm run build

# Verificar tamanho
du -sh dist/

# Análise detalhada
npm install -g serve
serve -s dist -p 3000
```

---

## 🌐 Deployment da API

### Option 1: Railway (⭐ Recomendado)

#### Passo 1: Preparar Repositório

```bash
# Criar arquivo .gitignore
echo "
node_modules/
.env
.env.local
.env.*.local
dist/
npm-debug.log
" > .gitignore

# Commit
git add .
git commit -m "chore: prepare for production"
git push origin main
```

#### Passo 2: Conectar Railway

1. Ir para [railway.app](https://railway.app)
2. Sign up com GitHub
3. Criar novo projeto
4. Selecionar repositório GitHub
5. Railway detectará automaticamente `package.json`
6. Adicionar variáveis de ambiente (Environment)

#### Passo 3: Configurar Variáveis

No painel Railway:
- Ir para "Variables"
- Adicionar todas as variáveis de `.env.production`

#### Passo 4: Deploy

```bash
# Railway fará auto-deploy ao fazer push
git push origin main

# Ver logs
# No painel Railway: Deployments → Logs
```

---

### Option 2: Render

#### Passo 1: Conectar GitHub

1. Ir para [render.com](https://render.com)
2. Sign up
3. Novo "Web Service"
4. Conectar repositório GitHub

#### Passo 2: Configurar Serviço

**Build Command:**
```bash
npm install
npm run migrate
```

**Start Command:**
```bash
npm start
```

#### Passo 3: Adicionar Variáveis

- Environment → Add Environment Variable
- Adicionar todas as variáveis necessárias

#### Passo 4: Deploy

- Render fará deploy automático

---

### Option 3: Heroku (Padrão)

#### Passo 1: Instalar Heroku CLI

```bash
# Windows
choco install heroku-cli

# Mac
brew tap heroku/brew && brew install heroku

# Linux
curl https://cli-assets.heroku.com/install.sh | sh
```

#### Passo 2: Login e Criar App

```bash
heroku login
heroku create seu-app-aqui
heroku addons:create heroku-postgresql:hobby-dev
```

#### Passo 3: Configurar Variáveis

```bash
heroku config:set NODE_ENV=production
heroku config:set JWT_SECRET=sua-chave-secreta
heroku config:set DATABASE_URL=$(heroku config:get DATABASE_URL)
```

#### Passo 4: Deploy

```bash
git push heroku main
```

#### Passo 5: Verificar

```bash
heroku logs --tail
heroku open
```

---

## 🎨 Deployment do Frontend

### Option 1: Vercel (⭐ Recomendado)

#### Passo 1: Login no Vercel

```bash
npm install -g vercel
vercel login
```

#### Passo 2: Deploy

```bash
cd web
vercel --prod
```

**Configuração automática:**
- Build command: `npm run build`
- Output directory: `dist`
- Install command: `npm install`

#### Passo 3: Configurar Variáveis

No painel Vercel:
- Settings → Environment Variables
- Adicionar `VITE_API_URL=https://sua-api.com`

#### Passo 4: Deploy via Git

```bash
# Conectar repositório GitHub
vercel --prod --link
```

---

### Option 2: Netlify

#### Passo 1: Conectar GitHub

1. Ir para [netlify.com](https://netlify.com)
2. Sign up com GitHub
3. "New site from Git"
4. Selecionar repositório

#### Passo 2: Configurar Build

**Build command:**
```bash
npm run build
```

**Publish directory:**
```
web/dist
```

#### Passo 3: Adicionar Variáveis

- Site settings → Build & deploy → Environment
- Adicionar `VITE_API_URL`

#### Passo 4: Deploy

Netlify fará deploy automático ao fazer push

---

### Option 3: AWS S3 + CloudFront

#### Passo 1: Criar Bucket S3

```bash
aws s3 mb s3://seu-app-bucket
```

#### Passo 2: Upload da Build

```bash
npm run build
aws s3 sync dist/ s3://seu-app-bucket --delete
```

#### Passo 3: Configurar CloudFront

1. AWS Console → CloudFront
2. Create Distribution
3. S3 bucket como origin
4. Criar custom domain (Route 53)

#### Passo 4: Invalidar Cache

```bash
aws cloudfront create-invalidation \
  --distribution-id YOUR_DIST_ID \
  --paths "/*"
```

---

## 💾 Configuração do Banco de Dados

### PostgreSQL em Produção

#### Railway PostgreSQL

1. Railway → Add Service → Database → PostgreSQL
2. Copiar `DATABASE_URL`
3. Usar como variável de ambiente

#### Configurar Knex para PostgreSQL

```javascript
// knexfile.js
module.exports = {
  production: {
    client: 'postgresql',
    connection: process.env.DATABASE_URL,
    ssl: { rejectUnauthorized: false },
    pool: { min: 2, max: 10 },
    migrations: {
      directory: path.resolve(__dirname, 'src', 'database', 'knex', 'migrations')
    }
  }
};
```

#### Rodar Migrations em Produção

```bash
NODE_ENV=production npm run migrate
```

### Backup do Banco

#### PostgreSQL

```bash
# Backup
pg_dump -U user -h host dbname > backup.sql

# Restore
psql -U user -h host dbname < backup.sql
```

#### SQLite (NÃO recomendado para produção)

```bash
# Backup do arquivo
cp database.db database.backup.db
```

---

## 🔐 Segurança

### Certificado SSL/TLS

#### Usar HTTPS

```javascript
// API - src/server.js
const https = require('https');
const fs = require('fs');

const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem')
};

https.createServer(options, app).listen(3333);
```

Ou usar serviço como:
- **Let's Encrypt** (grátis)
- **AWS Certificate Manager** (grátis)
- **Cloudflare** (grátis)

### Headers de Segurança

```javascript
// API - src/server.js
const helmet = require('helmet');

app.use(helmet());
```

Instalar:
```bash
npm install helmet
```

### Rate Limiting

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100 // 100 requisições por IP
});

app.use(limiter);
```

### Validação de Entrada

```javascript
const { body, validationResult } = require('express-validator');

router.post('/users', [
  body('email').isEmail(),
  body('password').isLength({ min: 6 })
], (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  // Processar
});
```

---

## 📊 Monitoramento

### Logging

#### Winston Logger

```bash
npm install winston
```

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

logger.info('Aplicação iniciada');
logger.error('Erro ao conectar banco');
```

### APM (Application Performance Monitoring)

#### Sentry para Erros

```bash
npm install @sentry/node
```

```javascript
const Sentry = require('@sentry/node');

Sentry.init({ dsn: 'seu-dsn-aqui' });
app.use(Sentry.Handlers.errorHandler());
```

#### DataDog para Métricas

```bash
npm install dd-trace
```

```javascript
const tracer = require('dd-trace').init();
```

### Health Check

```javascript
app.get('/health', (req, res) => {
  res.json({
    status: 'ok',
    timestamp: new Date(),
    uptime: process.uptime()
  });
});
```

---

## 📈 Performance

### Compressão Gzip

```javascript
const compression = require('compression');
app.use(compression());
```

### Caching

```javascript
app.set('etag', false);
app.use((req, res, next) => {
  res.set('Cache-Control', 'public, max-age=3600');
  next();
});
```

### CDN para Assets

Configurar CloudFlare ou similar para servir arquivos estáticos.

---

## ❌ Troubleshooting

### Erro: "Cannot find module"

```bash
npm install --production
npm install --save-dev  # Se dev
```

### API não responde

1. Verificar logs: `heroku logs --tail`
2. Testar em local: `curl http://localhost:3333`
3. Verificar variáveis de ambiente
4. Reiniciar dynos: `heroku restart`

### Frontend não carrega

1. Verificar build: `npm run build`
2. Teste localmente: `npm run preview`
3. Verificar CORS na API
4. Limpar cache do navegador

### Banco de dados não conecta

1. Verificar `DATABASE_URL`
2. Testar conexão: `psql $DATABASE_URL`
3. Rodar migrations: `npm run migrate`

### Token JWT inválido

1. Verificar `JWT_SECRET` em produção
2. Não pode ser diferente entre requisiçõse
3. Gerar novo: `crypto.randomBytes(32).toString('hex')`

---

## 📋 Checklist de Deployment

- [ ] Variáveis de ambiente configuradas
- [ ] Migrations rodadas
- [ ] Build testado localmente
- [ ] Certificado SSL/TLS ativo
- [ ] CORS configurado corretamente
- [ ] Logging ativo
- [ ] Backups configurados
- [ ] Monitoramento ativo
- [ ] Health check funcionando
- [ ] DNS/Domínio apontando corretamente
- [ ] Testes de carga realizados
- [ ] Documentação atualizada

---

## 🔄 Atualização Contínua

### Deploy Automático (CI/CD)

#### GitHub Actions

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '18'
      - run: npm install
      - run: npm run lint
      - run: npm test
      - run: npm run build
      - name: Deploy to Railway
        env:
          RAILWAY_TOKEN: ${{ secrets.RAILWAY_TOKEN }}
        run: |
          npm i -g @railway/cli
          railway up
```

---

## 📚 Recursos

- [Railway Docs](https://docs.railway.app/)
- [Vercel Docs](https://vercel.com/docs)
- [Render Docs](https://render.com/docs)
- [Express Production](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Node Best Practices](https://github.com/goldbergyoni/nodebestpractices)

---

**Última atualização:** 30 de Dezembro de 2025

Deploy com confiança! 🚀

