# Introdução as APIs com C#

## Introdução as API's

### Introdução
    
  Objetivo desenvolver uma API integrada com Entity Framework

### O que é uma API

  API : Application Programming Interface, é uma forma de comunicação entre programas. Um software que se comunica com um outro software. Ou seja faz uma integração entre sistemas.

  API de feriados

  Exeplos de API:

  - Nager.Date : https://date.nager.at
  - Dogs: https://dog.ceo/dog-api
  
### Documentação e retornos
    
  É a maneira na qual a API informa quais parâmetros que os endpoints da API necessitam para processar determinada informação bem como os respectivos retornos esperados. Tudo isso será definido na documentação. Possibilitando assim o consumo da API.

### Exemplo de uso
    
  Uma empresa de e-commerce emite uma nota, que dispara através de uma API a informação para transportadora, notificando que terá mercadorias com quantidade X de volumes e tal peso estimado para recolher da empresa de e-commerce.

  Uma API também pode ser consumida internamente em uma mesma empresa que necessita buscar e retornar informações entre sistemas de uma mesma empresa.

### Usando o Dog API
 
### Criando nossa API
    
  Criar uma pasta para o projeto:
  Exemplo: **c:\projetos\ModuloAPI**

  Criar o projeto:
  ```
  dotnet new webapi
  ```

  Executar API:
  ```
  dotnet watch run
  ```

  Obs: o "watch" na execução da API tem por função ficar observando a API e caso seja feita alguma mudança ele irá recompilar o projeto sem haver necessidade de pausar o mesmo.
 
  Por padrão o .NET ao criar uma API ele irá direcionar para o Swagger.
  
  **Swagger**: Uma ferramenta de documentação da API. 
   
  Entretanto isso não é uma obrigatoriedade para utilização da API a mesma pode ser consumida e aberta no navegador. Copiando o link do Swagger para outro navegador.

  Contudo temos uma API template padrão de quando se cria um projeto no dotnet.

  A seguir teremos o passo a passo para a criação da API.

### Criando a controller
    
  Ao criar a API pelo dotnet note que é criado uma pasta chamada **Controllers**.
 
  Dentro desta pasta irão ficar classes controllers.

  Essas pasta representa um agrupamento de classes, nas quais cada classe deverá ser especialista em determinada operação do sistema. Exemplo, de uma controller que irá controlar as previsões do tempo a WeatherForecastController.cs, uma controler para usuários seria UsuarioController.cs e assim por diante.

  E uma controller nada mais é do que uma classe que vai agrupar as requisições http. E  irá disponibilizar os endpoints da API.

  A nomenclatura para classe deverá ser: **NomeController.cs**	

  O **Nome** dado a controller fará parte da URL de acesso a API.

  - Herdar de **ControllerBase**
  - Sobre a declaração da classe: **[ApiController] e [Route("[controller]")]**

  A classe de uma controller deve herdar por padrão de: **ControllerBase**


  ```
  [ApiController]  
  [Route("[controller]")]
  public class UsuarioController : ControllerBase
  {

  }
  ```

  Em seguida declarar o uso : **using Microsoft.AspNetCore.Mvc**;

  Desta forma já temos a nossa controller, entretanto ela não faz nada, é necessário definir alguma função para ela, portanto, vamos criar um método dentro da nossa classe controller.

  Criando um método para retornar data e hora:

  ```
  [HttpGet("ObterDataHoraAtual")]
  public IActionResult ObterDataHora()
  {
    var obj = new
    {
      Data = DateTime.Now.ToLongDateString(),
      Hora = DateTime.Now.ToShortTimeString()
    };
    return Ok(obj);
  }
  ```

  Acima do método declara-se o endpoint: **[HttpGet("ObterDataHoraAtual")]**      
   
### Entendendo as rotas

  As rotas dos são criadas com a seguinte formação:

  1. O endereço de onde a API está sendo executada, que para o nosso exemplo é: **http://localhost**

  2. Vem a porta que está sendo utilizada: **:5220**

  3. O nome da controller para o nosso caso é a classe UsuarioController.cs, em que automaticamente a parte do nome Controller.cs em diante é ignorada, sendo colocada na URL somente a parte: **/Usuario**

  4. E por fim o nome do endpoint declarado no método HttpGet  [HttpGet("ObterDataHoraAtual")], que fica: **/ObterDataHoraAtual**

  E temos ao final a formação da URL abaixo:
	
  ```
  http://localhost:5220/Usuario/ObterDataHoraAtual
  ```

### Endpoint com parâmetro

  Segue exemplo abaixo:

  ```
  [HttpGet("Aprensentar/{nome}")]
  public IActionResult Apresentar(string nome)
  {
    var menssagem = $"Olá {nome} seja bem vindo!";
    return Ok(new { menssagem });
  }

  ```

