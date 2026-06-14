# 🐙 Git & GitHub: Guia Rápido de Sobrevivência e Versionamento

Este guia reúne os conceitos fundamentais, as configurações iniciais do ambiente e os comandos essenciais do **Git** (controle de versão local) e do **GitHub** (plataforma colaborativa em nuvem) para o gerenciamento eficiente e seguro de fluxos de trabalho.

---

## 🧠 1. Conceitos Fundamentais e Arquitetura

*   **Repositório (Repository):** O diretório centralizado onde o Git rastreia de forma incremental todo o histórico de alterações do projeto.
*   **Commit:** Um registro fotográfico (*snapshot*) do estado do código em um determinado momento, indexado por um hash único e acompanhado por uma mensagem descritiva.
*   **Branch (Ramificação):** Uma linha de desenvolvimento independente. Permite construir novas funcionalidades ou correções isoladas sem impactar a versão principal de produção.
*   **Merge:** O processo de fusão e integração do histórico de alterações de uma branch secundária para a branch receptora.

### 🔄 Os 3 Estados de um Arquivo no Git

A engine do Git gerencia os arquivos do seu projeto através de três zonas lógicas de ciclo de vida:

```text
[ Working Directory ] ------( git add )------> [ Staging Area ] ------( git commit )------> [ .git Directory ]
    (Modified)                                     (Staged)                                    (Committed)
```

| Estado | Significado Prático | Localização Lógica |
| :--- | :--- | :--- |
| **Modified** | Arquivo alterado localmente, mas ainda sem rastro oficial no próximo pacote. | Diretório de Trabalho (*Working Directory*) |
| **Staged** | Arquivo selecionado e preparado para ser empacotado no próximo commit. | Área de Preparação (*Staging Area / Index*) |
| **Committed**| Arquivo salvo de forma permanente, segura e imutável no banco de dados do Git. | Repositório Local (*.git folder*) |

---

## ⚙️ 2. Configurações Iniciais do Ambiente

Antes de iniciar qualquer commit, parametrize sua identidade global no terminal para assinar as suas contribuições corretamente:

```bash
# Define o nome que será exibido no histórico de commits
git config --global user.name "Seu Nome"

# Define o e-mail atrelado à sua conta do GitHub
git config --global user.email "seu-email@provedor.com"
```

---

## 🚀 3. Fluxo de Trabalho Diário (Local)

Comandos essenciais utilizados na sua máquina para inicializar e registrar a evolução do código.

```bash
# Inicializa um repositório Git em um diretório existente ou vazio
git init

# Exibe o estado atual das três zonas (arquivos modificados, staged ou não rastreados)
git status

# Adiciona todas as modificações atuais do diretório de trabalho para a Staging Area
git add .

# Grava o pacote de alterações da Staging Area permanentemente no histórico local
git commit -m "Sua mensagem descritiva"

# Exibe o histórico cronológico de todos os commits realizados no repositório
git log
```

---

## ☁️ 4. Sincronização com Repositórios Remotos (GitHub)

Comandos para realizar o tráfego e backup de dados entre a sua máquina local e a nuvem.

```bash
# Faz o download completo de um repositório remoto para o seu ambiente local
git clone <url_do_repositorio>

# Vincula o repositório local a uma URL remota hospedada no GitHub (geralmente apelidada de 'origin')
git remote add origin <url_do_repositorio>

# Envia os commits da sua branch local para o repositório correspondente no GitHub
git push origin <nome-da-branch>

# Busca, baixa e mescla as atualizações do GitHub diretamente na sua branch local ativa
git pull origin <nome-da-branch>
```

---

## 🌿 5. Gerenciamento de Branches e Fusões

Controle de ramificações para desenvolvimento seguro de novas funcionalidades.

```bash
# Lista todas as branches locais existentes e destaca a branch ativa no momento
git branch

# Cria uma nova branch e alterna imediatamente para ela
git checkout -b <nome-da-branch>

# Alterna o ambiente de trabalho para uma branch existente
git checkout <nome-da-branch>

# Mescla o histórico da branch informada com a branch em que você está posicionado atualmente
git merge <nome-da-branch>
```

---

## 📚 Links Úteis e Colas de Consulta Rápida

*   [Guia Interativo Cheat Sheet do Git](https://github.com) *(Insira links de repositórios ou gists específicos que você use)*
*   [Comandos Essenciais do Git - Gist Leo Comelli](https://github.com)
