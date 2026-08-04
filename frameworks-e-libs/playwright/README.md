# 🎭 Playwright: Guia de Estudos e Referência Rápida

Este guia centraliza os conceitos cruciais, comandos de terminal indispensáveis, configurações ideais e exemplos práticos para dominar a automação de testes End-to-End (E2E) com o **Playwright**.

---

## 💡 Conceitos Cruciais

O Playwright baseia-se em conceitos modernos de automação para garantir velocidade, isolamento e resiliência.

*   **Browser vs. BrowserContext vs. Page**: Uma única instância do `Browser` pode abrir múltiplos `BrowserContext`. Eles funcionam como janelas anônimas isoladas que compartilham zero cookies ou cache. Cada contexto pode gerenciar várias `Pages` (abas).
*   **Auto-waiting**: O Playwright aguarda automaticamente que os elementos estejam visíveis, acionáveis e estáveis antes de interagir (como clicar ou digitar). Você quase nunca precisará usar esperas fixas (`sleep`).
*   **Locators (User-Facing)**: A recomendação oficial é sempre buscar elementos da forma como o usuário final os enxerga (por texto, rótulo ou papel acessível), evitando IDs técnicos ou classes CSS instáveis.
*   **Fixtures**: Argumentos pré-configurados (como `page`, `browser`, `context`) passados diretamente para a função de teste, gerenciando o ciclo de vida do ambiente de forma limpa.

---

## 💻 Linha de Comando (CLI) Essencial

Execute estes comandos na raiz do projeto (considerando um ambiente Node.js/TypeScript):

```bash
# Iniciar um novo projeto com a estrutura padrão
npm init playwright @latest

# Executar todos os testes
npx playwright test

# Executar um arquivo de teste específico
npx playwright test meu-teste.spec.ts

# Executar testes exibindo a interface do navegador (Headed)
npx playwright test --headed

# Abrir a interface interativa (UI Mode - excelente para debug)
npx playwright test --ui

# Abrir o Codegen (Gerador de código automático gravando sua navegação)
npx playwright codegen https://exemplo.com

# Visualizar o último relatório HTML gerado
npx playwright show-report
```

---

## 🎯 Localizadores Técnicos (Locators)

Priorize sempre os seletores acessíveis da API:

| Método | Descrição / Caso de Uso |
| :--- | :--- |
| `page.getByRole('button', { name: 'Enviar' })` | Busca pelo papel semântico do elemento. |
| `page.getByText('Login com sucesso')` | Busca pelo texto visível. |
| `page.getByLabel('Usuário')` | Busca o input associado à tag `<label>`. |
| `page.getByPlaceholder('Digite sua senha...')`| Busca pelo texto temporário do input. |
| `page.getByTestId('botao-salvar')` | Busca pelo atributo `data-testid` (ótima prática para quando os textos mudam muito). |
| `page.locator('#id-do-elemento')` | Fallback para seletores CSS tradicionais. |

---

## ⚡ Ações Comuns em Elementos

*Lembre-se de sempre utilizar o `await`, pois as interações retornam Promises.*

```typescript
// Navegar para uma URL
await page.goto('https://exemplo.com');

// Clicar em um elemento
await page.getByRole('button').click();

// Preencher campos de texto (limpa o campo e digita)
await page.getByLabel('E-mail').fill('user@email.com');

// Marcar e desmarcar Checkboxes ou Radios
await page.getByLabel('Aceito os termos').check();
await page.getByLabel('Aceito os termos').uncheck();

// Selecionar opção em um elemento <select> (Dropdown)
await page.getByLabel('País').selectOption('Brasil');

// Pressionar teclas específicas do teclado
await page.press('Enter');
```

---

## 🔍 Asserções (Assertions / Validações)

As asserções do Playwright têm **web-first retry**, ou seja, elas tentam validar a condição repetidamente por alguns segundos antes de falhar o teste.

```typescript
// Validar se a URL está correta
await expect(page).toHaveURL('https://exemplo.com');

// Validar se o elemento contém o texto esperado
await expect(page.getByRole('heading')).toHaveText('Bem-vindo');

// Validar visibilidade e estado
await expect(page.getByRole('button')).toBeVisible();
await expect(page.getByRole('button')).toBeDisabled();

// Validar se um checkbox está marcado
await expect(page.getByLabel('Termos')).toBeChecked();

// Asserção oposta (Negativa)
await expect(page.getByText('Erro')).not.toBeVisible();
```

