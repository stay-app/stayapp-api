FORMAT: 1A
HOST: https://api.stayapp.com.br

# StayApp

A API do StayApp disponibiliza uma forma de capturar os seus clientes
e acessar informações dos seus cards através de uma API no padrão REST.

## Endpoints

https://api.stayapp.com.br/


## Cards [/integration/tickets/]
Acessa todos os cards.


### Listar cards [GET]

+ request

    + Headers
    
            Token: Your token
            Content-type: application/json
     

+ Response 200 (application/json)
    
    
    + Body
    
            {
                "-Kv-QuesVKb5jH8SZ0lI": {
                    "description": "Descricao",
                    "franchise_id": "-L2L8rcJciDRLu5KcrR0",
                    "is_buy_value_enabled": false,
                    "is_canceled": true,
                    "is_cod_enabled": false,
                    "is_public": true,
                    "name": "Financeira",
                    "needs_approval": false,
                    "stamp_objective": 1000,
                    "stamp_type": "PERCENT"
                },
                "-Kv2Y43Py5E5q4Yi1nYJ": {
                    "company_id": "xyz",
                    "description": "A cada 10 pratos, 1 é por nossa conta.",
                    "image_path": "images/tickets/339ff8ba-a8f6-1ebe-e7c9-c11a6a4a1e16.jpeg",
                    "is_buy_value_enabled": true,
                    "is_canceled": false,
                    "is_cod_enabled": true,
                    "is_public": true,
                    "name": "10 pratos!",
                    "needs_approval": false,
                    "stamp_objective": 10,
                    "stamp_type": "COUNT"
                }
            }
    

+ Response 400 (application/json)

    + Body
    
            {
                "error": "token-invalid/token-not-found"
            }
     
     
## Captura de clientes [/integration/addStamp]

Endpoint para registro de captura de clientes

### Registrar uma captura de cliente (Fidelizar) [POST]

- number (number) - Número do cliente a ser fidelizado (DDI + DDD + numero).
- amount (number) - Valor da fidelização (Ex: 1 unidade, 1 selo ou 100 reais).
- ticket_id (string) - Identificador do card.
- buy_value (string, optional) - Valor da compra no estabelecimento.
- cod_desc (string, optional) -  Campo livre para o estabelecimento informar adicionar qualquer informação adicional da captura.
- alt_birthday (string, optional) - Data de nascimento no padrão YYYY-MM-DD.
- gender (string, optional) - Sexo do cliente, pode ser MALE, FEMALE ou OTHER.
- nickname (string, optional) - Nome do cliente.
- custom_segmentations (array, optional) - Array de identificadores de segmentação customizada.


+ request

    + Headers
    
            Token: Your token
            Content-type: application/x-www-form-urlencoded
    
    + Body
    
            {
                "number": "5562999999999",
                "amount": 10,
                "ticket_id": "-LFKA45HI3I3H5511",
                "buy_value": 49.99,
                "cod_desc": "mesa 22",
                "alt_birthday": "1990-03-13",
                "gender": "FEMALE",
                "nickname": "Jeniffer"
            }
          

+ Response 200 (application/json)

    Captura realizada com sucesso! Caso os dados opcionais alt_birthday,
    gender ou nickname sejam registrados incorretamente será mostrado seus respectivos erros, porém
    a captura prosseguirá sem inserção desses atributos opcionais:
    
    + Body

            {
                "stamp_id": "-LWkHkx6bZDvA1Z_59jy"
            }
            

+ Response 400 (application/json)

    Dados obrigatórios invalidos, ou Token de integração invalido, ou não informado no header:
    
    + Body

            {
                "error": "token-not-found/token-invalid/invalid-parameters"
            }
  
## Segmentações personalizadas [/integration/getCustomSegmentations{?is_canceled}]

Endpoint para consulta de segmentações personalizadas

+ Parameters

    + is_canceled: false (boolean, optional) - Filtrar pelo status da segmentação personalizada.
        + Default: `false`
          
### Consulta de segmentações personalizadas [GET]

+ request

    + Headers
    
            Token: Your token
            Content-type: application/json
            
+ Response 200 (application/json)
    
    + Body

            [
                {
                    "id": "-LXKYWPry1SUp7om20ts",
                    "name": "Comprou Sorvete",
                    "is_canceled": false
                },
                {
                    "id": "-LXKuvQjj7Cq_kFysUzF",
                    "name": "Campanha 2019",
                    "is_canceled": false
                },
                {
                    "id": "-LXsfZGeMYtrgo1ATTan",
                    "name": "Comprou via Delivery",
                    "is_canceled": true
                }
            ]
