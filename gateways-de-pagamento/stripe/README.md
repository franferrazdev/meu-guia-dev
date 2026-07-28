# 💳 Stripe: Comandos e Conceitos Essenciais

Guia rápido de referência com os comandos, conceitos e boas práticas que você não deve esquecer para dominar o ecossistema da Stripe no desenvolvimento backend.

⚠️ **Regra de ouro:** Os dados sensíveis de cartões de crédito **nunca** devem tocar o seu servidor.

---

## 📌 Conceitos Essenciais

| Conceito | Prefijo | Ambiente | Descrição |
| :--- | :--- | :--- | :--- |
| **Publishable Key** | `pk_` | Frontend | Chave pública usada no navegador. Segura para expor no código do cliente. |
| **Secret Key** | `sk_` | Backend | Chave secreta do servidor. Nunca deve ir para o controle de versão. |
| **Webhook Secret** | `whsec_` | Backend | Chave que valida se o evento recebido veio realmente da Stripe. |
| **PaymentIntent** | — | Backend | Objeto central que gerencia todo o ciclo de vida de um pagamento. |

---

## 💻 Comandos do Terminal (Stripe CLI)

Use estas ferramentas para testes locais e simulação de eventos em ambiente de desenvolvimento:

* **Autenticação:**
  ```bash
  stripe login
  ```
  *Autentica a CLI com a sua conta da Stripe através do navegador.*

* **Redirecionamento de Webhooks:**
  ```bash
  stripe listen --forward-to localhost:3333/webhook
  ```
  *Ouve eventos locais e redireciona diretamente para o seu servidor de desenvolvimento.*

* **Simulação de Eventos:**
  ```bash
  stripe trigger payment_intent.succeeded
  ```
  *Dispara um evento falso (fake) para testar se o seu webhook está funcionando corretamente.*

* **Logs em Tempo Real:**
  ```bash
  stripe logs tail
  ```
  *Visualiza as requisições e respostas da API em tempo real diretamente no terminal.*

---

## 📂 Arquivos Importantes

* **`.env`**: Armazena as variáveis de ambiente sensíveis que não devem ser expostas (`STRIPE_SECRET_KEY`, etc.).
* **`config.toml`**: Arquivo de configuração global da Stripe CLI (geralmente localizado em `~/.config/stripe/config.toml`).
* **`.gitignore`**: **Obrigatório.** Garante que o arquivo `.env` com suas chaves privadas nunca seja enviado ao GitHub.

---

## 🛠️ Linhas de Código (Node.js / Express)

### 1. Inicialização
```javascript
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);
```

### 2. Criar PaymentIntent (Backend)
```javascript
const intent = await stripe.paymentIntents.create({
  amount: 2000, // Valor em centavos (ex: 2000 equivale a R\$ 20,00)
  currency: 'brl',
});
```

### 3. Validar Webhook
⚠️ **Atenção:** Para o método `stripe.webhooks.constructEvent` funcionar, o Express precisa receber o corpo da requisição em formato **Raw (Buffer)**, e não como um JSON formatado tradicional.

```javascript
// O endpoint do webhook deve usar obrigatoriamente express.raw
app.post('/webhook', express.raw({ type: 'application/json' }), (req, res) => {
  const sig = req.headers['stripe-signature'];
  let event;

  try {
    event = stripe.webhooks.constructEvent(
      req.body, // Precisa ser o Buffer puro da requisição
      sig, 
      process.env.STRIPE_WEBHOOK_SECRET
    );
  } catch (err) {
    return res.status(400).send(`Erro no Webhook: ${err.message}`);
  }

  // Processar o evento após validação bem-sucedida (ex: payment_intent.succeeded)
  res.json({ received: true });
});
```

---

## 🔑 Gerenciamento de Chaves (`.env`)

Durante a fase de desenvolvimento, utilize sempre o **Test Mode** (Modo de Teste). Altere para o **Live Mode** (Modo de Produção) apenas quando o sistema for publicado oficialmente.

### Modelo de arquivo `.env`
```env
# Chave pública (usada no Frontend)
STRIPE_PUBLISHABLE_KEY=pk_test_...

# Chave secreta (usada no Backend)
STRIPE_SECRET_KEY=sk_test_...

# Segredo do Webhook (gerado pelo comando 'stripe listen' ou no Dashboard da Stripe)
STRIPE_WEBHOOK_SECRET=whsec_...
```

### Como identificar o tipo de chave:
* **Modo Teste:** Começa estritamente com `pk_test_` ou `sk_test_`.
* **Modo Produção:** Começa estritamente com `pk_live_` ou `sk_live_`.
