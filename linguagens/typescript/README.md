# 📘 TypeScript: Guia Rápido de Sobrevivência e Configuração

Este guia reúne os comandos essenciais de CLI (Linha de Comando), diretrizes fundamentais para o arquivo de configuração `tsconfig.json` e boas práticas de tipagem para garantir a segurança e a consistência do código em tempo de compilação.

---

## 💻 1. Comandos CLI (Terminal)

Comandos fundamentais do compilador do TypeScript (`tsc`) para gerenciamento e automação do fluxo de desenvolvimento.

```bash
# Instala o compilador do TypeScript globalmente no sistema
npm install -g typescript

# Inicializa um novo projeto criando o arquivo base tsconfig.json
tsc --init

# Compila um arquivo específico convertendo-o para JavaScript (Ex: arquivo.js)
tsc arquivo.ts

# Modo de observação: acompanha mudanças e recompila automaticamente ao salvar
tsc --watch

# Executa apenas a checagem de tipos, sem gerar arquivos JavaScript de saída (Ideal para CI/CD)
tsc --noEmit
```

---

## ⚙️ 2. Flags Essenciais no `tsconfig.json`

O arquivo `tsconfig.json` é o núcleo de segurança do projeto. Abaixo estão as diretrizes recomendadas pela comunidade para habilitar o modo estrito e otimizar a compilação:

```json
{
  "compilerOptions": {
    /* Segurança e Rigor */
    "strict": true,                        // Habilita todas as checagens estritas (altamente recomendado)
    "noImplicitAny": true,                 // Impede o uso acidental ou implícito do tipo 'any'
    "strictNullChecks": true,              // Força o tratamento rigoroso de valores null e undefined
    
    /* Qualidade de Código */
    "noUnusedLocals": true,                // Emite erros para variáveis locais declaradas mas nunca utilizadas
    "noUnusedParameters": true,            // Emite erros para parâmetros de funções não utilizados
    
    /* Compilação e Resolução */
    "target": "ES6",                       // Define a versão do JavaScript de saída (ECMAScript 6)
    "outDir": "./dist",                    // Diretório de destino para os arquivos compilados (.js)
    "moduleResolution": "node"             // Estratégia de resolução de módulos baseada no ecossistema Node
  }
}
```

---

## 🛑 3. Diretivas de Compilação (Comentários de Suporte)

Recursos para gerenciar exceções ou comportamentos específicos diretamente no código-fonte. Deve ser utilizado com moderação.

```typescript
// @ts-ignore
// Ignora silenciosamente o erro gerado na linha imediatamente abaixo.

// @ts-expect-error
// Indica que um erro é estritamente esperado na linha seguinte. Se o erro NÃO ocorrer, o TS emite um aviso.
```

💡 *Dica de arquitetura: Prefira sempre `// @ts-expect-error` em vez de `// @ts-ignore`. Se o código for corrigido no futuro, o TypeScript avisa que você pode remover o comentário, mantendo a base de código limpa.*

---

## 🎯 4. Melhores Práticas de Tipagem

Regras fundamentais seguidas por equipes de alta performance para manter o ecossistema escalável:

*   **Prefira o Singular:** Mantenha os nomes de tipos focados na entidade única. Use `type User` em vez de `type Users`.
*   **Contratos de Objetos:** Priorize o uso de `interface` para definir estruturas de objetos devido à melhor performance do compilador e legibilidade.
*   **Tipagem Segura:** Evite o uso de `any`. Utilize `unknown` quando o tipo do dado for incerto, pois ele força uma validação lógica antes do uso da variável.
