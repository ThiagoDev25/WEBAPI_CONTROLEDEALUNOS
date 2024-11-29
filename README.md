# 🎉 **Documentação da API .NET 8**

## 📌 **Sobre o Projeto**

Esta é uma API Web construída com .NET 8, utilizando Entity Framework para interação com o banco de dados e Injeção de Dependência para a gestão dos serviços e repositórios. Esta API foi projetada para integrar com um front-end de forma eficiente e escalável.

## 🔧 **Tecnologias Utilizadas**

- **.NET 8** 🚀
- **Entity Framework Core** 🛠️
- **Injeção de Dependência** 🔄
- **Pacotes NuGet** 📦

## 🛠️ **Instalação e Configuração**

### 1. **Clonando o Repositório**

Para começar, clone o repositório em seu ambiente local:

bash
``git clone https://github.com/seuusuario/seurepositorio.git
cd seurepositorio``

### 2. Restaurar Pacotes NuGet
Depois de clonar o repositório, é necessário restaurar os pacotes NuGet. Execute o seguinte comando no terminal:

bash
Copiar código
``dotnet restore``  

### 3. Configurar Banco de Dados

#### 3.1 Conexão com o Banco de Dados

A API utiliza o Entity Framework Core para comunicação com o banco de dados. O arquivo appsettings.json contém a string de conexão necessária. Verifique se a sua string de conexão está configurada corretamente:

json
Copiar código
``{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=nome_do_banco;User Id=usuario;Password=senha;"
  }
}``
3.2 Aplicando as Migrações
Se for a primeira vez que está configurando a API, aplique as migrações para criar as tabelas no banco de dados:

bash
Copiar código
``dotnet ef database update``  
  
  ### 📁 Estrutura de Pastas
A estrutura de pastas do projeto é organizada da seguinte forma:

rust
Copiar código
/src
  /Controllers      -> Controladores da API
  /Models           -> Modelos de dados
  /Services         -> Serviços para lógica de negócio
  /Repositories     -> Repositórios que interagem com o banco de dados
  /Data             -> Contexto do Entity Framework e migrações
  /DTOs             -> Objetos de Transferência de Dados (DTOs)
  /Configurations   -> Configurações da API (Injeção de Dependência, Automapper, etc.)
  /Migrations       -> Migrações do Entity Framework

### 1. Registrando os Serviços
No arquivo Program.cs, os serviços são registrados dentro do método ConfigureServices:

csharp
Copiar código
public class Program
{
    public static void Main(string[] args)
    {
        var builder = WebApplication.CreateBuilder(args);

        // Configurações de banco de dados
        builder.Services.AddDbContext<ApplicationDbContext>(options =>
            options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

        // Injeção de dependência para serviços
        builder.Services.AddScoped<IUserService, UserService>();

        // Injeção de dependência para repositórios
        builder.Services.AddScoped<IUserRepository, UserRepository>();

        builder.Services.AddControllers();
        var app = builder.Build();

        // Configurações de pipeline HTTP
        app.MapControllers();
        app.Run();
    }
}
### 2. Usar um Banco de Dados Local ou em Docker (Se Você Precisa de Persistência)
Se você deseja testar com um banco de dados real mas não tem um configurado, você pode usar um banco de dados local (como SQL Server LocalDB) ou até mesmo um banco de dados em um container Docker.

Exemplo de SQL Server LocalDB:
O SQL Server LocalDB é uma versão leve do SQL Server que roda localmente, sem a necessidade de um servidor completo. Ele pode ser usado para testes locais e desenvolvimento.

Para configurar o LocalDB, você pode modificar a string de conexão para apontar para o LocalDB:

json
Copiar código
{
    "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=MeuBancoDeDados;Trusted_Connection=True;MultipleActiveResultSets=true;"
    }
}
Essa configuração conecta sua aplicação ao LocalDB, que é um banco de dados que roda localmente e é útil para desenvolvimento.

Usando Docker para um Banco de Dados:
Se você precisa de um banco de dados real e não quer instalar um servidor local, pode rodar o banco de dados dentro de um container Docker.

Instale o Docker (se ainda não tiver).

Execute um banco de dados em um container Docker, por exemplo, rodando um banco MySQL:

bash
Copiar código
docker run --name meu-mysql -e MYSQL_ROOT_PASSWORD=minhasenha -p 3306:3306 -d mysql:latest
Isso cria e executa um container MySQL, que pode ser acessado através da porta 3306.

Atualize a string de conexão no appsettings.json:

json
Copiar código
{
    "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Port=3306;Database=MeuBancoDeDados;User=root;Password=minhasenha;"
    }
}
Isso permite que você continue trabalhando com um banco de dados real, mas sem precisar instalar um servidor localmente.

### 3. Usar um Banco de Dados na Nuvem (Ex: Azure ou AWS)
Caso você tenha acesso a um banco de dados na nuvem, como o Azure SQL ou o Amazon RDS, você pode configurar sua string de conexão para se conectar a esses serviços. Isso pode ser uma boa opção para quando você não deseja configurar um banco de dados localmente, mas quer persistir dados na nuvem.

Para configurar um banco de dados na nuvem, você precisará de informações como:

Endereço do servidor de banco de dados na nuvem (exemplo: servidor.database.windows.net)
Nome do banco de dados
Usuário e senha de autenticação
Exemplo para Azure SQL:

json
Copiar código
{
    "ConnectionStrings": {
        "DefaultConnection": "Server=tcp:servidor.database.windows.net,1433;Initial Catalog=MeuBancoDeDados;Persist Security Info=False;User ID=usuario;Password=senha;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
    }
}
### 4. Usar Dados Fakes ou Mockar o Banco de Dados
Para testes, você também pode utilizar dados fakes ou mockar o banco de dados (simular a interação com o banco de dados).

Existem bibliotecas como Moq que permitem criar objetos falsos ou simular respostas do banco de dados. Isso é útil para testar lógica de aplicação sem realmente conectar a um banco de dados.

Por exemplo, você pode simular um repositório que retorna dados fictícios em vez de consultar um banco real.
