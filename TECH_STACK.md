# 🛠️ Tech Stack e Versões do Projeto

Documentação completa das tecnologias utilizadas no projeto **Authentication and Authorization System**.

---

## 📦 Stack Geral

### Categoria | Tecnologia | Versão | Uso
|---|---|---|---|
| **Linguagem (Backend)** | Node.js | v18+ | Runtime JavaScript |
| **Linguagem (Frontend)** | JavaScript/JSX | ES2020+ | Código frontend |
| **Framework (Backend)** | Express.js | ^4.22.1 | Web framework |
| **Framework (Frontend)** | React | ^18.3.1 | UI library |
| **Build Tool** | Vite | ^4.5.14 | Bundler frontend |
| **Banco de Dados** | SQLite3/PostgreSQL | ^5.1.6 | Persistência |
| **ORM/Query Builder** | Knex.js | ^3.1.0 | SQL query builder |

---

## 🔌 Dependências Backend (API)

### Autenticação e Segurança

| Pacote | Versão | Descrição |
|---|---|---|
| **jsonwebtoken** | ^9.0.1 | Criação e validação de JWT tokens |
| **bcryptjs** | ^3.0.3 | Hashing seguro de senhas |
| **cors** | ^2.8.5 | CORS middleware para requisições cross-origin |
| **cookie-parser** | ^1.4.6 | Parser de cookies HTTP |

**Uso:**
```javascript
// JWT
const token = jwt.sign({ id: user.id }, JWT_SECRET, { expiresIn: '24h' });
const decoded = jwt.verify(token, JWT_SECRET);

// Bcrypt
const hash = await bcrypt.hash(password, 10);
const isValid = await bcrypt.compare(password, hash);
```

---

### Web Framework

| Pacote | Versão | Descrição |
|---|---|---|
| **express** | ^4.22.1 | Web framework Node.js |
| **express-async-errors** | ^3.1.1 | Error handling para async/await |

**Uso:**
```javascript
const express = require('express');
const app = express();

app.use(express.json());
app.post('/users', controller.create); // Async sem try-catch
```

---

### Banco de Dados

| Pacote | Versão | Descrição |
|---|---|---|
| **sqlite3** | ^5.1.6 | SQLite driver |
| **knex** | ^3.1.0 | SQL query builder |

**Uso:**
```javascript
// Migrations
knex.schema.createTable('users', (table) => {
  table.increments('id');
  table.string('email').unique();
  table.timestamps();
});

// Queries
const user = await knex('users').where('email', email).first();
```

---

### Utilidades

| Pacote | Versão | Descrição |
|---|---|---|
| **dotenv** | (Implícito) | Variáveis de ambiente |

**Uso:**
```javascript
require('dotenv').config();
const port = process.env.PORT || 3333;
```

---

### DevDependencies

| Pacote | Versão | Descrição |
|---|---|---|
| **nodemon** | ^3.0.1 | Auto-reload em desenvolvimento |

**Uso:**
```bash
npm run dev  # Usa nodemon para hot-reload
```

---

## 🎨 Dependências Frontend (Web)

### UI Framework

| Pacote | Versão | Descrição |
|---|---|---|
| **react** | ^18.3.1 | Biblioteca UI |
| **react-dom** | ^18.3.1 | Renderização React no DOM |

**Uso:**
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

ReactDOM.createRoot(document.getElementById('root')).render(
  <App />
);
```

---

### Roteamento

| Pacote | Versão | Descrição |
|---|---|---|
| **react-router-dom** | ^6.30.2 | Roteamento cliente-side |

**Uso:**
```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/product" element={<Product />} />
  </Routes>
</BrowserRouter>
```

---

### Styling

| Pacote | Versão | Descrição |
|---|---|---|
| **styled-components** | ^6.1.19 | CSS-in-JS |

**Uso:**
```jsx
import styled from 'styled-components';

const Container = styled.div`
  display: flex;
  padding: 1rem;
  background: ${({ theme }) => theme.colors.bg};
`;
```

---

### HTTP Client

| Pacote | Versão | Descrição |
|---|---|---|
| **axios** | ^1.13.2 | Cliente HTTP moderno |

**Uso:**
```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:3333'
});