### Encerramento

## Materiais de apoio e Questionário

### Materiais de apoio

### Certifique seu conhecimento

---
---
---

# Trabalhando com Entity Framework com C#

## Entity Frmework e CRUD

### Introdução

  O EF é um framework **ORM** (Object-Relational Mapping) criado para facilitar a integração com bancos de dados. Mapeando tabelas e gerando comando SQL de forma automática.

### Entendendo o CRUD

  **CRUD**: **C**REATE (Insert) **R**EAD (Select) **U**pdate (Update) **D**ELETE (Delete).

  Com o EF é possível criarmos uma classe no projeto C# e esta classe criada poderá ser uma tabela no banco de dado.
  Exemplo uma Classe: Contatos
  ```
  Contatos
  ----------------
  + Id: int
  + Nome: string
  + Telefone: string
  + Ativo: bool
  ----------------
  ```

  Com esta classe possibilita utilizarmos as quatro operações do CRUD.

### Instalando pacotes

  Para o funcionamento do Entity Framework devemos fazer a seguinte instalações a adições abaixo em nosso projeto. Algumas necessitam somente uma unica fez ser adicionada em nossa IDE/Projeto. Outras necessitam ser para cada projeto criado.

  Instalação **única** a nível global desta ferramenta, via prompt do terminal DOS ou VSCode:
  ```
  dotnet tool install --global dotnet-ef
  ```

  Instalação por **projeto** os pacotes a seguir devem ser instalados sempre que iniciado um novo projeto do EF via prompt do terminal DOS ou VSCode:
  ```
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

  ```
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

  Caso seja necessário incluir um pacote de versão difernte inserir ao final do comando 
  Exemplo:

  ```
  -- version 7.0.0
  ```

  ```
  dotnet add package Microsoft.EntityFrameworkCore.NOMEDOPACOTE -- version 7.0.0
  ```


  Os pacotes do EF são modulares para cada de é necessário instalar somente o pacote do bando que for trabalhar.

  Para checar a instalação dos pacotes, ir no arquivo .csproj.

### Criando a classe entidade

  Criar uma pasta específica para representar as classes que terão uma tabela a se relacionar no banco:
  1. Crie a pasta Entityes.

  ```
  ..\Entityes

  ```

  2. Crie a classe na qual terá uma tabela no BD.

  ```
  ..\Entityes\Contatos.cs
  ```
  Todas as classes que tiverem uma tabela vinculada ao banco deverá ficar dentro desta pasta.

  Criada a classe Contato.cs com os seguintes atributos:
  ```
  public class Contatos
  {
      public int Id { get; set; }
      public string Nome { get; set; }
      public string Telefone { get; set; }
      public bool Ativo { get; set; }
  }

  ```

### Criando o Contexto

  O Contexto representará o nome do nosso banco de dados. E também fará a conexão com o BD. Siga os passos a seguir:

  1. Crie a pasta Contexto ou Context. 

  ```
  ..\Context

  ```

  2. Crie a classe contexto que será o **nome** do BD. Para o nosso caso será **Agenda**. Logo a dentro da pasta Context deverá ser criado o arquivo AgendaContext.cs

  ```
  ..\Context\AgendaContext.cs
  ```

  Ao criar classe AgendaContext na sua declaração deverá ser colocado para herdar na declaração da classe : DbContext consequentemente fazer o import de: **Microsoft.EntityFrameworkCore**. Observe se o editor fez a importação caso não tenha feito certifique-se de fazer:

  ```
  using Microsoft.EntityFrameworkCore;

  namespace ModuloAPIb.Context
  {
      public class AgendaContext : DbContext
      {
         
      }
  }

  ```

  Nesta mesma classe Context deverá ser criado um construtor para definir a conexão com o nosso banco de dados. Inicalmente ficará vazio conforme exemplo abaixo:

  ```
  public class AgendaContext : DbContext
  {
      public AgendaContext(DbContextOptions<AgendaContext> options) : base(options)
      {

      }
  }
  ```

  Em seguida inserir uma propriedade abaixo do construtor que refere-se a entidade, para nosso caso entidade é uma classe que também estamos nos referindo a uma tabela no BD SQL Server.

  ```
  public class AgendaContext : DbContext
  {
      public AgendaContext(DbContextOptions<AgendaContext> options) : base(options)
      {

      }

      public DbSet<Contato> Contatos { get; set; }
  }

  ```

  O **DbSet<Contato>** pois está representando uma classe no programa e uma tabela no banco de dados desta forma sendo uma **entidade**.


