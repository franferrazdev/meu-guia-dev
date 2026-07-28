# ⚡ Supabase: Comandos e Conceitos Essenciais

Guia rápido de referência para o ecossistema do Supabase. Os comandos essenciais dividem-se entre o gerenciamento do projeto via terminal (CLI) e o uso prático do cliente no código da sua aplicação.

⚠️ **Regra de ouro:** Dominar a inicialização local, o controle de migrações e o gerenciamento de permissões (RLS) são as chaves para um desenvolvimento rápido e seguro.

---

## 💻 1. Supabase CLI (Terminal)

Essenciais para gerenciar seu ambiente local, rodar migrações e testar sua API antes de subir para a nuvem através do Docker:

* **`supabase init`**: Inicializa a estrutura de configuração do Supabase na sua pasta de desenvolvimento.
* **`supabase start`**: Inicia todos os serviços (PostgreSQL, Kong, Studio, GoTrue) localmente em contêineres Docker.
* **`supabase stop`**: Interrompe os serviços locais sem apagar os dados do seu banco.
* **`supabase status`**: Exibe URLs locais, portas, chaves de API e o link de acesso ao painel do Supabase Studio local.
* **`supabase link`**: Vincula o seu repositório local a um projeto existente na nuvem do Supabase.
* **`supabase db pull`**: Baixa o esquema atual do banco em nuvem e gera os arquivos de migração (`.sql`) locais.
* **`supabase db push`**: Aplica as alterações e migrações criadas localmente direto no banco de dados em nuvem.

---

## 🔐 2. Autenticação e Usuários (Frontend/Client)

Métodos padrão mais utilizados no lado do cliente para controle de sessões e acessos:

* **Cadastro:**
  ```javascript
  await supabase.auth.signUp({ email, password });
  ```
* **Login tradicional:**
  ```javascript
  await supabase.auth.signInWithPassword({ email, password });
  ```
* **Logout:**
  ```javascript
  await supabase.auth.signOut();
  ```
* **Recuperação de senha:**
  ```javascript
  await supabase.auth.resetPasswordForEmail(email);
  ```
* **Obter sessão atual:**
  ```javascript
  await supabase.auth.getSession();
  ```

---

## 📊 3. Consultas e Manipulação de Dados (CRUD)

O Supabase fornece um *Query Builder* fluido que traduz JavaScript/TypeScript diretamente em comandos PostgreSQL:

* **Buscar Dados (Select):**
  ```javascript
  await supabase.from('nome_da_tabela').select('*');
  ```
* **Filtrar Dados (Where):**
  * `.eq('coluna', 'valor')` *(Igualdade)*
  * `.ilike('coluna', '%termo%')` *(Busca textual insensível a maiúsculas)*
* **Inserir Dados (Insert):**
  ```javascript
  await supabase.from('nome_da_tabela').insert({ coluna: 'valor' });
  ```
* **Atualizar Dados (Update):**
  ```javascript
  await supabase.from('nome_da_tabela').update({ coluna: 'novo_valor' }).eq('id', 1);
  ```
* **Deletar Dados (Delete):**
  ```javascript
  await supabase.from('nome_da_tabela').delete().eq('id', 1);
  ```

---

## 🚨 4. Cuidados Importantes (Erros a evitar)

* **Segurança Baseada em Linhas (RLS):** **Sempre** habilite o *Row Level Security* (RLS) nas tabelas pelo Dashboard. Sem isso, qualquer usuário com a sua chave pública (anon key) poderá ler, alterar ou deletar dados do seu banco.
* **Filtros no Servidor:** Sempre use os filtros do Query Builder do Supabase (como `.eq()`, `.limit()`). Nunca puxe milhares de linhas para filtrar usando funções JavaScript no cliente, pois isso consome tráfego de rede desnecessário e gera gargalos na aplicação.
* **Vazamento da `service_role`:** Nunca exponha ou use a chave `service_role` no Frontend. Ela ignora todas as regras de RLS e possui privilégios totais de administrador de banco de dados. Use-a apenas em ambientes isolados de backend protegidos.
