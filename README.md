# Finanças API
![GitHub repo size](https://img.shields.io/github/repo-size/samuelmsilva2v/finances-api?style=for-the-badge)
![GitHub language count](https://img.shields.io/github/languages/count/samuelmsilva2v/finances-api?style=for-the-badge)
![GitHub forks](https://img.shields.io/github/forks/samuelmsilva2v/finances-api?style=for-the-badge)

[🇺🇸 Read in English](#finances-api)

🖥️ API RESTful desenvolvida em Spring Boot para controle de finanças pessoais: cadastro e autenticação de usuário, categorias e contas (receitas e despesas), com um evento assíncrono via RabbitMQ que dispara um e-mail de confirmação a cada conta cadastrada.

> Projeto de portfólio / estudo pessoal. As credenciais em `application.properties` e `docker-compose.yml` são apenas os defaults locais dos containers Docker deste projeto — não é o padrão indicado para produção (o ideal ali é usar variáveis de ambiente).

### Tecnologias utilizadas
- Java 21
- Spring Boot 3.4
- Spring Data JPA / Hibernate
- MySQL 8
- JWT (autenticação) + SHA-256 (hash de senha)
- RabbitMQ (evento ao cadastrar uma conta)
- Spring Mail + MailHog (e-mail de confirmação, para testes locais)
- ModelMapper
- Swagger / OpenAPI (documentação)
- Docker Compose (MySQL, RabbitMQ e MailHog)
- Maven

### Funcionalidades
- Cadastro e autenticação de usuário com token JWT
- CRUD de categorias
- CRUD de contas, classificadas como receita (`INCOME`) ou despesa (`EXPENSE`)
- Ao cadastrar uma conta, um evento é publicado no RabbitMQ e um consumidor da própria aplicação envia um e-mail de confirmação com os dados da conta (visível no MailHog em ambiente local)

## Endpoints

### Usuário
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| POST | `/api/user/register` | Cadastra um usuário |
| POST | `/api/user/authenticate` | Autentica o usuário e retorna um token JWT (expira em 30 minutos) |

### Categorias
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| POST | `/api/category` | Cadastra uma categoria |
| GET | `/api/category` | Lista todas as categorias |
| GET | `/api/category/{id}` | Consulta uma categoria por ID |
| PUT | `/api/category/{id}` | Atualiza uma categoria |
| DELETE | `/api/category/{id}` | Remove uma categoria |

### Contas
| Método | Endpoint | Descrição |
|--------|----------|-----------|
| POST | `/api/bill` | Cadastra uma conta e publica um evento no RabbitMQ |
| GET | `/api/bill` | Lista todas as contas |
| GET | `/api/bill/{id}` | Consulta uma conta por ID |
| PUT | `/api/bill/{id}` | Atualiza uma conta |
| DELETE | `/api/bill/{id}` | Remove uma conta |
| POST | `/api/bill-category` | Associa uma categoria a uma conta |

Exemplo de corpo da requisição (`/api/bill`):
```json
{
  "name": "Aluguel",
  "date": "05/09/2026",
  "value": 1500.00,
  "type": "EXPENSE"
}
```

## Configuração e Execução

### 1. Clone o repositório:
```bash
git clone https://github.com/samuelmsilva2v/finances-api.git
cd finances-api
```

### 2. Suba os containers (MySQL, RabbitMQ e MailHog):
```bash
docker-compose up -d
```

### 3. Instale as dependências:
```bash
mvn clean install
```

### 4. Execute o projeto:
```bash
mvn spring-boot:run
```

### 5. Acesse a aplicação:
- Documentação da API: http://localhost:8080/swagger-ui/index.html
- Painel do RabbitMQ: http://localhost:15672 (usuário/senha: `guest`/`guest`)
- E-mails enviados (MailHog): http://localhost:8025

---

# Finances API
[🇧🇷 Leia em Português](#finanças-api)

🖥️ RESTful API developed in Spring Boot for personal finance management: user registration and authentication, categories and bills (income and expenses), with an asynchronous RabbitMQ event that triggers a confirmation e-mail for every new bill.

> Personal portfolio / study project. Credentials in `application.properties` and `docker-compose.yml` are just the local Docker container defaults for this project — that's not the recommended pattern for production (environment variables would be the way to go there).

### Technologies used
- Java 21
- Spring Boot 3.4
- Spring Data JPA / Hibernate
- MySQL 8
- JWT (authentication) + SHA-256 (password hashing)
- RabbitMQ (event on bill creation)
- Spring Mail + MailHog (confirmation e-mail, for local testing)
- ModelMapper
- Swagger / OpenAPI (documentation)
- Docker Compose (MySQL, RabbitMQ and MailHog)
- Maven

### Features
- User registration and authentication with JWT
- Category CRUD
- Bill CRUD, classified as income (`INCOME`) or expense (`EXPENSE`)
- When a bill is created, an event is published to RabbitMQ and a consumer in the same application sends a confirmation e-mail with the bill's data (visible in MailHog locally)

## Endpoints

### User
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/user/register` | Registers a user |
| POST | `/api/user/authenticate` | Authenticates the user and returns a JWT token (expires in 30 minutes) |

### Categories
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/category` | Creates a category |
| GET | `/api/category` | Lists all categories |
| GET | `/api/category/{id}` | Retrieves a category by ID |
| PUT | `/api/category/{id}` | Updates a category |
| DELETE | `/api/category/{id}` | Deletes a category |

### Bills
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/bill` | Creates a bill and publishes an event to RabbitMQ |
| GET | `/api/bill` | Lists all bills |
| GET | `/api/bill/{id}` | Retrieves a bill by ID |
| PUT | `/api/bill/{id}` | Updates a bill |
| DELETE | `/api/bill/{id}` | Deletes a bill |
| POST | `/api/bill-category` | Links a category to a bill |

Example request body (`/api/bill`):
```json
{
  "name": "Rent",
  "date": "05/09/2026",
  "value": 1500.00,
  "type": "EXPENSE"
}
```

## Configuration and Execution

### 1. Clone the repository:
```bash
git clone https://github.com/samuelmsilva2v/finances-api.git
cd finances-api
```

### 2. Start the containers (MySQL, RabbitMQ and MailHog):
```bash
docker-compose up -d
```

### 3. Install the dependencies:
```bash
mvn clean install
```

### 4. Run the project:
```bash
mvn spring-boot:run
```

### 5. Access the application:
- API Documentation: http://localhost:8080/swagger-ui/index.html
- RabbitMQ dashboard: http://localhost:15672 (username/password: `guest`/`guest`)
- Sent e-mails (MailHog): http://localhost:8025
