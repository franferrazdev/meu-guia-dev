# 🔐 NextAuth.js / Auth.js: Guia Rápido de Autenticação

Este guia reúne as funções essenciais da biblioteca **NextAuth.js (Auth.js)** para controle de sessão em camadas de cliente (Client-side) e proteção de rotas/componentes em camadas de servidor (Server-side).

---

## 🖥️ 1. Funções de Cliente (Client-Side)

Utilizadas dentro de componentes dinâmicos que possuem a diretiva `'use client'` para gerenciar fluxos visuais e estados locais de login.

```tsx
'use client';

import { signIn, signOut, useSession } from "next-auth/react";

// Dispara o fluxo de login redirecionando para o provedor (Ex: "google", "github")
signIn('provedor');

// Encerra a sessão ativa do usuário e limpa cookies/tokens de autenticação
signOut();

// Hook para ler o estado atual da sessão
const { data: session, status } = useSession();
// status possíveis: "loading" | "authenticated" | "unauthenticated"
```

---

## ⚙️ 2. Funções de Servidor (Server-Side)

Abordagens assíncronas para validação e proteção de dados em **Server Components**, **Route Handlers** (APIs) e **Server Actions**. Garante que informações sensíveis não vazem para o cliente.

### NextAuth v4 (Padrão de Mercado)
```ts
import { getServerSession } from "next-auth/next";
import { authOptions } from "@/app/api/auth/[...nextauth]/route";

// Busca a sessão de forma eficiente diretamente no servidor
const session = await getServerSession(authOptions);
```

### Auth.js / NextAuth v5 (Versão Recente)
Nas arquiteturas mais novas, a função global `auth()` simplifica o acesso:
```ts
import { auth } from "@/auth";

// Substitui o getServerSession unificando a chamada no servidor
const session = await auth();
```

---

## 🔄 3. Ciclo de Vida e Callbacks Essenciais

Os callbacks interceptam o fluxo de autenticação e permitem customizar o payload de dados que trafega entre o banco de dados e o frontend. Configurados no arquivo `auth.ts` ou `[...nextauth].ts`:

```ts
const authOptions = {
  callbacks: {
    // 1. Executado na criação ou atualização do token JWT
    async jwt({ token, user }) {
      if (user) {
        token.id = user.id; // Injeta dados do banco (Ex: ID, Role) no token criptografado
      }
      return token;
    },
    
    // 2. Executado sempre que a sessão é consultada pelo cliente/front-end
    async session({ session, token }) {
      if (session.user) {
        session.user.id = token.id; // Expõe as propriedades do token para o useSession()
      }
      return session;
    }
  }
};
```

⚠️ *Atenção Técnica: Os dados passam primeiro pelo `jwt()` e só depois chegam ao `session()`. Se você esquecer de injetar um dado no token, ele não ficará disponível na sessão do cliente.*

---

## 📚 Documentação e Referências

*   [Documentação Oficial do Auth.js / NextAuth](https://authjs.dev)
*   [Guia de Calbacks e Segurança - NextAuth.js](https://js.org)
