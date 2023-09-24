# Introdução as APIs com C#

- Criando nossa API
  - Criar uma pasta para o projeto:
    - Exemplo: c:\projetos\ModuloAPI
  - Criar o projeto:
    - dotnet new webapi
  - Executar API:
    - dotnet watch run
    - Obs: o "watch" na execução da API tem por função ficar observando a API e caso seja feita alguma mudança ele irá recompilar o projeto sem haver necessidade de pausar o mesmo.
  - Por padrão o .NET ao criar uma API ele irá direcionar para o Swagger.
  - Swagger: Uma ferramenta de documentação da API. 
  - Entretanto isso não é uma obrigatoriedade para utilização da API a mesma pode ser consumida e aberta no navegador. Copiando o link do Swagger para outro navegador.

- Criando a Controller
  - A Controller é uma classe que vai agrupar suas requisições http e disponibilizar os endpoints.
  - Nesta classe deve-se agrupar classes e domínio em comum. 
  - Exemplo: classe usuario
    - Tudo relacionado ao usuário será feito dentro desta classe, pesquisr por id, nome, alterar, apagar.
    - Criando uma UsuarioController.cs
      - Cria-se uma nova classe como o nome UsuarioController.cs
      - Esta classe irá herdar de ControllerBase
      - Importar: using Microsoft.AspNetCore.Mvc;
      - Inserir entre couchetes acima da declaração da classe:
        - [ApiController]
        - [Route("controller")]
      - Controller criada, agora deve-se criar um método para que ela execute alguma ação.
