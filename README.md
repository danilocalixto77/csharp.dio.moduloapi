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
  --version 7.0.0
  ```

  ```
  dotnet add package Microsoft.EntityFrameworkCore.NOMEDOPACOTE --version 7.0.0
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

  As migrations geram dois comandos um **Up** e um **Down** um executa o comando incialmente e o outro desfaz. Exemplo se no Up eu tiver um Create table, no Down teremos uma Drop table.


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

### Criando o endpoint obter por ID

  Criaremos o método para **PesquisarPorId** dentro da classe Controller, conforme método a seguir, este método representa a select, consulta, pesquisa, e o verbo utilizado é o GET e nesse verbo do endpoint será passado o parâmetro Id.

  ```
  [HttpGet("{id}")]
    public IActionResult PesquisaPorId(int id)
    {
        var contato = _context.Contatos.Find(id);

        if (contato == null)
            return NotFound();

        return Ok(contato);

  }
  ```

### Criando o endpoint de update

  Criamos o método para **Atualizar** dentro da classe Controller, esse método representa o update, na qual faz atualizações e alterações nos registros de contatos. O verbo utilizado é o PUT que também receberá um Id e mais as informações do contato que será alterado/atualizado.

  ```
  [HttpPut("{id}")]
  public IActionResult Atualizar(int id, Contato contato)
  {
      var contatoBD = _context.Contatos.Find(id);

      if (contatoBD == null)
          return NotFound();

      contatoBD.Nome = contato.Nome;
      contatoBD.Telefone = contato.Telefone;
      contatoBD.Ativo = contato.Ativo;

      _context.Contatos.Update(contatoBD);
      _context.SaveChanges();

      return Ok(contatoBD);
  }
  ```

### Criando o método para **Deletar** dentro da classe ContatoController, esse método representa o delete, no qual é responsável por apagar os registros. O verbo delete.

  ```
  [HttpDelete("{id}")]
  public IActionResult Deletar(int id)
  {
      var contatoBD = _context.Contatos.Find(id);

      if (contatoBD == null)
            return NotFound();

      _context.Contatos.Remove(contatoBD);
      _context.SaveChanges();

      return NoContent();
  }
  ```

### Criando o endpoint de obter por nome

  Agora mais um método para agora pesquisar por **Nome**. Semelhantemente ao por Id.

  ```
  [HttpGet("PesquisaPorNome")]
  public IActionResult PesquisaPorNome(string nome)
  {
    var contatos = _context.Contatos.Where(x => x.Nome.Contains(nome));
    return Ok(contatos);
  }
  ```

### Entendendo os verbos HTTP


  |Http Verb | CRUD        | Retornos    | 
  |----------|-------------|-------------|
  |POST      |Create       | 201/404/409 |
  |GET       |Read         | 200/404     |
  |PUT       |Update       | 405/200/409 |
  |PATCH     |Update/Modify| 405/200/409 |
  |DELETE    |Delete       | 405/200/404 |


### Recapitulando a construção da API

  1. Conexão: a string de conexão é escrita no arquivo **appsettings.Development.json**.

  2. Entidade: as entidade/tabelas são criadas a partir da pasta Entity com as classes nela existentes.

  3. Contexto: criando contexto na pasta **Context**, na qual contem a classe que herdará de **DbContext** e fará o acesso ao banco de dados. A partir dela chamaremos para ela para fazer o acesso ao banco.

  3.1 Propriedades: a classe de contexto possui várias propriedasde dentre elas a **DbSet** que a partir dela poderemos chamar uma entidade para acessá-la. 

  3.2 A classe da pasta entidade tem que está declarada dentro do Contexto para que o sistema possa identifica-la como uma tabela.

  4. Program.cs: aqui será declarado o **builder.Services** nesta declaração o AgendaContext passa o options recebendo a ConexaoPadrao configurada no .json. E aqui também diz qual o tipo do banco está sendo obtido.

  5. Migrations: executando os comando de migrations com todos os passos devidamente executados corretamente, neste momento as migrations se encarregarão de gerar os arquivos scripts sql automaticamente.

  6. Controllers: são o ponto de entrada por onde serão disponibilizados os métodos. Caracteristicas de uma controller. O NomeController.cs e a classe internamente irá herdar de **ControllerBase**. Terá os **[ApiController]** e **[Route("[controller]")]**. E via **constructor** recebe a classe de contexto que será atribuido internamente a uma variável **_conext = context;** isso se chama injeção de dependência.

  7. Endpoints: dentro da classe controller teremos vários métodos que retornam um **IActionResult** (Um retorno Http). E dentro da classe há os verbos **Post, Get, Put e Delete**. Para todos esses verbos a exceção do Get necessitará usar um _context.SaveChanges().

  8. Swagger: além de possibilitar testar a APi, também é uma ferramenta de documentação.

### Alterando o endpoint create
  
  Ajustando a forma de gravar com retorno da URL para pesquisa da informação gravada.

  ```
  [HttpPost]
  public IActionResult Create(Contato contato)
  {
      _context.Add(contato);
      _context.SaveChanges();
      //Modificando return do método
      //return Ok(contato);
      return CreatedAtAction(nameof(PesquisaPorId), new { id = contato.Id }, contato);
  }
  ```

### Atribuição das Pastas no projeto:

  **Context**: armazena a classe responsável pela criação do contexto ou banco de dados.

  **Controllers**: armazena as classes controladoras que terão os atributos e métodos que serão responsáveis pela criação e também controle das ações e requisições a serem feitas pelos endpoints da API.

  **Migrations**: armazena os arquivos responsáveis pelas instruções SQL para o banco de dados, CRUD.



      
