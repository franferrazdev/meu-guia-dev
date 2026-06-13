# 🔐 BcryptJS: Guia Rápido de Criptografia de Senhas

Este guia reúne os comandos fundamentais da biblioteca **BcryptJS** para criptografia segura de senhas (*hashing*) e validação de credenciais em fluxos de autenticação no ecossistema Node.js.

---

## 📦 1. Importação e Inicialização

O BcryptJS é uma implementação pura em JavaScript, o que elimina a necessidade de compilação nativa no ambiente.

```javascript
// Importação utilizando o padrão CommonJS
const bcrypt = require('bcryptjs');
```

---

## 🛠️ 2. Gerar Hash (Criptografia com Salt)

O método `hash` transforma a senha em uma string segura e irreversível. O parâmetro `saltRounds` (geralmente definido como `10`) determina o custo computacional do processamento para mitigar ataques de força bruta.

### Abordagem Padrão (Async via Callbacks)
```javascript
const password = 'sua_senha_secreta';
const saltRounds = 10;

bcrypt.hash(password, saltRounds, (err, hash) => {
  if (err) {
    // Tratar erro de criptografia
    return;
  }
  // Salve a string 'hash' gerada de forma segura no banco de dados
  console.log(hash);
});
```

---

## 🔍 3. Comparação de Senhas (Fluxo de Login)

Como o processo de hash é criptograficamente irreversível, a validação de login é feita comparando o texto puro digitado pelo usuário com a string criptografada recuperada do banco de dados.

### Abordagem Padrão (Async via Callbacks)
```javascript
const passwordTyped = 'senha_digitada_pelo_usuario';
const hashFromDatabase = 'hash_recuperado_do_banco';

bcrypt.compare(passwordTyped, hashFromDatabase, (err, isMatch) => {
  if (err) {
    // Tratar erro interno de processamento
    return;
  }

  if (isMatch) {
    // Senha correta! Prossiga com a geração de token/sessão
  } else {
    // Senha incorreta! Retorne erro de credenciais inválidas
  }
});
```

---

## ⚡ 4. Padrão Moderno (Recomendado: Async / Await)

Para evitar o aninhamento de código (*Callback Hell*) no trabalho assíncrono e manter a arquitetura limpa, dê preferência ao uso de **Promises**:

```javascript
// Exemplo prático dentro de um fluxo de Controller / API
async function gerenciarAutenticacao(passwordTyped, hashFromDatabase) {
  try {
    // Criptografando de forma limpa
    const novoHash = await bcrypt.hash('nova_senha', 10);
    
    // Comparando de forma assíncrona limpa
    const isMatch = await bcrypt.compare(passwordTyped, hashFromDatabase);
    
    return isMatch;
  } catch (error) {
    console.error('Falha na operação de segurança:', error);
  }
}
```

---

## 📚 Documentação e Referências

*   [Repositório Oficial e Documentação do BcryptJS](https://npmjs.com)
*   [Guias de Segurança OWASP - Armazenamento de Senhas](https://owasp.org)
