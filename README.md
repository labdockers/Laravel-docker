[INSTAGRAM](https://www.instagram.com/wesllycode/)
[LINKEDIN](https://www.linkedin.com/in/weslly-sousa-a0bb2647/)
[DEV.TO](https://dev.to/wesllycode)
[TWITTER](https://twitter.com/wesllycode)

# SOBRE O PROJETO
- Uma estrutura completa para criar um ambiente no docker para se trabalhar com Laravel. 
- Essa estrutura inicial é excelente para desenvolver uma SPA com Laravel ou ambiente para API em localhost

# ESTRUTURA
 * [Laravel](https://laravel.com)
 * [nginx:latest](https://hub.docker.com/_/nginx) - [versions](https://nginx.org/en/CHANGES)
 * [php:8.3-fpm](https://hub.docker.com/_/php)
 * [redis:latest](https://hub.docker.com/_/redis)
 * [phpmyadmin:latest](https://hub.docker.com/_/phpmyadmin)
 * [Mysql 8.0](https://hub.docker.com/_/mysql)
 * [Node 20 LTS](https://github.com/nodesource/distributions#debmanual)


### AMBIENTE LOCAL COM DOCKER
- Para rodar o projeto, é necessário ter o Docker instalado na máquina.
- Para rodar o projeto, é ncessário ter o Docker Compose instalado na máquina.
- Faça o clone do projeto do seu projeto
- Uma vez que você fez o download do projeto, copie a pasta do seu projeto existente para dentro da pasta **workspace** que dentro tem a pasta code, ao copiar
  delete a pasta code e renomeie a pasta do seu projeto para **code**.

- Execute o comando abaixo para subir o ambiente do docker.

```bash
docker compose build --no-cache
```

- Após o comando acima, execute o comando abaixo só depois que os volumes tiver montados.

```bash
docker compose exec nomedoseucontainer composer install
docker compose exec nomedoseucontainer cp .env.example .env
docker compose exec nomedoseucontainer php artisan key:generate
docker compose exec nomedoseucontainer npm install
docker compose exec nomedoseucontainer npm run dev
docker compose exec nomedoseucontainer php artisan storage:link
```


## SOBRE O BANCO DE DADOS
Ao iniciar aplicação usando docker compose up o banco de dados pode dar erro ao criar pasta hot-data precisar dar permissão chmod -R 777 nesta pasta hot-data.
