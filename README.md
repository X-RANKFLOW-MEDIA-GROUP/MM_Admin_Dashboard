# MasseurMatch Admin Dashboard

Dashboard administrativo completo para gerenciamento da plataforma MasseurMatch — um marketplace de massoterapeutas.

---

## 📑 Índice

1. [Visão Geral](#-visão-geral)
2. [Tecnologias](#-tecnologias)
3. [Estrutura do Projeto](#-estrutura-do-projeto)
4. [Instalação e Execução](#-instalação-e-execução)
5. [Variáveis de Ambiente](#-variáveis-de-ambiente)
6. [Páginas e Funcionalidades](#-páginas-e-funcionalidades)
7. [Rotas de API](#-rotas-de-api)
8. [Banco de Dados (Supabase)](#-banco-de-dados-supabase)
9. [Autenticação e Permissões](#-autenticação-e-permissões)
10. [Funções e Onde Alterar](#-funções-e-onde-alterar)
11. [Componentes de Interface](#-componentes-de-interface)
12. [Hooks Customizados](#-hooks-customizados)
13. [Deploy](#-deploy)

---

## 🌐 Visão Geral

O **MasseurMatch Admin Dashboard** é um painel administrativo construído com Next.js 15 e Supabase. Ele permite que administradores da plataforma gerenciem:

- Usuários cadastrados
- Terapeutas e seus processos de verificação
- Assinaturas e pagamentos
- Fila de moderação (aprovações, edições)
- Conteúdo/aplicações submetidas
- Configurações de SEO
- Logs e auditoria
- Configurações globais do sistema

---

## 🚀 Tecnologias

| Tecnologia | Versão | Função |
|---|---|---|
| **Next.js** | 15.3.6 | Framework full-stack com App Router |
| **React** | 19.2.1 | Interface do usuário |
| **TypeScript** | 5 | Tipagem estática |
| **Supabase** | latest | Banco de dados PostgreSQL + Auth |
| **Tailwind CSS** | 3 | Estilização |
| **shadcn/ui** | latest | Componentes de UI (Radix UI) |
| **Stripe** | latest | Integração de pagamentos |
| **Recharts** | latest | Gráficos |
| **react-hook-form + zod** | latest | Formulários e validação |
| **date-fns** | latest | Manipulação de datas |
| **lucide-react** | latest | Ícones |

---

## 📁 Estrutura do Projeto

```
MM_Admin_Dashboard/
├── src/
│   ├── app/                        # Páginas e API (Next.js App Router)
│   │   ├── dashboard/              # Página: Visão geral / estatísticas
│   │   ├── users/                  # Página: Gerenciamento de usuários
│   │   ├── therapists/             # Página: Diretório de terapeutas
│   │   │   └── [id]/               # Página: Detalhes de um terapeuta
│   │   ├── review/                 # Página: Fila de moderação
│   │   ├── verification/           # Redireciona para /review
│   │   ├── moderation/             # Redireciona para /review
│   │   ├── subscriptions/          # Página: Assinaturas
│   │   ├── billing/                # Página: Faturamento e pagamentos
│   │   ├── content/                # Página: Aplicações/submissões
│   │   ├── seo/                    # Página: Ferramentas de SEO
│   │   ├── logs/                   # Página: Logs do sistema
│   │   ├── legal/                  # Página: Aceites legais
│   │   ├── support/                # Página: Suporte / edições de perfil
│   │   ├── settings/               # Página: Configurações do sistema
│   │   ├── login/                  # Página: Login
│   │   ├── forgot-password/        # Página: Esqueci minha senha
│   │   ├── reset-password/         # Página: Redefinir senha
│   │   ├── api/                    # Rotas de API (backend)
│   │   │   ├── auth/               # Autenticação (login, logout, OAuth)
│   │   │   ├── users/              # CRUD de usuários
│   │   │   ├── therapists/         # CRUD + ações de terapeutas
│   │   │   ├── subscriptions/      # Ativar / cancelar assinaturas
│   │   │   ├── payments/           # Sincronizar pagamentos
│   │   │   ├── verification/       # Aprovar / rejeitar verificações
│   │   │   ├── profile-edits/      # Resolver edições de perfil
│   │   │   ├── therapist-edits/    # Resolver edições de terapeuta
│   │   │   ├── api-keys/           # Salvar chaves de API
│   │   │   ├── settings/           # Configurações gerais
│   │   │   └── sitemap/            # Geração de sitemap
│   │   ├── layout.tsx              # Layout raiz (providers globais)
│   │   ├── page.tsx                # Raiz → redireciona para /login
│   │   └── globals.css             # Estilos globais
│   ├── components/
│   │   ├── ui/                     # Componentes shadcn/ui (30+ componentes)
│   │   ├── layout/
│   │   │   ├── main-layout.tsx     # Layout principal: sidebar + conteúdo
│   │   │   ├── header.tsx          # Barra superior com menu de usuário
│   │   │   └── page-header.tsx     # Cabeçalho de página (título + descrição)
│   │   └── common/
│   │       └── data-table.tsx      # Tabela genérica reutilizável
│   ├── lib/
│   │   ├── auth/
│   │   │   ├── server.ts           # Auth server-side (requireAdmin, roles)
│   │   │   └── client.ts           # Auth client-side (OAuth, OTP, senha)
│   │   ├── supabase/
│   │   │   ├── crud.ts             # Operações CRUD genéricas
│   │   │   └── types.ts            # Interfaces e tipos TypeScript
│   │   ├── http/
│   │   │   └── responses.ts        # Utilitários de resposta HTTP
│   │   ├── supabaseAdmin.ts        # Cliente admin do Supabase
│   │   ├── supabaseClient.ts       # Cliente anônimo do Supabase
│   │   ├── supabaseServer.ts       # Cliente server-side do Supabase
│   │   ├── supabaseBrowser.ts      # Cliente browser-side do Supabase
│   │   └── utils.ts                # Funções utilitárias gerais
│   ├── hooks/
│   │   ├── use-toast.ts            # Hook de notificações toast
│   │   └── use-mobile.tsx          # Hook de detecção de dispositivo móvel
│   ├── middleware.ts               # Middleware Next.js (auth + roles)
│   └── middleware_old.ts           # Versão anterior do middleware (referência)
├── docs/
│   ├── COMPLETE_DATABASE_SCHEMA.sql    # Schema completo do banco de dados
│   ├── SUPABASE_SETUP.md               # Guia de configuração do Supabase
│   ├── ADMIN_USERS_SETUP.md            # Criação de usuários admin
│   ├── CREATE_QUICK_ADMIN.sql          # SQL rápido para criar admin
│   └── MIGRATION_FIX_SETTINGS_TABLE.sql# Migração da tabela de settings
├── .env.example                    # Modelo de variáveis de ambiente
├── package.json                    # Dependências e scripts
├── tsconfig.json                   # Configuração TypeScript
├── tailwind.config.ts              # Configuração Tailwind
├── next.config.ts                  # Configuração Next.js
├── components.json                 # Configuração shadcn/ui
├── render.yaml                     # Deploy no Render
└── apphosting.yaml                 # Deploy no Firebase App Hosting
```

---

## 🔧 Instalação e Execução

### Pré-requisitos

- Node.js 18+
- npm ou yarn
- Conta no [Supabase](https://supabase.com)

### Passos

```bash
# 1. Instalar dependências
npm install

# 2. Configurar variáveis de ambiente
cp .env.example .env.local
# Edite .env.local com suas credenciais

# 3. Executar schema no Supabase
# Acesse o SQL Editor do Supabase e rode: docs/COMPLETE_DATABASE_SCHEMA.sql

# 4. Criar usuário admin inicial
# Rode: docs/CREATE_QUICK_ADMIN.sql no SQL Editor do Supabase

# 5. Rodar em desenvolvimento (porta 9002)
npm run dev

# Build de produção
npm run build
npm start
```

Acesse: **http://localhost:9002**

### Scripts disponíveis

| Comando | Função |
|---|---|
| `npm run dev` | Inicia servidor de desenvolvimento (porta 9002) |
| `npm run build` | Gera build de produção |
| `npm start` | Inicia servidor de produção |
| `npm run lint` | Executa ESLint |
| `npm run typecheck` | Verifica tipos TypeScript |

---

## 🔑 Variáveis de Ambiente

Configure no arquivo `.env.local`:

| Variável | Obrigatória | Descrição |
|---|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | ✅ | URL do projeto Supabase |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | ✅ | Chave anônima pública do Supabase |
| `SUPABASE_SERVICE_ROLE_KEY` | ✅ | Chave de serviço secreta (acesso admin ao banco) |
| `NEXT_PUBLIC_APP_URL` | ✅ | URL pública da aplicação (ex: `http://localhost:9002` em dev, `https://admin.seudominio.com` em produção) |
| `NODE_ENV` | — | `development` ou `production` |

> ⚠️ **Nunca** commite o `.env.local` com credenciais reais.

---

## 📄 Páginas e Funcionalidades

### `/dashboard` — Visão Geral

**O que faz:** Exibe cartões de estatísticas em tempo real com métricas gerais da plataforma.

**Métricas exibidas:**
- Total de usuários cadastrados
- Total de assinaturas ativas
- Total de pagamentos processados
- Total de terapeutas (com status)
- Receita total (calculada dos pagamentos)
- Novos cadastros nas últimas 24 horas
- Atividade recente de terapeutas

**Onde alterar:** `src/app/dashboard/page.tsx`

**Efeito de alterações:** Mudar as queries altera quais dados aparecem nos cards. Mudar os títulos dos cards altera o que o admin vê na tela inicial.

---

### `/users` — Gerenciamento de Usuários

**O que faz:** Lista todos os usuários cadastrados na plataforma com paginação e badges de status.

**Funcionalidades:**
- Listagem paginada de usuários
- Badge de status de cada usuário
- Data do último login
- Ações: criar, editar, deletar (via API)

**Onde alterar:** `src/app/users/page.tsx`

**Efeito de alterações:** Adicionar colunas na tabela exige alteração no componente DataTable. Alterar ações modifica o que o admin pode fazer com cada usuário.

---

### `/therapists` — Gerenciamento de Terapeutas

**O que faz:** Exibe o diretório completo de terapeutas com status e plano de assinatura.

**Funcionalidades:**
- Listagem paginada com filtros
- Badges de status: `Pending`, `Active`, `Rejected`, `Suspended`
- Rastreamento de plano/assinatura
- Link para página de detalhes de cada terapeuta

**Onde alterar:** `src/app/therapists/page.tsx`

**Efeito de alterações:** Alterar os filtros modifica quais terapeutas aparecem. Alterar as colunas modifica as informações visíveis na lista.

---

### `/therapists/[id]` — Detalhes do Terapeuta

**O que faz:** Exibe o perfil completo de um terapeuta específico, incluindo documentos de verificação.

**Informações exibidas:**
- Dados pessoais (nome, email, telefone)
- Status de verificação
- Documentos enviados (documento, cartão, selfie, termo assinado)
- Histórico de ações

**Onde alterar:** `src/app/therapists/[id]/page.tsx`

**Efeito de alterações:** Adicionar campos exige atualização da query ao Supabase e do layout visual.

---

### `/review` — Fila de Moderação

**O que faz:** Central de moderação com 3 abas para revisar itens pendentes.

**Abas:**
1. **Verificação** — Revisar documentos submetidos por terapeutas. Ações: Aprovar / Rejeitar
2. **Edições de Perfil** — Revisar solicitações de alteração de perfil de usuários. Ação: Resolver
3. **Edições de Terapeuta** — Revisar solicitações de alteração de perfil de terapeutas. Ação: Resolver

**Onde alterar:** `src/app/review/page.tsx`

**Efeito de alterações:** Alterar os critérios de aprovação modifica o processo de verificação. Adicionar uma nova aba exige criação de nova API e tabela no banco.

**Notas técnicas:** Usa `startTransition` do React para atualizações otimistas na UI.

---

### `/subscriptions` — Assinaturas

**O que faz:** Lista e gerencia as assinaturas de terapeutas na plataforma.

**Funcionalidades:**
- Listagem de assinaturas com status
- Ativar assinatura (`POST /api/subscriptions/[id]/activate`)
- Cancelar assinatura (`POST /api/subscriptions/[id]/cancel`)

**Onde alterar:** `src/app/subscriptions/page.tsx`

**Efeito de alterações:** Alterar as ações de assinatura impacta diretamente o acesso dos terapeutas às funcionalidades pagas da plataforma.

---

### `/billing` — Faturamento

**O que faz:** Painel financeiro com 3 abas de visualização de receita e pagamentos.

**Abas:**
1. **Visão Geral** — Receita total, número de pagamentos, gráficos
2. **Assinaturas** — Lista de assinaturas com datas e valores
3. **Pagamentos** — Histórico de transações (valor, status, data)

**Onde alterar:** `src/app/billing/page.tsx`

**Efeito de alterações:** Alterar o cálculo de receita impacta os totais exibidos. Adicionar colunas modifica o relatório financeiro.

---

### `/content` — Conteúdo / Aplicações

**O que faz:** Lista as aplicações/submissões de candidatos à plataforma.

**Informações exibidas:**
- Nome e email do candidato
- Status: `Approved`, `Rejected`, `Pending`
- Data de submissão e revisão
- Notas do revisor

**Onde alterar:** `src/app/content/page.tsx`

**Efeito de alterações:** Alterar os status exibidos modifica o fluxo de aplicação.

---

### `/seo` — Ferramentas de SEO

**O que faz:** Editor de SEO para configurar metadados e sitemap da plataforma principal.

**Funcionalidades:**
- Editor de título, descrição e palavras-chave
- Geração e preview do sitemap XML
- Preview de JSON-LD (dados estruturados)
- Geração automática de URLs de terapeutas

**Onde alterar:** `src/app/seo/page.tsx` e `src/app/api/sitemap/route.ts`

**Efeito de alterações:** Alterar metadados impacta o SEO da plataforma MasseurMatch. Alterar o sitemap modifica as URLs indexadas pelos buscadores.

---

### `/logs` — Logs do Sistema

**O que faz:** Exibe um histórico de transações de pagamento como entradas de log para auditoria.

**Informações exibidas:**
- ID da transação
- Usuário relacionado
- Valor e status do pagamento
- Data/hora

**Onde alterar:** `src/app/logs/page.tsx`

**Efeito de alterações:** Adicionar novos tipos de log exige criar ações adicionais em `src/lib/supabase/crud.ts` (função `logAdminAction`).

---

### `/legal` — Aceites Legais

**O que faz:** Rastreia quais usuários aceitaram os termos legais da plataforma.

**Informações exibidas:**
- ID do usuário
- Versão do documento aceita
- Data/hora do aceite
- Endereço IP do aceite

**Onde alterar:** `src/app/legal/page.tsx`

**Efeito de alterações:** Útil para conformidade legal (LGPD/GDPR). Alterar a query pode filtrar por versão específica de termos.

---

### `/support` — Suporte

**O que faz:** Exibe solicitações de edição de perfil submetidas por usuários para revisão do admin.

**Funcionalidades:**
- Lista de solicitações pendentes
- Preview das mudanças solicitadas (em JSON)
- Rastreamento de status (resolvido / pendente)

**Onde alterar:** `src/app/support/page.tsx`

**Efeito de alterações:** Alterar a exibição das mudanças modifica como o admin visualiza as solicitações. Adicionar ações de aprovação exige nova API.

---

### `/settings` — Configurações

**O que faz:** Painel de configurações globais do sistema com 3 abas.

**Abas:**
1. **Chaves de API** — Cadastrar chaves de APIs externas (Stripe, etc.)
2. **Notificações** — Preferências de notificação do admin
3. **Permissões** — Configurações de acesso por papel (role)

**Onde alterar:** `src/app/settings/page.tsx` e `src/app/api/settings/route.ts`

**Efeito de alterações:** Alterar chaves de API afeta integrações com serviços externos. Alterar permissões restringe ou amplia o acesso de managers e viewers.

---

### `/login`, `/forgot-password`, `/reset-password` — Autenticação

**O que fazem:** Páginas públicas de autenticação do admin.

**Fluxo:**
1. Admin acessa `/login` → insere email e senha
2. Credenciais são validadas via `POST /api/auth/login`
3. Sessão é armazenada em cookies HttpOnly (`sb-access-token`, `sb-refresh-token`)
4. Middleware verifica a sessão em cada rota protegida

**Onde alterar:**
- Login: `src/app/login/page.tsx`
- API: `src/app/api/auth/login/route.ts`
- Middleware: `src/middleware.ts`

**Efeito de alterações:** Alterar o middleware pode bloquear ou liberar acesso a rotas. Alterar a validação de roles pode conceder acesso indevido.

---

## 🔌 Rotas de API

### Autenticação (`/api/auth/`)

| Método | Rota | Função |
|---|---|---|
| POST | `/api/auth/login` | Login com email e senha |
| POST | `/api/auth/logout` | Limpa cookies de sessão |
| GET | `/api/auth/callback` | Callback OAuth (Google, Apple, etc.) |
| POST | `/api/auth/refresh` | Renovar token de sessão |
| POST | `/api/auth/oauth` | Iniciar fluxo OAuth |
| POST | `/api/auth/forgot-password` | Solicitar redefinição de senha |
| POST | `/api/auth/reset-password` | Confirmar nova senha |

### Usuários (`/api/users/`)

| Método | Rota | Função |
|---|---|---|
| GET | `/api/users` | Listar usuários (paginado) |
| POST | `/api/users` | Criar usuário |
| GET | `/api/users/[id]` | Detalhes de um usuário |
| PUT | `/api/users/[id]` | Atualizar usuário |
| DELETE | `/api/users/[id]` | Deletar usuário |

### Terapeutas (`/api/therapists/`)

| Método | Rota | Função |
|---|---|---|
| GET | `/api/therapists` | Listar terapeutas (paginado) |
| POST | `/api/therapists` | Criar terapeuta |
| GET | `/api/therapists/[id]` | Detalhes de um terapeuta |
| PUT | `/api/therapists/[id]` | Atualizar terapeuta |
| DELETE | `/api/therapists/[id]` | Deletar terapeuta |
| POST | `/api/therapists/[id]/approve` | Aprovar terapeuta |
| POST | `/api/therapists/[id]/reject` | Rejeitar terapeuta |
| POST | `/api/therapists/[id]/review` | Marcar para revisão |

### Assinaturas, Verificação e Outros

| Método | Rota | Função |
|---|---|---|
| POST | `/api/subscriptions/[id]/activate` | Ativar assinatura |
| POST | `/api/subscriptions/[id]/cancel` | Cancelar assinatura |
| POST | `/api/verification/[id]/approve` | Aprovar verificação de documento |
| POST | `/api/verification/[id]/reject` | Rejeitar verificação de documento |
| POST | `/api/profile-edits/[id]/resolve` | Resolver edição de perfil |
| POST | `/api/therapist-edits/[id]/resolve` | Resolver edição de terapeuta |
| POST | `/api/payments/sync` | Sincronizar pagamentos com Stripe |
| GET | `/api/settings` | Buscar configurações |
| POST | `/api/settings` | Salvar configurações |
| POST | `/api/api-keys` | Salvar chaves de API |
| GET | `/api/sitemap` | Gerar sitemap XML |

---

## 🗃️ Banco de Dados (Supabase)

O schema completo está em `docs/COMPLETE_DATABASE_SCHEMA.sql`.

### Tabelas Principais

#### `auth.users` (Supabase nativo)
Gerencia autenticação. Email/senha, OAuth (Google, Apple, Facebook, OTP).

#### `profiles`
Extensão do usuário autenticado.
```sql
id          uuid  (FK → auth.users)
display_name text
bio         text
avatar_url  text
metadata    jsonb
created_at, updated_at
```

#### `admins` (Controle de Acesso)
Define quem é admin e qual seu papel.
```sql
id          uuid
user_id     uuid  (FK → auth.users, UNIQUE)
role        text  ('superadmin' | 'manager' | 'viewer')
permissions jsonb (permissões granulares opcionais)
created_at
created_by  uuid  (FK → admins)
```

#### `therapists`
Perfil completo de cada terapeuta.
```sql
id                  uuid
user_id             uuid  (FK → auth.users)
full_name, email, phone
status              text  ('Pending' | 'Active' | 'Rejected' | 'Suspended')
plan, plan_name, subscription_status
slug                text  (UNIQUE, URL amigável)
reviewed_at, reviewed_by, rejection_reason
document_url, card_url, selfie_url, signed_term_url
created_at, updated_at
```

#### `verification_data`
Submissões de documentos de verificação.
```sql
id              uuid
therapist_id    uuid  (FK → therapists)
status          text  ('Pending' | 'Approved' | 'Rejected')
document_url, card_url, selfie_url, signed_term_url
submitted_at, reviewed_at, reviewed_by
notes           text
```

#### `profile_edits` e `therapist_edits`
Solicitações de alteração pendentes de aprovação admin.
```sql
id          uuid
user_id / therapist_id  uuid
changes     jsonb  (campos alterados e novos valores)
status      text   ('pending' | 'resolved')
created_at, resolved_at
```

#### `applications`
Candidaturas à plataforma.
```sql
id, user_id, full_name, email
status      text  ('Pending' | 'Approved' | 'Rejected')
submitted_at, reviewed_at, notes
```

#### `legal_acceptances`
Aceites de termos legais (LGPD/GDPR).
```sql
id, user_id, version, accepted_at, ip_address
```

#### `payments`
Transações financeiras.
```sql
id, user_id, amount, status, paid_at, invoice_id, created_at
```

#### `subscriptions`
Assinaturas de planos.
```sql
id, user_id, plan_id, status, start_date, end_date, canceled_at, created_at
```

#### `admin_logs`
Auditoria de ações administrativas.
```sql
id, admin_id, action_name, metadata jsonb, created_at
```

#### `settings`
Configurações do sistema em formato chave-valor JSONB.
```sql
id       text  (PK, ex: 'api-keys', 'admin-settings')
metadata jsonb
created_at
```

---

## 🔐 Autenticação e Permissões

### Sistema de Roles (RBAC)

```
superadmin (nível 3)  → Acesso total: gerenciar admins, alterar settings, deletar dados
     ↓
manager (nível 2)     → Aprovar/rejeitar terapeutas, editar usuários, moderar conteúdo
     ↓
viewer (nível 1)      → Apenas leitura (sem ações destrutivas)
```

### Middleware de Proteção (`src/middleware.ts`)

Rotas **públicas** (sem autenticação):
- `/login`, `/forgot-password`, `/reset-password`
- `/api/auth/*`
- `/favicon.ico`

**Todas as outras rotas** exigem:
1. Cookie de sessão válido (`sb-access-token` e/ou `sb-refresh-token`)
2. Registro na tabela `admins` com role válida

### Funções de Autenticação

**Server-side** (`src/lib/auth/server.ts`):

| Função | O que faz |
|---|---|
| `getAdminContext()` | Retorna o admin e usuário atual da sessão |
| `requireAdmin()` | Verifica e exige acesso de admin; lança erro se não autorizado |
| `getAdminByUserId(userId)` | Busca admin pelo user_id no banco |
| `validateAdmin(admin)` | Valida se o registro de admin é válido e ativo |
| `hasPermission(admin, action)` | Verifica se admin tem permissão para uma ação específica |
| `hasRequiredRole(admin, role)` | Verifica hierarquia de roles |
| `getRoleName(role)` | Retorna nome legível do role (ex: "Superadmin") |
| `getRoleColor(role)` | Retorna cor para badge do role na UI |

**Client-side** (`src/lib/auth/client.ts`):

| Função | O que faz |
|---|---|
| `signInWithOAuth(provider)` | Login via OAuth (Google, Apple, Facebook) |
| `signInWithEmailOTP(email)` | Envia OTP por email |
| `signInWithPhoneOTP(phone)` | Envia OTP por SMS |
| `verifyOTP(token, type)` | Verifica o código OTP informado |
| `signInWithPassword(email, pw)` | Login com email e senha |
| `signOut()` | Faz logout e limpa sessão |
| `getSession()` | Retorna sessão atual |
| `refreshSession()` | Renova o token de acesso |

---

## ⚙️ Funções e Onde Alterar

### CRUD Genérico (`src/lib/supabase/crud.ts`)

A função `makeCrud(table)` cria automaticamente operações CRUD para qualquer tabela do Supabase:

```typescript
list(page, pageSize)   // Lista registros com paginação e contagem total
get(id)                // Busca um registro pelo ID
create(payload)        // Cria novo registro
update(id, payload)    // Atualiza registro existente
remove(id)             // Remove registro
```

**Instâncias exportadas:**

| Variável | Tabela | Operações disponíveis |
|---|---|---|
| `listUsers`, `getUser`, `createUser`, `updateUser`, `deleteUser` | `auth.users` | list, get, create, update, delete |
| `listTherapists`, `getTherapist`, `createTherapist`, `updateTherapist`, `deleteTherapist` | `therapists` | list, get, create, update, delete |
| `listSubscriptions`, `getSubscription`, `createSubscription`, `updateSubscription`, `deleteSubscription` | `subscriptions` | list, get, create, update, delete |
| `listPayments`, `getPayment`, `createPayment`, `updatePayment`, `deletePayment` | `payments` | list, get, create, update, delete |
| `listVerificationData`, `getVerificationEntry`, `createVerificationEntry`, `updateVerificationEntry`, `deleteVerificationEntry` | `verification_data` | list, get, create, update, delete |
| `listProfileEdits`, `getProfileEdit`, `createProfileEdit`, `updateProfileEdit`, `deleteProfileEdit` | `profile_edits` | list, get, create, update, delete |
| `listTherapistEdits`, `getTherapistEdit`, `createTherapistEdit`, `updateTherapistEdit`, `deleteTherapistEdit` | `therapist_edits` | list, get, create, update, delete |
| `listApplications` | `applications` | list |
| `listLegalAcceptances` | `legal_acceptances` | list |
| `logAdminAction` | `admin_logs` | create |

**Efeito de alterar `crud.ts`:** Mudanças aqui afetam **todas** as queries do sistema. Use com cuidado.

### Respostas HTTP (`src/lib/http/responses.ts`)

Funções para padronizar respostas das APIs:
- `ok(data)` → `200 OK`
- `created(data)` → `201 Created`
- `badRequest(message)` → `400 Bad Request`
- `unauthorized()` → `401 Unauthorized`
- `forbidden()` → `403 Forbidden`
- `notFound()` → `404 Not Found`
- `serverError(message)` → `500 Internal Server Error`

**Efeito de alterar:** Mudanças afetam o formato de todas as respostas da API.

### Tipos TypeScript (`src/lib/supabase/types.ts`)

Define as interfaces de todos os objetos do banco de dados. **Alterar tipos aqui** sem atualizar as tabelas do Supabase causará erros de tipagem em tempo de compilação.

---

## 🧩 Componentes de Interface

### Layout Principal (`src/components/layout/main-layout.tsx`)

Wrapper global com sidebar de navegação. **Alterar a sidebar aqui** adiciona/remove itens do menu lateral visível em todas as páginas.

**Links de navegação da sidebar:**
- Dashboard, Usuários, Terapeutas, Fila de Revisão, Assinaturas, Faturamento, Conteúdo, SEO, Logs, Legal, Suporte, Configurações

### Header (`src/components/layout/header.tsx`)

Barra superior com:
- Nome da página atual
- Menu do usuário logado (nome, email, logout)

**Alterar aqui** modifica o cabeçalho visível em todas as páginas.

### DataTable (`src/components/common/data-table.tsx`)

Componente genérico de tabela reutilizado em todas as listagens.

**Props:**
- `columns` — definição das colunas
- `data` — array de dados
- `pagination` — configuração de paginação

**Alterar aqui** modifica o comportamento de **todas** as tabelas do dashboard.

### Componentes UI (`src/components/ui/`)

30+ componentes do shadcn/ui prontos para uso:

| Categoria | Componentes |
|---|---|
| Layout | `Card`, `Separator`, `ScrollArea`, `Sidebar` |
| Formulários | `Input`, `Label`, `Button`, `Checkbox`, `Switch`, `Select`, `Form`, `Textarea`, `Slider` |
| Dados | `Table`, `Badge`, `Avatar`, `Progress`, `Skeleton`, `Alert` |
| Feedback | `Toast`, `Toaster`, `AlertDialog`, `Dialog` |
| Navegação | `Tabs`, `DropdownMenu`, `Accordion`, `Breadcrumb`, `Menubar` |
| Visualização | `Chart` (Recharts), `Carousel`, `Calendar`, `Popover`, `Tooltip` |

---

## 🪝 Hooks Customizados

### `useToast()` (`src/hooks/use-toast.ts`)

Sistema de notificações toast.

```typescript
const { toast, dismiss } = useToast()

// Criar notificação
toast({
  title: "Sucesso!",
  description: "Terapeuta aprovado.",
  variant: "default" // ou "destructive"
})

// Fechar notificação
dismiss(toastId)
```

**Limite configurável:** `TOAST_LIMIT = 1` (máximo de 1 toast visível por vez). Altere para mostrar mais.

**Duração padrão:** `TOAST_REMOVE_DELAY = 1000000ms` (~16 minutos). Esse valor alto é intencional — faz com que o toast permaneça visível até ser fechado manualmente. Para remover automaticamente após alguns segundos, altere para, por exemplo, `5000` (5 segundos).

### `useMobile()` (`src/hooks/use-mobile.tsx`)

Detecta se o usuário está em dispositivo móvel com base no breakpoint do Tailwind (`768px`).

```typescript
const isMobile = useMobile() // true | false
```

---

## 🚢 Deploy

### Render (`render.yaml`)

Configurado para deploy automático no [Render](https://render.com). Detecta pushes na branch principal e faz build e deploy automaticamente.

### Firebase App Hosting (`apphosting.yaml`)

Configurado para deploy no [Firebase App Hosting](https://firebase.google.com/docs/app-hosting).

### Variáveis necessárias em produção

Configure as mesmas variáveis do `.env.example` no painel do seu serviço de hospedagem:
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`
- `NEXT_PUBLIC_APP_URL` (URL pública de produção)

---

## 🔐 Sistema de Permissões

| Role | Acesso | Pode fazer |
|---|---|---|
| `superadmin` | Total | Tudo: gerenciar admins, alterar settings, deletar dados, aprovar/rejeitar |
| `manager` | Parcial | Aprovar/rejeitar terapeutas, editar usuários, resolver edições, moderar conteúdo |
| `viewer` | Leitura | Apenas visualizar listas e detalhes; sem ações destrutivas |

---

## 📄 Licença

Propriedade de MasseurMatch. Todos os direitos reservados.
