# ◭ Prisma ORM: Guia Rápido de Sobrevivência e Migrações

Este guia reúne os comandos essenciais da CLI do **Prisma ORM** para sincronização de esquemas, geração de tipagem estrita no TypeScript, gerenciamento de migrações e manipulação visual de dados.

---

## 💻 1. Comandos CLI Fundamentais (Terminal)

Comandos utilizados no ciclo de desenvolvimento para automatizar a comunicação e estruturação do banco de dados.

```bash
# O mais importante para o dia a dia. Compara o schema.prisma com o banco de dados, gera o arquivo SQL de migração e o aplica automaticamente.
npx prisma migrate dev

# Lê o arquivo de schema e gera/atualiza o Prisma Client com tipagem forte para o TypeScript. (Execute sempre após alterar o schema ou atualizar o pacote).
npx prisma generate

# Sincroniza o schema diretamente com o banco de dados sem gerar histórico de arquivos de migração. (Ideal para prototipagem rápida e testes locais).
npx prisma db push

# Engenharia reversa: examina um banco de dados existente e gera automaticamente o arquivo schema.prisma correspondente à estrutura atual.
npx prisma db pull

# Inicializa um servidor web local com uma interface gráfica rica para visualizar, criar, editar e deletar registros diretamente nas tabelas.
npx prisma studio

# Apaga todo o banco de dados (Drop), recria o esquema e executa todas as migrações desde o início. (Útil para redefinir o ambiente local).
npx prisma migrate reset
```

---

## 🛑 2. Alertas Importantes de Ambiente (Dev vs. Produção)

> ⚠️ **`prisma db push` em Produção:** Nunca utilize este comando no ambiente de produção. Por não registrar um histórico incremental via arquivos de migração (`.sql`), ele pode aplicar alterações destrutivas diretamente nas tabelas ativas, resultando em perda irreversível de dados.
> 
> 🚨 **`prisma migrate reset`:** Comando estritamente proibido em ambientes compartilhados, homologação ou produção. Ele limpa completamente todas as tabelas antes de reexecutar o histórico.
> 
> 🚀 **Deploy em Produção:** Para aplicar migrações em servidores de produção de forma segura dentro do seu fluxo de CI/CD, o comando correto que deve ser automatizado é o:
> ```bash
> npx prisma migrate deploy
> ```

---

## 📝 3. Estrutura Base Prática (`schema.prisma`)

Exemplo conceitual de como as entidades e conexões são estruturadas para o funcionamento do motor de geração:

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  createdAt DateTime @default(now())
}
```

---

## 📚 Documentação e Referências

*   [Documentação Oficial do Prisma ORM CLI](https://prisma.io)
*   [Guia de Fluxos de Trabalho com Prisma Migrate](https://prisma.io)
*   [Referência de Tipos e Atributos do Schema](https://prisma.io)
