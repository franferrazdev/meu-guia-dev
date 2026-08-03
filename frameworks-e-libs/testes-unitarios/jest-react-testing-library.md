# 🧪 Testes Unitários: Jest & React Testing Library (RTL)

Guia prático e conceitual com os comandos, boas práticas e padrões de mercado essenciais para testes unitários no ecossistema React utilizando Jest.

---

## ⚡ Resumo Direto e Imediato
Para testes em React, lembre-se do tripé essencial: 
1. **`render`**: Exibe o componente no DOM virtual.
2. **`screen`**: Busca elementos (priorizando sempre `screen.getByRole`).
3. **`userEvent`**: Simula interações reais (cliques e digitação).

Use `expect(...).toBeInTheDocument()` para validar o resultado final e `jest.fn()` para criar funções espiãs (*mocks*).

---

## 🧠 Conceitos Essenciais

* **Testar o comportamento, não a implementação:** Foque no que o usuário final vê e faz na tela, e não no estado interno (`useState`) ou em funções privadas do componente.
* **Queries prioritárias:** Siga a ordem de acessibilidade do RTL: `getByRole` (primeira opção) ➡️ `getByLabelText` ➡️ `getByText` ➡️ `getByTestId` (último recurso).
* **Async/Await com `findBy`:** Sempre use `screen.findBy...` quando o elemento depender de uma ação assíncrona (como o retorno de uma API) para aparecer na tela.
* **Queries seguras com `queryBy`:** Use `screen.queryBy...` exclusivamente para verificar se um elemento **não** está presente na tela. Usar `getBy` para elementos ausentes quebra a execução do teste imediatamente.

---

## 💻 Comandos Práticos

### Renderização e Busca

| Comando | Descrição |
| :--- | :--- |
| `render(<Componente />)` | Monta o componente no DOM virtual de teste. |
| `screen.debug()` | Imprime o HTML atual do componente no terminal (ótimo para debugar). |
| `screen.getByRole('button', { name: /enviar/i })` | Busca um botão pelo papel acessível de forma insensível a maiúsculas. |

### Interações e Eventos
* `await userEvent.click(elemento)`: Simula um clique real do usuário (preferível ao `fireEvent` por disparar microeventos encadeados).
* `await userEvent.type(input, 'texto')`: Digita um texto dentro de um campo de formulário.
* `fireEvent.click(elemento)`: Dispara um evento de clique de forma síncrona (legado).

### Asserções Comuns (`expect`)
* `expect(elemento).toBeInTheDocument()`: Verifica se o elemento existe no documento.
* `expect(elemento).toHaveTextContent('Olá')`: Valida o texto contido no elemento.
* `expect(elemento).toBeDisabled()`: Verifica se o botão ou input está desativado.

### Mocks e Funções Falsas
* `const funcaoMock = jest.fn()`: Cria uma função falsa para monitorar execuções no Jest.
* `expect(funcaoMock).toHaveBeenCalledTimes(1)`: Confirma quantas vezes a função foi chamada.
* `jest.mock('axios')`: Substitui um módulo ou biblioteca inteira por uma versão simulada para evitar requisições reais.

---

## 🛠️ Exemplos Práticos por Cenário

### 1. Componente de Formulário (Interação do Usuário)
*Valida a digitação, o envio dos dados e o disparo da função de callback.*

```tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import LoginForm from './LoginForm';

test('deve permitir digitar e enviar o formulário', async () => {
  const handleSubmit = jest.fn();
  render(<LoginForm onSubmit={handleSubmit} />);

  // 1. Buscar os elementos de forma acessível
  const inputEmail = screen.getByRole('textbox', { name: /e-mail/i });
  const inputPassword = screen.getByLabelText(/senha/i);
  const botaoEnviar = screen.getByRole('button', { name: /entrar/i });

  // 2. Simular a ação do usuário
  await userEvent.type(inputEmail, 'user@teste.com');
  await userEvent.type(inputPassword, 'senha123');
  await userEvent.click(botaoEnviar);

  // 3. Validar se a função foi chamada com os dados certos
  expect(handleSubmit).toHaveBeenCalledTimes(1);
  expect(handleSubmit).toHaveBeenCalledWith({
    email: 'user@teste.com',
    password: 'senha123',
  });
});
```

### 2. Componente Assíncrono (Chamada de API)
*Simula uma requisição HTTP usando mocks do Jest e aguarda a renderização do resultado.*

```tsx
import { render, screen } from '@testing-library/react';
import UserProfile from './UserProfile';
import axios from 'axios';

// Mockando a biblioteca inteira com o Jest
jest.mock('axios');

test('deve buscar e exibir os dados do perfil', async () => {
  // Configura o retorno falso da API antes de renderizar
  (axios.get as jest.Mock).mockResolvedValueOnce({ data: { name: 'João Silva' } });

  render(<UserProfile userId="1" />);

  // Verifica o estado de carregamento inicial
  expect(screen.getByText(/carregando/i)).toBeInTheDocument();

  // Espera assíncrona até que o elemento apareça na tela
  const nomeUsuario = await screen.findByRole('heading', { name: 'João Silva' });
  expect(nomeUsuario).toBeInTheDocument();
  
  // Garante que o texto de carregamento sumiu
  expect(screen.queryByText(/carregando/i)).not.toBeInTheDocument();
});
```

### 3. Componente Condicional (Mudança de Estado)
*Valida se elementos aparecem ou desaparecem da tela com base em um clique de expansão (Toggle).*

```tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Accordion from './Accordion';

test('deve ocultar e exibir o conteúdo condicionalmente', async () => {
  render(<Accordion title="Termos de Uso" content="Texto secreto aqui." />);
  const botaoToggle = screen.getByRole('button', { name: /mostrar conteúdo/i });

  // Garante que o texto secreto NÃO está na tela inicialmente
  expect(screen.queryByText('Texto secreto aqui.')).not.toBeInTheDocument();

  // Abre o accordion
  await userEvent.click(botaoToggle);

  // Garante que o texto agora ESTÁ na tela
  expect(screen.getByText('Texto secreto aqui.')).toBeInTheDocument();
});
```
