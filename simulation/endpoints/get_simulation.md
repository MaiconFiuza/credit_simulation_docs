# GET /simulation

> confira o endereço completo desse endpoint [aqui](../README.md#endpoints)

## Por que esse endpoint existe?

O endpoint de get simulation é reponsável por realizar uma simulação de crédito 

## Regras de negócio

<details>
  <summary><b>Para realizar um empréstimo é necessário ser maior de idade </b></summary>

O endpoint valida a idade no momento da simulação de crédito e caso o usuário seja menor de idade é retornado uma `exception` como resposta.
</details>

<details>
  <summary><b>A taxa de juros deve ser calculada de acordo com a faixa etária </b></summary>

 - Até 25 anos: 5% ao ano
 - De 26 a 40 anos: 3% ao ano
 - De 41 a 60 anos: 2% ao ano
 -  Acima de 60 anos: 4% ao ano

 </details>

## Requisitos técnicos

<details>
  <summary><b>A data de nascimento enviada na request deve estar no formato BR</b></summary>

O programa só aceita datas de nascimento enviadas no formato `dd/MM/yyyy` caso contrário é retornado uma `exception` do tipo `Bad_request`.
</details>

<details>
  <summary><b>Todos os pathParms são obrigatórios</b></summary>

Os dados de `birthdate`, `value` e `paymentTerm` são obrigatórios e são revisados na API em caso de ausência de algum deles é retornado um `Bad_request`.
</details>

## Schema

### Request

| nome    | valor            | descrição                                                       |
| ------- | ---------------- | --------------------------------------------------------------- |
| url     | `/simulation` | URL do endpoint relativo à URL base do serviço.                 |
| method  | `GET`           | -                                                               |
| queryParam  | `birthdate`           | Data de nascimento no formato `dd/MM/yyy` ex: `07/12/1995`                |
| queryParam  | `value`           | Valor que o cliente deseja emprestado em `Double`                |
| queryParam  | `paymentTerm`           | Número de meses(parcelas) que o cliente deseja realizar o pagamento do empréstimo                |



## Resposta

| nome        | valor      | descrição                                            |
| ----------- | ---------- | ---------------------------------------------------- |
| status code | 200        | -                                                    |
| body        | `Simulation` | Deve conter um objeto do tipo [Simulation](#simulation). |


<a name="simulation"></a> **Simulation**
| nome | tipo | descrição |
| ---------- | -------------- | -------------------------------------------------------------------------------------------------------- |
| `totalValue` | `String` | Valor total a ser pago formatado em reais ex: `R$ 53.906,07`  |
| `installmentValue` | `String` | Valor da parcela a ser pago mensalmente ex: `R$ 898,43`  |
| `totalInterest` | `String` | Total de juros pagos ex: `R$ 3.906,07`  |



### Possíveis erros

| status code | code                | message                                            | quando esse erro acontece?                                                                          |
| ----------- | ------------------- | -------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| 400         | `"BadRequestException"` | `""` | Quando algum dado de entrada é passado no formato errado ou não é enviado |
| 500         | `"InternalServerErrorException"`   | `"Um erro inesperado ocorreu. por favor tente novamente"`                                | Quando ocorrer um erro desconhecido na aplicação |     

## Exemplos

<details>
  <summary>Simulation</summary>

## Request

```shell
curl --location 'http://localhost:8080/simulation?birthdate=07%2F12%2F1995&value=50000&paymentTerm=60'
```

## Resposta

```json
{
    "totalValue": "R$ 53.906,07",
    "installmentValue": "R$ 898,43",
    "totalInterest": "R$ 3.906,07"
}
```
</details>
