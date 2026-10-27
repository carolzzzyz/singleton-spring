# API de Produtos — CRUD e Singleton Logger

Projeto acadêmico em Java 17, Spring Boot, Spring Data JPA e banco H2. O recurso escolhido foi `Produto`.

## Requisitos
- Java JDK 17 ou superior
- Maven 3.6 ou superior
- IDE Java (IntelliJ IDEA, Eclipse ou VS Code)

## Como executar
Na pasta do projeto, execute:

```bash
mvn spring-boot:run
```

A API ficará disponível em `http://localhost:8080`. O banco H2 usa arquivo local em `./data`, para manter os dados entre reinicializações. O arquivo `app.log` é criado na pasta em que a aplicação é iniciada.

## Rotas
| Método | Rota | Resposta esperada |
|---|---|---|
| POST | `/produtos` | `201 Created` |
| GET | `/produtos` | `200 OK` |
| GET | `/produtos/{id}` | `200 OK` ou `404 Not Found` |
| PUT | `/produtos/{id}` | `200 OK` ou `404 Not Found` |
| DELETE | `/produtos/{id}` | `204 No Content` ou `404 Not Found` |

### Exemplo de cadastro
Envie um `POST` para `http://localhost:8080/produtos` com o corpo JSON:

```json
{
  "nome": "Teclado",
  "preco": 120.5,
  "quantidade": 10
}
```

### Exemplo de atualização
Envie um `PUT` para `http://localhost:8080/produtos/1`:

```json
{
  "nome": "Teclado mecânico",
  "preco": 199.9,
  "quantidade": 8
}
```

### Teste rápido com curl
```bash
curl -i -X POST http://localhost:8080/produtos -H "Content-Type: application/json" -d "{\"nome\":\"Teclado\",\"preco\":120.5,\"quantidade\":10}"
curl -i http://localhost:8080/produtos
curl -i http://localhost:8080/produtos/1
curl -i -X PUT http://localhost:8080/produtos/1 -H "Content-Type: application/json" -d "{\"nome\":\"Teclado mecânico\",\"preco\":199.9,\"quantidade\":8}"
curl -i -X DELETE http://localhost:8080/produtos/1
```

## Logger Singleton clássico
A versão final usa `AppLogger`, que possui construtor privado, `getInstance()` com inicialização lazy e thread-safe usando `volatile`, `synchronized` e dupla checagem. Os registros são gravados no console e em `app.log`, com os níveis DEBUG, INFO e ERRO.

`AppLoggerSpring` é uma cópia alternativa anotada com `@Component`, incluída apenas para comparação. Ela não é usada na versão final.

## Respostas para a comparação
**Quem garante a instância única em cada versão?**  
No Singleton clássico, a própria classe controla a criação da instância por meio do construtor privado e do método `getInstance()`. Na versão Spring, o contêiner gerencia a instância do componente dentro do contexto da aplicação, usando o escopo singleton por padrão.

**Quando o Singleton clássico ainda faz sentido?**  
Ele faz sentido quando é necessário controlar manualmente uma instância e utilizar a classe sem depender de um framework. Em aplicações Spring, a injeção de dependências geralmente facilita testes, manutenção e substituição de implementações.

## Justificativa da versão final
Foi escolhido o Singleton clássico porque o objetivo do exercício é demonstrar a implementação manual do padrão, incluindo inicialização lazy e segurança para múltiplas threads. A versão Spring foi mantida separadamente para comparação, mas não é utilizada junto com o logger clássico.

## Observações
- O arquivo `app.log` incluído é um exemplo ilustrativo; a execução real acrescentará novos registros.
- Não são registrados dados sensíveis.
- O tratamento global de exceções evita retornar detalhes internos da aplicação ao cliente.
