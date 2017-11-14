---
title: Order API
category: Pedidos
order: 5
---

Pedidos são criados a partir do checkout de um carrinho de compras. Um pedido é composto por ao menos um item de pedido.  

<a name="attributes"></a>

### Propriedades do Objeto  

|-----------------+------------|
| Nome | Descrição |
|-----------------|:-----------|
| Id | Identificador do pedido (gerado automaticamente). |
| CheckoutId | Identificador do checkout (gerado automaticamente). Vários pedidos podem estar associados ao mesmo checkout. |
| CreatedOn | Data da criação. |
| UpdatedOn | Data da última atualização. |
| Status | Status atual do pedido. Valores possíveis são: 0 (criado), 1 (pagamento autorizado), 2 (pagamento capturado), 5 (enviado), 10 (concluído), 20 (pagamento negado), 21 (cancelado pelo lojista) e 22 (cancelado pelo usuário). |
| MerchantId | Identificador único da loja. |
| MerchantName | Nome da loja (ou do vendedor). |
| Customer | Dados do comprador (nome, email, cpf, id do usuário e endereço IP). |
| Shipping | Dados do endereço de entrega. |
| PaymentMethod | Dados do meio de pagamento. |
| Currency | Código ISO 4217 da moeda utilizada no pedido. | 
| Amount | Valor em centavos do total do pedido. | 
| Device | Identificador do dispositivo acionador. Só é utilizado quando o pedido for gerado a partir de um dispositivo IoT. |
| OrderItems | Coleção de itens do pedido. Um item de pedido é composto por SKU, preço e quantidade. |
|-----------------+------------|


``` json-doc
// -------------------------
// Exemplo do objeto Pedido
// -------------------------

{
    "Id": 100,
    "CheckoutId": 500,
    "CreatedOn": "2017-09-10T12:03:45.5796387",
    "UpdatedOn": "2017-09-11T01:55:15.9133333",
    "Status": {
        "Code": 0,
        "Label": "Created"
    },
    "MerchantId": "99999999-9999-9999-9999-999999999999",
    "MerchantName": "BlackBox Store",
    "Customer": {
        "UserId": "99999999-9999-9999-9999-000000000000",
        "Name": "Comprador",
        "Email": "comprador@blackbox.com",
        "LegalId": "096.296.777-99",
        "IpAddress": "127.0.0.1"
    },
    "Shipping": {
        "Type": 0,
        "Address": {
            "Alias": "Casa",
            "Street": "Av. Marechal Câmara",
            "Number": "160",
            "Complement": "Sala 934",
            "District": "Centro",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "ZipCode": "99999-999",
            "Country": "Brasil"
        }
    },
    "PaymentMethod": {
        "Id": 500,
        "Type": 0,
        "Data": {
            "MaskedValue": "100010******1000",
            "Brand": "visa",
            "ValidThru": "09/2020",
            "Token": "C70A7B33-9999-9999-9999-000808008888"
        }
    },
    "Currency": "BRL",
    "Amount": 9000,
    "Device": {
        "Id": "88888888-8888-8888-8888-000000000000"
    },
    "OrderItems": [
        {
            "Id": 10,
            "ProductId": 1,
            "SkuId": 9999,
            "SkuDescription": "Descrição do SKU #1 para Produto de Teste #1", 
            "Sku": "PROD9999",
            "Amount": 9000,
            "Quantity": 2,
            "Status": 0
        }
    ]
}
```
{: title="Pedido"}

<a name="http_operations"></a>

### Operações HTTP  

