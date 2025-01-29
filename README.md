
# **Microserviço de Agendamento de Tarefas**

## **Visão Geral**
Este microserviço fornece uma solução para o agendamento e execução de tarefas programadas, como envio de notificações, e-mails ou outros processos em segundo plano. Ele permite criar tarefas com data e hora específicas, gerenciar seu status e executá-las automaticamente no momento agendado.

O serviço é construído com **Node.js**, **TypeScript**, **Sequelize** para persistência de dados em **MySQL**, e **node-cron** para agendamento das tarefas.

## **Funcionalidades**

- **Agendamento de Tarefas**: Permite criar tarefas com data e hora específicas para execução.
- **Monitoramento de Status**: Visualize o status das tarefas, como "pendente", "executada" ou "erro".
- **Execução Automática**: As tarefas são executadas automaticamente no horário agendado.
- **Persistência de Dados**: Utiliza o Sequelize com MySQL para armazenar as tarefas e seus status.

## **Arquitetura**

A arquitetura do microserviço é baseada em uma API RESTful, que interage com o banco de dados MySQL usando **Sequelize** como ORM. O agendamento e execução das tarefas é feito com **node-cron**.

### **Tecnologias Utilizadas**
- **Node.js**: Plataforma de backend para JavaScript.
- **TypeScript**: Superset do JavaScript com tipagem estática.
- **Express**: Framework para criação de APIs RESTful.
- **Sequelize**: ORM para MySQL, facilitando a interação com o banco de dados.
- **node-cron**: Biblioteca para agendamento de tarefas com base em cron.
- **MySQL**: Banco de dados relacional para armazenar as informações das tarefas.
- **Docker**: Para containerizar a aplicação e facilitar a execução em qualquer ambiente.

## **Estrutura de Diretórios**

```plaintext
task-scheduler/
├── src/
│   ├── controllers/       # Controladores que manipulam as requisições da API
│   ├── models/            # Modelos de dados com Sequelize
│   ├── routes/            # Definição das rotas da API
│   ├── services/          # Lógica de negócios (como execução de tarefas)
│   ├── config.ts          # Configurações gerais, como conexão ao DB
│   └── app.ts             # Arquivo principal da aplicação
├── Dockerfile             # Arquivo para containerizar a aplicação
└── package.json           # Dependências do projeto
```

## **Modelos de Dados (Sequelize)**

A tabela **tasks** é usada para armazenar as tarefas agendadas, incluindo informações como título, descrição, data e hora agendada, e status.

### **Modelo de Tarefa**
- **title**: Título da tarefa (string).
- **description**: Descrição opcional da tarefa (string).
- **status**: Status da tarefa, que pode ser "pendente", "executada" ou "erro".
- **scheduledTime**: Data e hora agendadas para a execução da tarefa.

## **Endpoints da API**

### **POST /tasks**
Cria uma nova tarefa.

#### **Request Body**
```json
{
  "title": "Título da Tarefa",
  "description": "Descrição da Tarefa",
  "scheduledTime": "2025-01-29T10:00:00Z"
}
```

#### **Respostas**
- **201 Created**: Quando a tarefa é criada com sucesso.
- **400 Bad Request**: Se algum campo obrigatório estiver ausente ou mal formatado.

### **GET /tasks**
Retorna todas as tarefas cadastradas.

#### **Respostas**
- **200 OK**: Retorna um array de todas as tarefas.

```json
[
  {
    "id": 1,
    "title": "Título da Tarefa",
    "description": "Descrição da Tarefa",
    "status": "pending",
    "scheduledTime": "2025-01-29T10:00:00Z"
  }
]
```

### **GET /tasks/:id**
Retorna os detalhes de uma tarefa específica.

#### **Respostas**
- **200 OK**: Retorna os detalhes da tarefa.

```json
{
  "id": 1,
  "title": "Título da Tarefa",
  "description": "Descrição da Tarefa",
  "status": "pending",
  "scheduledTime": "2025-01-29T10:00:00Z"
}
```

## **Conexão com MySQL**

### **Configuração do Sequelize**
A aplicação se conecta ao banco de dados MySQL usando Sequelize, que facilita a interação com o banco. A configuração é feita no arquivo **config.ts**.

```typescript
import { Sequelize } from 'sequelize';

export const sequelize = new Sequelize({
  dialect: 'mysql',
  host: 'localhost',
  username: 'root', // Usuário do MySQL
  password: 'senha', // Senha do MySQL
  database: 'task_scheduler', // Nome do banco de dados
});
```

## **Agendamento de Tarefas**

O microserviço utiliza a biblioteca **node-cron** para agendar a execução das tarefas. A cada minuto, o cron job verifica as tarefas agendadas e as executa quando chega o momento.

## **Como Rodar o Projeto**

### **Pré-requisitos**
- Node.js (versão 14 ou superior)
- MySQL (ou Docker para MySQL)
- Docker (opcional, para containerizar a aplicação)

### **Passos para Execução**

1. **Clone o repositório:**
   ```bash
   git clone <url-do-repositorio>
   cd task-scheduler
   ```

2. **Instale as dependências:**
   ```bash
   npm install
   ```

3. **Configure o banco de dados MySQL**: 
   Crie um banco de dados no MySQL com o nome `task_scheduler`.

4. **Configure as credenciais de banco de dados**: 
   Edite o arquivo `src/config.ts` para configurar as credenciais de acesso ao seu banco de dados MySQL.

5. **Rodando a aplicação**: 
   Para rodar a aplicação localmente, execute o comando:
   ```bash
   npm run dev
   ```

6. **Rodando com Docker**:
   Se você preferir rodar o microserviço usando Docker, utilize o arquivo **Dockerfile** para criar uma imagem Docker.

### **Rodando o Docker**

1. Crie a imagem Docker:
   ```bash
   docker build -t task-scheduler .
   ```

2. Execute o contêiner:
   ```bash
   docker run -p 3000:3000 task-scheduler
   ```

## **Testes**

A aplicação possui testes para verificar o correto funcionamento das funcionalidades de agendamento, execução e status das tarefas. Você pode executar os testes utilizando o comando:

```bash
npm test
```

## **Considerações Finais**

Este microserviço oferece uma solução simples e eficaz para o agendamento e execução de tarefas no backend, usando tecnologias modernas e amplamente utilizadas como Node.js, Sequelize e MySQL. Ele pode ser expandido com mais funcionalidades, como notificações em tempo real ou integração com outros serviços.
