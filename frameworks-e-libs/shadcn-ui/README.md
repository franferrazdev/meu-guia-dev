# 🎨 shadcn/ui: Guia Rápido de Inicialização e Componentização

Este guia reúne os comandos essenciais da CLI do **shadcn/ui** para configuração de ambiente, instalação modular de componentes e gerenciamento de temas utilizando classes utilitárias do Tailwind CSS.

---

## 🛠️ 1. Preparação e Inicialização

O comando principal para configurar a estrutura de pastas e injetar as variáveis globais de CSS em um projeto existente que já possua o Tailwind CSS configurado (Ex: Next.js ou Vite).

```bash
# Inicializa a configuração guiada do shadcn/ui no projeto
npx shadcn@latest init

# Dica avançada: Adicione a flag -d para aceitar automaticamente as configurações padrão (Estilo Zinc, variáveis CSS)
npx shadcn@latest init -d
```

---

## 🧩 2. Instalação Modular de Componentes

Diferente de bibliotecas de componentes tradicionais, o shadcn/ui não instala um pacote fechado na pasta `node_modules`. A CLI baixa e copia o código-fonte legível do componente diretamente para dentro do seu diretório de trabalho.

```bash
# Instala o componente de Botão (Button)
npx shadcn@latest add button

# Instala o componente de Caixa de Diálogo / Modal (Dialog)
npx shadcn@latest add dialog

# Instala o componente de Cartão Estrutural (Card)
npx shadcn@latest add card

# Instala o componente de Menu Retrátil (Dropdown Menu)
npx shadcn@latest add dropdown-menu
```

---

## ⚙️ 3. Gerenciamento e Utilitários da CLI

Comandos complementares para verificar integridades, atualizações de segurança e consultar parâmetros suportados pela ferramenta.

```bash
# Exibe a lista completa de comandos disponíveis e flags de ajuda
npx shadcn@latest --help

# Verifica o status dos componentes instalados e atualiza para as versões mais recentes
npx shadcn@latest update
```

---

## 💡 4. Conceito Fundamental: Controle Total do Código

> 🚀 **Nota de Arquitetura:** Por não residir em `node_modules`, os arquivos são baixados diretamente no seu diretório (geralmente em `components/ui/`). 
> 
> Isso significa que você tem **100% de propriedade sobre o código**. Você pode abrir o arquivo `button.tsx`, alterar suas propriedades nativas, customizar variantes de estilo com as classes do Tailwind ou modificar seu comportamento lógico de acordo com as necessidades do produto sem quebrar atualizações.

### Exemplo de Uso Prático
```tsx
import { Button } from "@/components/ui/button"

export default function MeuComponente() {
  return (
    <Button variant="outline" className="mt-4">
      Conectar ao Supabase
    </Button>
  )
}
```

---

## 📚 Documentação e Referências

*   [Documentação Oficial do shadcn/ui](https://shadcn.com)
*   [Galeria de Componentes e Exemplos](https://shadcn.com/docs/components/accordion)
