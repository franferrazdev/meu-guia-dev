# 🐘 PostgreSQL: Guia Rápido de Sobrevivência, Comandos e Tipagem

Este guia centraliza os meta-comandos de terminal da interface `psql`, a sintaxe padrão SQL para manipulação e estruturação de dados (DML/DDL), rotinas de backup, exemplos práticos de população de tabelas e o mapeamento dos tipos de dados nativos do ecossistema.

---

## 💻 1. Conexão e Navegação no Terminal `psql`

Meta-comandos utilitários executados diretamente dentro do terminal interativo do PostgreSQL.

```bash
# Conecta ao servidor PostgreSQL como um usuário específico (Ex: postgres)
psql -U postgres

# \l                 -> Lista todos os bancos de dados do servidor
# \c nome_do_banco   -> Conecta-se a um banco de dados específico
# \dt                -> Lista todas as tabelas do banco atual
# \dt+               -> Lista tabelas com detalhes adicionais (tamanho e descricao)
# \d nome_da_tabela  -> Descreve a estrutura da tabela (colunas, tipos e constraints)
# \du                -> Lista todos os usuarios e suas respectivas permissoes
# \dn                -> Lista todos os schemas cadastrados
# \q                 -> Encerra o terminal psql e retorna ao sistema operacional
```

---

## 🔍 2. Comandos SQL Fundamentais (DML / DQL)

Utilizados no dia a dia para manipulação e consulta de registros em tabelas.

```sql
-- Buscar todos os dados de uma tabela
SELECT * FROM clientes;

-- Buscar valores únicos removendo duplicatas
SELECT DISTINCT uf FROM clientes;

-- Inserir um registro simples
INSERT INTO clientes (nome, idade) VALUES ('Ana Silva', 28);

-- Atualizar dados com cláusula condicional de segurança
UPDATE clientes SET idade = 29 WHERE id = 1;

-- Remover registros com cláusula condicional de segurança
DELETE FROM clientes WHERE id = 1;

-- Ordenar resultados de forma decrescente
SELECT * FROM clientes ORDER BY idade DESC;

-- Paginação de resultados (Limita o retorno e pula os primeiros registros)
SELECT * FROM clientes LIMIT 10 OFFSET 20;
```

---

## 🛠️ 3. Definição de Estrutura de Dados (DDL)

Comandos para criação, modificação e exclusão física de objetos no banco de dados.

```sql
-- Criar um novo banco de dados
CREATE DATABASE meu_projeto_db;

-- Excluir fisicamente uma tabela e todos os seus dados
DROP TABLE clientes;

-- Criar um índice para otimização de performance em buscas frequentes
CREATE INDEX idx_clientes_nome ON clientes(nome);

-- Alterar a estrutura de uma tabela existente (Adicionar coluna)
ALTER TABLE clientes ADD COLUMN telefone VARCHAR(20);
```

### 🧩 Exemplos Práticos de Criação de Tabelas

```sql
-- Padrão Inicial de Tabela com Serial (Auto-incremento)
CREATE TABLE clientes (
    id SERIAL PRIMARY KEY,
    idade INT,
    nome VARCHAR(150),
    uf VARCHAR(2)
);

-- Padrão com Restrições Estritas de Integridade (NOT NULL e CHAR fixo)
CREATE TABLE clientes_estrito (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(150) NOT NULL,
    idade INT NOT NULL,
    uf CHAR(2) NOT NULL
);
```

---

## 📝 4. Inserção Avançada de Linhas (INSERT INTO)

Ao inserir textos (`strings`) ou dados de data/hora no PostgreSQL, utilize obrigatoriamente **aspas simples (`'`)**. Números podem ser passados diretamente.

### Inserção Única
```sql
INSERT INTO clientes (nome, idade, uf)
VALUES ('Diego Souza', 34, 'SP');
```

