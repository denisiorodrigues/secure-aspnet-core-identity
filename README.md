# Welcome!

This is the starter project for [Secure a .NET web app with the ASP.NET Core Identity framework](https://docs.microsoft.com/learn/modules/secure-aspnet-core-identity/). A completed version of the project is located on the `solution` branch.

# Configuration / Libraries

### Entityframework Core instalation
``` bash
dotnet tool install dotnet-ef --version 8.0.* --global
```

# Base de dados
Vamos utilizar o dokcer e criaremos a imagem com o comendo abaixo;

``` bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=@Passw0rd"  -p 1433:1433 --name sql1 --hostname 
sql1 -d mcr.microsoft.com/mssql/server:2019-latest
```

## Criar usu[ario
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' --name 'sql1' -p 1401:1433 -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```

```bash
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourStrong!Passw0rd>' -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong!Passw0rd>"'
```

```bash
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourNewStrong!Passw0rd>' -Q 'SELECT Name FROM sys.Databases'
```

```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=SuaSenhaForte123' -p 1433:1433 --name sqlserver -d mcr.microsoft.com/mssql/server:2019-latest
```

1. Verifique as configurações de autenticação
Certifique-se de que o SQL Server está configurado para aceitar autenticação SQL Server (em vez de apenas autenticação do Windows). Isso pode ser verificado e ajustado dentro do contêiner usando o comando sqlcmd ou via Azure Data Studio:
docker exec -it sqlserver /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'SuaSenhaForte123'

```bash
CREATE LOGIN NovoLogin WITH PASSWORD = 'NovaSenhaForte123';
GO
```

Criar o Usuário no Banco de Dados:

Agora, associe o login a um usuário em um banco de dados específico. Aqui vamos criar o usuário no banco de dados master, mas você pode escolher outro banco de dados se desejar:

```sql
USE master;
CREATE USER <NovoLogin> FOR LOGIN <NovoLogin>;
GO
```

Se você deseja criar o usuário em um banco de dados específico, substitua master pelo nome do banco de dados:

```sql
Copy code
USE <NomeDoBancoDeDados>;
CREATE USER <NovoLogin> FOR LOGIN <NovoLogin>;
GO
```
Atribuir Permissões ao Usuário:

Dependendo do que você deseja que o usuário faça, você pode atribuir diferentes permissões. Por exemplo, para torná-lo um db_owner no banco de dados:

```sql
Copy code
ALTER ROLE db_owner ADD MEMBER <NovoLogin>;
GO
```

Consultar usuários novos detro do container
```sql
SELECT name, type_desc, is_disabled 
FROM sys.server_principals
WHERE type_desc IN ('SQL_LOGIN', 'WINDOWS_LOGIN', 'WINDOWS_GROUP');
GO
```

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation and other content
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE](LICENSE) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.


