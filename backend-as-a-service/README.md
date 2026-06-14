# ⚡ Supabase: Guia Rápido de Sobrevivência, CLI e Integração Client-Side

Este guia reúne os comandos essenciais da ferramenta de linha de comando (**Supabase CLI**) para gerenciamento de ambientes locais e migrações, além dos métodos fundamentais de Autenticação e manipulação de dados (CRUD) através do *Query Builder*.

---

## 💻 1. Supabase CLI (Gerenciamento e Ambiente Local)

A CLI permite rodar uma infraestrutura completa do Supabase localmente utilizando contêineres Docker, facilitando testes e fluxos de integração antes do deploy em produção.

```bash
# Inicializa a estrutura de configuração do Supabase na pasta do projeto
supabase init

# Inicia todos os serviços locais (PostgreSQL, Studio local, GoTrue, etc.) via Docker
supabase start

# Interrompe e remove os contêineres locais sem apagar os dados armazenados
supabase stop

# Exibe as credenciais locais, chaves de API anônimas e URLs do painel Studio local
supabase status

# Vincula o seu diretório de desenvolvimento local a um projeto existente na nuvem do Supabase
supabase link

# Sincroniza o ambiente local trazendo a estrutura atual do banco de dados na nuvem para arquivos .sql
supabase db pull

# Aplica todas as migrações locais pendentes diretamente no banco de dados em produção
supabase db push
```

---

## 🔐 2. Autenticação e Gerenciamento de Usuários

Métodos integrados do serviço de autenticação (`supabase.auth`) para consumo direto na camada de cliente do ecossistema Frontend.

```typescript
// Cadastro de novas credenciais
const { data, error } = await supabase.auth.signUp({ 
  email: 'usuario@email.com', 
  password: 'senha_segura_123' 
});

// Autenticação / Login tradicional por e-mail e senha
const { data, error } = await supabase.auth.signInWithPassword({ 
  email: 'usuario@email.com', 
  password: 'senha_segura_123' 
});

// Encerramento de sessão (Logout) e limpeza automática de tokens locais
await supabase.auth.signOut();

// Dispara e-mail de recuperação e redefinição de credenciais
await supabase.auth.resetPasswordForEmail('usuario@email.com');

// Recupera os dados e metadados da sessão ativa atual do usuário no cliente
const { data: { session } } = await supabase.auth.getSession();
```

---

## 🗃️ 3. Consultas e Manipulação de Dados (CRUD)

O Supabase simplifica a comunicação com o PostgreSQL fornecendo um gerador de consultas (*Query Builder*) fluído, assíncrono e tipado.

```typescript
// 1. Buscar dados (Read) com filtros de correspondência estrita ou parcial
const { data, error } = await supabase
  .from('clientes')
  .select('*')
  .eq('uf', 'SP')
  .ilike('nome', '%termo%'); // Case-insensitive match

// 2. Inserir dados (Create)
const { error } = await supabase
  .from('clientes')
  .insert({ nome: 'Bruna Melo', idade: 27, uf: 'MG' });

// 3. Atualizar dados (Update) com cláusula condicional obrigatória
const { error } = await supabase
  .from('clientes')
  .update({ uf: 'RJ' })
  .eq('id', 1);

// 4. Deletar dados (Delete) com cláusula condicional obrigatória
const { error } = await supabase
  .from('clientes')
  .delete()
  .eq('id', 1);
```

---

## 🛑 4. Boas Práticas e Segurança Arquitetural

> ⚠️ **Segurança Estrita (RLS):** Nunca exponha tabelas sem ativar o **Row Level Security (RLS)** no painel de controle do Supabase. Sem políticas (*policies*) de segurança habilitadas, qualquer requisição feita pelo cliente pode injetar, alterar ou vazar dados confidenciais do banco de dados.
> 
> 📉 **Filtros e Performance:** Sempre execute ordenações, limites (`.limit()`) e filtros de busca diretamente na chamada do Supabase. Trazer coleções massivas de dados (Ex: +1.000 linhas) para tratá-las com métodos JavaScript na aplicação do cliente gera gargalos severos de tráfego, lentidão em conexões móveis e riscos desnecessários de exposição.
> 
> 🔑 **Segredos e Chaves Privadas:** Jamais disponibilize ou commite no código frontend a sua chave `service_role`. Ela possui privilégios de administrador de sistema (*bypass RLS*) e deve residir estritamente em variáveis de ambiente seguras no backend (Server Components ou Edge Functions).

---

## 📚 Documentação e Referências

*   [Documentação Oficial do Supabase CLI](https://supabase.com)
*   [Guia de Boas Práticas com Supabase via CLI](https://supabase.com)
*   [Referência Completa de Métodos da JavaScript Client Library](https://supabase.com)
