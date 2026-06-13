# 🌐 Next.js: Guia Rápido de Sobrevivência (App Router)

Este guia reúne os comandos de terminal fundamentais, a convenção de arquivos especiais do ecossistema **App Router** e as diretivas de otimização essenciais para o desenvolvimento de aplicações modernas com renderização híbrida.

---

## 💻 1. Comandos CLI (Terminal)

Comandos mapeados no `package.json` para gerenciar todo o ciclo de vida, testes de padronização e compilação do projeto.

```bash
# Inicializa um novo projeto Next.js com as configurações mais recentes (Configura TS, Tailwind, ESLint)
npx create-next-app@latest

# Inicializa o servidor de desenvolvimento local com Hot Reloading
npm run dev

# Compila a aplicação para produção (Gera arquivos estáticos otimizados e analisa rotas SSR/SSG)
npm run build

# Inicializa o servidor Node.js em modo de produção (Executar obrigatoriamente após o build)
npm run start

# Executa a checagem do ESLint para garantir a qualidade e conformidade dos padrões do código
npm run lint
```

---

## 📂 2. Estrutura de Arquivos Especiais (`app/` Directory)

O Next.js utiliza o sistema de roteamento baseado em arquivos. Dentro do diretório `app/`, determinados arquivos possuem funções e comportamentos automáticos controlados por convenção:

| Arquivo Especial | Função Principal | Comportamento Relevante |
| :--- | :--- | :--- |
| `page.tsx` | **Interface Única** | Define a interface visual e o conteúdo acessível da rota. |
| `layout.tsx` | **Layout Compartilhado** | Preserva o estado e engloba sub-rotas (O layout raiz é obrigatório). |
| `loading.tsx` | **Estado de Carregamento** | Renderiza automaticamente interfaces de *Skeleton* via React Suspense. |
| `error.tsx` | **Tratamento de Falhas** | Captura e isola erros em runtime utilizando *React Error Boundaries*. |
| `not-found.tsx` | **Erro 404** | Exibe uma interface personalizada quando a rota ou recurso não existe. |
| `template.tsx` | **Layout Dinâmico** | Similar ao layout, porém cria uma nova instância e zera estados na navegação. |

---

## 🧩 3. Componentes e Recursos Nativos

Funcionalidades integradas para otimização de performance, SEO e manipulação assíncrona de dados.

### Diretiva de Escopo (`'use client'`)
Por padrão, todos os componentes no App Router são **Server Components** (executados no servidor). Para habilitar interatividade, Hooks (`useState`, `useEffect`) ou APIs do navegador, declare explicitamente no topo do arquivo:
```tsx
'use client';

import { useState } from 'react';
// ... rest do componente
```

### Otimizações de Interface
```tsx
import Link from 'next/link';
import Image from 'next/image';

// Navegação otimizada com pre-fetching automático em background
<Link href="/dashboard">Ir para Painel</Link>

// Otimização de imagens: impede CLS, aplica lazy loading nativo e redimensionamento responsivo
<Image src="/foto.jpg" width={500} height={300} alt="Descricao" />
```

### Navegação Programática e Ações de Servidor
*   **`useRouter`**: Hook importado de `next/navigation` utilizado para disparar redirecionamentos ou manipulações de histórico de forma imperativa.
*   **Server Actions**: Funções assíncronas identificadas pela diretiva `'use server'`. Executam lógicas seguras diretamente no backend, ideais para submissão de formulários e mutação de dados sem a necessidade de criar rotas de API dedicadas.

---

## 🚀 4. Estratégias de Renderização e Otimização

*   **Geração Estática (SSG):** Comportamento padrão do Next.js para páginas sem dados dinâmicos por requisição. Oferece a melhor performance de carregamento e otimização para motores de busca (SEO).
*   **Renderização no Servidor (SSR):** Processamento de páginas em tempo real no servidor Node.js a cada requisição. Ativado automaticamente ao consumir dados dinâmicos não cacheados.

---

## 📚 Links Úteis e Referências

*   [Documentação Oficial do Next.js (App Router)](https://nextjs.org)
*   [Aprenda Next.js: Curso Oficial Interativo](https://nextjs.org)
*   [Melhores Práticas de Performance no Next.js - freeCodeCamp](https://freecodecamp.org)
