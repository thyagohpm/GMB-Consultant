# GMB Consultant - Setup e Acesso Administrativo

## 🚀 Configuração Inicial

### 1. Configurar Supabase

1. Crie um projeto no [Supabase](https://supabase.com)
2. Copie as credenciais do projeto
3. Crie um arquivo `.env` na raiz do projeto:

```env
VITE_SUPABASE_URL=https://seu-projeto.supabase.co
VITE_SUPABASE_ANON_KEY=sua-chave-anonima
```

### 2. Configurar Banco de Dados

Execute os scripts SQL na seguinte ordem no Supabase SQL Editor:

1. **`scripts/setup-database.sql`** - Cria todas as tabelas e estruturas
2. **`scripts/setup-auth.sql`** - Configura autenticação e políticas
3. **`scripts/create-admin-user.sql`** - Cria usuário administrador

### 3. Acesso Administrativo

#### Opção 1: Usuário Admin Pré-configurado
Após executar os scripts, você pode criar um usuário admin via painel do Supabase:

1. Vá para Authentication > Users no painel do Supabase
2. Clique em "Add user"
3. Use as credenciais:
   - **Email**: `admin@gmbconsultant.com`
   - **Senha**: `admin123456`
4. Após criar, execute no SQL Editor:

```sql
UPDATE profiles 
SET is_admin = true, 
    plan = 'agency', 
    credits_limit = 999999 
WHERE email = 'admin@gmbconsultant.com';
```

#### Opção 2: Criar Admin via Código
Use a função administrativa no código:

```typescript
import { adminFunctions } from './src/lib/admin';

// Criar usuário admin
await adminFunctions.createAdminUser('admin@gmbconsultant.com', 'admin123456');
```

### 4. Credenciais de Acesso

**Usuário Administrador:**
- Email: `admin@gmbconsultant.com`
- Senha: `admin123456`
- Plano: Agency (créditos ilimitados)
- Acesso: Painel administrativo completo

### 5. Funcionalidades Admin

O usuário administrador tem acesso a:

- ✅ **Dashboard completo** com todas as funcionalidades
- ✅ **Painel administrativo** em `/admin`
- ✅ **Gerenciamento de usuários** (visualizar, editar planos)
- ✅ **Controle de créditos** (resetar, ajustar limites)
- ✅ **Estatísticas do sistema** (usuários, planos, uso)
- ✅ **Créditos ilimitados** para testar todas as funcionalidades

### 6. Iniciar Aplicação

```bash
npm install
npm run dev
```

Acesse: `http://localhost:5173`

### 7. Estrutura de Planos

| Plano | Créditos | Funcionalidades |
|-------|----------|-----------------|
| **Free** | 5/mês | Básicas |
| **Pro** | 50/mês | Todas + Análise concorrência |
| **Agency** | 200/mês | Todas + White label |
| **Admin** | Ilimitado | Todas + Painel admin |

### 8. Troubleshooting

**Problema**: Não consegue fazer login
- Verifique se executou todos os scripts SQL
- Confirme as variáveis de ambiente
- Verifique se o usuário foi criado no Supabase Auth

**Problema**: Erro de permissão
- Verifique se RLS está configurado corretamente
- Confirme se o usuário tem `is_admin = true`

**Problema**: Funcionalidades não carregam
- Verifique se todas as tabelas foram criadas
- Confirme se as políticas de segurança estão ativas

### 9. Próximos Passos

1. **Integrar IA real** (OpenAI/Anthropic)
2. **Configurar pagamentos** (Stripe/Kiwify)
3. **Deploy em produção**
4. **Configurar domínio personalizado**

---

## 🔐 Segurança

- RLS habilitado em todas as tabelas
- Políticas de acesso por usuário
- Funções administrativas protegidas
- Validação de permissões em tempo real
