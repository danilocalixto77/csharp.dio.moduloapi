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
      - Inserir entre colchetes acima da declaração da classe:
        - [ApiController]
        - [Route("controller")]
      - Controller criada, agora deve-se criar um método para que ela execute alguma ação.  	
        - Exemplo: ObterDataHora
        - Declarar entre colchetes acima do método o endpoint que chamará este método na url da API.
        - [HttpGet("ObterDataHoraAtual")] 
        - É uma local de acesso para a API.

---

# Trabalhando com Entity Framework com C#

- Instalando pacotes
  - Para o funcionamento do Entity Framework devemos fazer a seguinte instalações a adições abaixo em nosso projeto. Algumas necessitam somente uma unica fez ser adicionada em nossa IDE/Projeto. Outras necessitam ser para cada projeto criado.
    - Instalação/add única:
      - dotnet tool install --global dotnet-ef
    - Instalação/add por projeto:
      - dotnet add packageMicrosoft.EntityFrameworkCore.Design
      - dotnet add packageMicrosoft.EntityFrameworkCore.SqlServer   
    - Para checar a instalação dos pacotes, ir no arquivo .csproj.
- Criando a classe entidade
  - Criar uma pasta para a classes:
    - Exemplo : Entityes e uma classe Contatos com os atributos Id,Nome,Telefone,Ativo.
- Criando o Contexto
  - Criar uma pasta e classe de Contexto.
    - Exemplo : Context e uma classe AgendaContext
    - Criar classe construtor para conexão:
      - public AgendaContext(DbContextOptions<AgendaContext> options) : base(options)
        {
        }

    - Inserir uma propriedade que refere a entidade.  
      - public DbSet<Contato> Contatos { get; set; }

- Configurando a conexão
  - Arquivo JSON. Criar strings de conexão:
    - appsettings.Development.json (Configurações para modo desenvolvimento)
    - appsettings.json (Configurações para modo produção)
    - Exemplo de conexão para banco de dados SQL local:
      - "ConnectionStrings": {"ConexaoPadrao": "Server=localhost\\SQLEXPRESSInitial Catalog=Agenda;Integrated Security=True"}
     - Caso ocorra erro de SSL insira ao final da string: TrustServerCertificate=True	
     - "ConnectionStrings": {"ConexaoPadrao": "Server=localhost\\SQLEXPRESS; Initial Catalog=Agenda;Integrated Security=True; TrustServerCertificate=True"}

  - Arquivo Program.cs. Informar a string de conexão:
    - Iniciar com os imports:
      - using ModuloAPI.Context;
      - using Microsoft.EntityFrameworkCore;
    - Abaixo da área comentada inserir a conexão:
    - // Add services to the container.
    - builder.Services.AddDbContext<AgendaContext>(options =>
  options.UseSqlServer(builder.Configuration.GetConnectionString("ConexaoPadrao")));

- Entendendo as migrations
  - Migrations são mapeamentos que o Entity faz das classes para a criação das entidades. E esse mapeamento e criação de tabelas devem ser feitas de maneira antecipada, e este procedimento são as migrations.
  - Comando para criação da Migration:
  - dotnet-ef migrations add CriacaoTabelaContato
  - Após a execução será criada a pasta migrations caso ainda não exista, e criará as classes dentro dessa pasta que irá criar tabelas e campos. Entretanto até este momento as classes foram criadas, mas as entidades e atributos ainda não foram criadas na base de dados.
  - Para de fato a criação ser executada, deve-se executar o seguinte comando:
    - dotnet-ef database update

- Criando a controle e o endpoint de Create
  - Criar a classe dentro da pasta Controller:
    - ContatoController.cs
    - Desenvolvido o método de de inserção/criação(create)

- Criando o endpoint obter por ID
  - Inserir na classe ContatoController o método(endpoint) para consultar pelo id do contato.
  - [HttpGet("{id}")]

- Criando o endpoint de update
  - Inserir na classe ContatoController o método(endpoint) para atualizar os dados do contato.
  - [HttpPut("{id}")]

- Criando o endpoint de delete
  - Inserir na classe ContatoController o método(endpoint) para atualizar os deletar o contato.
  - [HttpDelete("{id}")]

- Criando o endpoint de obter por nome
  - Inserir na classe ContatoController o método(endpoint) para consultar pelo nome do contato.
  - [HttpGet("ObterPorNome")]

- Entendendo os verbos HTTP
  - POST 	: Create	Retornos: 201/404/409
    - Cria/Insere uma nova informação.
  - GET  	: Read		Retornos: 200/404
    - Obtem/Pesquisa/Lista alguma informação.
  - PUT  	: Update	Retornos: 405/200/409
    - Atualização de dados. Este verbo obriga atualizar/passar toda entidade.
  - PATCH 	: Update/Modify Retornos: 405/200/409
    - Atualização de dados. Este verbo permite atualizar toda atributos específicos da entidade.      - DELETE	: Delete        Retornos: 405/200/404
    - Apaga/Deleta um atributo/recurso.


      