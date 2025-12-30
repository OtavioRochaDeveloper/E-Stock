# 🌐 Frontend Web - Authentication and Authorization

Interface web moderna para o sistema de autenticação e autorização. Desenvolvido com **React 18**, **Vite**, **Styled Components** e **React Router**, oferecendo uma experiência de usuário intuitiva e responsiva.

---

## 📋 Índice

- [Quick Start](#quick-start)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Páginas da Aplicação](#páginas-da-aplicação)
- [Componentes](#componentes)
- [Hooks Customizados](#hooks-customizados)
- [Roteamento](#roteamento)
- [Integração com API](#integração-com-api)
- [Estilos e Tema](#estilos-e-tema)
- [Scripts Disponíveis](#scripts-disponíveis)

---

## 🚀 Quick Start

### 1. Instalar Dependências
```bash
cd web
npm install
```

### 2. Iniciar o Servidor de Desenvolvimento
```bash
npm run dev
```

A aplicação estará disponível em: **http://localhost:5173**

### 3. Build para Produção
```bash
npm run build
```

---

## 📁 Estrutura do Projeto

```
web/
├── src/
│   ├── components/              # Componentes reutilizáveis
│   │   ├── Button/
│   │   │   ├── index.jsx
│   │   │   └── styles.js
│   │   ├── Input/
│   │   │   ├── index.jsx
│   │   │   └── styles.js
│   │   ├── Header/
│   │   │   ├── index.jsx
│   │   │   └── styles.js
│   │   └── Feature/
│   │       ├── index.jsx
│   │       └── styles.js
│   │
│   ├── hooks/                   # Custom hooks
│   │   └── auth.jsx             # useAuth() - Contexto de autenticação
│   │
│   ├── pages/                   # Páginas da aplicação
│   │   ├── Home/
│   │   ├── SignIn/
│   │   ├── SignUp/
│   │   ├── Product/
│   │   ├── SalesReport/
│   │   ├── Suppliers/
│   │   └── NotFound/
│   │
│   ├── routes/                  # Configuração de rotas
│   │   ├── index.jsx            # Router principal
│   │   ├── auth.routes.jsx      # Rotas de autenticação
│   │   ├── admin.routes.jsx     # Rotas do admin
│   │   ├── customer.routes.jsx  # Rotas do customer
│   │   └── sale.routes.jsx      # Rotas do vendedor
│   │
│   ├── services/                # Integração com API
│   │   └── api.js               # Cliente Axios
│   │
│   ├── styles/                  # Estilos globais
│   │   ├── global.js            # Estilos globais (reset, variáveis)
│   │   └── theme.js             # Tema da aplicação
│   │
│   ├── utils/                   # Funções utilitárias
│   │   └── roles.js             # Definição de roles
│   │
│   └── main.jsx                 # Arquivo de entrada React
│
├── index.html                   # Template HTML
├── vite.config.js               # Configuração do Vite
└── package.json                 # Dependências do projeto
```

---

## 📄 Páginas da Aplicação

### 🔐 Autenticação

#### **SignIn** (Login)
- **Rota**: `/signin`
- **Público**: Sim (não requer autenticação)
- **Funcionalidade**: Permite login de usuários existentes
- **Campos**: Email, Senha
- **Ação**: Envia requisição POST para `/sessions` na API

```jsx
// Uso do hook
const { signIn } = useAuth();

const handleLogin = async (email, password) => {
  await signIn(email, password);
};
```

#### **SignUp** (Registro)
- **Rota**: `/signup`
- **Público**: Sim (não requer autenticação)
- **Funcionalidade**: Permite criação de nova conta
- **Campos**: Nome, Email, Senha, Confirmar Senha
- **Ação**: Envia requisição POST para `/users` na API

---

### 📱 Dashboard - Customer (Cliente)

#### **Home**
- **Rota**: `/`
- **Protegido**: Sim (requer token)
- **Funcionalidade**: Página inicial com informações do usuário
- **Componentes**: Header, boas-vindas, navegação

#### **Product** (Catálogo)
- **Rota**: `/product`
- **Protegido**: Sim (requer token)
- **Funcionalidade**: Lista produtos disponíveis
- **Ações**: Visualizar produtos

---

### 📊 Dashboard - Admin (Administrador)

Acesso a todas as páginas do customer PLUS:

#### **Product Management**
- **Rota**: `/product`
- **Protegido**: Sim (requer admin)
- **Funcionalidade**: CRUD de produtos
- **Ações**: 
  - Visualizar produtos
  - Criar novo produto
  - Editar produto
  - Deletar produto

#### **Sales Report**
- **Rota**: `/sales-report`
- **Protegido**: Sim (requer admin)
- **Funcionalidade**: Visualizar relatório de vendas
- **Dados**: 
  - Total de vendas
  - Produtos mais vendidos
  - Vendedor top
  - Gráficos e estatísticas

#### **Suppliers**
- **Rota**: `/suppliers`
- **Protegido**: Sim (requer admin)
- **Funcionalidade**: Gerenciar fornecedores
- **Ações**:
  - Listar fornecedores
  - Adicionar fornecedor
  - Atualizar fornecedor
  - Remover fornecedor

---

### 🛍️ Dashboard - Sale (Vendedor)

Acesso limitado para gerenciamento de vendas:

#### **Home**
- Dashboard com resumo de vendas

#### **Product**
- Visualizar catálogo para vender

#### **Sales Report**
- Suas vendas e comissões

---

### ❌ Not Found
- **Rota**: `*`
- **Funcionalidade**: Página de erro 404
- **Ação**: Redireciona para home

---

## 🧩 Componentes

### 1. **Button**
Botão reutilizável com estilos customizáveis.

```jsx
import { Button } from './components/Button';

export function MyPage() {
  return (
    <Button 
      onClick={() => console.log('Clicado!')}
      disabled={false}
    >
      Enviar
    </Button>
  );
}
```

**Props:**
- `children` - Texto do botão
- `onClick` - Função de callback
- `disabled` - Desabilita o botão
- `type` - Tipo do botão (button, submit, reset)

---

### 2. **Input**
Campo de entrada com validação integrada.

```jsx
import { Input } from './components/Input';

export function MyForm() {
  return (
    <Input 
      placeholder="Digite seu email"
      type="email"
      onChange={(e) => setEmail(e.target.value)}
      required={true}
    />
  );
}
```

**Props:**
- `placeholder` - Texto de placeholder
- `type` - Tipo do input (text, email, password, etc)
- `onChange` - Callback ao alterar
- `value` - Valor do input
- `required` - Campo obrigatório
- `disabled` - Desabilita o input

---

### 3. **Header**
Barra de navegação com menu de usuário.

```jsx
import { Header } from './components/Header';

export function MyApp() {
  return (
    <>
      <Header />
      {/* Conteúdo da página */}
    </>
  );
}
```

**Funcionalidades:**
- Logo da aplicação
- Menu de navegação baseado em role
- Menu de usuário (dropdown)
- Botão de logout

---

### 4. **Feature**
Card de apresentação de funcionalidade.

```jsx
import { Feature } from './components/Feature';

export function HomePage() {
  return (
    <Feature 
      icon={<FeatureIcon />}
      title="Título da Feature"
      description="Descrição da funcionalidade"
    />
  );
}
```

**Props:**
- `icon` - Ícone da feature
- `title` - Título
- `description` - Descrição

---

## 🎣 Hooks Customizados

### useAuth()

Hook que fornece acesso ao contexto de autenticação.

```jsx
import { useAuth } from './hooks/auth';

export function MyComponent() {
  const { user, signIn, signOut, loading } = useAuth();

  if (loading) {
    return <p>Carregando...</p>;
  }

  if (!user) {
    return <p>Não autenticado</p>;
  }

  return (
    <div>
      <p>Bem-vindo, {user.name}!</p>
      <p>Role: {user.role}</p>
      <button onClick={signOut}>Logout</button>
    </div>
  );
}
```

**Propriedades:**
- `user` - Objeto do usuário autenticado
  - `id` - ID do usuário
  - `name` - Nome do usuário
  - `email` - Email do usuário
  - `role` - Role do usuário (admin, customer, sale)
- `signIn(email, password)` - Função para fazer login
- `signOut()` - Função para fazer logout
- `loading` - Status de carregamento

**Exemplo de Login:**
```jsx
const { signIn } = useAuth();

const handleLogin = async () => {
  try {
    await signIn('usuario@email.com', 'senha123');
    // Usuário autenticado, redirecionado automaticamente
  } catch (error) {
    console.error('Erro ao fazer login:', error.message);
  }
};
```

---

## 🗺️ Roteamento

### Estrutura de Rotas

A aplicação usa **React Router v6** com roteamento baseado em roles.

```
/                       (Login/Home baseado em autenticação)
├── /signin             (Página de Login)
├── /signup             (Página de Registro)
├── /                   (Home - Protegido)
├── /product            (Catálogo/Gerenciamento de Produtos)
├── /sales-report       (Admin/Sale - Relatório de Vendas)
├── /suppliers          (Admin - Gerenciamento de Fornecedores)
└── *                   (404 - Not Found)
```

### Router Principal (`routes/index.jsx`)

Lógica de roteamento baseada no papel do usuário:

```jsx
export function Routes() {
  const { user } = useAuth();

  if (!user) {
    return <AuthRoutes />;  // Login e Registro
  }

  switch (user.role) {
    case USER_ROLE.ADMIN:
      return <AdminRoutes />;     // Admin routes
    case USER_ROLE.CUSTOMER:
      return <CustomerRoutes />;  // Customer routes
    case USER_ROLE.SALE:
      return <SaleRoutes />;      // Sale routes
    default:
      return <CustomerRoutes />;
  }
}
```

### Auth Routes (`routes/auth.routes.jsx`)

```jsx
export function AuthRoutes() {
  return (
    <Routes>
      <Route path="/signin" element={<SignIn />} />
      <Route path="/signup" element={<SignUp />} />
      <Route path="*" element={<SignIn />} />
    </Routes>
  );
}
```

### Admin Routes (`routes/admin.routes.jsx`)

```jsx
export function AdminRoutes() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/product" element={<Product />} />
      <Route path="/sales-report" element={<SalesReport />} />
      <Route path="/suppliers" element={<Suppliers />} />
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}
```

---

## 🔗 Integração com API

### Cliente Axios (`services/api.js`)

```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:3333'
});

// Interceptor para adicionar token em requisições
api.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('@auth:token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

export { api };
```

### Uso em Componentes

```jsx
import { api } from '../services/api';

export function ProductPage() {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    api.get('/products')
      .then(response => {
        setProducts(response.data);
        setLoading(false);
      })
      .catch(error => {
        console.error('Erro ao carregar produtos:', error);
        setLoading(false);
      });
  }, []);

  if (loading) return <p>Carregando...</p>;

  return (
    <div>
      {products.map(product => (
        <div key={product.id}>
          <h3>{product.name}</h3>
          <p>R$ {product.price}</p>
        </div>
      ))}
    </div>
  );
}
```

### Endpoints Disponíveis

```javascript
// Autenticação
api.post('/sessions', { email, password });
api.post('/users', { name, email, password, role });

// Usuários
api.get('/users/validated');

// Produtos
api.get('/products');
api.post('/products', { name, price, description });

// Vendas
api.get('/sales');
```

---

## 🎨 Estilos e Tema

### Styled Components

A aplicação usa **Styled Components** para CSS-in-JS.

```jsx
import styled from 'styled-components';

const Container = styled.div`
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: ${({ theme }) => theme.colors.background};
`;

export function MyComponent() {
  return <Container>Conteúdo</Container>;
}
```

### Tema Global (`styles/theme.js`)

Define cores, fonts e breakpoints:

```javascript
const theme = {
  colors: {
    background: '#F5F5F5',
    foreground: '#333333',
    primary: '#007bff',
    danger: '#dc3545',
    success: '#28a745'
  },
  fonts: {
    size: {
      small: '12px',
      medium: '16px',
      large: '20px',
      xlarge: '28px'
    }
  }
};
```

### Estilos Globais (`styles/global.js`)

```javascript
import { createGlobalStyle } from 'styled-components';

const GlobalStyles = createGlobalStyle`
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen;
    background: ${({ theme }) => theme.colors.background};
    color: ${({ theme }) => theme.colors.foreground};
  }
`;

export default GlobalStyles;
```

---

## 📝 Scripts Disponíveis

### Desenvolvimento
```bash
npm run dev
```
Inicia o servidor de desenvolvimento do Vite.

**Acesso**: http://localhost:5173

---

### Build para Produção
```bash
npm run build
```
Cria uma versão otimizada para produção na pasta `dist/`.

---

### Preview da Build
```bash
npm run preview
```
Prévia local da build de produção.

---

### Linting
```bash
npm run lint
```
Verifica erros de código com ESLint.

```bash
npm run lint -- --fix
```
Corrige problemas automaticamente.

---

## 🔑 Roles e Permissões

### Definição de Roles (`utils/roles.js`)

```javascript
export const USER_ROLE = {
  ADMIN: 'admin',
  CUSTOMER: 'customer',
  SALE: 'sale'
};
```

### Permissões por Role

| Ação | Admin | Customer | Sale |
|---|---|---|---|
| Visualizar Produtos | ✅ | ✅ | ✅ |
| Criar Produtos | ✅ | ❌ | ❌ |
| Editar Produtos | ✅ | ❌ | ❌ |
| Deletar Produtos | ✅ | ❌ | ❌ |
| Visualizar Vendas | ✅ | ❌ | ✅ |
| Criar Vendas | ✅ | ❌ | ✅ |
| Gerenciar Fornecedores | ✅ | ❌ | ❌ |
| Gerenciar Usuários | ✅ | ❌ | ❌ |

---

## 🔐 Armazenamento de Token

O token JWT é armazenado no **localStorage**:

```javascript
// Salvar token
localStorage.setItem('@auth:token', token);

// Recuperar token
const token = localStorage.getItem('@auth:token');

// Remover token
localStorage.removeItem('@auth:token');
```

**Localização**: Hook `useAuth()` em `hooks/auth.jsx`

---

## 🌐 Configuração CORS

A aplicação está configurada para fazer requisições para `http://localhost:3333`.

Para alterar a URL da API, edite `services/api.js`:

```javascript
const api = axios.create({
  baseURL: 'http://sua-api.com' // Altere aqui
});
```

---

## 📦 Dependências

| Pacote | Versão | Descrição |
|---|---|---|
| react | ^18.3.1 | Biblioteca UI |
| react-dom | ^18.3.1 | Renderização DOM |
| react-router-dom | ^6.30.2 | Roteamento |
| styled-components | ^6.1.19 | CSS-in-JS |
| axios | ^1.13.2 | Cliente HTTP |
| react-icons | ^4.12.0 | Ícones SVG |
| vite | ^4.5.14 | Build tool |
| eslint | ^8.57.1 | Linter |

---

## 🐛 Troubleshooting

### Erro: "Module not found"
```bash
npm install
```

### Erro: "CORS error"
- Certifique-se de que a API está rodando em `http://localhost:3333`
- Verifique a configuração CORS na API
- Reinicie o servidor

### Erro: "Token inválido"
1. Limpe o localStorage:
   ```javascript
   localStorage.clear()
   ```
2. Faça login novamente
3. Verifique se o token não expirou

### Porta 5173 já em uso
```powershell
# Windows
netstat -ano | findstr :5173
taskkill /PID <PID> /F

# Linux/Mac
lsof -i :5173
kill -9 <PID>
```

---

## ⚡ Performance

### Otimizações Implementadas

- ✅ **Lazy Loading** de componentes
- ✅ **Code Splitting** do Vite
- ✅ **Styled Components** com server-side rendering
- ✅ **React Hooks** para estado eficiente

### Melhorias Sugeridas

1. Implementar memoização de componentes
2. Adicionar virtualization para listas grandes
3. Otimizar imagens
4. Implementar service workers para offline

---

## 🚀 Deployment

### Build
```bash
npm run build
```

### Servir Localmente
```bash
npm run preview
```

### Deploy para Vercel
```bash
npm i -g vercel
vercel
```

### Deploy para Netlify
```bash
npm i -g netlify-cli
netlify deploy --prod --dir=dist
```

---

## 📞 Suporte

Para dúvidas ou problemas:
1. Consulte a documentação do [React](https://react.dev/)
2. Verifique [React Router](https://reactrouter.com/)
3. Leia sobre [Styled Components](https://styled-components.com/)
4. Abra uma issue no repositório

---

**Última atualização:** 30 de Dezembro de 2025

Desenvolvido com ❤️

