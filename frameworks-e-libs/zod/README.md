# 🛡️ Zod: Comandos e Conceitos Essenciais

Guia rápido de referência para o ecossistema do Zod, uma biblioteca de declaração e validação de dados *schema-first* para TypeScript. Defina a estrutura uma única vez e ganhe validação em tempo de execução + tipagem automática.

---

## 📌 1. Validações e Métodos de Execução

Como o Zod avalia os dados e gerencia o fluxo de erros:

* **`.parse(data)`**: 
  * Executa a validação imediatamente.
  * Retorna os dados limpos ou lança uma exceção (`ZodError`) se falhar.
  * *Uso ideal:* Fluxos onde o erro deve interromper a execução do código.

* **`.safeParse(data)`**: 
  * Valida sem lançar exceções. Retorna um objeto estruturado.
  * *Uso ideal:* Controladores de API, formulários e fluxos condicionais.
  * **Sucesso:** `{ success: true, data: dadoProcessado }`
  * **Falha:** `{ success: false, error: ZodError }`

* **`.optional()`**: 
  * Permite que o campo seja omitido ou receba o valor `undefined`.

* **`.nullable()`**: 
  * Permite que o campo receba o valor explícito `null`.

---

## 🧩 2. Tipos Primitivos e Modificadores

Blocos de construção básicos para a criação de regras rigorosas:

* **`z.string()`**: Valida textos e strings.
  * *Modificadores comuns:* `.email()`, `.url()`, `.uuid()`, `.min(n)`, `.max(n)`, `.regex(pattern)`.
* **`z.number()`**: Valida números (inteiros ou flutuantes).
  * *Modificadores comuns:* `.int()`, `.positive()`, `.nonnegative()`, `.min(n)`, `.max(n)`.
* **`z.boolean()`**: Valida valores booleanos (`true` / `false`).
* **`z.date()`**: Valida instâncias de objetos do tipo `Date`.
* **`z.array(schema)`**: Valida se todos os itens de uma lista seguem o schema informado.
* **`z.enum([opções])`**: Restringe a string a valores específicos. Ex: `z.enum(["ativo", "inativo"])`.

---

## 🏢 3. Esquemas de Objetos e Composição

Métodos para manipular, estender e reaproveitar estruturas de dados complexas:

* **`z.object({ ... })`**: Cria a estrutura base de validação para um objeto JavaScript.
* **`.pick({ campo: true })`**: Cria um novo esquema selecionando **apenas** as propriedades indicadas.
* **`.omit({ campo: true })`**: Cria um novo esquema **removendo** as propriedades indicadas.
* **`.partial()`**: Cria uma cópia do esquema original transformando **todos** os campos em opcionais.

---

## 🪄 4. Inferência de Tipos (TypeScript)

Elimina a necessidade de duplicar código escrevendo interfaces manuais. O tipo do TypeScript nasce diretamente a partir do esquema do Zod:

```typescript
import { z } from 'zod';

const UserSchema = z.object({
  name: z.string(),
  age: z.number().min(18)
});

// Gera automaticamente a tipagem exata para o compilador do TypeScript
type User = z.infer<typeof UserSchema>;
```

---

## ⚡ 5. Transformação e Refinamento Advanced

Recursos para manipular dados durante a validação ou aplicar lógicas customizadas:

* **`.refine((val) => boolean, { message })`**: 
  * Insere uma lógica de validação customizada (ex: checar se duas senhas coincidem). 
  * Se a função retornar `false`, a validação falha com a mensagem definida.

* **`.transform((val) => novoValor)`**: 
  * Altera ou formata o dado de entrada durante o processamento.
  * *Exemplo:* Pegar uma string e retornar apenas letras maiúsculas.

* **`.coerce.tipo()`**: 
  * Força o Zod a tentar converter os dados recebidos para o tipo final desejado antes de validar.
  * *Uso comum:* Converter strings de parâmetros de URL (`/users?page=2`) em números reais (`2`).
  ```typescript
  const PageSchema = z.coerce.number(); // Converte "2" (string) para 2 (number)
  ```
