# Padrão de criação de Bots do telegram para Goiás : Implementações de Referência utilizando a tecnologia Java

## Documentação
[API Telegram 2.0]	
[Guia de implementação](https://github.com/goias/telegram-goias/blob/master/telegram-guide-doc/guide.odt?raw=true)
		
## Samples

### Java		

#### Principais Tecnologias

SpringBoot, Java 8, javaslang, Lombok, Telegram-api

#### SpringBoot

SpringBoot...

#### Telegram-api 2.0

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

