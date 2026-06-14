# 🌐 Arquitetura e Design de APIs RESTful: Guia de Boas Práticas

Este manual centraliza as diretrizes, padrões e convenções globais de engenharia para o design de APIs RESTful robustas, escaláveis e resilientes. Ele abrange desde asemântica básica do protocolo HTTP até conceitos avançados de produção para alta maturidade.

---

## 📌 1. Fundamentos da Arquitetura REST

* **REST vs. RESTfull:** REST (*Representational State Transfer*) é o conjunto teórico de restrições e diretrizes arquiteturais. **RESTful** é a denominação dada ao sistema ou API que implementa e respeita restrições na prática.
* **Recursos e URIs:** Tudo é tratado como um recurso único indexável através de um identificador uniforme (URI).
* **Stateless (Sem Estado):** Cada requisição do cliente para o servidor deve conter todo o contexto e dados necessários para seu processamento independente, simplificando a escalabilidade horizontal.
* **Cacheável:** Respostas devem ser explicitamente marcadas como cacheáveis ou não para otimizar o tráfego de rede.

---

## 🏷️ 2. Design de Endpoints e Nomenclatura

A URL deve representar estritamente a identidade do recurso, nunca a ação ou o comportamento do sistema.

* **Substantivos, Não Verbos:** Evite rotas procedurais.
  * ❌ `POST /createOrder` ou `GET /getUsers`
  * ✅ `POST /ourders`ou `GET /users`
* **Pluralização Consciente:** Utilize sempre substantivos no plural para coleções. Use `/products` em vez de `/product`.
* **Hierarquia de Recursos:** Represente relacionamentos lógicos aninhando os caminhos.
  * Exemplo: `GET /users/123/orders`(Busca todos os pedidos atrelados especificamente ao usuário 123).
  * **Estilo Visual:** Utilize letras minúsculas e hífens (**kebab-case**) para nomes compostos em endpoints. Use `/order-items`em vez de `/orderItems`.
 
* ## 🔀 3. Semântica dos Métodos HTTP

* Cada verbo deve respeitar estritamente o seu propósito lógico e características de contrato:

*   **`GET`:** Recuperação de dados. Não pode possuir efeitos colaterais de mutação no servidor. (Seguro e Idempotente).
*   **`POST`:** Criação de um novo recurso secundário ou execução de processamentos. (Não Idempotente).
*   **`PUT`:** Atualização ou substituição integral de um recurso. Se o recurso não existir, pode criá-lo. (Idempotente).
*   **`PATCH`:** Atualização parcial e cirúrgica de propriedades específicas de um recurso.
*   **`DELETE`:** Remoção física ou lógica de um recurso do sistema. (Idempotente).

---

## 📊 4. Códigos de Status HTTP (Status Codes)

As respostas das APIs devem usar os códigos de status corretos para descrever o resultado da operação, em vez de mascarar todas as respostas com sucesso genérico.

| Código | Mensagem | Cenário de Aplicação |
| :--- | :--- | :---|
| | **`200`** | **OK** | Sucesso em requisições de consulta (`GET`) ou atualizações síncronas. |
| **`201`** | **Created** | Sucesso na criação de um recurso. Retorna o payload criado (`POST`). |
| **`204`** | **No Content** | Processamento concluído com sucesso, mas sem corpo de retorno (`DELETE`). |
| **`400`** | **Bad Request** | Erro de validação de dados enviado pelo cliente (Payload inválido, regras de negócio). |
| **`401`** | **Unauthorized** | Falha de autenticação ou ausência de credenciais (Identidade desconhecida). |
| **`403`** | **Forbidden** | Cliente autenticado, porém sem privilégios de acesso autorizados para o recurso. |
| **`404`** | **Not Found** | Recurso ou endpoint solicitado não foi localizado no servidor. |
| **`500`** | **Internal Server Error**| Erro inesperado capturado na execução interna do servidor (Exceções não tratadas). |

---

## ⚙️ 5. Versionamento, Filtragem e Performance

*   **Versionamento por URL:** Proteja o contrato de consumo evitando quebrar clientes legados. Prefira o uso explícito da versão na rota principal: `/api/v1/users`.
*   **Paginação e Filtros:** Nunca entregue coleções completas sem limites em produção. Utilize parâmetros de busca (*Query Parameters*) para refinar o processamento de dados massivos:
    *   Exemplo: `/products?category=electronics&page=2&limit=20`
*   **Segurança:** Adote JSON exclusivamente como padrão de payload, utilize cabeçalhos baseados em tokens **JWT** (OAuth 2.1) e proteja o servidor com controle de requisições (**Rate Limiting / Throttling**) enviando metadados nos headers `X-RateLimit-Limit` e `X-RateLimit-Remaining`.

---

## 🚀 6. Padrões Avançados para APIs de Nível de Produção

Diretrizes exigidas para alcançar o **Nível 3 do Modelo de Maturidade de Richardson**:

### A. Idempotência Controlada
Garante que reexecuções de uma mesma chamada em caso de quedas de rede não causem efeitos colaterais repetidos (como cobranças duplicadas). Implementa-se através de chaves únicas enviadas pelo cliente no cabeçalho HTTP:
```text
Idempotency-Key: a4f8b2c6-d9e0-4123-bcde-56789abcdef0
```

### B. HATEOAS (*Hypermedia as the Engine of Application State*)
A API retorna os dados solicitados acompanhados por um contrato dinâmico de links de hipermídia, informando ao cliente quais ações estão contextualmente disponíveis a seguir:
```json
{
  "id": 4821,
  "status": "aguardando_pagamento",
  "total": 250.00,
  "_links": {
    "self": { "href": "/api/v1/orders/4821" },
    "pagar": {"href": "/api/v1/orders/4821/payments" },
    "cancelar": {"href': "/api/v1/orders/4821/calcel" }
  }
}
```

### C. Tratamento de Erros Padronizado (RFC 7807)
Substitua payloads de erro proprietários pelo padrão universal *Problem Details for HTTP APIs* (`application/problem+json`), facilitando o mapeamento automatizado no cliente:
```json
{
  "type": "https://meusite.com",
  "title": "Saldo insuficiente para transacao",
  "status": 400,
  "detail": "O saldo atual da conta (R\$ 50,00) e menor do que o valor do pedido (R\$ 250,00).",
  "instance": "/api/v1/orders/4821/payments"
}
```

---

## 📚 Ferramentas e Documentação

*   **OpenAPI / Swagger:** Mantém a especificação e documentação interativa viva do ecossistema.
*   [Especificação Completa da RFC 7807 - Problem Details](https://ietf.org)


