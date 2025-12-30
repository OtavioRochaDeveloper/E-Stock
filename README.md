# 🔐 Authentication and Authorization System

Um sistema completo de autenticação e autorização com **API REST (Node.js + Express)** e **Frontend (React + Vite)**, implementando controle de acesso baseado em papéis (RBAC - Role-Based Access Control).

---

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Arquitetura do Projeto](#arquitetura-do-projeto)
- [Pré-requisitos](#pré-requisitos)
- [Instalação](#instalação)
- [Configuração](#configuração)
- [Execução](#execução)
- [API REST](#api-rest)
- [Interface Web](#interface-web)
- [Estrutura de Papéis (Roles)](#estrutura-de-papéis-roles)
- [Fluxo de Autenticação](#fluxo-de-autenticação)
- [Banco de Dados](#banco-de-dados)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Contribuição](#contribuição)

---

## 🎯 Visão Geral

Este projeto implementa um sistema robusto de **autenticação com JWT** e **autorização baseada em papéis de usuário**. Os usuários podem ser classificados como:

- **Admin**: Acesso total ao sistema
- **Customer**: Cliente com acesso limitado
- **Sale**: Vendedor com permissões específicas

A aplicação é dividida em duas partes:
1. **API Backend** - Servidor Node.js com Express
2. **Frontend Web** - Interface React com Vite

---

## 🏗️ Arquitetura do Projeto

```
Authentication-and-Authorization/
├── api/                          # Backend - Node.js + Express
│   ├── src/
│   │   ├── configs/              # Configurações de autenticação
│   │   ├── controllers/          # Controladores da API
│   │   ├── database/             # Configuração e migrations do banco
│   │   ├── middlewares/          # Middlewares (autenticação, autorização)
│   │   ├── routes/               # Definição de rotas
│   │   ├── utils/                # Funções utilitárias
│   │   └── server.js             # Arquivo principal do servidor
│   ├── knexfile.js               # Configuração do Knex (migrations)
│   ├── insomnia_routes.json      # Coleção de rotas para teste
│   └── package.json              # Dependências do projeto
│
└── web/                          # Frontend - React + Vite
    ├── src/
    │   ├── components/           # Componentes React reutilizáveis
    │   ├── hooks/                # Custom hooks (autenticação)
    │   ├── pages/                # Páginas da aplicação
    │   ├── routes/               # Roteamento da aplicação
    │   ├── services/             # Chamadas à API (axios)
    │   ├── styles/               # Estilos globais e tema
    │   ├── utils/                # Funções utilitárias (roles)
    │   └── main.jsx              # Arquivo de entrada React
    ├── index.html                # Template HTML
    ├── vite.config.js            # Configuração do Vite
    └── package.json              # Dependências do projeto
```

---

## 📦 Pré-requisitos

Antes de começar, você precisa ter instalado:

- **Node.js** v16 ou superior ([Download](https://nodejs.org/))
- **npm** ou **yarn** (geralmente vem com Node.js)
- **Git** (opcional, para clonar o repositório)

Para verificar se está tudo instalado:

```bash
node --version
npm --version
```

---

## 🚀 Instalação

### 1. Clone ou navegue até o projeto

```bash
cd Authentication-and-Authorization-main
```

### 2. Instale as dependências da API

```bash
cd api
npm install
```

### 3. Instale as dependências do Frontend

```bash
cd ../web
npm install
```

---

## ⚙️ Configuração

### Configuração da API

O arquivo `api/src/configs/auth.js` contém as configurações de autenticação:

```javascript
// Configuração JWT
const jwtConfig = {
  secret: 'sua-chave-secreta', // Use uma chave forte em produção
  expiresIn: '24h'
};
```

### Configuração do Banco de Dados

O projeto usa **SQLite** com **Knex.js**. O arquivo `api/knexfile.js` define a conexão:

```javascript
development: {
  client: "sqlite3",
  connection: {
    filename: path.resolve(__dirname, "src", "database", "database.db")
  }
}
```

### CORS - Frontend e Backend

A API está configurada para aceitar requisições do frontend em `http://localhost:5173`:

```javascript
app.use(cors({
  origin: ["http://localhost:5173", "http://127.0.0.1:5173"],
  credentials: true
}));
```

---

## 🎮 Execução

### Executar a API (Backend)

```bash
cd api
npm run dev
```

**Saída esperada:**
```
> api@1.0.0 dev
> nodemon ./src/server.js

[nodemon] 3.1.11
Server is running on Port 3333
```

A API estará disponível em: **http://localhost:3333**

### Executar o Frontend (Web)

Em um novo terminal:

```bash
cd web
npm run dev
```

**Saída esperada:**
```
> web@0.0.0 dev
> vite

  VITE v4.5.14  ready in 256 ms

  ➜  Local:   http://127.0.0.1:5173/
  ➜  press h to show help
```

O frontend estará disponível em: **http://localhost:5173**

### Build para Produção

**Frontend:**
```bash
cd web
npm run build
```

Gera a pasta `dist/` pronta para deploy.

---

## 🔌 API REST

### Base URL
```
http://localhost:3333
```

### Autenticação

Todas as requisições protegidas requerem um **JWT Token** no header:

```
Authorization: Bearer seu_jwt_token_aqui
```

### Endpoints

#### 🔑 **Autenticação (Sessions)**

##### Login
```
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
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

---

#### 👥 **Usuários**

##### Criar Usuário
```
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

##### Listar Usuários Validados (Protegido)
```
GET /users/validated
Authorization: Bearer seu_token_aqui
```

**Resposta (200 OK):**
```json
[
  {
    "id": 1,
    "name": "João Silva",
    "email": "usuario@email.com",
    "role": "admin"
  },
  {
    "id": 2,
    "name": "Novo Usuário",
    "email": "novo@email.com",
    "role": "customer"
  }
]
```

---

#### 📦 **Produtos**

##### Listar Produtos (Protegido)
```
GET /products
Authorization: Bearer seu_token_aqui
```

**Resposta (200 OK):**
```json
[
  {
    "id": 1,
    "name": "Produto A",
    "price": 99.99,
    "description": "Descrição do produto"
  }
]
```

##### Criar Produto (Admin Only)
```
POST /products
Authorization: Bearer seu_token_aqui
Content-Type: application/json

{
  "name": "Novo Produto",
  "price": 149.99,
  "description": "Descrição"
}
```

**Resposta (201 Created):**
```json
{
  "id": 2,
  "name": "Novo Produto",
  "price": 149.99,
  "description": "Descrição"
}
```

---

#### 💰 **Vendas**

##### Listar Vendas (Admin ou Sale)
```
GET /sales
Authorization: Bearer seu_token_aqui
```

**Resposta (200 OK):**
```json
[
  {
    "id": 1,
    "product_id": 1,
    "quantity": 5,
    "total_price": 499.95,
    "created_at": "2024-12-30T10:30:00Z"
  }
]
```

---

## 🌐 Interface Web

### Páginas Disponíveis

#### 🔐 **Autenticação**
- **Login** (`/signin`) - Acesso para usuários existentes
- **Registro** (`/signup`) - Criação de nova conta

#### 📱 **Dashboard - Customer**
- **Home** (`/`) - Página inicial
- **Produtos** (`/product`) - Catálogo de produtos

#### 📊 **Dashboard - Admin**
- **Home** (`/`) - Página inicial
- **Produtos** (`/product`) - Gerenciamento de produtos
- **Relatório de Vendas** (`/sales-report`) - Visualização de vendas
- **Fornecedores** (`/suppliers`) - Gerenciamento de fornecedores

#### 🛍️ **Dashboard - Sale**
- Acesso similar ao admin (vendas)
- Permissões específicas para vendedores

#### ❌ **Not Found** (`*`) - Página para rotas não existentes

### Componentes Principais

- **Header** - Navegação e menu de usuário
- **Button** - Componente reutilizável de botão
- **Input** - Campo de entrada com validação
- **Feature** - Card de apresentação de funcionalidades

### Hooks Customizados

#### `useAuth()`
Hook que fornece contexto de autenticação:

```javascript
const { user, signIn, signOut, loading } = useAuth();

// Fazer login
signIn(email, password);

// Fazer logout
signOut();

// Acessar dados do usuário
console.log(user.role); // 'admin', 'customer', 'sale'
```

---

## 👤 Estrutura de Papéis (Roles)

O sistema implementa 3 papéis principais:

### 1. **Admin** (Administrador)
- ✅ Acesso total ao sistema
- ✅ Gerenciar produtos (CRUD)
- ✅ Visualizar relatórios de vendas
- ✅ Gerenciar fornecedores
- ✅ Visualizar todos os usuários

### 2. **Customer** (Cliente)
- ✅ Visualizar catálogo de produtos
- ✅ Realizar compras
- ✅ Perfil pessoal
- ❌ Sem acesso à área administrativa

### 3. **Sale** (Vendedor)
- ✅ Visualizar catálogo de produtos
- ✅ Registrar vendas
- ✅ Visualizar relatórios de vendas
- ✅ Gerenciar clientes
- ❌ Sem acesso total (limitado)

---

## 🔐 Fluxo de Autenticação

### 1. **Registro do Usuário**
```
Usuário preenche formulário → API cria usuário (hash de senha) → Redirecionado ao Login
```

### 2. **Login**
```
Usuário faz login → Validação de email/senha → API retorna JWT Token → Token armazenado localmente
```

### 3. **Requisições Autenticadas**
```
Frontend envia requisição + Token → Middleware valida Token → Executa ação → Retorna dados
```

### 4. **Logout**
```
Usuário clica logout → Token removido localmente → Redirecionado ao Login
```

### 5. **Autorização Baseada em Papéis**
```
Acessa rota protegida → Middleware verifica role do usuário → Se autorizado → Acesso concedido
```

---

## 💾 Banco de Dados

### Configuração
- **Tipo**: SQLite3
- **ORM**: Knex.js
- **Localização**: `api/src/database/database.db`

### Migrations

Executar migrations:
```bash
cd api
npm run migrate
```

Reverter migrations:
```bash
cd api
npx knex migrate:rollback
```

### Estrutura de Tabelas

#### `users`
```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT NOT NULL UNIQUE,
  password TEXT NOT NULL,
  role TEXT NOT NULL DEFAULT 'customer',
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

#### `products`
```sql
CREATE TABLE products (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  price REAL NOT NULL,
  description TEXT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

#### `sales`
```sql
CREATE TABLE sales (
  id INTEGER PRIMARY KEY,
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

## 🛠️ Tecnologias Utilizadas

### Backend (API)

| Tecnologia | Versão | Descrição |
|---|---|---|
| **Node.js** | v18+ | Runtime JavaScript |
| **Express** | ^4.22.1 | Framework web |
| **JWT** | ^9.0.1 | Autenticação com tokens |
| **Bcryptjs** | ^3.0.3 | Hash de senhas |
| **Knex.js** | ^3.1.0 | Query builder SQL |
| **SQLite3** | ^5.1.6 | Banco de dados |
| **CORS** | ^2.8.5 | Requisições cross-origin |
| **Cookie Parser** | ^1.4.6 | Parsing de cookies |
| **Nodemon** | ^3.0.1 | Auto-reload em desenvolvimento |

### Frontend (Web)

| Tecnologia | Versão | Descrição |
|---|---|---|
| **React** | ^18.3.1 | Biblioteca UI |
| **Vite** | ^4.5.14 | Build tool moderno |
| **React Router DOM** | ^6.30.2 | Roteamento |
| **Styled Components** | ^6.1.19 | CSS-in-JS |
| **Axios** | ^1.13.2 | Cliente HTTP |
| **React Icons** | ^4.12.0 | Ícones SVG |
| **ESLint** | ^8.57.1 | Linter de código |

---

## 🚦 Executar Testes com Insomnia/Postman

O arquivo `api/insomnia_routes.json` contém uma coleção pronta de requisições. 

### Para importar no Insomnia:
1. Abra o Insomnia
2. Clique em "Create" → "Import"
3. Selecione o arquivo `api/insomnia_routes.json`
4. Todas as rotas serão importadas automaticamente

### Ordem de Testes Recomendada:
1. ✅ **POST /sessions** - Login (obter token)
2. ✅ **POST /users** - Criar novo usuário
3. ✅ **GET /users/validated** - Listar usuários (com token)
4. ✅ **GET /products** - Listar produtos
5. ✅ **POST /products** - Criar produto (admin)
6. ✅ **GET /sales** - Listar vendas

---

## 📝 Variáveis de Ambiente

Crie um arquivo `.env` na raiz da API (se necessário):

```env
# api/.env
PORT=3333
NODE_ENV=development
JWT_SECRET=sua-chave-secreta-aqui
JWT_EXPIRES_IN=24h
DATABASE_URL=sqlite:./src/database/database.db
```

---

## 🐛 Troubleshooting

### Erro: "Port 3333 already in use"
```bash
# Windows: Find e kill o processo
netstat -ano | findstr :3333
taskkill /PID <PID> /F

# Linux/Mac:
lsof -i :3333
kill -9 <PID>
```

### Erro: "CORS error"
- Verifique se o frontend está em `http://localhost:5173`
- Verifique a configuração de CORS em `api/src/server.js`

### Erro: "Token inválido"
- Certifique-se de que o token está sendo enviado corretamente
- Verifique se o token não expirou
- Limpe os cookies e tente novamente

### Banco de dados não encontrado
```bash
cd api
npm run migrate
```

---

## 📚 Documentação Adicional

### Estrutura de Middlewares

#### `ensureAuthenticated.js`
Valida se o usuário está autenticado via JWT token.

```javascript
// Uso em rotas protegidas
router.get("/", ensureAuthenticated, controller.method);
```

#### `verifyUserAuthorization.js`
Verifica se o usuário tem a role necessária.

```javascript
// Uso em rotas de admin
router.post("/", verifyUserAuthorization("admin"), controller.create);

// Uso com múltiplas roles
router.get("/", verifyUserAuthorization(["admin", "sale"]), controller.index);
```

---

## 🤝 Contribuição

Para contribuir com o projeto:

1. **Fork** o repositório
2. **Clone** seu fork localmente
3. **Crie** uma branch para sua feature (`git checkout -b feature/MinhaFeature`)
4. **Commit** suas mudanças (`git commit -m 'Adiciona MinhaFeature'`)
5. **Push** para a branch (`git push origin feature/MinhaFeature`)
6. **Abra** um Pull Request

---

## 📄 Licença

Este projeto está sob a licença **ISC**. Veja o arquivo `LICENSE` para mais informações.

---

## 👨‍💻 Autor

Projeto desenvolvido como exemplo educacional de autenticação e autorização em aplicações web.

---

## 📞 Suporte

Caso encontre algum problema ou tenha dúvidas:

1. Verifique o arquivo `.md` deste README
2. Consulte a documentação das tecnologias utilizadas
3. Abra uma issue no repositório

---

## 🎉 Próximos Passos

- [ ] Implementar refresh tokens
- [ ] Adicionar autenticação OAuth2
- [ ] Implementar 2FA (Two-Factor Authentication)
- [ ] Adicionar testes automatizados
- [ ] Melhorar tratamento de erros
- [ ] Adicionar logging estruturado
- [ ] Documentação Swagger/OpenAPI

---

**Última atualização:** 30 de Dezembro de 2025

Aproveite! 🚀