### Configurando a conexão

  Arquivo JSON. Criar strings de conexão:

  JSON para modo desenvolvimento o arquivo: **appsettings.Development.json**

  JSON para modo produção o arquivo: **appsettings.json** 

  Exemplo de conexão para banco de dados SQL local:

  ```
  "ConnectionStrings": {
    "ConexaoPadrao": "Server=localhost\\SQLEXPRESSInitial Catalog=Agenda;Integrated Security=True"
  }
  ```

  Caso ocorra erro de SSL insira ao final da string: **TrustServerCertificate=True**

  ```	
  "ConnectionStrings": {
    "ConexaoPadrao": "Server=localhost\\SQLEXPRESS; Initial Catalog=Agenda;Integrated Security=True; TrustServerCertificate=True"
  }
  ```

  Após a conclusão da string de conexão, agora é o momento em que se deve informar ao programa para utiliza-la, que é feito da seguinte forma.

  1. Vá até ao arquivo: **Program.cs**

  2. Dentro do arquivo Program.cs faça as seguintes importações e declarações:

  Importar:

  ```
  using ModuloAPI.Context;
  using Microsoft.EntityFrameworkCore;
  ```

  3. Adicionar no começo do código podendo ser onde consta o comentário:

  ```
  // Add services to the container.
  ```

  O seguinte comando:

  ```
  builder.Services.AddDbContext<AgendaContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("ConexaoPadrao")));

  ```

  Após a conclusão destes passo estará configurado para acessar o banco.


### Entendendo as migrations
  
  Migrations são mapeamentos que o Entity faz das classes para a criação das entidades/tabelas. E esse mapeamento e criação de tabelas devem ser feitas de maneira antecipada, e este procedimento são as migrations.

  Verifique se o SQL está sendo executado e com o serviço funcionando. Após essa checagem executar o comando para criação da Migration:
  
  ```
  dotnet-ef migrations add CriacaoTabelaContato
  ```

  Após a execução será criada a pasta **Migrations** caso ainda não exista, e criará as classes dentro dessa pasta que irá criar tabelas e campos. Entretanto até este momento as classes foram criadas, mas as entidades e atributos ainda não foram criadas na base de dados.

  ```
  ..\Migrations
  ```

  Para de fato a criação ser aplicada no banco de dados, deve ser executar o seguinte comando:

  ```
  dotnet-ef database update
  ```
  
  Após o comando acima foi criado o banco de dados **Agenda** e a tabela **Contatos** com todos os campos e tipos de acordo com a classe Contato.cs. Tudo através do Entity Framework.


### Criando a controle e o endpoint de Create

  Criando classe Controller para contato, vá na pasta Controller e crie a classe chamada:

  ```
  ContatoController.cs
  ```

  Abaixo o a classe inicial com os comando que indicam uma classe Controller:
  1. [ApiController]
  
  2. [Route("[controller]")]

  3. public class ContatoController : **ControllerBase**

  4. Observar o import: **using Microsoft.AspNetCore.Mvc;**
  

  ```
  using Microsoft.AspNetCore.Mvc;

  [ApiController]
  [Route("[controller]")]
  public class ContatoController : ControllerBase
  {


  }
  ```

  Criando método Create, para iniciarmos inserindo dados na tabela contato.

  ```
  public class ContatoController : ControllerBase
  {

    public IActionResult Create(Contato contato)
    {

    }

  }
  ```

  Precisamos criar um constructor para receber o context.

  ```
  [ApiController]
  [Route("[controller]")]
  public class ContatoController : ControllerBase
  {

      private readonly AgendaContext _context;

      public ContatoController(AgendaContext context)
      {
          _context = context;
      }


      public IActionResult Create(Contato contato)
      {

      }

  }
  ```

  Implementado o método **Create**

  ```
  [ApiController]
  [Route("[controller]")]
  public class ContatoController : ControllerBase
  {

      private readonly AgendaContext _context;

      public ContatoController(AgendaContext context)
      {
          _context = context;
      }


      [HttpPost]
      public IActionResult Create(Contato contato)
      {
          _context.Add(contato);
          _context.SaveChanges();
          return Ok(contato);
      }

  }
  ```

  Executando o projeto C#, procure no Swagger a controller e o endpoint para testar o método de inserção de contatos:

  ```
  dotnet watch run
  ```












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


### Atribuição das Pastas no projeto:

  **Context**: armazena a classe responsável pela criação do contexto ou banco de dados.

  **Controllers**: armazena as classes controladoras que terão os atributos e métodos que serão responsáveis pela criação e também controle das ações e requisições a serem feitas pelos endpoints da API.

  **Migrations**: armazena os arquivos responsáveis pelas instruções SQL para o banco de dados, CRUD.



      
