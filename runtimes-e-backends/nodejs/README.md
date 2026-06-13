# 🟢 Node.js & NPM: Guia Rápido de Sobrevivência

Este guia reúne os comandos essenciais para o gerenciamento de pacotes e execução de ambientes com **Node.js** e **NPM (Node Package Manager)**. Ele serve como uma referência rápida para o desenvolvimento diário e automação de fluxos de trabalho.

---

## 🔍 1. Verificação de Ambiente

Antes de iniciar qualquer projeto, certifique-se de que o ambiente está instalado corretamente checando as versões:

```bash
# Verifica a versão do Node.js instalada
node -v

# Verifica a versão do NPM instalada
npm -v
```

---

## 📦 2. Gerenciamento de Projetos e Dependências

Comando base para inicializar aplicações e gerenciar bibliotecas externas.

### Inicialização
```bash
# Inicia um novo projeto criando o arquivo package.json (passo a passo)
npm init

# Inicia um projeto rapidamente com as configurações padrões (pula perguntas)
npm init -y
```

### Instalação e Remoção
```bash
# Instala todas as dependências listadas no package.json
npm i

# Instala um pacote específico como dependência de produção
npm i <pacote>

# Instala um pacote globalmente no sistema
npm i -g <pacote>

# Instala um pacote como dependência de desenvolvimento (Ex: Typescript, Linters)
npm i -D <pacote>

# Remove um pacote do projeto
npm uninstall <pacote>
```

💡 *Dica de legibilidade: No ambiente profissional, costuma-se usar os atalhos abaixo para economizar tempo:*

| Comando Longo | Atalho Equivalente | Objetivo |
| :--- | :--- | :--- |
| `npm install` | `npm i` | Instalar tudo ou um pacote |
| `---save-dev` | `-D` | Dependência de desenvolvimento |

---

## 🚀 3. Execução de Arquivos e Scripts

Formas de rodar o seu código JavaScript e gerenciar os ciclos de vida da aplicação através do `package.json`.

```bash
# Executa um arquivo JavaScript diretamente no terminal
node <arquivo.js>

# Executa o script principal de inicialização (start)
npm start

# Executa scripts customizados (Ex: npm run dev, npm run build)
npm run <nome-do-script>
```

---

## 🛠️ 4. Manutenção, Produtividade e CI/CD

Comandos para manter o projeto saudável, atualizado e performático em ambientes de deploy automatizado.

```bash
# Executa um pacote sem precisar instalá-lo globalmente (Ex: npx shadcn@latest init)
npx <pacote>

# Verifica quais pacotes do projeto estão desatualizados
npm outdated

# Atualiza os pacotes para as versões compatíveis mais recentes
npm update

# Instala dependências de forma limpa e idêntica ao lock (Ideal para CI/CD)
npm ci
```

---

## 📚 Links Úteis e Documentação

*   **Ajuda integrada:** Para ver os detalhes de qualquer comando direto no terminal, utilize:
    ```bash
    npm help <comando>
    ```
*   [Apostila Completa de Node.js - Reativa Tecnologia](https://reativatecnologia.com.br) *(Substitua pelo link correto se houver)*
*   [Guia Prático de Node.js - DevMedia](https://devmedia.com.br)
