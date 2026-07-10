# 🚀 TanStack Query: Comandos e Conceitos Essenciais

O **TanStack Query** (antigo React Query) é uma biblioteca poderosa voltada para o gerenciamento de estado assíncrono e do lado do servidor (*server-state*). Em vez de gerenciar estados manuais de carregamento e erro, ele introduz reatividade, sincronização automática e políticas inteligentes de cache.

---

## 📌 Conteúdos que não devo esquecer

### 1. Conceitos Fundamentais
* **Query (Consulta):** Usado para operações de leitura (`GET`). Associa uma função assíncrona (como `fetch` ou `axios`) a uma chave única. O TanStack Query gerencia o ciclo de vida completo: busca, pausa, carregamento e cache.
* **Query Key (Chave da Consulta):** Um array único usado para identificar cada consulta. Sempre que a chave muda, a query busca novos dados automaticamente. Inclua no array todas as dependências que alteram o resultado.
  ```javascript
  ['usuario', userId, filtros]
  ```
* **Mutation (Mutação):** Usado exclusivamente para operações que alteram dados no servidor (`POST`, `PUT`, `DELETE`).
* **Caching (Cache) e Stale Time (Tempo Obsoleto):** Os dados são guardados em cache para evitar requisições redundantes. O `staleTime` define por quanto tempo o dado em cache é considerado "fresco". Após esse tempo, a busca é refeita em segundo plano (*background refetch*).
* **Query Invalidation (Invalidação):** Sinaliza que os dados no servidor mudaram e o cache atual está obsoleto. Isso força uma atualização automática e garante a sincronização da interface.
* **Optimistic Updates (Atualizações Otimistas):** Atualiza a interface localmente antes mesmo do servidor responder, gerando uma experiência instantânea. Caso a operação falhe, o estado anterior é revertido automaticamente.

### 2. Comandos e Hooks Essenciais

#### `useQuery`
O gancho central para buscar dados. Retorna estados úteis diretamente para o componente.
```javascript
const { data, isLoading, isError } = useQuery({
  queryKey: ['usuarios'],
  queryFn: buscarUsuarios,
});
```

#### `useMutation`
Gancho para realizar alterações no servidor, disparado de forma manual através da função `mutate`.
```javascript
const { mutate } = useMutation({
  mutationFn: criarUsuario,
  onSuccess: () => {
    // Invalida o cache e força a atualização da listagem
    queryClient.invalidateQueries({ queryKey: ['usuarios'] });
  },
});
```

#### `queryClient.invalidateQueries`
Método imperativo para invalidar chaves de consulta específicas. É a prática padrão para atualizar telas após mutações bem-sucedidas.

#### `useInfiniteQuery`
Hook especializado essencial para criar estruturas de paginação infinita ou sistemas com botões de "Carregar Mais".

### 3. Comportamentos Padrão (Defaults) do Ciclo de Vida
* **Window Focus Refetching:** Quando você muda de aba no navegador e volta para o app, o TanStack Query refaz a busca dos dados ativos automaticamente em segundo plano.
* **Retentativas Automáticas (Retry):** Se uma requisição falhar por problemas de rede ou erro no servidor, a biblioteca tenta refazer a chamada mais **3 vezes**, de forma progressiva e automática.
* **Garbage Collection (Coleta de Lixo):** Dados em cache que ficam inativos (sem nenhum componente ativo na tela os utilizando) são limpos da memória após **5 minutos**.
