---
title: SKU API
category: Catálogo de Produtos
order: 4
---

SKUs, ou *Stock Keeping Units*, representam variações específicas de um [produto](/catalog/product/) existente no catálogo. 

Um produto pode possuir diversas características variáveis, como por exemplo: cor, capacidade ou tamanho. SKUs distintos de um mesmo produto podem diferir não apenas em suas qualidades, mas também no preço e no controle de estoque.

<a name="attributes"></a>

### Propriedades do Objeto  

|-----------------+------------|
| Nome | Descrição |
|-----------------|:-----------|
| Id | Identificador do SKU (gerado automaticamente). |
| UpdatedOn | Data da última atualização. |
| Sku | Nome do SKU. Deve ser único no contexto de uma loja. |
| Status | Status atual do SKU. Valores possíveis são: 0 (ativo) e 1 (inativo). SKUs inativos não podem ser incluídos em carrinhos e não são considerados durante o processamento de um pedido. |
| Description | Descrição do SKU. |
| Images | Array com até 8 URLs de imagens para ilustrar o SKU. As URLs precisam obrigatóriamente serem disponibilizadas via HTTPS. |
| Attributes | Conjunto de pares chave-valor para armazenar até 5 atributos ao SKU. |
| Inventory | Gestão de estoque do SKU. |
| Dimensions | Dimensões do SKU: peso do item em gramas; altura, comprimento. e largura em centímetros. |
| Barcode | Código de barras do SKU. |
| BarcodeType | Tipo de representação de código de barras utilizado. Valores possíveis são: 8 (EAN-8), 13 (EAN-13), 20 (UPC-A) e 24 (UPC-E).  |
| Price | Preço do SKU, representado através do seu valor em centavos e do código ISO 4217 da moeda escolhida. |
|-----------------+------------|

``` json-doc
// -------------------------
// Exemplo do objeto SKU
// -------------------------

{
    "Id": 9999999,
    "Sku": "PROD9999",
    "UpdatedOn": "2017-01-23T09:44:15.16", // formato UTC
    "Description": "Descrição do item",
    "Status": 0, // 0 (Ativo), 1 (Inativo)
    "Inventory": {
        "UpdatedOn": "2017-08-26T21:05:25.2166667",
        "IsEnabled": true, // Habilita o controle de estoque para o SKU
        "Quantity": 100, 
    },
    "Dimensions": {
        "Weight": 1.0, // gramas
        "Height": 2.5, // centímetros
        "Lenght": 2.5, // centímetros
        "Width": 10 // centímetros
    },
    "Barcode": "4006381333930", // Código de barras do SKU
    "BarcodeType": 13, // Representação EAN-13
    "Images": [
        // utilize endpoints seguros (HTTPS)
        "https://www.myimages.com/image1.png",
        "https://www.myimages.com/image2.png",
        "https://www.myimages.com/image3.png"
    ],
    "Attributes": {
        "Cor": "vermelho", 
        "Tamanho": "médio" 
    },
    "Price": {
        "UpdatedOn": "2017-08-26T21:05:25.21",
        // valor em centavos
        // exemplo: 1,00 => 100
        "Amount": 9000, 
        // código ISO 4217
        // exemplo: BRL, USD, EUR
        "Currency": "BRL" 
    }
}
```
{: title="SKU"}

<a name="http_operations"></a>

### Operações HTTP  