`GET`{:.http-get} [/api/order/sent/{orderId}](#getbyid_order_sent)  
Obtenção de um pedido enviado pelo comprador através do seu Id. 

`GET`{:.http-get} [/api/order/sent](#get_order_sent)  
Consulta dos pedidos enviados pelo comprador.  

`GET`{:.http-get} [/api/order/received/{orderId}](#getbyid_order_received)  
Obtenção de um pedido recebido pelo vendedor através do seu Id. 

`GET`{:.http-get} [/api/order/received](#get_order_received)  
Consulta dos pedidos recebidos pelo vendedor.  

  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>

<a name="getbyid_order_sent"></a>

### **GET** /api/order/sent/*{orderId}*  

**DESCRIÇÃO:**  
Obtenção de um pedido enviado pelo comprador através do seu Id. O Id é gerado automaticamente quando o pedido é criado, como resultado de uma operação de checkout.  

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| orderId | Identificador do pedido | `Number` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
GET /api/order/sent/{orderId} HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```

``` json-doc
{ 
    // objeto
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="get_order_sent"></a>

### **GET** /api/order/sent  

**DESCRIÇÃO:**  
Consulta dos pedidos enviados pelo comprador. Permite a aplicação de diversos filtros de pesquisa para maior controle sobre os resultados obtidos.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| pageIndex | Índice para paginação (default: 1) | `Number` `opcional` |
| pageSize | Quantidade de registros retornados (default: 25) | `Number` `opcional` |
| status | Filtro por status do pedido | `Number` `opcional` |
| id | Filtro por Id do pedido | `Number` `opcional` |
| checkout | Filtro por Id do checkout | `Number` `opcional` |
| order | Ordenação ascendente (asc) ou descentende (desc) dos registros retornados (default: asc) | `String` `opcional` |
| startDate | Filtro por data inicial no formato "dd-MM-yyyy" | `String` `opcional` |
| endDate | Filtro por data final no formato "dd-MM-yyyy" | `String` `opcional` |
|-----------------+------------+----------------|


**REQUEST:**  

Sem parâmetros de consulta:

``` http
GET /api/order/sent HTTP/1.1
Content-Type: application/json
```

Com parâmetros de consulta:

``` http
GET /api/order/sent/?pageIndex=1&pageSize=25&status=0&checkout=500&order=desc&startDate=01-06-2017&endDate=10-06-2017 HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```

``` json-doc
{
    "Total": 100,
    "PageSize": 25,
    "PageIndex": 1,
    "Data": [
        { ... },
        { ... },
        { ... }
    ]
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="getbyid_order_received"></a>

### **GET** /api/order/received/*{orderId}*  

**DESCRIÇÃO:**  
Obtenção de um pedido recebido pelo vendedor através do seu Id. 

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| orderId | Identificador do pedido | `Number` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
GET /api/order/received/{orderId} HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```

``` json-doc
{ 
    // objeto
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="get_order_received"></a>

### **GET** /api/order/received  

**DESCRIÇÃO:**  
Consulta dos pedidos recebidos pelo vendedor. Permite a aplicação de diversos filtros de pesquisa para maior controle sobre os resultados obtidos.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| pageIndex | Índice para paginação (default: 1) | `Number` `opcional` |
| pageSize | Quantidade de registros retornados (default: 25) | `Number` `opcional` |
| status | Filtro por status do pedido | `Number` `opcional` |
| merchantId | Filtro para Identificador de loja | `String` `36` `opcional` |
| checkout | Filtro por Id do checkout | `Number` `opcional` |
| order | Ordenação ascendente (asc) ou descentende (desc) dos registros retornados (default: asc) | `String` `opcional` |
| startDate | Filtro por data inicial no formato "dd-MM-yyyy" | `String` `opcional` |
| endDate | Filtro por data final no formato "dd-MM-yyyy" | `String` `opcional` |
|-----------------+------------+----------------|


**REQUEST:**  

Sem parâmetros de consulta:

``` http
GET /api/order/received HTTP/1.1
Content-Type: application/json
```

Com parâmetros de consulta:

``` http
GET /api/order/received/?pageIndex=1&pageSize=25&merchantId=99999999-9999-9999-9999-999999999999&status=0&order=desc&startDate=01-06-2017&endDate=10-06-2017 HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```

``` json-doc
{
    "Total": 100,
    "PageSize": 25,
    "PageIndex": 1,
    "Data": [
        { ... },
        { ... },
        { ... }
    ]
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  


----------------