### Inserção Múltipla (Bulk Insert)
Abordagem muito mais performática e recomendada para o ambiente profissional, reduzindo o número de requisições enviadas ao servidor:
```sql
INSERT INTO clientes (nome, idade, uf)
VALUES 
    ('Beatriz Lima', 22, 'RJ'),
    ('Carlos Eduardo', 41, 'MG'),
    ('Fernanda Costa', 29, 'SC');
```

---

## 💾 5. Backup e Restauração (Comandos do Sistema)

Estes comandos devem ser executados diretamente no terminal do seu sistema operacional, **fora** da interface do `psql`.

```bash
# Exportar/Gerar backup completo de um banco de dados específico para um arquivo .sql
pg_dump -U username -d dbname -f backup.sql

# Importar/Restaurar dados a partir de um arquivo de backup existente
psql -U username -d dbname -f backup.sql
```

---

## 🗃️ 6. Dicionário de Tipos de Dados

O PostgreSQL possui um sistema de tipagem rico e estrito. Abaixo estão listados os principais grupos utilizados:

### 🔢 Numéricos

| Nome | Tamanho | Descrição | Alcance (Range) |
| :--- | :--- | :--- | :--- |
| `smallint` | 2 bytes | Inteiro de curto alcance | -32.768 a +32.767 |
| `integer` | 4 bytes | Escolha padrão para números inteiros | -2.147.483.648 a +2.147.483.647 |
| `bigint` | 8 bytes | Inteiro de longo alcance | -9,22e18 a +9,22e18 |
| `decimal` / `numeric` | Variável | Precisão exata definida pelo usuário | Sem limite técnico |
| `real` | 4 bytes | Precisão variável, inexata | Precisão de 6 dígitos decimais |
| `double precision` | 8 bytes | Precisão variável, inexata | Precisão de 15 dígitos decimais |
| `serial` | 4 bytes | Inteiro de auto-incremento automático | 1 a 2.147.483.647 |
| `bigserial` | 8 bytes | Inteiro longo de auto-incremento automático | 1 a 9.223.372.036.854.775.807 |

### 🔤 Caractere (Textos)
*   `char(n)`: Comprimento fixo. Preenche com espaços em branco caso a string seja menor que `n`.
*   `varchar(n)`: Comprimento variável limitado ao máximo de `n` caracteres. Não ocupa espaços extras.
*   `text`: Comprimento variável e ilimitado. Escolha ideal para grandes blocos de texto.

### 🏁 Booleano (Lógicos)
O tipo `BOOLEAN` aceita estados lógicos e aceita diversas representações válidas em suas queries:
*   **Verdadeiro:** `TRUE`, `'t'`, `'true'`, `'y'`, `'yes'`, `'1'`
*   **Falso:** `FALSE`, `'f'`, `'false'`, `'n'`, `'no'`, `'0'`

### 📅 Data e Hora
*   `timestamp`: 8 bytes. Armazena data e hora combinadas (sem fuso horário), precisão de 1 microssegundo.
*   `timestamptz`: 8 bytes. Data e hora combinadas **com** fuso horário (Timezone). Recomendado para aplicações globais.
*   `date`: 4 bytes. Armazena estritamente a data (ano, mês, dia).
*   `time`: 8 bytes. Armazena apenas o horário do dia.
*   `interval`: 16 bytes. Armazena faixas de tempo passadas (Ex: '1 day 2 hours').

---

## 📚 Documentação e Utilitários Úteis

*   `\h` ou `\h CREATE TABLE` -> Exibe ajuda de sintaxe para comandos SQL direto no terminal.
*   `\?` -> Exibe ajuda com a lista completa de comandos internos do `psql`.
*   `\i arquivo.sql` -> Executa em lote todas as instruções SQL contidas no arquivo informado.
*   [Documentação Oficial do Comando INSERT no PostgreSQL](https://postgresql.org)
*   [Tutoriais Práticos Avançados - Neon Database](https://neon.tech)
