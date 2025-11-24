# 🚦 Algatransito API: Sistema de Gerenciamento de Trânsito

> API RESTful desenvolvida em Spring Boot para o controle de veículos, proprietários, autuações (multas) e apreensões, seguindo boas práticas de arquitetura em camadas e tratamento de exceções.

---

## 1. Visão Geral e Objetivo

A Algatransito API simula um sistema de gerenciamento de trânsito, implementando as regras de negócio essenciais para o cadastro de entidades e a gestão de infrações.

### 🔑 Principais Funcionalidades

* **Registro de Proprietários:** CRUD completo com validação de e-mail único.
* **Cadastro de Veículos:** Vinculação a um proprietário existente e validação de placa única.
* **Gestão de Autuações:** Registro de multas associadas a um veículo específico.
* **Controle de Apreensão:** Operações transacionais para apreender e liberar veículos, garantindo a integridade do status.

## 2. Tecnologias e Arquitetura

Este projeto utiliza o ecossistema Java/Spring para entregar uma aplicação robusta e manutenível.

### 🛠️ Stack Tecnológica

| Categoria | Tecnologia |
| :--- | :--- |
| **Linguagem** | Java 17+ |
| **Framework** | Spring Boot 3 |
| **Persistência** | Spring Data JPA (Hibernate) |
| **Banco de Dados** | H2 (em desenvolvimento, configurável para PostgreSQL/MySQL) |
| **Mapeamento** | ModelMapper |
| **Validação** | Jakarta Bean Validation |

### 🏗️ Arquitetura (DDD e Camadas)

O projeto está estruturado em camadas, separando as responsabilidades de forma clara:

1.  **`api` (Apresentação):** Controladores (REST), DTOs (Input/Model) e Assemblers para mapeamento de objetos.
2.  **`domain` (Domínio/Negócio):** Entidades (`model`), Regras de Negócio (`service`) e Repositórios (`repository`).
3.  **Tratamento de Exceções:** Implementação de `ApiExceptionHandler` para padronizar as respostas de erro usando o formato **Problem Details (RFC 7807)**.

## 3. Endpoints da API

Abaixo estão os principais recursos e rotas disponíveis na aplicação:

### A. Proprietários (`/proprietarios`)

| Método | Rota | Descrição | Status de Retorno |
| :--- | :--- | :--- | :--- |
| `GET` | `/proprietarios` | Lista todos os proprietários. | `200 OK` |
| `GET` | `/proprietarios/{id}` | Busca um proprietário por ID. | `200 OK` / `404 Not Found` |
| `POST` | `/proprietarios` | Cadastra um novo proprietário. | `201 Created` |
| `PUT` | `/proprietarios/{id}` | Atualiza um proprietário. | `200 OK` / `404 Not Found` |
| `DELETE` | `/proprietarios/{id}` | Remove um proprietário. | `204 No Content` / `404 Not Found` |

---

### B. Veículos (`/veiculos`)

| Método | Rota | Descrição | Status de Retorno |
| :--- | :--- | :--- | :--- |
| `GET` | `/veiculos` | Lista todos os veículos. | `200 OK` |
| `POST` | `/veiculos` | Cadastra um novo veículo. **Requer** `proprietario.id` no payload. | `201 Created` / `400 Bad Request` |
| `PUT` | `/veiculos/{id}/apreensao` | **Apreende** o veículo. | `204 No Content` / `400 Bad Request` |
| `DELETE` | `/veiculos/{id}/apreensao` | **Libera** o veículo da apreensão. | `204 No Content` / `400 Bad Request` |

**Exemplo de Request (POST /veiculos):**

```json
{
    "marca": "Chevrolet",
    "modelo": "Onix Plus",
    "placa": "XYZ1B23",
    "proprietario": {
        "id": 1
    }
}