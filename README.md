[English](#english) | [Português](#português)

---

<a name="english"></a>

## English

### Project Setup and Usage Guide

This guide provides instructions on how to set up and run this project in both development and production environments.

#### Prerequisites

- Docker
- Docker Compose

#### 1. Initial Environment Setup

Before running any commands, you need to set up your environment variables.

1.  **Create and configure the `.env` file:**
    Copy the example file and then append your user and group ID to it. This ensures that files created in the Docker containers have the correct ownership on your local machine.

    ```bash
    cp .env.example .env
    echo "UID=$(id -u)" >> .env
    echo "GID=$(id -g)" >> .env
    ```

2.  **Review the other variables:**
    Open the `.env` file and adjust any other values if necessary (e.g., database credentials, secret keys).

#### 2. First-Time Backend Setup

To create the initial Django project structure inside the `backend` directory, run the following command. This only needs to be done once.

```bash
docker-compose run --rm --entrypoint /bin/sh backend -c "django-admin startproject config ."
```

#### 3. First-Time Frontend Setup

The frontend setup requires a few steps to initialize the Vite + React project and install all dependencies, including TailwindCSS. This only needs to be done once.

1.  **Scaffold the Vite + React project:**
    This command runs interactively. When prompted about the directory not being empty, choose the **"Ignore files and continue"** option.

    ```bash
    docker run --rm -it -v "$(pwd)/frontend:/app" -w /app node:24-alpine npm create vite@latest . -- --template react
    ```

2.  **Install Node.js dependencies:**

    ```bash
    docker run --rm -v "$(pwd)/frontend:/app" -w /app node:24-alpine npm install
    ```

3.  **Install TailwindCSS dependencies:**
    This installs TailwindCSS v4 and the official Vite plugin.
    ```bash
    docker run --rm -v "$(pwd)/frontend:/app" -w /app node:24-alpine npm install -D tailwindcss @tailwindcss/vite
    ```

After these commands, the frontend is configured. The necessary changes to `vite.config.js` and `frontend/src/index.css` have already been applied.

#### 4. Running in Development Mode

To start all services (backend, frontend, database, etc.) for development, run:

```bash
docker-compose up --build
```

- **Backend API** will be available at `http://localhost:8000`
- **Frontend Dev Server** will be available at `http://localhost:5173` (with hot-reloading)

#### 5. Running in Production Mode

To build and run the production-ready containers, use the `docker-compose.prod.yml` file. This will build the static frontend files and serve them via Nginx.

```bash
docker-compose -f docker-compose.prod.yml up --build
```

- The **production application** will be available at `http://localhost` (port 80).

---

<a name="português"></a>

## Português

### Guia de Configuração e Uso do Projeto

Este guia fornece instruções sobre como configurar e executar este projeto nos ambientes de desenvolvimento e produção.

#### Pré-requisitos

- Docker
- Docker Compose

#### 1. Configuração Inicial do Ambiente

Antes de executar qualquer comando, você precisa configurar suas variáveis de ambiente.

1.  **Crie e configure o arquivo `.env`:**
    Copie o arquivo de exemplo e, em seguida, adicione o ID do seu usuário e grupo a ele. Isso garante que os arquivos criados nos contêineres Docker tenham a propriedade correta na sua máquina local.

    ```bash
    cp .env.example .env
    echo "UID=$(id -u)" >> .env
    echo "GID=$(id -g)" >> .env
    ```

2.  **Revise as outras variáveis:**
    Abra o arquivo `.env` e ajuste quaisquer outros valores se necessário (ex: credenciais do banco de dados, chaves secretas).

#### 2. Configuração Inicial do Backend

Para criar a estrutura inicial do projeto Django dentro do diretório `backend`, execute o seguinte comando. Isso só precisa ser feito uma vez.

```bash
docker-compose run --rm --entrypoint /bin/sh backend -c "django-admin startproject config ."
```

#### 3. Configuração Inicial do Frontend

A configuração do frontend requer alguns passos para inicializar o projeto Vite + React e instalar todas as dependências, incluindo o TailwindCSS. Isso só precisa ser feito uma vez.

1.  **Crie o projeto Vite + React:**
    Este comando é executado de forma interativa. Quando perguntado sobre o diretório não estar vazio, escolha a opção **"Ignore files and continue"** (Ignorar arquivos e continuar).

    ```bash
    docker run --rm -it -v "$(pwd)/frontend:/app" -w /app node:24-alpine npm create vite@latest . -- --template react
    ```

2.  **Instale as dependências do Node.js:**

    ```bash
    docker run --rm -v "$(pwd)/frontend:/app" -w /app node:24-alpine npm install
    ```

3.  **Instale as dependências do TailwindCSS:**
    Isso instala o TailwindCSS v4 e o plugin oficial para Vite.
    ```bash
    docker run --rm -v "$(pwd)/frontend:/app" -w /app node:24-alpine npm install -D tailwindcss @tailwindcss/vite
    ```

Após esses comandos, o frontend está configurado. As alterações necessárias no `vite.config.js` e `frontend/src/index.css` já foram aplicadas.

#### 4. Executando em Modo de Desenvolvimento

Para iniciar todos os serviços (backend, frontend, banco de dados, etc.) para desenvolvimento, execute:

```bash
docker-compose up --build
```

- A **API do Backend** estará disponível em `http://localhost:8000`
- O **Servidor de Desenvolvimento do Frontend** estará disponível em `http://localhost:5173` (com hot-reloading)

#### 5. Executando em Modo de Produção

Para construir e executar os contêineres prontos para produção, use o arquivo `docker-compose.prod.yml`. Este comando irá construir os arquivos estáticos do frontend e servi-los através do Nginx.

```bash
docker-compose -f docker-compose.prod.yml up --build
```

- A **aplicação em produção** estará disponível em `http://localhost` (porta 80).
