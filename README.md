# Padrão de criação de Bots do telegram do Estado de Goiás

# Implementações de Referência em Java

## Documentação
[API Telegram 2.0](https://core.telegram.org/bots/api).
		
#### Principais Tecnologias

SpringBoot, Java 8

#### SpringBoot
Uma das formas mais indicadas de implementar um bot é usando uma arquitetura de microserviço onde o Spring Boot se mostra como uma  uma das alternativas de framework seja 

O Spring Boot é um projeto da Spring que veio para facilitar o processo de configuração e publicação de nossas aplicações. A intenção é ter o seu projeto rodando o mais rápido possível e sem complicação.
Ele consegue isso favorecendo a convenção sobre a configuração. Basta que você diga pra ele quais módulos deseja utilizar (WEB, Template, Persistência, Segurança, etc.) que ele vai reconhecer e configurar.
Você escolhe os módulos que deseja através dos starters que inclui no pom.xml do seu projeto. Eles, basicamente, são dependências que agrupam outras dependências. Inclusive, como temos esse grupo de dependências representadas pelo starter, nosso pom.xml acaba por ficar mais organizado.
Apesar do Spring Boot, através da convenção, já deixar tudo configurado, nada impede que você crie as suas customizações caso sejam necessárias.
O maior benefício do Spring Boot é que ele nos deixa mais livres para pensarmos nas regras de negócio da nossa aplicação.


A implementação da especificação utilizada foi a especificão da API 2.0, que fornece um conjunto de métodos.

##### @API

```java
    @Path("/aluno")
    @Api(value = "Operações de CRUD em Aluno", description = "Operações CRUD de Aluno")
    public class AlunoCmds {
```

##### @ApiOperation

```java
    /**
     * Armazena um Aluno no sistema.
     *
     * @param aluno Informações do aluno
     */
    @PUT
    @Path("/salvar")
    @Consumes({MediaType.APPLICATION_JSON})
    @ApiOperation(value = "Armazena o registro do aluno.", notes = "Armazena o registro do aluno na base de dados.")
    @ApiResponses(value = {
            @ApiResponse(code = 200, message = "OK."),
            @ApiResponse(code = 500, message = "Erro interno.")
    })
    public Response salvar(Aluno aluno) {
        gov.goias.sistema.entidades.Aluno a = mapper.map(aluno, gov.goias.sistema.entidades.Aluno.class);
        alunoService.salvar(a);

        return Response.ok().build();
    }
```

##### @ApiModel e @ApiModelProperty

```java
@ApiModel(value = "Aluno", description = "Informações de aluno")
public class Aluno {

    @ApiModelProperty(value = "Identificador do aluno", name = "id", dataType = "integer")
    private Integer id;

    @ApiModelProperty(value = "Nome do aluno", name = "nome", dataType = "string")
    private String nome;
```

#### Padrão da Informação

Descrição dos tipos de dados e semântica utilizadas na comunicação entre os sistemas. As informações transmitidas entre os sistemas não devem utilizar máscaras em sua representação.

```
    "/aluno/{id}": {
      "get": {
        "tags": [
          "Query em Aluno"
        ],
        "summary": "Obtem o aluno.",
        "description": "Obtem o aluno a partir do ID.",
        "operationId": "consultar",
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "required": true,
            "type": "integer",
            "format": "int32"
          }
        ],
        "responses": {
          "200": {
            "description": "OK.",
            "schema": {
              "$ref": "#/definitions/Aluno"
            }
          },
          "404": {
            "description": "Aluno não encontrado."
          },
          "500": {
            "description": "Erro interno."
          }
        },
        "x-mask": {
          "nascimento": "dd/MM/yyyy"
        }
      }
    }
  }
```

#### Estrutura do Projeto: pin-goias

O projeto pin-goias está dividido em dois módulos: pin-dominio e pin-api.

##### pin-dominio
Contém as classes de serviços para acesso aos dados de domínio da aplicação.
Neste exemplo, os dados retornados estão armazenados em mapas no próprio código, simulando um acesso à base de dados.

##### pin-api
Contém as classes de definição dos serviços REST e sua documentação utilizando o Swagger 2.0.
Este módulo contém uma interface fornecida pelo framework Swagger para acesso à documentação dos serviços REST disponibilizados, além de fornecer uma API para testes dos serviços.

