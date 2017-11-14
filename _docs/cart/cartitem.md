---
title: Cart Item API
category: Carrinho de Compras
order: 2
---

Um item de carrinho é um [SKU](/catalog/sku/) que foi adicionado a um [carrinho de compras](/cart/cart/).  

<a name="attributes"></a>

### Propriedades do Objeto  

|-----------------+------------|
| Nome | Descrição |
|-----------------|:-----------|
| Id | Identificador do item de carrinho (gerado automaticamente). |
| IsActive | Flag que indica se item pertence a lista de itens ativos. |
| ProductId | Identificador do produto. |
| ProductName | Nome do produto. |
| MerchantId | Identificador único da loja que vende o produto. |
| MerchantName | Nome da loja que vende o produto. |
| Quantity | Quantidade do item colocado no carrinho. |
| Price | Preço de venda final do produto (pode ser diferente do preço do SKU). |
| SKU | Dados do SKU do produto. |
|-----------------+------------|


``` json-doc
// -----------------------------------
// Exemplo do objeto Item de Carrinho
// -----------------------------------

{
  "Id": 920,
  "IsActive": true,
  "ProductId": 999999,
  "ProductName": "Produto de Teste",
  "MerchantId": "612a64c0-22e7-4fb9-aa2b-eb31670f207f",
  "MerchantName": "BlackBox Store",
  "Quantity": 1,
  "Price": {
    "Amount": 9000,
    "Currency": "BRL"
  },
  "Sku": {
    "Id": 9999,
    "Sku": "PROD9999",
    "Status": 0,
    "Description": "Descrição do SKU",
    "Images": [
      "http://www.myimages.com/image1.png"
    ]
  }
}
```
{: title="Item de Carrinho"}

<a name="http_operations"></a>

### Operações HTTP  

`POST`{:.http-post} [/api/cart/{cartId}/items](#post_cartitem)  
Inclusão de item no carrinho.  

`PUT`{:.http-put} [/api/cart/{cartId}/items/{itemId}](#put_cartitem)  
Atualização dos dados de um item do carrinho.  

`DELETE`{:.http-delete} [/api/cart/{cartId}/items/{itemId}](#delete_cartitem)  
Remoção de item do carrinho.  

`GET`{:.http-get} [/api/cart/{cartId}/items/{itemId}](#getbyid_cartitem)  
Obtenção de item existente no carrinho através do seu Id.  

`GET`{:.http-get} [/api/cart/{cartId}/items/](#get_cartitem)  
Listagem dos itens existentes no carrinho.  

`GET`{:.http-get} [/api/cart/{cartId}/active](#get_active)  
Listagem dos itens ativos do carrinho.  

`GET`{:.http-get} [/api/cart/{cartId}/saved](#get_saved)  
Listagem dos itens salvos do carrinho.  

  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>

<a name="post_cartitem"></a>

### **POST** /api/cart/*{cartId}*/items  

**DESCRIÇÃO:**  
Inclusão de item no carrinho.

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
| SkuId | Identificador do SKU | `Number` |
| Quantity | Quantidade do SKU a ser incluída no carrinho | `Number` |
| Price.Amount | Preço do SKU no carrinho. Se não for informado, o preço já cadastrado no SKU é utilizado | `Number` `opcional` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
POST /api/cart/{cartId}/items HTTP/1.1
Content-Type: application/json
```

``` json-doc
{
    "SkuId": 999999,
    "Quantity": 1,
    // Price é opcional
    // Quando utilizado, substitui o preço cadastrado no SKU
    "Price": {
        "Amount": 100
    }
}
```

**RESPONSE:**  

``` http
HTTP/1.1 201 Created
Content-Type: application/json;charset=UTF-8
```
``` json
{
  "Id": 999999,
  "Links": {
    "self": {
      "href": "/api/cart/{cartId}/items/999999",
      "method": "GET"
    }
  }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="put_cartitem"></a>

### **PUT** /api/cart/*{cartId}*/items/*{itemId}*  
  
**DESCRIÇÃO:**  
Atualização de item no carrinho.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| cartId | Identificador do carrinho | `Number` |
| itemId | Identificador do item de carrinho | `Number` |
|-----------------+------------+----------------|

**PARÂMETROS JSON:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| Quantity | Quantidade do SKU a ser incluída no carrinho | `Number` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
PUT /api/cart/{cartId}/items/{itemId} HTTP/1.1
Content-Type: application/json
```

``` json
{
    "Quantity": 2
}
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
``` json
{
  "Id": 999999,
  "Links": {
    "self": {
      "href": "/api/cart/{cartId}/items/999999",
      "method": "GET"
    }
  }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>


<a name="delete_cartitem"></a>

### **DELETE** /api/cart/*{cartId}*/items/*{itemId}*  
  
**DESCRIÇÃO:**  
Remoção de item do carrinho.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| cartId | Identificador do carrinho | `Number` |
| itemId | Identificador do item de carrinho | `Number` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
DELETE /api/cart/{cartId}/items/{itemId} HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
``` json
{
  "Links": {
    "add": {
      "href": "/api/cart/{cartId}/items/",
      "method": "POST"
    },
    "cart": {
      "href": "/api/cart/{cartId}",
      "method": "GET"
    },
    "cart-items": {
      "href": "/api/cart/{cartId}/items/",
      "method": "GET"
    }
  }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>

<a name="getbyid_cartitem"></a>

### **GET** /api/cart/*{cartId}*/items/*{itemId}*  

**DESCRIÇÃO:**  
Obtém um item existente no carrinho através do seu Id. O Id é gerado automaticamente quando o item é adicionado ao carrinho.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| cartId | Identificador do carrinho | `Number` |
| itemId | Identificador do item de carrinho | `Number` |
|-----------------+------------+----------------|


**REQUEST:**  

``` http
GET /api/cart/{cartId}/items/{itemId} HTTP/1.1
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
  

<a name="get_cartitem"></a>

### **GET** /api/cart/*{cartId}*/items  

**DESCRIÇÃO:**  
Lista os itens existentes no carrinho. 

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| cartId | Identificador do carrinho | `Number` |
|-----------------+------------+----------------|


**REQUEST:**  

``` http
GET /api/cart/{cartId}/items HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
``` json
[
  { ... },
  { ... },
  { ... }
]
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="get_active"></a>

### **GET** /api/cart/*{cartId}*/active  

**DESCRIÇÃO:**  
Listagem dos itens ativos do carrinho. 

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| cartId | Identificador do carrinho | `Number` |
|-----------------+------------+----------------|


**REQUEST:**  

``` http
GET /api/cart/{cartId}/active HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
``` json
[
  { ... },
  { ... },
  { ... }
]
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="get_saved"></a>

### **GET** /api/cart/*{cartId}*/saved  

**DESCRIÇÃO:**  
Listagem dos itens salvos do carrinho. 

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| cartId | Identificador do carrinho | `Number` |
|-----------------+------------+----------------|


**REQUEST:**  

``` http
GET /api/cart/{cartId}/saved HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
``` json
[
  { ... },
  { ... },
  { ... }
]
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

----------------