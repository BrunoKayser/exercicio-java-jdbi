# 🧪 Exercício Prático – Spring Boot + JDBI + H2

## 🎯 Objetivo

Neste exercício, você irá desenvolver uma pequena API REST em **Java 11** utilizando **Spring Boot** e o framework **JDBI** para realizar operações de leitura e escrita em um banco de dados relacional **H2** em memória.

O principal objetivo é aprender:

- Como utilizar o **JDBI com SQL externo em arquivos `.sql`**
- Como configurar e usar um banco H2 em memória
- Como estruturar o projeto com **Controller → Service → Repository**
- Como fazer um **INSERT** e um **SELECT com filtro e `INNER JOIN`**
- Como criar e consumir endpoints REST

---

## 📦 O que deve ser implementado

Você irá desenvolver um sistema simples de **cadastro de livros** e **categorias**.

### 📚 Entidades:

1. **Categoria**
   - `id` (Long, PK)
   - `nome` (String, obrigatório)

2. **Livro**
   - `id` (Long, PK)
   - `titulo` (String, obrigatório)
   - `autor` (String, obrigatório)
   - `categoria_id` (FK para Categoria, obrigatório)

---

## 🧱 O que o sistema deve fazer:

### ✅ Endpoint 1 – Criar livro

- Método: `POST`
- URL: `/api/livros`
- Request body:

```json
{
  "titulo": "Clean Code",
  "autor": "Robert C. Martin",
  "categoriaId": 1
}
```
- O endpoint deve chamar o **Service**, que chama o **Repository (DAO JDBI)**, que executa um `INSERT` utilizando uma **query definida em um arquivo `.sql`** localizado em `resources/sql/LivroRepository/inserirLivro.sql`.

---

### ✅ Endpoint 2 – Listar livros por categoria

- **Método:** `GET`
- **URL:** `/api/livros?categoria=Tecnologia`
- O endpoint deve retornar todos os livros da categoria informada via query param.
- O resultado deve vir de uma **query SQL externa com `INNER JOIN`** entre `livro` e `categoria`, localizada em:

- O endpoint deve chamar o **Service**, que chama o **Repository (DAO JDBI)**, que executa a query SQL usando `@SqlQuery`.

---

## ⚙️ Requisitos Técnicos

1. Utilizar **Spring Boot** com:
   - `spring-boot-starter-web`
   - `spring-boot-starter-jdbc`
   - `jdbi3-core`
   - `H2 Database`

2. Banco de dados: **H2 em memória**
3. Toda SQL (select e insert) deve estar em arquivos `.sql` no classpath:
   - Exemplo: `resources/sql/LivroRepository/inserirLivro.sql`
4. Usar estrutura:
   - `controller/`
   - `service/`
   - `repository/`

---

## 📁 Estrutura Esperada de Diretórios
```
src
└── main
├── java
│ └── com.seu.pacote
│ ├── controller
│ ├── service
│ ├── repository
│ └── model
└── resources
├── application.properties
└── sql
└── LivroRepository
├── inserirLivro.sql
└── buscarPorCategoria.sql
```

## 🧠 Tarefas

- [ ] Criar as **tabelas no banco** (via `data.sql` ou programaticamente).
- [ ] Implementar os **modelos** (`Livro`, `Categoria`).
- [ ] Criar os arquivos `.sql` com as queries:
  - `inserirLivro.sql` → comando `INSERT`
  - `buscarPorCategoria.sql` → comando `SELECT` com filtro + `INNER JOIN`
- [ ] Implementar o JDBI**, utilizando as anotações `@SqlUpdate` e `@SqlQuery`.
- [ ] Expor os endpoints via **Controller**.
- [ ] Organizar corretamente as responsabilidades entre:
  - `Controller`
  - `Service`
  - `Repository`


## 🧪 Exemplos de SQL esperados
`inserirLivro.sql`
```SQL
INSERT INTO livro (titulo, autor, categoria_id)
VALUES (:titulo, :autor, :categoriaId)
```

`buscarPorCategoria.sql`
```SQL 
SELECT l.id, l.titulo, l.autor, c.nome as categoria
FROM livro l
INNER JOIN categoria c ON l.categoria_id = c.id
WHERE c.nome = :categoria
```

## 🔐 Extra (Opcional)
Se quiser avançar:
- Criar um método update.
- Realizar consulta em lista.
