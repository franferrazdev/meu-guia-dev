# 🚀 Guia de Boas Práticas e Engenharia de Pull Requests (PR)

Este manual centraliza as diretrizes essenciais, critérios de validação e padrões de comunicação para a abertura e revisão de **Pull Requests (PR)**. No ambiente de trabalho remoto e assíncrono, o PR é o principal ponto de convergência de uma equipe: ele deve ser direto, seguro, autoexplicativo e fácil de revisar para evitar gargalos na esteira de entrega.

---

## 🏗️ 1. Elementos Fundamentais de um PR de Sucesso

Para garantir que a sua alteração seja revisada com agilidade e segurança, atente-se aos três pilares de entrega:

### A. Roteiro e Contexto (Comunicação)
*   **Título Semântico:** Utilize o prefixo padrão dos Commits Semânticos (Ex: `feat:`, `fix:`, `docs:`) seguido de um título curto.
*   **Contexto e Motivo:** Explique claramente o problema original que está sendo resolvido e a motivação por trás da abordagem adotada.
*   **Rastreabilidade:** Vincule sempre o Pull Request à tarefa correspondente, utilizando links ou referências diretas (Ex: `Fixes #123` ou links do Jira/Trello).
*   **Evidências Visuais:** Sempre que houver alteração na interface do usuário (UI), inclua capturas de tela, GIFs ou vídeos curtos demonstrando o antes e o depois.

### B. Validação e Qualidade do Código (Técnico)
*   **Ausência de Conflitos:** Certifique-se de que a sua branch de origem foi atualizada com a branch de destino (geralmente a `main` ou `develop`) antes de abrir o PR.
*   **Histórico de Commits Limpo:** Evite commits fragmentados (Ex: *'correção de digitação'*, *'ajuste de bug 2'*). Faça um *squash* dos seus commits se necessário para manter a linha do tempo limpa.
*   **Esteira de CI/CD Ativa:** O PR só deve ser submetido para avaliação humana se todas as automações, testes unitários, linters e builds estiverem com os "checks verdes" aprovados.
*   **Escopo Restrito (Small PRs):** Pull Requests gigantescos dificultam a revisão e aumentam o risco de bugs em produção. Divida grandes funcionalidades em tarefas menores e entregas incrementais.

---

## 🛒 2. Modelo de Exemplo (Preenchido em Produção)

Abaixo está uma demonstração prática de como um Pull Request deve ser apresentado à equipe de engenharia para revisão:

```markdown
# 🚀 feat: Adiciona contador de itens no carrinho

## 📝 Descrição
Este PR adiciona um badge numérico sobre o ícone do carrinho de compras. O objetivo é atualizar a quantidade em tempo real sempre que o usuário adicionar ou remover um produto, melhorando a experiência de compra.

Fixes #124

## 🛠️ O que foi feito
- Criado o componente visual `CartBadge`.
- Integrado o componente ao estado global do carrinho (`useCart`).
- Adicionados testes unitários para validar a contagem com 0, 1 e múltiplos itens.

## 📸 Evidências (Antes / Depois)

| Antes | Depois |
| --- | --- |
| ![Sem badge](https://placehold.co) | ![Com badge](https://placehold.co) |

## 🧪 Como testar?
1. Faça o checkout desta branch: `git checkout feat/cart-badge`.
2. Instale as novas dependências: `npm install`.
3. Execute o projeto: `npm run dev`.
4. Acesse a página de produtos e clique em "Adicionar ao carrinho".
5. Verifique se o número no ícone atualiza corretamente.

## 🏁 Checklist
- [x] O código segue os padrões de estilo do projeto.
- [x] Testes unitários foram criados e estão passando.
- [x] Nenhum novo alerta de linter ou build foi gerado.
```

---

## 📋 3. Template Limpo (Copiar e Preencher)

Utilize a estrutura abaixo como padrão base para as entregas do seu projeto:

```markdown
# 🚀 Tipo: Título curto da alteração

## 📝 Descrição
<!-- Escreva um breve resumo do que este PR faz e por que ele é necessário -->

Fixes # <!-- Adicione o número da issue vinculada, ex: #123 -->

## 🛠️ O que foi feito
- <!-- Item 1 -->
- <!-- Item 2 -->

## 📸 Evidências
<!-- Cole capturas de tela, GIFs ou vídeos das alterações (obrigatório para UI) -->

## 🧪 Como testar?
1. <!-- Passo 1 -->
2. <!-- Passo 2 -->

## 🏁 Checklist
- [ ] O código segue os padrões de estilo do projeto.
- [ ] Testes unitários foram criados e estão passando.
- [ ] Nenhum novo alerta de linter ou build foi gerado.
```
