# 🤝 Guia de Contribuição e Desenvolvimento

Obrigado por se interessar em contribuir para o projeto **Authentication and Authorization System**! Este documento fornece orientações para garantir qualidade e consistência no código.

---

## 📋 Índice

- [Código de Conduta](#código-de-conduta)
- [Como Contribuir](#como-contribuir)
- [Padrões de Código](#padrões-de-código)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Guidelines](#pull-request-guidelines)
- [Desenvolvimento Local](#desenvolvimento-local)
- [Testes](#testes)
- [Reportar Bugs](#reportar-bugs)

---

## 📜 Código de Conduta

### Nossa Promessa

Nós, como membros, colaboradores e líderes, nos comprometemos a tornar a participação em nossa comunidade uma experiência livre de assédio para todos, independentemente de idade, tamanho do corpo, deficiência, etnia, identidade e expressão de gênero, nível de experiência, educação, status socioeconômico, nacionalidade, aparência pessoal, raça, religião ou identidade e orientação sexual.

### Nossos Padrões

Exemplos de comportamento que contribuem para criar um ambiente positivo incluem:

- Usar linguagem acolhedora e inclusiva
- Ser respeitoso com pontos de vista e experiências divergentes
- Aceitar críticas construtivas graciosamente
- Focar no que é melhor para a comunidade
- Mostrar empatia com outros membros da comunidade

Exemplos de comportamento inaceitável incluem:

- Linguagem ou imagens sexualizadas
- Ataques pessoais
- Comentários insultuosos/depreciativos
- Assédio público ou privado
- Publicação de informações privadas sem consentimento

---

## 🚀 Como Contribuir

### Tipos de Contribuição

1. **Bug Fixes** - Correção de erros
2. **Features** - Novas funcionalidades
3. **Documentation** - Melhorias na documentação
4. **Tests** - Adição de testes
5. **Performance** - Otimizações
6. **Security** - Melhorias de segurança

### Processo de Contribuição

#### 1. Fork o Repositório

```bash
# Vá para o repositório no GitHub
# Clique em "Fork" no canto superior direito
```

#### 2. Clone Seu Fork

```bash
git clone https://github.com/seu-usuario/Authentication-and-Authorization.git
cd Authentication-and-Authorization
```

#### 3. Adicione o Repositório Original como Remote

```bash
git remote add upstream https://github.com/usuario-original/Authentication-and-Authorization.git
```

#### 4. Crie Uma Branch Para Sua Feature

```bash
# Atualize a branch main
git checkout main
git pull upstream main

# Crie uma nova branch
git checkout -b feature/sua-feature-aqui
```

#### 5. Faça Suas Alterações

```bash
# Faça as mudanças no código
# Teste suas alterações
# Adicione testes se aplicável
```

#### 6. Commit Suas Mudanças

```bash
git add .
git commit -m "feat: descrição clara da mudança"
```

(Veja [Commit Guidelines](#commit-guidelines))

#### 7. Push Para Seu Fork

```bash
git push origin feature/sua-feature-aqui
```

#### 8. Abra Um Pull Request

- Vá para o repositório original
- Clique em "New Pull Request"
- Selecione sua branch
- Preencha a descrição do PR (use o template)

---

## 📐 Padrões de Código

### JavaScript/Node.js (Backend)

#### Estilo

```javascript
// ✅ Bom
const getUserById = (id) => {
  return users.find(user => user.id === id);
};

// ❌ Ruim
const get_user_by_id = (id) => {
  return users.find(u => u.id === id);
};
```

#### Nomeação

- **Variáveis/Funções**: `camelCase`
- **Constantes**: `UPPER_SNAKE_CASE`
- **Classes**: `PascalCase`

```javascript
// ✅ Bom
const MAX_RETRIES = 3;
const getUserData = () => {};
class UserController {}

// ❌ Ruim
const max_retries = 3;
const get_user_data = () => {};
class user_controller {}
```

#### Async/Await

```javascript
// ✅ Bom
try {
  const user = await api.getUser(id);
  console.log(user);
} catch (error) {
  console.error('Erro:', error.message);
}

// ❌ Ruim
api.getUser(id).then(user => {
  console.log(user);
}).catch(error => {
  console.log(error);
});
```

#### Comentários

```javascript
// ✅ Bom
/**
 * Obtém um usuário pelo ID
 * @param {number} id - ID do usuário
 * @returns {Promise<User>} Dados do usuário
 */
const getUserById = async (id) => {
  // Validar ID
  if (!id) throw new Error('ID é obrigatório');
  
  return await db.users.findById(id);
};

// ❌ Ruim
const getUserById = (id) => {
  // pegando usuario
  return db.users.findById(id);
};
```

### React/JavaScript (Frontend)

#### Componentes Funcionais

```javascript
// ✅ Bom
import styled from 'styled-components';

const Container = styled.div`
  display: flex;
  padding: 1rem;
`;

export function UserCard({ user, onDelete }) {
  return (
    <Container>
      <h2>{user.name}</h2>
      <button onClick={() => onDelete(user.id)}>
        Deletar
      </button>
    </Container>
  );
}

// ❌ Ruim
export function userCard(props) {
  return (
    <div style={{ display: 'flex', padding: '1rem' }}>
      <h2>{props.user.name}</h2>
      <button onClick={() => props.onDelete(props.user.id)}>
        Deletar
      </button>
    </div>
  );
}
```

#### Hooks

```javascript
// ✅ Bom
export function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchUsers();
  }, []);

  const fetchUsers = async () => {
    try {
      const response = await api.get('/users');
      setUsers(response.data);
    } finally {
      setLoading(false);
    }
  };

  if (loading) return <p>Carregando...</p>;

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

// ❌ Ruim
export function UserList() {
  let users = [];
  
  useEffect(() => {
    api.get('/users').then(res => {
      users = res.data;  // NÃO fazer isso
    });
  }, []);

  return <p>{users.length}</p>;
}
```

### SQL/Database

```javascript
// ✅ Bom
const user = await db('users')
  .where('email', email)
  .first();

// ❌ Ruim
const user = await db.raw('SELECT * FROM users WHERE email = ?', [email]);
```

---

## 📝 Commit Guidelines

Seguimos a especificação de **Conventional Commits**.

### Formato

```
<tipo>(<escopo>): <assunto>

<corpo>

<footer>
```

### Tipos

- **feat**: Nova funcionalidade
- **fix**: Correção de bug
- **docs**: Mudanças na documentação
- **style**: Formatação, sem mudança de lógica
- **refactor**: Refatoração de código
- **perf**: Melhoria de performance
- **test**: Adição ou correção de testes
- **chore**: Atualizações de dependências

### Exemplos

```bash
# Feature
git commit -m "feat(auth): adicionar autenticação com 2FA"

# Bug Fix
git commit -m "fix(products): corrigir cálculo de preço"

# Documentation
git commit -m "docs(readme): atualizar instruções de instalação"

# Refactor
git commit -m "refactor(api): melhorar estrutura de middlewares"

# Performance
git commit -m "perf(database): otimizar queries de produtos"
```

---

## 🔄 Pull Request Guidelines

### Template de PR

```markdown
## Descrição
Descrição clara e concisa das mudanças.

## Tipo de Mudança
- [ ] Bug fix
- [ ] Nova funcionalidade
- [ ] Breaking change
- [ ] Documentação

## Como Testar
Passos para testar a mudança:
1. ...
2. ...
3. ...

## Checklist
- [ ] Meu código segue o estilo do projeto
- [ ] Executei linting e corrigi erros
- [ ] Adicionei testes
- [ ] Atualizei a documentação
- [ ] Sem warnings na build

## Issues Relacionadas
Fecha #123
```

### Critérios de Aceitação

- ✅ Código segue os padrões do projeto
- ✅ Todos os testes passam
- ✅ Documentação atualizada
- ✅ Sem conflitos com a branch main
- ✅ Pelo menos 1 aprovação de review

---

## 💻 Desenvolvimento Local

### Configuração do Ambiente

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/Authentication-and-Authorization.git
cd Authentication-and-Authorization

# Instale dependências da API
cd api
npm install

# Instale dependências do Frontend
cd ../web
npm install
```

### Executar em Desenvolvimento

**Terminal 1 - API:**
```bash
cd api
npm run dev
# Roda em http://localhost:3333
```

**Terminal 2 - Frontend:**
```bash
cd web
npm run dev
# Roda em http://localhost:5173
```

### EditorConfig

O projeto usa `.editorconfig` para manter consistência entre editores.

```
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false
```

### ESLint

Execute linter:
```bash
npm run lint

# Corrigir automaticamente
npm run lint -- --fix
```

---

## ✅ Testes

### Adicionar Testes

Os testes devem estar no mesmo diretório do arquivo, com sufixo `.test.js`:

```javascript
// users.test.js
describe('Users', () => {
  test('deve criar um usuário', () => {
    const user = createUser('João', 'joao@email.com');
    expect(user.name).toBe('João');
  });
});
```

### Rodar Testes

```bash
npm test
npm test -- --coverage  # Com cobertura
```

---

## 🐛 Reportar Bugs

### Antes de Reportar

1. Verifique se o bug já foi reportado
2. Verifique a documentação
3. Limpe o cache (`npm cache clean`)
4. Tente reproduzir em versão mais recente

### Template de Bug

```markdown
## Descrição
Descrição clara do bug.

## Passos para Reproduzir
1. ...
2. ...
3. ...

## Comportamento Esperado
O que deveria acontecer.

## Comportamento Atual
O que realmente acontece.

## Environment
- SO: Windows/Mac/Linux
- Node: v18.0.0
- npm: 9.0.0

## Logs/Screenshots
Cole logs e screenshots se aplicável.
```

---

## 🔐 Segurança

### Reportar Vulnerabilidades

**NÃO** abra um issue público para vulnerabilidades!

Envie um email para: `security@projeto.com` com:
- Descrição da vulnerabilidade
- Passos para reproduzir
- Impacto potencial
- Sugestão de fix (se tiver)

---

## 📚 Recursos Úteis

### Documentação
- [Express.js](https://expressjs.com/)
- [React Docs](https://react.dev/)
- [Styled Components](https://styled-components.com/)
- [Knex.js](http://knexjs.org/)
- [Git](https://git-scm.com/doc)

### Ferramentas Recomendadas
- **Editor**: VS Code
- **Extensões**: ESLint, Prettier, Thunder Client
- **Terminal**: PowerShell, Bash, Zsh

---

## 🎓 Boas Práticas

### Geral

✅ **Faça:**
- Escrever código limpo e legível
- Testar suas mudanças
- Documentar mudanças importantes
- Pedir ajuda quando necessário
- Ser respeitoso com outros contribuidores

❌ **Não faça:**
- Commitar código que quebra a build
- Adicionar dependências sem discussão
- Fazer múltiplas mudanças em um PR
- Mergear seu próprio PR
- Usar linguagem inadequada

### Backend

✅ Sempre validar entrada de usuário
✅ Usar try-catch em operações async
✅ Retornar mensagens de erro claras
✅ Logar eventos importantes
✅ Usar variáveis de ambiente para dados sensíveis

### Frontend

✅ Manter componentes pequenos e focados
✅ Usar custom hooks para lógica reutilizável
✅ Memorizr componentes quando necessário
✅ Otimizar renderizações com useCallback/useMemo
✅ Testar integração com API

---

## 📊 Processo de Review

### O Que Esperamos

1. **Code Quality**: Código limpo e bem estruturado
2. **Tests**: Cobertura adequada de testes
3. **Docs**: Documentação clara
4. **Performance**: Sem regressões de performance
5. **Security**: Sem vulnerabilidades

### Timeline

- PR submetido → Review em até 2 dias
- Comentários do revisor → Resposta esperada em 1 dia
- Aprovação → Merge automático após CI passar

---

## 🏆 Reconhecimento

Todos os contribuidores são reconhecidos em:
- [CONTRIBUTORS.md](CONTRIBUTORS.md)
- Seção de contribuidores no README

---

## 📞 Contato

Dúvidas? Entre em contato:
- GitHub Issues
- Email: `help@projeto.com`
- Discord: [link do servidor]

---

**Obrigado por contribuir! 🎉**

Última atualização: 30 de Dezembro de 2025

