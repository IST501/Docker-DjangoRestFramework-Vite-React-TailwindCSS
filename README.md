[English](#english) | [Português](#português)

---

<a name="english"></a>

# Dockerized Django, React, and Vite Boilerplate

This project provides a comprehensive, containerized boilerplate for building modern web applications. It integrates a Django Rest Framework backend with a Vite-powered React and TailwindCSS frontend, all managed by Docker and Docker Compose for seamless development and production workflows.

### Technologies Used

- **Containerization:** Docker, Docker Compose
- **Backend:** Django, Django Rest Framework, Gunicorn
- **Frontend:** React, Vite, TailwindCSS
- **Database:** PostgreSQL
- **Caching:** Redis
- **Async Tasks:** Celery (with Celery Beat for scheduling)
- **Web Server (Production):** Nginx

### Project Structure

```
.
├── backend/            # Django application source code
├── frontend/           # React + Vite application source code
├── mobile/             # Placeholder for a mobile application
├── .env.example        # Example environment variables
├── docker-compose.yml  # Docker Compose for development
└── docker-compose.prod.yml # Docker Compose for production
```

## Project Setup and Usage Guide

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

#### 4. Configure TailwindCSS

After installing the dependencies, you need to configure Vite and your main CSS file to use Tailwind.

1.  **Configure the Vite Plugin:**
    Modify `frontend/vite.config.js` to include the `@tailwindcss/vite` plugin.

    ```javascript
    import { defineConfig } from "vite";
    import react from "@vitejs/plugin-react";
    import tailwindcss from "@tailwindcss/vite";

    // https://vite.dev/config/
    export default defineConfig({
      plugins: [react(), tailwindcss()],
    });
    ```

2.  **Import Tailwind Styles:**
    Replace the content of `frontend/src/index.css` with the following line to import Tailwind's base styles.

    ```css
    @import "tailwindcss";
    ```

3.  **(Optional) Test with a Sample Component:**
    To verify that Tailwind is working, you can replace the content of `frontend/src/App.jsx` with this sample component that uses Tailwind's utility classes.

    ```jsx
    function App() {
      return (
        <div className="min-h-screen bg-gray-900 text-white flex flex-col items-center justify-center font-sans">
          <header className="text-center">
            <h1 className="text-5xl font-bold text-cyan-400 mb-4">
              Vite + React + TailwindCSS
            </h1>
            <p className="text-lg text-gray-400">
              Your frontend environment is ready.
            </p>
          </header>
        </div>
      );
    }

    export default App;
    ```

#### 5. Running in Development Mode

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

# Boilerplate Docker com Django, React e Vite

Este projeto fornece um boilerplate abrangente e containerizado para a construção de aplicações web modernas. Ele integra um backend Django Rest Framework com um frontend React e TailwindCSS alimentado por Vite, tudo gerenciado pelo Docker e Docker Compose para fluxos de trabalho de desenvolvimento e produção contínuos.

### Tecnologias Utilizadas

- **Containerização:** Docker, Docker Compose
- **Backend:** Django, Django Rest Framework, Gunicorn
- **Frontend:** React, Vite, TailwindCSS
- **Banco de Dados:** PostgreSQL
- **Cache:** Redis
- **Tarefas Assíncronas:** Celery (com Celery Beat para agendamento)
- **Servidor Web (Produção):** Nginx

### Estrutura do Projeto

```
.
├── backend/            # Código-fonte da aplicação Django
├── frontend/           # Código-fonte da aplicação React + Vite
├── mobile/             # Espaço reservado para uma aplicação móvel
├── .env.example        # Exemplo de variáveis de ambiente
├── docker-compose.yml  # Docker Compose para desenvolvimento
└── docker-compose.prod.yml # Docker Compose para produção
```

## Guia de Configuração e Uso do Projeto

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

#### 4. Configurar o TailwindCSS

Após instalar as dependências, você precisa configurar o Vite e seu arquivo CSS principal para usar o Tailwind.

1.  **Configure o Plugin do Vite:**
    Modifique o arquivo `frontend/vite.config.js` para incluir o plugin `@tailwindcss/vite`.

    ```javascript
    import { defineConfig } from "vite";
    import react from "@vitejs/plugin-react";
    import tailwindcss from "@tailwindcss/vite";

    // https://vite.dev/config/
    export default defineConfig({
      plugins: [react(), tailwindcss()],
    });
    ```

2.  **Importe os Estilos do Tailwind:**
    Substitua o conteúdo de `frontend/src/index.css` pela seguinte linha para importar os estilos base do Tailwind.

    ```css
    @import "tailwindcss";
    ```

3.  **(Opcional) Teste com um Componente de Exemplo:**
    Para verificar se o Tailwind está funcionando, você pode substituir o conteúdo de `frontend/src/App.jsx` por este componente de exemplo que utiliza as classes utilitárias do Tailwind.

    ```jsx
    function App() {
      return (
        <div className="min-h-screen bg-gray-900 text-white flex flex-col items-center justify-center font-sans">
          <header className="text-center">
            <h1 className="text-5xl font-bold text-cyan-400 mb-4">
              Vite + React + TailwindCSS
            </h1>
            <p className="text-lg text-gray-400">
              Seu ambiente de frontend está pronto.
            </p>
          </header>
        </div>
      );
    }

    export default App;
    ```

#### 5. Executando em Modo de Desenvolvimento

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