---

## 🛠️ Configurações Estruturais do Projeto

Para começar um projeto com o Playwright da forma correta, você precisa configurar principalmente dois arquivos estruturais: o `playwright.config.ts` (ou `.js`) e o `package.json` (se estiver usando Node.js). Abaixo estão as configurações essenciais e o que você deve modificar nelas.

### 1. `playwright.config.ts` (O Coração do Projeto)
```typescript
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  //  Diretório onde estão os arquivos de teste
  testDir: './tests',

  //  Timeout máximo para CADA teste individual (ex: 30 segundos)
  timeout: 30 * 1000,

  //  Timeout máximo para asserções do expect (ex: 5 segundos)
  expect: {
    timeout: 5000,
  },

  //  Executar testes em paralelo (true ajuda a economizar muito tempo)
  fullyParallel: true,

  //  Impedir o uso acidental de test.only em ambiente de CI (Pipeline)
  forbidOnly: !!process.env.CI,

  //  Número de tentativas em caso de falha (Flaky tests)
  retries: process.env.CI ? 2 : 0,

  //  Quantos workers rodam em paralelo (ex: metade da CPU no CI)
  workers: process.env.CI ? 1 : undefined,

  //  Tipo de relatório gerado
  reporter: [['html', { open: 'never' }]],

  //  Configurações globais para todos os navegadores
  use: {
    // URL base para não precisar digitar o link completo no page.goto()
    baseURL: 'http://localhost:3000',

    // Capturar o que aconteceu no teste (útil para debugar)
    trace: 'on-first-retry', // Opções: 'on', 'off', 'retain-on-failure'
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    
    // Ignorar erros de certificado HTTPS (comum em ambientes de homologação)
    ignoreHTTPSErrors: true,
  },

  //  Ambientes e Navegadores onde os testes vão rodar
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
  ],

  //  Opcional: Iniciar o seu servidor local automaticamente antes dos testes
  // webServer: {
  //   command: 'npm run start',
  //   url: 'http://localhost:3000',
  //   reuseExistingServer: !process.env.CI,
  // },
});
```

### 2. `package.json` (Atalhos de Scripts)
```json
{
  "scripts": {
    "test": "npx playwright test",
    "test:headed": "npx playwright test --headed",
    "test:ui": "npx playwright test --ui",
    "test:debug": "npx playwright test --debug",
    "test:report": "npx playwright show-report"
  }
}
```
*Para rodar agora, basta usar: `npm run test:ui`.*

### 3. `.gitignore` (O que NÃO enviar para o Git)
```text
# Relatórios de testes e traces
/playwright-report/
/blob-report/
/playwright/.cache/

# Evidências de testes que falharam (fotos/vídeos)
/test-results/

# Variáveis de ambiente com senhas e tokens
.env
```

### 4. `.env` (Gerenciamento de Credenciais)
*Nunca coloque senhas, logins ou tokens de API direto no arquivo de configuração ou nos testes.* Crie um arquivo `.env` na raiz: 
```env
BASE_URL=https://meusite.com
USER_EMAIL=teste@provedor.com
USER_PASSWORD=SenhaSegura123
```
*E no seu `playwright.config.ts`, você pode ler essas variáveis usando `process.env.USER_EMAIL`.*

---

## 🧪 Exemplos de Testes

Exemplos práticos de testes com Playwright cobrindo os cenários mais comuns do dia a dia. Eles utilizam as melhores práticas de Locators estruturais e asserções web-first. Tudo foi planejado considerando que os arquivos serão salvos dentro do diretório `tests/` (ex: `tests/login.spec.ts`).

### 1. Teste de Fluxo de Login (Sucesso e Erro)
*Este exemplo demonstra preenchimento de formulário, cliques em botões e validação de mensagens de erro ou sucesso.* 

