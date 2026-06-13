# ⚛️ React: Guia de Sobrevivência, Sintaxe e Hooks Essenciais

Este guia centraliza os comandos de ambiente, padrões de sintaxe JSX, conceitos fundamentais de componentização, gerenciamento de estado com Hooks e manipulação de listas sob as diretrizes oficiais e melhores práticas do ecossistema.

---

## 💻 1. Comandos de Terminal (Ambiente e Projeto)

Fluxo recomendado utilizando o **Vite**, que oferece maior performance e agilidade em comparação ao ecossistema legado do *create-react-app*.

```bash
# Cria um novo projeto utilizando o template do React (Vite)
npm create vite@latest meu-app -- --template react

# Instala as dependências listadas no package.json
npm install

# Inicializa o servidor de desenvolvimento local
npm run dev

# Gera o build otimizado para o ambiente de produção
npm run build
```

---

## 📝 2. Sintaxe JSX (JavaScript XML)

Regras fundamentais para estruturação de interfaces em arquivos React:

*   **Imports:** `import React from 'react';` (Opcional a partir das versões mais recentes do React 17+).
*   **Fragmentos:** Utilize `<> ... </>` ou `<React.Fragment> ... </React.Fragment>` para agrupar múltiplos elementos sem adicionar nós extras à árvore do DOM.
*   **Interpolação:** Insira expressões ou lógicas JavaScript diretamente na interface utilizando chaves: `{minhaVariavel}`.
*   **Atributos:** Devem seguir rigorosamente o padrão `camelCase` (Ex: `className` em vez de *class*, `onClick` em vez de *onclick*).
*   **Tags Auto-fechadas:** Elementos sem filhos precisam obrigatoriamente terminar com uma barra (Ex: `<img />`, `<br />`, `<Componente />`).

---

## 🧩 3. Componentes e Props

Estrutura básica de um componente funcional utilizando desestruturação para receber parâmetros externos (*props*):

```jsx
// Definição do Componente Funcional
function MeuComponente({ prop1, prop2 }) {
  return (
    <div>
      <p>Valor recebido por prop: {prop1}</p>
    </div>
  );
}

// Exportação Padrão
export default MeuComponente;
```

Para utilizá-lo em outro local do projeto:
```jsx
import MeuComponente from './MeuComponente';
```

---

## ⚓ 4. Hooks Principais (Estado e Ciclo de Vida)

Os Hooks controlam as engrenagens de renderização da interface e persistência de dados.

### `useState` (Gerenciamento de Estado)
```jsx
const [state, setState] = useState(initialValue);
```

### `useEffect` (Efeitos Colaterais e Ciclo de Vida)
```jsx
useEffect(() => {
  // Código do efeito (Ex: chamadas de API, inscrições de eventos)

  return () => { 
    // Função de limpeza (cleanup) - Executada antes de desmontar o componente
  };
}, [dependencia]); // Array de dependências: monitora variáveis para reexecutar o efeito
```

### Outros Hooks Fundamentais
*   `useContext`: Consome dados globais providos por um Contexto sem a necessidade de passar *props* manualmente entre múltiplos níveis.
*   `useRef`: Cria uma referência mutável que persiste durante todo o ciclo de vida, muito utilizado para acessar elementos do DOM diretamente de forma imperativa.

---

## 🔄 5. Manipulação de Listas e Renderização Condicional

Abordagens eficientes para exibição dinâmica de dados baseadas em lógica e imutabilidade de arrays.

### Mapeamento de Listas
Sempre atribua uma propriedade `key` única ao elemento raiz retornado para otimizar o algoritmo de reconciliação do React:
```jsx
{lista.map(item => (
  <li key={item.id}>{item.nome}</li>
))}
```

### Condicionais na Interface
```jsx
// Operador Ternário (Se verdadeiro renderiza X, se falso renderiza Y)
{condicao ? <ComponenteVerdadeiro /> : <ComponenteFalso />}

// Curto-circuito lógico com operador AND (Renderiza apenas se a condição for verdadeira)
{condicao && <ComponenteOpcional />}
```

---

## 🧠 6. JavaScript (ES6+) Essencial para React

Para dominar o ecossistema React, é mandatório dominar estes conceitos fundamentais do JavaScript moderno:

*   **Desestruturação de Objetos/Arrays:** `const { nome } = props;`
*   **Arrow Functions:** Sintaxe concisa para declaração de funções: `const minhaFuncao = () => { ... }`
*   **Métodos HOF (High-Order Functions):** Uso intensivo de `.map()`, `.filter()` e `.reduce()` para manipulação previsível de coleções de dados dentro do estado.
*   **Módulos do ES:** Organização e componentização através de sintaxes explícitas de `import` e `export`.

---

## 🚀 7. Melhores Práticas de Desenvolvimento

*   **Imutabilidade Obligatória:** Nunca altere o estado do React de forma direta (Ex: *state = novoValor*). Utilize sempre a função modificadora (`setState`) para garantir o disparo correto do ciclo de re-renderização.
*   **Modularização:** Separe interfaces complexas em pequenos componentes reutilizáveis e focados em uma única responsabilidade.
*   **Validação (Prototipagem):** Em projetos sem TypeScript, utilize a biblioteca `prop-types` para mapear e documentar os tipos esperados das propriedades, minimizando falhas estruturais em runtime.

---

## 📚 Documentação e Links Úteis

*   [Documentação Oficial do React (pt-br)](https://react.dev)
*   [Guia Prático do React - freeCodeCamp](https://www.freecodecamp.org/portuguese/news/react-para-principiantes-um-guia-do-react-js-para-programadores-de-front-end/)
*   [Aprenda React: Primeiros Passos](https://react.dev/learn)
