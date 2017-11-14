---
title: Cart API
category: Carrinho de Compras
order: 1
---

Carrinho
O carrinho de compras permite ao usuário adicionar os itens desejados em uma compra, e é composto por uma coleção de itens de carrinho. 

Um [item de carrinho](/cart/cartitem/) agrega as informações do SKU, sua quantidade, preço e o método de envio. 

<a name="attributes"></a>

### Propriedades do Objeto  

|-----------------+------------|
| Nome | Descrição |
|-----------------|:-----------|
| Id | Identificador único do carrinho (gerado automaticamente). |
| CreatedOn | Data da criação. |
| UpdatedOn | Data da última atualização. |
| UserId | Identificador do usuário proprietário do carrinho. |
| Type | Tipo do carrinho. Valores possíveis são: 0 (privado), 1 (público) e 2 (oferta do vendedor). |
| MerchantId | Identificador da loja. Só é utilizado quando o tipo de carrinho for uma oferta do vendedor (Type = 2). |
| Label | Nome do carrinho. |
| CouponCode | Código de cupom de desconto. |
| CartItems | Coleção de Itens de Carrinho. |
|-----------------+------------|


``` json-doc
// --------------------------------------
// Exemplo do objeto Carrinho de Compras
// --------------------------------------

{
  "Id": 999,
  "CreatedOn": "2017-02-11T14:10:39.1083643",
  "UpdatedOn": "2017-02-11T14:10:39.442996",
  "UserId": "7a6acd59-a7b1-4a7f-bc2d-0a3f6014cc4a",
  "Type": 2,
  "MerchantId": "99999999-9999-9999-9999-999999999999",
  "MerchantName": "BlackBox Store",
  "Label": "Exemplo de carrinho Oferta",
  "CartItems": [
    {
      "Id": 100,
      "IsActive": true,
      "ProductId": 9999,
      "ProductName": "Nome do produto",
      "ManufacturerName": "Retail Company Inc.",
      "Caption": "Uma breve descrição do produto",
      "MerchantId": "99999999-9999-9999-9999-999999999999",
      "MerchantName": "BlackBox Store",
      "Quantity": 10,
      "Price": {
        "Amount": 1000,
        "Currency": "BRL"
      },
      "Sku": {
        "Id": 332,
        "Sku": "SKU332",
        "Status": 0,
        "Description": "Descrição do SKU de Produto",
        "Images": [
            "https://www.myimages.com/image1.png",
            "https://www.myimages.com/image2.png",
            "https://www.myimages.com/image3.png"
          ],
        "Shipping": {
          "Type": 1,
          "Price": {
            "Amount": 500,
            "Currency": "BRL"
          }
        }
      }
    }
  ]
}
```
{: title="Carrinho de Compras"}

<a name="http_operations"></a>

### Operações HTTP  

`POST`{:.http-post} [/api/cart](#post_cart)  
Criação de um novo carrinho.  

`PUT`{:.http-put} [/api/cart/{cartId}](#put_cart)  
Atualização do carrinho existente.  

`GET`{:.http-get} [/api/cart/{cartId}](#getbyid_cart)  
Obtenção de um carrinho existente através do seu Id.

`GET`{:.http-get} [/api/cart](#get_cart)  
Consulta dos carrinhos existentes.  

  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>

<a name="post_cart"></a>

### **POST** /api/cart/  

**DESCRIÇÃO:**  
Criação de um novo carrinho de compras.

**PARÂMETROS JSON:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| Type | Tipo do carrinho: 0 (privado), 1 (público) e 2 (oferta do vendedor) | `Number` |
| Label | Nome do carrinho de compras | `String` `128`  |
| MerchantId | Identificador da loja. Obrigatório quando tipo do carrinho for uma oferta de vendedor (Type = 2), opcional caso contrário | `String` `36` `opcional` |
| CouponCode | Código de cupom de desconto | `String` `36` `opcional` |
| CartItems | Array de objetos representando itens existentes no carrinho | `Object` `opcional` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
POST /api/cart/ HTTP/1.1
Content-Type: application/json
```

``` json-doc
{
  "Type": 0, // Carrinho privado
  "Label": "Meu Carrinho de Compras",
  "CouponCode": "PROMOCODE", 
  "CartItems": [{
    "SkuId": 9999,
    "Quantity": 1
   }]
}
```

**RESPONSE:**  

``` http
HTTP/1.1 201 Created
Content-Type: application/json;charset=UTF-8
```
``` json
{
  "Id": 999,
  "Links": {
    "self": {
      "href": "/api/cart/999",
      "method": "GET"
    }
  }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="put_cart"></a>

### **PUT** /api/cart/*{cartId}*  
  
**DESCRIÇÃO:**  
Atualiza os atributos de um carrinho existente.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| cartId | Identificador do carrinho | `Number` |
|-----------------+------------+----------------|

**PARÂMETROS JSON:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| Type | Tipo do carrinho: 0 (privado), 1 (público) e 2 (oferta do vendedor) | `Number` |
| Label | Nome do carrinho de compras | `String` `128`  |
| MerchantId | Identificador da loja. Obrigatório quando tipo do carrinho for uma oferta de vendedor (Type = 2), opcional caso contrário | `String` `36` `opcional` |
| CouponCode | Código de cupom de desconto | `String` `36` `opcional` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
PUT /api/cart/{cartId} HTTP/1.1
Content-Type: application/json
```

``` json
{
  "Type": 1,
  "Label": "Meu novo Carrinho de Compras"
}
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
``` json
{
  "Id": 9999,
  "Links": {
    "self": {
      "href": "/api/product/9999",
      "method": "GET"
    }
  }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>

<a name="getbyid_cart"></a>

### **GET** /api/cart/*{cartId}*  

**DESCRIÇÃO:**  
Obtém um carrinho existente através do seu Id. O Id é gerado automaticamente quando o carrinho é criado.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| cartId | Identificador do carrinho | `Number` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
GET /api/cart/{cartId} HTTP/1.1
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
  

<a name="get_cart"></a>

### **GET** /api/cart/  

**DESCRIÇÃO:**  
Lista os carrinhos de compras existentes. Permite a aplicação de diversos filtros de pesquisa para maior controle sobre os resultados obtidos.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| pageIndex | Índice para paginação (default: 1) | `Number` `opcional` |
| pageSize | Quantidade de registros retornados (default: 25) | `Number` `opcional` |
| cartType | Filtro para tipo de carrinho | `Array[Number]` `opcional` |
| merchantId | Filtro para Identificador de loja | `String` `opcional` |
| order | Ordenação ascendente (asc) ou descentende (desc) dos registros retornados (default: asc) | `String` `opcional` |
| label | Filtro por nome do carrinho | `String` `opcional` |
| startDate | Filtro por data inicial no formato "dd-MM-yyyy" | `String` `opcional` |
| endDate | Filtro por data final no formato "dd-MM-yyyy" | `String` `opcional` |
|-----------------+------------+----------------|


**REQUEST:**  

Sem parâmetros de consulta:

``` http
GET /api/cart/ HTTP/1.1
Content-Type: application/json
```

Com parâmetros de consulta:

``` http
GET /api/cart/?pageIndex=1&pageSize=25&cartType=0&label=keyword&merchantId=99999999-9999-9999-9999-999999999999&order=desc&startDate=01-06-2017&endDate=10-06-2017 HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
``` json
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