api.get('/products').then(res => console.log(res.data));
```

---

### Ícones

| Pacote | Versão | Descrição |
|---|---|---|
| **react-icons** | ^4.12.0 | Ícones SVG para React |

**Uso:**
```jsx
import { FiMenu, FiLogOut } from 'react-icons/fi';

<FiMenu size={24} />
```

---

### Build Tool

| Pacote | Versão | Descrição |
|---|---|---|
| **vite** | ^4.5.14 | Build tool ultrarrápido |

**Uso:**
```bash
npm run dev      # Dev server
npm run build    # Build otimizado
npm run preview  # Preview de build
```

**vite.config.js:**
```javascript
import react from '@vitejs/plugin-react';

export default {
  plugins: [react()]
};
```

---

### TypeScript (Opcional)

| Pacote | Versão | Descrição |
|---|---|---|
| **@types/react** | ^18.3.27 | Type definitions React |
| **@types/react-dom** | ^18.3.7 | Type definitions React DOM |

---

### ESLint (Linting)

| Pacote | Versão | Descrição |
|---|---|---|
| **eslint** | ^8.57.1 | Linter de código |
| **eslint-plugin-react** | ^7.37.5 | Rules React para ESLint |
| **eslint-plugin-react-hooks** | ^4.6.2 | Rules React Hooks |
| **eslint-plugin-react-refresh** | ^0.4.26 | Rules Vite React Refresh |

**Uso:**
```bash
npm run lint          # Verificar código
npm run lint -- --fix # Corrigir automaticamente
```

---

### Plugin Vite para React

| Pacote | Versão | Descrição |
|---|---|---|
| **@vitejs/plugin-react** | ^4.7.0 | Plugin para Fast Refresh |

---

## 📊 Comparação Versões

### Versões Atualizadas (em 30/12/2025)

| Pacote | Versão Anterior | Versão Atual | Status |
|---|---|---|---|
| bcryptjs | 2.4.3 | 3.0.3 | ✅ Atualizado |
| knex | 2.5.1 | 3.1.0 | ✅ Atualizado |
| express | 4.18.2 | 4.22.1 | ✅ Atualizado |
| nodemon | (antigo) | 3.0.1 | ✅ Atualizado |

---

## 🔄 Compatibilidade entre Pacotes

### Express e Express-Async-Errors

```
express@4.22.1 ✅ Compatível com express-async-errors@3.1.1
express@5.x    ❌ NÃO compatível com express-async-errors@3.1.1
```

**Nota:** Mantemos Express 4.x por compatibilidade com express-async-errors.

---

### React e React Router

```
react@18.3.1 ✅ Compatível com react-router-dom@6.30.2
react@16.x   ❌ Requer react-router-dom@5.x
```

---

### Styled Components e React

```
styled-components@6.1.19 ✅ Compatível com react@18.3.1
styled-components@4.x    ❌ Requer react@16.x
```

---

## 🚀 Versões por Ambiente

### Desenvolvimento (Dev)

```bash
Node.js:    v18+
npm:        v8+ ou yarn v3+
nodemon:    v3.0.1
eslint:     v8.57.1
```

### Produção (Prod)

```bash
Node.js:    v18.17.0 LTS (mínimo recomendado)
npm:        v9+
```

---

## 📈 Performance por Versão

### Bundle Size (Frontend)

| Framework | Tamanho |
|---|---|
| React | ~40 KB |
| React Router | ~35 KB |
| Styled Components | ~15 KB |
| Axios | ~13 KB |
| **Total Min** | ~103 KB |
| **Total Gzip** | ~30 KB |

---

### Tempo de Build

| Ferramenta | Tempo |
|---|---|
| Vite Build | <1 segundo |
| Rollup (Vite) | <500ms |
| Esbuild (Vite) | <100ms |

---

## 🔐 Segurança das Versões

### Vulnerabilidades Conhecidas

**Data:** 30/12/2025

```bash
$ npm audit
found 0 vulnerabilities
```

Todas as dependências estão atualizadas e sem vulnerabilidades conhecidas.

---

## 📦 Como Instalar Dependências

### Instalar Tudo

```bash
# API
cd api
npm install