`POST`{:.http-post} [/api/product/{productId}/sku](#post_sku){:.custom-attrib}  
Criação de SKU para o produto {productId}.  

`PUT`{:.http-put} [/api/product/{productId}/sku/{skuId}](#put_sku){:.custom-attrib}  
Atualização de SKU {skuId} para o produto {productId}.  

`PUT`{:.http-put} [/api/product/{productId}/sku/{skuId}/inventory](#put_inventory){:.custom-attrib}  
Atualização de estoque para o SKU {skuId} do produto {productId}.  

`PUT`{:.http-put} [/api/product/{productId}/sku/{skuId}/price](#put_price){:.custom-attrib}  
Atualização do preço do SKU {skuId}.  

`DELETE`{:.http-delete} [/api/product/{productId}/sku/{skuId}/](#delete_sku)  
Remoção (inativação) de um SKU.  

`GET`{:.http-get} [/api/product/{productId}/sku/{skuId}](#getbyid_sku){:.custom-attrib}  
Obtenção de SKU do produto {productId} por Id.

`GET`{:.http-get} [/api/product/{productId}/sku/{skuId}/inventory](#get_inventory){:.custom-attrib}  
Obtenção de estoque do SKU {skuId}.  

`GET`{:.http-get} [/api/product/{productId}/sku/{skuId}/price](#get_price){:.custom-attrib}  
Obtenção do preço do SKU {skuId}. 

`GET`{:.http-get} [/api/product/{productId}/sku](#get_sku){:.custom-attrib}  
Listagem dos SKUs do produto {productId}.  


  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>

<a name="post_sku"></a>

### **POST** /api/product/*{productId}*/sku  

**DESCRIÇÃO:**  
Criação de novo SKU para o produto.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
|-----------------+------------+----------------|

**PARÂMETROS JSON:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| Sku | Nome único do SKU | `String` `64` |
| Description | Descrição do SKU | `String` `1024`  |
| Inventory.IsEnabled | Habilita ou não o controle de estoque para o SKU | `Boolean`  |
| Inventory.Quantity | Quantidade de itens disponíveis no inventório | `Number` `0-1000000` |
| Images | Array com até 8 URLs de imagens para ilustrar o SKU | `Object` `opcional` |
| Attributes | Conjunto de pares chave-valor para armazenar até 5 atributos ao SKU | `Object` `opcional` |
| Price.Currency | Código ISO 4217 da moeda | `String` `3` |
| Price.Amount | Valor em centavos do preço de venda | `Number` |
| Barcode | Código de barras do SKU | `String` `16` `opcional` |
| BarcodeType | Tipo de representação de código de barras | `Number` `opcional` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
POST /api/product/{productId}/sku HTTP/1.1
Content-Type: application/json
```

``` json-doc
{
    "Sku": "PROD9999",
    "Description": "Descrição do SKU de Produto",
    "Inventory": {
        "IsEnabled": true, // Habilita o controle de estoque para o SKU
        "Quantity": 100 
    },
    "Dimensions": {
        "Weight": 1.0,
        "Height": 2.5,
        "Lenght": 2.5,
        "Width": 10
    },
    "Barcode": "4006381333930",
    "BarcodeType": 13,
    "Images": [
        "https://www.myimages.com/image1.png",
        "https://www.myimages.com/image2.png",
        "https://www.myimages.com/image3.png"
    ],
    "Attributes": {
        "Cor": "vermelho",
        "Tamanho": "médio" 
    },
    "Price": {
        "Currency": "BRL",
        "Amount" : 9000
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
    "Id": 9999999,
    "Links": {
        "self": {
            "href": "/api/product/9999/sku/9999999",
            "method": "GET"
        },
        "inventory": {
            "href": "/api/product/9999/sku/9999999/inventory",
            "method": "GET"
        },
        "price": {
            "href": "/api/product/9999/sku/9999999/price",
            "method": "GET"
        },
        "all": {
            "href": "/api/product/9999/sku",
            "method": "GET"
        },
        "update": {
            "href": "/api/product/9999/sku/9999999",
            "method": "PUT"
        },
        "delete": {
            "href": "/api/product/9999/sku/9999999",
            "method": "DELETE"
        }
    }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="put_sku"></a>

### **PUT** /api/product/*{productId}*/sku/*{skuId}*  

**DESCRIÇÃO:**  
Atualização de SKU do produto.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
| skuId | Identificador do SKU | `Number` |
|-----------------+------------+----------------|

**PARÂMETROS JSON:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| Sku | Nome único do SKU | `String` `64` |
| Description | Descrição do SKU | `String` `1024`  |
| Images | Array com até 8 URLs de imagens para ilustrar o SKU | `Object` `opcional` |
| Attributes | Conjunto de pares chave-valor para armazenar até 5 atributos ao SKU | `Object` `opcional` |
| Barcode | Código de barras do SKU | `String` `16` `opcional` |
| BarcodeType | Tipo de representação de código de barras | `Number` `opcional` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
PUT /api/product/{productId}/sku/{skuId} HTTP/1.1
Content-Type: application/json
```

``` json
{
    "Description": "Nova descrição do SKU de Produto",
    "Dimensions": {
        "Weight": 1.0,
        "Height": 2.5,
        "Lenght": null,
        "Width": 10
    },
    "Barcode": null,
    "BarcodeType": null,
    "Attributes": {
        "Cor": "verde", 
        "Tamanho": "grande" 
    } 
}
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```

``` json
{
    "Id": 9999999,
    "Links": {
        "self": {
            "href": "/api/product/9999/sku/9999999",
            "method": "GET"
        },
        "inventory": {
            "href": "/api/product/9999/sku/9999999/inventory",
            "method": "GET"
        },
        "price": {
            "href": "/api/product/9999/sku/9999999/price",
            "method": "GET"
        },
        "all": {
            "href": "/api/product/9999/sku",
            "method": "GET"
        },
        "update": {
            "href": "/api/product/9999/sku/9999999",
            "method": "PUT"
        },
        "delete": {
            "href": "/api/product/9999/sku/9999999",
            "method": "DELETE"
        }
    }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="put_inventory"></a>

### **PUT** /api/product/*{productId}*/sku/*{skuId}*/inventory  

**DESCRIÇÃO:**  
Atualização de estoque do SKU.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
| skuId | Identificador do SKU | `Number` |
|-----------------+------------+----------------|

**PARÂMETROS JSON:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| IsEnabled | Habilita ou não o controle de estoque para o SKU | `Boolean` |
| Quantity | Quantidade de itens disponíveis no inventório | `Number` `0-1000000`  |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
PUT /api/product/{productId}/sku/{skuId}/inventory HTTP/1.1
Content-Type: application/json
```

``` json
{
    "IsEnabled": true,
    "Quantity": 100
}
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```

``` json
{
    "Id": 9999999,
    "Links": {
        "self": {
            "href": "/api/product/9999/sku/9999999",
            "method": "GET"
        },
        "inventory": {
            "href": "/api/product/9999/sku/9999999/inventory",
            "method": "GET"
        },
        "price": {
            "href": "/api/product/9999/sku/9999999/price",
            "method": "GET"
        },
        "all": {
            "href": "/api/product/9999/sku",
            "method": "GET"
        },
        "update": {
            "href": "/api/product/9999/sku/9999999",
            "method": "PUT"
        },
        "delete": {
            "href": "/api/product/9999/sku/9999999",
            "method": "DELETE"
        }
    }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="put_price"></a>

### **PUT** /api/product/*{productId}*/sku/*{skuId}*/price  

**DESCRIÇÃO:**  
Atualização do preço do SKU.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
| skuId | Identificador do SKU | `Number` |
|-----------------+------------+----------------|

**PARÂMETROS JSON:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| Amount | Valor em centavos do preço de venda | `Number` |
| Currency | Código ISO 4217 da moeda | `String` `3`  |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
PUT /api/product/{productId}/sku/{skuId}/price HTTP/1.1
Content-Type: application/json
```

``` json
{
    "Amount": 10000,
    "Currency": "BRL"  
}
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```

``` json
{
    "Id": 9999999,
    "Links": {
        "self": {
            "href": "/api/product/9999/sku/9999999",
            "method": "GET"
        },
        "inventory": {
            "href": "/api/product/9999/sku/9999999/inventory",
            "method": "GET"
        },
        "price": {
            "href": "/api/product/9999/sku/9999999/price",
            "method": "GET"
        },
        "all": {
            "href": "/api/product/9999/sku",
            "method": "GET"
        },
        "update": {
            "href": "/api/product/9999/sku/9999999",
            "method": "PUT"
        },
        "delete": {
            "href": "/api/product/9999/sku/9999999",
            "method": "DELETE"
        }
    }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="delete_sku"></a>

### **DELETE** /api/product/*{productId}*/sku/*{skuId}*  
  
**DESCRIÇÃO:**  
Remoção (inativação) de um SKU. Ao ser inativado, um SKU não pode mais ser adicionado a um carrinho.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
| skuId | Identificador do SKU | `Number` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
DELETE /api/product/{productId}/sku/{skuId} HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
``` json
{
    "Id": 9999999,
    "Links": {
        "self": {
            "href": "/api/product/9999/sku/9999999",
            "method": "GET"
        },
        "inventory": {
            "href": "/api/product/9999/sku/9999999/inventory",
            "method": "GET"
        },
        "price": {
            "href": "/api/product/9999/sku/9999999/price",
            "method": "GET"
        },
        "all": {
            "href": "/api/product/9999/sku",
            "method": "GET"
        },
        "update": {
            "href": "/api/product/9999/sku/9999999",
            "method": "PUT"
        },
        "delete": {
            "href": "/api/product/9999/sku/9999999",
            "method": "DELETE"
        }
    }
}
```
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>


<a name="getbyid_sku"></a>

### **GET** /api/product/*{productId}*/sku/*{skuId}*  

**DESCRIÇÃO:**  
Obtenção de SKU por Id.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
| skuId | Identificador do SKU | `Number` |
|-----------------+------------+----------------|


**REQUEST:**  

``` http
GET /api/product/{productId}/sku/{skuId} HTTP/1.1
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
  

<a name="get_inventory"></a>

### **GET** /api/product/*{productId}*/sku/*{skuId}*/inventory  

**DESCRIÇÃO:**  
Obtenção de estoque do SKU.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
| skuId | Identificador do SKU | `Number` |
|-----------------+------------+----------------|


**REQUEST:**  

``` http
GET /api/product/{productId}/sku/{skuId}/inventory HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```

``` json-doc
{
    "UpdatedOn": "2017-08-27T18:19:55.2023038", // última atualização em formato UTC
    "IsEnabled": true,
    "Quantity": 100
}
```  
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="get_price"></a>

### **GET** /api/product/*{productId}*/sku/*{skuId}*/price  

**DESCRIÇÃO:**  
Obtenção do preço do SKU.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
| skuId | Identificador do SKU | `Number` |
|-----------------+------------+----------------|


**REQUEST:**  

``` http
GET /api/product/{productId}/sku/{skuId}/price HTTP/1.1
Content-Type: application/json
```

**RESPONSE:**  

``` http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```

``` json-doc
{
    "UpdatedOn": "2017-08-27T18:27:39.1055263", // última atualização em formato UTC
    "Amount": 10000,
    "Currency": "BRL"
}
```  
  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>
  

<a name="get_sku"></a>

### **GET** /api/product/*{productId}*/sku  

**DESCRIÇÃO:**  
Lista os SKUs existentes no produto.  

**REQUEST:**  

``` http
GET /api/product/{productId}/sku HTTP/1.1
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