```typescript
import { test, expect } from '@playwright/test';

test.describe('Fluxo de Autenticação', () => {
  
  // Executa antes de cada teste deste bloco
  test.beforeEach(async ({ page }) => {
    await page.goto('/login'); // Usa a baseURL do arquivo de config
  });

  test('Deve realizar login com sucesso', async ({ page }) => {
    // Localiza os campos pelas labels visíveis e preenche
    await page.getByLabel('E-mail').fill('usuario@exemplo.com');
    await page.getByLabel('Senha').fill('SenhaValida123');
    
    // Clica no botão com o texto 'Entrar'
    await page.getByRole('button', { name: 'Entrar' }).click();

    // Valida se a URL mudou para a Dashboard
    await expect(page).toHaveURL('/dashboard');
    
    // Valida se um elemento de boas-vindas ficou visível
    await expect(page.getByRole('heading', { name: 'Bem-vindo' })).toBeVisible();
  });

  test('Deve exibir mensagem de erro com credenciais inválidas', async ({ page }) => {
    await page.getByLabel('E-mail').fill('errado@exemplo.com');
    await page.getByLabel('Senha').fill('SenhaIncorreta');
    await page.getByRole('button', { name: 'Entrar' }).click();

    // Valida o texto da mensagem de alerta flutuante ou erro
    const mensagemErro = page.getByText('Usuário ou senha inválidos');
    await expect(mensagemErro).toBeVisible();
    await expect(mensagemErro).toHaveClass(/alert-danger/); // Opcional: valida classe CSS
  });
});
```

### 2. Teste de E-commerce (Lista e Interação com Elementos)
*Este exemplo mostra como lidar com múltiplos elementos semelhantes na tela, como uma lista de produtos.*

```typescript
import { test, expect } from '@playwright/test';

test('Deve adicionar um produto ao carrinho', async ({ page }) => {
  await page.goto('/produtos');

  // Garante que a página de produtos carregou
  await expect(page).toHaveURL(/.*produtos/);

  // Seleciona um card de produto específico pelo texto dele
  const produtoCard = page.locator('.produto-item').filter({ hasText: 'Fone de Ouvido Bluetooth' });
  
  // Clica no botão 'Adicionar' apenas dentro daquele card específico
  await produtoCard.getByRole('button', { name: 'Adicionar ao carrinho' }).click();

  // Valida se o contador do carrinho no topo mudou para '1'
  const contadorCarrinho = page.getByTestId('cart-badge'); // Boa prática: usar data-testid
  await expect(contadorCarrinho).toHaveText('1');
});
```

### 3. Teste de Elementos Dinâmicos (Modais e Aguardar Elemento)
*O Playwright espera o elemento aparecer automaticamente, mas este exemplo mostra como interagir com Modais (Pop-ups) que demoram um pouco para surgir na tela.*

```typescript
import { test, expect } from '@playwright/test';

test('Deve interagir com janela modal dinâmica', async ({ page }) => {
  await page.goto('/configuracoes');

  // Clica no botão que dispara a abertura do Modal
  await page.getByRole('button', { name: 'Excluir Conta' }).click();

  // O Playwright vai aguardar o modal aparecer devido ao auto-waiting interno da asserção
  const modal = page.locator('#modal-confirmacao');
  await expect(modal).toBeVisible();

  // Valida o conteúdo dentro do modal antes de interagir
  await expect(modal).toContainText('Esta ação não pode ser desfeita');

  // Confirma a ação clicando no botão de confirmação dentro do modal
  await modal.getByRole('button', { name: 'Confirmar' }).click();

  // Garante que o modal sumiu da tela
  await expect(modal).not.toBeVisible();
});
```

### 4. Mockando uma API Rest (Intercepção de Rede)
*No Playwright você pode simular respostas do backend sem precisar que o banco de dados de verdade funcione.*

```typescript
import { test, expect } from '@playwright/test';

test('Deve exibir lista vazia quando a API falhar ou retornar vazia', async ({ page }) => {
  // Intercepta a rota da API que busca os dados e devolve um JSON mockado
  await page.route('**/api/v1/usuarios', async route => {
    const jsonFake = []; // Simulando nenhum usuário cadastrado
    await route.fulfill({ 
      status: 200,
      contentType: 'application/json',
      body: JSON.stringify(jsonFake) 
    });
  });

  // Navega para a página que consome essa API
  await page.goto('/usuarios');

  // Valida se a interface se comportou corretamente exibindo a mensagem amigável de lista vazia
  await expect(page.getByText('Nenhum usuário encontrado.')).toBeVisible();
});
```
