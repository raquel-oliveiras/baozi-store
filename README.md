# Baozi Store API

API REST para controle básico de **clientes**, **produtos** e **pedidos** da Baozi Store, uma pequena loja que vende pão chinês (baozi).

Atividade prática da disciplina **Desenvolvimento Web Back-End** (UNINTER).

## Tecnologias

- Java
- Spring Boot (Spring Web)
- Spring Data JPA
- Banco relacional H2 (armazenado em arquivo local)
- Maven
- Postman (testes)

## Estrutura do projeto

O projeto segue o padrão MVC do Spring:

```
src/main/java/com/example/baozi
├── controller   # endpoints REST (ClienteController, ProdutoController, PedidoController)
├── model        # entidades JPA (Cliente, Produto, Pedido)
├── repository   # interfaces JpaRepository
└── BaoziApplication.java
```

## Como executar

Pré-requisito: JDK instalado (a versão está definida no `pom.xml`).

```bash
# Linux / macOS
./mvnw spring-boot:run

# Windows
mvnw.cmd spring-boot:run
```

A API sobe em `http://localhost:8080`. O banco H2 é criado automaticamente na pasta `data/` e as tabelas são geradas pelo Hibernate na primeira execução.

Também é possível abrir o projeto no IntelliJ IDEA e executar a classe `BaoziApplication`.

Console do H2 (opcional): `http://localhost:8080/h2-console`
JDBC URL: `jdbc:h2:file:./data/baozidb` · usuário: `sa` · senha: em branco.

## Entidades

| Entidade | Campos |
|---|---|
| Cliente | `id` (Long), `nome` (String), `clienteDesde` (LocalDate) |
| Produto | `id` (Long), `nome` (String), `preco` (BigDecimal), `estoque` (Boolean) |
| Pedido | `id` (Long), `cliente` (Cliente), `produto` (Produto), `quantidade` (Integer) |

Um cliente pode fazer vários pedidos e um produto pode estar em vários pedidos. Cada pedido tem exatamente um cliente e um produto.

## Endpoints

Os mesmos quatro endpoints existem para `/clientes`, `/produtos` e `/pedidos`:

| Método | Endpoint | Descrição | Resposta |
|---|---|---|---|
| POST | `/{recurso}` | Cria um registro | 201 Created |
| GET | `/{recurso}` | Lista todos | 200 OK |
| GET | `/{recurso}/{id}` | Consulta por ID | 200 OK ou 404 |
| DELETE | `/{recurso}/{id}` | Apaga por ID | 204 No Content ou 404 |

Ao criar um pedido, a API retorna 400 Bad Request se o cliente ou o produto informado não existir.

## Exemplos de requisição

Todos os POSTs usam `Content-Type: application/json`.

**POST /clientes**

```json
{ "nome": "Raquel5282752", "clienteDesde": "2026-11-14" }
```

**POST /produtos**

```json
{ "nome": "Baozi de Carne", "preco": 12.50, "estoque": true }
```

**POST /pedidos**

```json
{ "cliente": { "id": 1 }, "produto": { "id": 1 }, "quantidade": 5 }
```

Crie o cliente e o produto antes do pedido. Para apagar, apague primeiro o pedido e depois o cliente ou o produto ligados a ele.

## Autora

Raquel
