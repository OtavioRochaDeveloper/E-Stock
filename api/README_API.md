# 🔌 API REST - Authentication and Authorization

Servidor backend para o sistema de autenticação e autorização. Desenvolvido com **Node.js** e **Express**, implementando **JWT** para autenticação e controle de acesso baseado em papéis.

---

## 📋 Índice

- [Quick Start](#quick-start)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Endpoints da API](#endpoints-da-api)
- [Autenticação](#autenticação)
- [Middlewares](#middlewares)
- [Controllers](#controllers)
- [Banco de Dados](#banco-de-dados)
- [Tratamento de Erros](#tratamento-de-erros)
- [Variáveis de Ambiente](#variáveis-de-ambiente)

---

## 🚀 Quick Start

### 1. Instalar Dependências
```bash
npm install
```

### 2. Rodar Migrations (Criar banco de dados)
```bash
npm run migrate
```

### 3. Iniciar o Servidor
```bash
# Modo desenvolvimento (com auto-reload)
npm run dev

# Modo produção
npm start
```

O servidor estará disponível em: **http://localhost:3333**

---

## 📁 Estrutura do Projeto

```
api/
├── src/
│   ├── configs/
│   │   └── auth.js              # Configuração JWT
│   │
│   ├── controllers/
│   │   ├── ProductsController.js
│   │   ├── SalesController.js
│   │   ├── SessionsController.js
│   │   ├── UsersController.js
│   │   └── UsersValidatedController.js
│   │
│   ├── database/
│   │   └── knex/
│   │       ├── index.js
│   │       └── migrations/
│   │           └── 20230811164833_createUsers.js
│   │
│   ├── middlewares/
│   │   ├── ensureAuthenticated.js      # Valida JWT token
│   │   └── verifyUserAuthorization.js  # Verifica role do usuário
│   │
│   ├── routes/
│   │   ├── index.js
│   │   ├── products.routes.js
│   │   ├── sales.routes.js
│   │   ├── sessions.routes.js
│   │   └── users.routes.js
│   │
│   ├── utils/
│   │   └── AppError.js          # Classe para tratamento de erros
│   │
│   └── server.js                # Arquivo principal
│
├── knexfile.js                  # Configuração do Knex
├── insomnia_routes.json         # Coleção de requisições para teste
└── package.json                 # Dependências do projeto
```

---

## 🔌 Endpoints da API

### Base URL
```
http://localhost:3333
```

---

## 🔐 Autenticação (Sessions)

### Login - Obter Token JWT

```http
POST /sessions
Content-Type: application/json

{
  "email": "usuario@email.com",
  "password": "senha123"
}
```

**Resposta (200 OK):**
```json
{
  "user": {
    "id": 1,
    "name": "João Silva",
    "email": "usuario@email.com",
    "role": "admin"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNzAzODU5NDAwLCJleHAiOjE3MDM5NDU4MDB9.xyz..."
}
```

**Possíveis Erros:**
- `400` - Email ou senha inválidos
- `404` - Usuário não encontrado

---

## 👥 Usuários

### Criar Novo Usuário (Registro)

```http
POST /users
Content-Type: application/json

{
  "name": "Novo Usuário",
  "email": "novo@email.com",
  "password": "senha123",
  "role": "customer"
}
```

**Resposta (201 Created):**
```json
{
  "id": 2,
  "name": "Novo Usuário",
  "email": "novo@email.com",
  "role": "customer"
}
```

**Possíveis Erros:**
- `400` - Email já cadastrado
- `400` - Dados inválidos

---

### Listar Usuários Autenticados

```http
GET /users/validated
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Resposta (200 OK):**
```json
[
  {
    "id": 1,
    "name": "João Silva",
    "email": "usuario@email.com",
    "role": "admin",
    "created_at": "2024-08-03T10:37:00Z"
  },
  {
    "id": 2,
    "name": "Novo Usuário",
    "email": "novo@email.com",
    "role": "customer",
    "created_at": "2024-12-30T14:20:00Z"
  }
]
```

**Possíveis Erros:**
- `401` - Token inválido ou não fornecido
- `401` - Token expirado

---

## 📦 Produtos

### Listar Produtos (Protegido)

```http
GET /products
Authorization: Bearer seu_token_aqui
```

**Resposta (200 OK):**
```json
[
  {
    "id": 1,
    "name": "Laptop",
    "price": 1500.00,
    "description": "Laptop de alta performance"
  },
  {
    "id": 2,
    "name": "Mouse",
    "price": 50.00,
    "description": "Mouse wireless"
  }
]
```

**Possíveis Erros:**
- `401` - Não autenticado

---

### Criar Produto (Admin Only)

```http
POST /products
Authorization: Bearer seu_token_admin_aqui
Content-Type: application/json

{
  "name": "Teclado",
  "price": 150.00,
  "description": "Teclado mecânico RGB"
}
```

**Resposta (201 Created):**
```json
{
  "id": 3,
  "name": "Teclado",
  "price": 150.00,
  "description": "Teclado mecânico RGB"
}
```

**Possíveis Erros:**
- `401` - Não autenticado
- `403` - Sem permissão (não é admin)

---

## 💰 Vendas

### Listar Vendas (Admin ou Sale)

```http
GET /sales
Authorization: Bearer seu_token_aqui
```

**Resposta (200 OK):**
```json
[
  {
    "id": 1,
    "user_id": 1,
    "product_id": 1,
    "quantity": 2,
    "total_price": 3000.00,
    "created_at": "2024-12-30T10:30:00Z"
  }
]
```

**Possíveis Erros:**
- `401` - Não autenticado
- `403` - Sem permissão (não é admin ou sale)

---

## 🔐 Autenticação

### Como Funciona

1. **Login**: Usuário envia email e senha
2. **Validação**: Senha é comparada com hash no banco usando bcrypt
3. **Token JWT**: Se válido, um token JWT é gerado
4. **Armazenamento**: Frontend armazena o token (localStorage/cookie)
5. **Uso**: Token é enviado em cada requisição protegida no header `Authorization`

### JWT Token

O token JWT contém:
```json
{
  "id": 1,
  "iat": 1703859400,
  "exp": 1703945800
}
```

- `id`: ID do usuário
- `iat`: Issued at (quando foi criado)
- `exp`: Expiration (quando expira - 24h após criação)

### Header Authorization

Todas as requisições protegidas devem incluir:
```
Authorization: Bearer <seu_jwt_token>
```

---

## 🛡️ Middlewares

### 1. ensureAuthenticated.js

Valida se o usuário está autenticado via JWT token.

**Uso:**
```javascript
router.get("/", ensureAuthenticated, controller.method);
```

**Como funciona:**
1. Extrai o token do header `Authorization`
2. Valida a assinatura do token
3. Verifica se o token não expirou
4. Se válido, passa o `user` para o próximo middleware/controller
5. Se inválido, retorna erro 401

---

### 2. verifyUserAuthorization.js

Verifica se o usuário tem a role (papel) necessária para acessar a rota.

**Uso com uma role:**
```javascript
router.post("/", verifyUserAuthorization("admin"), controller.create);
```

**Uso com múltiplas roles:**
```javascript
router.get("/", verifyUserAuthorization(["admin", "sale"]), controller.index);
```

**Como funciona:**
1. Verifica o `role` do usuário autenticado
2. Se o role está na lista de roles permitidas
3. Se sim, permite acesso
4. Se não, retorna erro 403

---

## 🎮 Controllers

### SessionsController

Gerencia autenticação de usuários.

**Métodos:**
- `create(email, password)` - Fazer login

---

### UsersController

Gerencia criação de usuários.

**Métodos:**
- `create(name, email, password, role)` - Criar novo usuário

---

### UsersValidatedController

Gerencia listagem de usuários autenticados.

**Métodos:**
- `index()` - Listar todos os usuários

---

### ProductsController

Gerencia produtos (CRUD).

**Métodos:**
- `index()` - Listar produtos
- `create(name, price, description)` - Criar produto (admin)

---

### SalesController

Gerencia vendas.

**Métodos:**
- `index()` - Listar vendas (admin/sale)

---

## 💾 Banco de Dados

### Configuração

- **Tipo**: SQLite3
- **Localização**: `src/database/database.db`
- **ORM**: Knex.js
- **Migrations**: `src/database/knex/migrations/`

### Executar Migrations

```bash
# Criar banco e tabelas
npm run migrate

# Reverter última migration
npx knex migrate:rollback

# Reverter todas as migrations
npx knex migrate:rollback --all
```

### Schema do Banco

#### Tabela `users`
```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  email TEXT NOT NULL UNIQUE,
  password TEXT NOT NULL,
  role TEXT NOT NULL DEFAULT 'customer',
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

#### Tabela `products`
```sql
CREATE TABLE products (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  price REAL NOT NULL,
  description TEXT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

#### Tabela `sales`
```sql
CREATE TABLE sales (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  product_id INTEGER NOT NULL,
  quantity INTEGER NOT NULL,
  total_price REAL NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id),
  FOREIGN KEY (product_id) REFERENCES products(id)
);
```

---

## ⚠️ Tratamento de Erros

A API usa uma classe customizada `AppError` para tratamento consistente de erros.

### AppError Class

```javascript
class AppError extends Error {
  constructor(message, statusCode) {
    super();
    this.message = message;
    this.statusCode = statusCode;
  }
}
```

### Uso

```javascript
throw new AppError("Email já cadastrado", 400);
```

### Resposta de Erro

```json
{
  "status": "error",
  "message": "Email já cadastrado"
}
```

### Códigos de Status Comuns

| Código | Significado | Exemplo |
|---|---|---|
| 200 | OK | Requisição bem-sucedida |
| 201 | Created | Recurso criado |
| 400 | Bad Request | Dados inválidos |
| 401 | Unauthorized | Token inválido/expirado |
| 403 | Forbidden | Sem permissão (role insuficiente) |
| 404 | Not Found | Recurso não encontrado |
| 500 | Internal Server Error | Erro interno do servidor |

---

## 🔧 Variáveis de Ambiente

Crie um arquivo `.env` na raiz da API:

```env
# .env
PORT=3333
NODE_ENV=development
JWT_SECRET=sua-chave-secreta-muito-segura-aqui
JWT_EXPIRES_IN=24h
DATABASE_URL=sqlite:./src/database/database.db
```

**Variáveis:**
- `PORT` - Porta que o servidor vai rodar
- `NODE_ENV` - Ambiente (development/production)
- `JWT_SECRET` - Chave secreta para assinar JWT
- `JWT_EXPIRES_IN` - Tempo de expiração do token

---

## 📊 CORS Configuration

A API está configurada para aceitar requisições do frontend:

```javascript
app.use(cors({
  origin: ["http://localhost:5173", "http://127.0.0.1:5173"],
  credentials: true
}));
```

Para adicionar outras origens, edite `src/server.js`.

---

## 🧪 Testando com Insomnia/Postman

### 1. Importar Coleção

Arquivo: `insomnia_routes.json`

### 2. Ordem de Testes Recomendada

1. **POST /sessions** - Login para obter token
2. **POST /users** - Criar novo usuário
3. **GET /users/validated** - Listar usuários (copie o token do login)
4. **GET /products** - Listar produtos
5. **POST /products** - Criar produto (admin apenas)
6. **GET /sales** - Listar vendas

### 3. Variáveis no Insomnia

Após fazer login, salve o token em uma variável:

```
Base Env:
{
  "base_url": "http://localhost:3333",
  "token": "<cole_o_token_aqui>"
}
```

Use em requisições:
```
Authorization: Bearer {{ token }}
```

---

## 🐛 Troubleshooting

### Erro: "Cannot find module 'express'"

```bash
npm install
```

### Erro: "Port 3333 already in use"

**Windows:**
```powershell
netstat -ano | findstr :3333
taskkill /PID <PID> /F
```

**Linux/Mac:**
```bash
lsof -i :3333
kill -9 <PID>
```

### Erro: "ENOENT: no such file or directory 'database.db'"

```bash
npm run migrate
```

### Erro: "Invalid token"

- Certifique-se de que o token está sendo enviado corretamente
- Verifique se o token não expirou
- Tente fazer login novamente

---

## 📈 Performance e Segurança

### Boas Práticas Implementadas

- ✅ **Hashing de Senhas**: Bcryptjs com salt
- ✅ **JWT Tokens**: Autenticação sem estado (stateless)
- ✅ **CORS**: Apenas origens permitidas
- ✅ **Erro Handling**: Mensagens de erro consistentes
- ✅ **Middleware**: Autenticação e autorização em todas as rotas protegidas

### Recomendações para Produção

1. **Usar HTTPS** em vez de HTTP
2. **Variáveis de Ambiente** para dados sensíveis
3. **JWT_SECRET** muito forte e secreto
4. **Rate Limiting** para prevenir ataques
5. **Validação de Dados** mais rigorosa
6. **Logging** estruturado
7. **Monitoramento** de erros

---

## 📚 Dependências

| Pacote | Versão | Descrição |
|---|---|---|
| express | ^4.22.1 | Framework web |
| jsonwebtoken | ^9.0.1 | Criação/validação de JWT |
| bcryptjs | ^3.0.3 | Hash de senhas |
| knex | ^3.1.0 | Query builder |
| sqlite3 | ^5.1.6 | Banco de dados |
| cors | ^2.8.5 | CORS middleware |
| cookie-parser | ^1.4.6 | Parser de cookies |
| express-async-errors | ^3.1.1 | Handle async errors |
| nodemon | ^3.0.1 | Dev: auto-reload |

---

## 🚀 Deployment

### Preparar para Produção

1. **Instalar dependências:**
   ```bash
   npm install --production
   ```

2. **Criar arquivo .env:**
   ```bash
   PORT=3333
   NODE_ENV=production
   JWT_SECRET=<chave-super-secreta>
   ```

3. **Rodar migrations:**
   ```bash
   npm run migrate
   ```

4. **Iniciar servidor:**
   ```bash
   npm start
   ```

### Opções de Deployment

- **Heroku** - PaaS com suporte a Node.js
- **Railway** - Deploy simples de apps Node.js
- **Render** - Similar ao Railway
- **AWS EC2** - Mais complexo, maior controle
- **DigitalOcean** - VPS acessível

---

## 📞 Suporte

Para dúvidas ou problemas:
1. Verifique este README
2. Consulte documentações: [Express](https://expressjs.com/), [Knex](http://knexjs.org/), [JWT](https://jwt.io/)
3. Abra uma issue no repositório

---

**Última atualização:** 30 de Dezembro de 2025

Desenvolvido com ❤️

