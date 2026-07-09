# 🐻 Zustand: Comandos e Conceitos Essenciais

O **Zustand** é um gerenciador de estado global para React que se destaca por sua simplicidade e alta performance. Ele elimina por completo a necessidade de *Providers* globais e *boilerplate* excessivo, permitindo que os componentes acessem diretamente a *store* de forma otimizada.

---

## 📌 Conteúdos que não devo esquecer

### 1. Criação da Store
A *store* é a base central do Zustand. Utilizamos a função `create` para definir tanto o estado inicial quanto as funções (ações) que vão alterá-lo.

```javascript
import { create } from 'zustand';

const useStore = create((set) => ({
  contador: 0,
  incrementar: () => set((state) => ({ contador: state.contador + 1 })),
  decrementar: () => set((state) => ({ contador: state.contador - 1 })),
  resetar: () => set({ contador: 0 }),
}));

export default useStore;
```

### 2. Conceito de Actions e o comando `set`
O método `set` é o responsável por atualizar o estado da store. Ele realiza uma fusão (*merge*) superficial com o estado atual, ou seja, você só precisa passar a propriedade que deseja modificar.

* **Modo Simples:** Passar o novo objeto diretamente.
  ```javascript
  set({ contador: 10 })
  ```
* **Modo Funcional:** Utilizar o estado anterior para calcular o novo valor.
  ```javascript
  set((state) => ({ contador: state.contador + 1 }))
  ```

### 3. Seletores e Performance
Para evitar renderizações desnecessárias e manter a aplicação rápida, busque apenas as "fatias" (*slices*) do estado que o componente realmente precisa.

```javascript
// ✅ RECOMENDADO: O componente só renderiza se "contador" mudar
const contador = useStore((state) => state.contador);

// ⚠️ ATENÇÃO: Evite extrair funções diretamente se não for necessário,
// para prevenir re-renders indesejados no componente.
const incrementar = useStore((state) => state.incrementar); 
```

### 4. Acessando múltiplos valores de forma otimizada
Se você precisar extrair mais de um dado ao mesmo tempo sem perder performance, use o `shallow` (comparação rasa) do `zustand/shallow` ou faça a seleção individual de cada propriedade. 

A melhor prática para buscar dados otimizados é usar seletores corretamente. Para entender melhor os padrões de estrutura e arquitetura, vale a pena consultar as discussões sobre boas práticas na comunidade do Reddit.

### 5. O Comando `get`
Utilizado quando você precisa **ler** o estado atual dentro de uma ação assíncrona ou de uma lógica complexa, sem a necessidade imediata de disparar uma alteração visual.

```javascript
const useStore = create((set, get) => ({
  contador: 0,
  verificarEIncrementar: () => {
    const atual = get().contador; // Acessa o valor atual do estado
    if (atual < 10) {
      set({ contador: atual + 1 });
    }
  }
}));
```

### 6. Middlewares Essenciais
O Zustand facilita a extensão de comportamento da sua *store* envolvendo a função de criação com middlewares nativos:

* **`persist`:** Salva e sincroniza os dados no `localStorage` ou `sessionStorage` de forma totalmente automática.
* **`devtools`:** Permite inspecionar as mudanças de estado diretamente através da extensão *Redux DevTools* no navegador.

---

## 🔗 Links Úteis e Referências
* Para entender a fundo como simplificar a gestão de estado e estruturar sua lógica, acesse o guia de **Gerenciamento de Estado em React da Alura**.
* Para exemplos práticos do dia a dia, consulte o **Tutorial Completo de Zustand do Programador Viking**.