# Web
cd ../web
npm install
```

### Instalar Versão Específica

```bash
npm install express@4.22.1
npm install react@18.3.1
```

### Atualizar para Versão Mais Recente

```bash
npm update            # Atualizar com base em ^x.y.z
npm update --save     # Atualizar package.json
npm outdated          # Ver quais estão desatualizadas
```

---

## 🔍 Verificar Versões Instaladas

### Listar Versões

```bash
npm list              # Árvore de dependências
npm list --depth=0    # Apenas dependências diretas
npm outdated          # Ver atualizações disponíveis
```

### Verificar Versão de Um Pacote

```bash
npm list express
npm list react
npm list knex
```

---

## 🛡️ Segurança de Dependências

### Verificar Vulnerabilidades

```bash
npm audit             # Verificar vulnerabilidades
npm audit fix         # Corrigir automaticamente
npm audit fix --force # Corrigir mesmo que quebre compatibilidade
```

### Lock File

Ambos projetos têm `package-lock.json` que garante versões exatas.

```
npm ci  # CI/CD: instala exatamente como package-lock.json
npm install # Dev: pode atualizar patch versions
```

---

## 📚 Recursos Oficiais

### Documentação Oficial

| Tecnologia | URL |
|---|---|
| **Node.js** | https://nodejs.org/ |
| **Express** | https://expressjs.com/ |
| **React** | https://react.dev/ |
| **Vite** | https://vitejs.dev/ |
| **React Router** | https://reactrouter.com/ |
| **Styled Components** | https://styled-components.com/ |
| **Knex.js** | http://knexjs.org/ |
| **Axios** | https://axios-http.com/ |
| **Bcryptjs** | https://github.com/dcodeIO/bcrypt.js |
| **JWT** | https://jwt.io/ |

---

## 🔄 Plano de Atualização

### Crítico (Segurança)
- Atualizar imediatamente
- Exemplo: Vulnerabilidade em bcryptjs

### Alto (Breaking Changes)
- Atualizar em próxima versão minor
- Exemplo: Express 5.0

### Médio (Features)
- Atualizar regularmente
- Exemplo: Novas features no React

### Baixo (Patches)
- Atualizar conforme necessário
- Exemplo: Bug fixes menores

---

## ⚠️ Dependências com Atenção

### Express 4.x → 5.x Migration
- ❌ Não recomendado agora
- ⏰ Planejar para próxima versão major
- 📝 Requer atualização de express-async-errors

### Node.js 16 → 18+ Migration
- ✅ Já realizado
- 📈 Melhor performance
- 🔐 Melhor segurança

---

## 🎯 Recomendações

### Para Desenvolvimento

```bash
# Use NVM para gerenciar versões Node.js
nvm install 18
nvm use 18

# Use npm ci para instalar dependências exatas
npm ci

# Instale nodemon globalmente (opcional)
npm install -g nodemon
```

### Para Produção

```bash
# Sempre use versões LTS
Node.js: v18.17.0 LTS

# Instale com --production
npm install --production

# Use npm ci ao invés de npm install
npm ci --production
```

---

## 📋 Próximas Atualizações Planejadas

| Pacote | Versão Futura | Data Estimada | Notas |
|---|---|---|---|
| React | 19.x | 2025 | Breaking changes |
| Vite | 5.x | 2025 | Novas features |
| Express | 5.x | TBD | Requer planejamento |
| Knex | 4.x | 2025+ | Minor updates |

---

## 🆘 Troubleshooting de Versões

### Erro: "Cannot find module"

```bash
rm -rf node_modules package-lock.json
npm install
```

### Erro: "npm ERR! peer dep missing"

```bash
npm install --legacy-peer-deps
```

### Erro: "Module not compatible"

```bash
npm outdated                    # Verificar versões
npm install express@4.22.1     # Instalar versão específica
```

---

## 📊 Matriz de Compatibilidade

```
Node.js 18+ ✅
├── Express 4.22.1 ✅
├── React 18.3.1 ✅
├── Vite 4.5.14 ✅
├── Knex 3.1.0 ✅
├── SQLite3 5.1.6 ✅
├── PostgreSQL 14+ ✅
├── Bcryptjs 3.0.3 ✅
└── JWT 9.0.1 ✅
```

---

**Última atualização:** 30 de Dezembro de 2025
**Status:** ✅ Todas as dependências atualizadas e compatíveis

