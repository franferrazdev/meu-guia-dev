# ✍️ Guia Prático de Commits Semânticos (Conventional Commits)

Este guia estabelece os padrões e convenções para a escrita de mensagens de commit baseadas na especificação global **Conventional Commits**. A padronização do histórico de mensagens é um pilar crítico para a automação de releases, clareza em auditorias de código e eficiência em fluxos de trabalho assíncronos.

---

## 🏗️ 1. Estrutura e Regras de Sintaxe

O padrão estrutural obrigatório para a composição de uma mensagem de commit semântico é:

```text
tipo(escopo-opcional): descrição curta em letras minúsculas
```

### 🚨 Regras Cruciais de Conformidade (Padrão Oficial)
*   **Caixa Baixa Estrita:** O tipo e o escopo devem ser escritos obrigatoriamente em letras minúsculas. 
    *   ❌ `FIX(Cart): ...` ou `Fix(cart): ...`
    *   ✅ `fix(cart): ...`
*   **Verbos e Descrição:** A comunidade recomenda iniciar a descrição com letra minúscula e utilizar o verbo no tempo presente ou imperativo (Ex: `adicionar` ou `add` em vez de *adicionado/added*).

---

## 🏷️ 2. Dicionário de Tipos de Commits

Mapeamento dos prefixos semânticos de acordo com a natureza da alteração realizada na base de código:

| Tipo | Finalidade Técnica | Exemplo Prático |
| :--- | :--- | :--- |
| **`feat`** | Adiciona uma nova funcionalidade ou recurso ao sistema. | `feat(auth): adicionar suporte para login com Google` |
| **`fix`** | Corrige um bug, falha ou comportamento inesperado em produção. | `fix(api): corrigir erro de timeout no carrinho` |
| **`docs`** | Alterações exclusivas em documentações (Ex: arquivos `.md`). | `docs(readme): atualizar instrucoes de instalacao` |
| **`style`**| Ajustes estéticos ou de formatação que não alteram a lógica (Lint/Espaços). | `style: formatar codigo de acordo com regras do ESLint` |
| **`refactor`**| Melhorias estruturais na lógica que não alteram o comportamento externo. | `refactor(users): simplificar validacao de e-mail` |
| **`test`** | Inserção, correção ou modificação de testes automatizados. | `test(payment): criar testes unitarios para juros` |
| **`perf`** | Modificações focadas especificamente em ganho de performance. | `perf(images): otimizar carregamento de fotos na home` |
| **`build`**| Mudanças que afetam scripts de compilação ou dependências externas. | `build: atualizar biblioteca axios para versao recente` |
| **`chore`**| Tarefas de manutenção rotineiras que não afetam código ou testes. | `chore: remover logs de depuracao antigos` |
| **`ci`** | Ajustes em arquivos e scripts de Integração Contínua (CI/CD). | `ci: configurar GitHub Actions para rodar testes` |

---

## 💥 3. Quebra de Compatibilidade (*Breaking Change*)

Quando uma alteração introduz uma modificação estrutural que quebra o funcionamento anterior do sistema ou da API, deve-se sinalizar o impacto adicionando um ponto de exclamação (`!`) imediatamente após o tipo da mensagem.

```bash
# Indica uma alteração crítica na rota que exigirá refatoração por parte dos clientes da API
git commit -m "feat(api)!: alterar os parametros obrigatorios da rota de cadastro"
```

---

## 📚 Referências Oficiais

*   [Especificação Completa do Conventional Commits](https://conventionalcommits.org)
*   [Padrões de Escrita Semântica - GitHub Guides](https://github.com)
