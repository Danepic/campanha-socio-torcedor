## Projeto de Microsserviços(Campanha API e Socio Torcedor API)

### Configuração
Para configurar, clone esse repositório para usar o .yml do Docker-Compose e clone os repositórios das aplicações. Clonados na sua máquina, siga os passos a seguir.

### Repositórios
Sócio Torcedor API: https://github.com/Danepic/socio-torcedor
Campanha API: https://github.com/Danepic/campanha-api
Registry(Eureka): https://github.com/Danepic/registry

### Registry
A aplicação Registry, é reponsável por descobrir sistemas de um mesmo contexto e trabalhar em conjunto com eles, auxiliando em números de intancias,
load-balance e saúde das nossas APIs.

### Campanha API
Essa API fornece operações básicas de uma Campanha sobre algum time esportivo.

### Sócio Torcedor API
Essa API é para o cadastro do usuário e associação com campanhas, usando nossa API de camapnha.

### Estratégia usada na criação da Aplicação
As APIs de Campanha e Sócio Torcedor seguem o modelo de CONTROLLER/SERVICE/REPOSITORY que é o padrão mais usada em aplicações Spring Boot,
usando as dependencias do Spring Cloud com suporte do Netflix OSS, nossas APIs se registram no nosso sistema Registry(Eureka), se interagem
usando Feign, caso as APIs não estejam rodando, é ativado o "Fallback" com o Hystrix, e em caso de sobrecarregamento da instancia, é usado
o Load-Balance do Ribbon para controlar a sobrecarga.

#### Repository
As aplicações usam o banco H2, por ser extremamente facil e produtivo para ser usado em um exemplo, e ser substituivel por qualquer outro
banco relacional, tendo apenas que configurar o banco desejado.

Para o acesso ao banco em consultas, inserções e etc, as aplicações usam o framework QueryDSL.

#### Service
Na camada de serviço, está concentrado as regras de negócio, para proteger nossa aplicação da exposição das nossas entidades, usamos o conceito
de DTO, e para fazer esse mapeamento entre nosso objeto "Entidade" e nosso objeto "DTO" usamos o framework MapStruct.

#### Controller
Para controllar as requisições temos a camada de controller, que responde e consome em JSON, seguindo os padrões REST para as APIs.

### Rodar as Aplicações
As aplicações necessitam do Java 11, Maven e Docker instalados.

Cada aplicação tem um plugin para gerar imagens Docker, na raiz de cada repositório, use o comando:
```shell script
mvn clean package dockerfile:build
```

Feito isso, vai ser gerado imagens locais no seu Docker.

Para facilitar, na raiz de todo o ecosistema desses projetos, foi criado um arquivo de configuração do Docker-Compose. Para iniciar de fato
as aplicações, use o comando:
```shell script
docker-compose up
```

Após alguns segundos ele vai iniciar as 3 aplicações.

### Testando
Para usar a API de Campanha acesse: http://localhost:8082/swagger-ui.html
Para usar a API de Sócio Torcedor: http://localhost:8084/swagger-ui.html
Para acessar o sistema de Registry: http://localhost:8086

Nesses links voce vai ter todas as operações documentadas e poderá testar as aplicações.
