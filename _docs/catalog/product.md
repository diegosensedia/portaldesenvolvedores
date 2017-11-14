---
title: Product API
category: Catálogo de Produtos
order: 3
---

Permite gerenciar os produtos do catálogo da loja. Cada produto possui uma coleção de [SKUs](/catalog/sku/).  


<a name="attributes"></a>

### Propriedades do Objeto  

|-----------------+------------|
| Nome | Descrição |
|-----------------|:-----------|
| Id | Identificador único do produto (gerado automaticamente). |
| CreatedOn | Data da criação. |
| UpdatedOn | Data da última atualização. |
| MerchantId | Identificador único da loja. |
| Name | Nome do produto. |
| Description | Descrição detalhada do produto. |
| Caption | Descrição curta do produto. |
| Status | Status atual do produto. Valores possíveis são: 0 (ativo), 10 (publicado) e 20 (descontinuado). |
| DefaultSku | SKU padrão para exibição do produto. |
| Manufacturer | Fabricante do produto. |
| Attributes | Coleção com os nomes dos atributos utilizados para descrever os SKUs do produto. Os nomes são case sensitive. |
| Metadata | Conjunto de pares chave-valor para armazenar informações adicionais sobre o produto. |
| SKUs | Coleção de SKUs do produto. |
|-----------------+------------|


``` json-doc
// -------------------------
// Exemplo do objeto Produto
// -------------------------

{
    "Id": 9999,
    "CreatedOn": "2017-01-23T09:44:13.3466667", // formato UTC
    "UpdatedOn": "2017-01-23T09:44:13.3466667",
    "MerchantId": "99999999-9999-9999-9999-999999999999",
    "MerchantName": "BlackBox Store",
    "Name": "Nome do produto",
    "Caption": "Uma breve descrição do produto",
    "Description": "Descrição mais detalhada do produto",
    "Status": 10, // 0 (ativo), 10 (publicado) e 20 (descontinuado) 
    "DefaultSku": "PROD9999", // nome do SKU padrão
    "Metadata": {
        "id-local": "101020",
        "categoria": "utilidades"
    },
    "Attributes": ["Cor", "Tamanho"], // case sensitive
    "Manufacturer": {
        "Name": "Fabricante do produto",
        "Warranty": 12, // meses
        "Model": "MODEL0001"
    },  
    "Skus": [
      {
        "Id": 9999999,
        "Sku": "PROD9999",
        "UpdatedOn": "2017-01-23T09:44:15.16",
        "Description": "Descrição do item",
        "Status": 0, 
        "Inventory": {
            "UpdatedOn": "2017-08-26T21:05:25.22",
            "Enabled": true,
            "Quantity": 100
        },
        "Dimensions": {
            "Weight": 1.0, // gramas
            "Height": 2.5, // centímetros
            "Lenght": 2.5, // centímetros
            "Width": 10 // centímetros
        },
        "Barcode": "4006381333930", // Código de barras do SKU
        "BarcodeType": 13, // EAN-13
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
            "UpdatedOn": "2017-08-26T21:05:25.21",
            "Amount": 9000, // valor em centavos
            "Currency": "BRL" // código ISO 4217 
        }
      }
    ]
}
```
{: title="Produto"}

<a name="http_operations"></a>

### Operações HTTP  

`POST`{:.http-post}  [/api/product/](#post_product){:.custom-attrib}  
Inclusão de um produto no catálogo.  

`PUT`{:.http-put} [/api/product/{productId}](#put_product){:.custom-attrib}  
Atualização de um produto existente no catálogo.  

`PUT`{:.http-put} [/api/product/{productId}/publish](#put_publish_product){:.custom-attrib}  
Publicação de um produto existente no catálogo.  

`DELETE`{:.http-delete} [/api/product/{productId}](#delete_product)  
Remoção (descontinuação) de um produto.  

`GET`{:.http-get} [/api/product/{productId}](#getbyid_product){:.custom-attrib}  
Obtenção de um produto existente no catálogo através do seu Id.   

`GET`{:.http-get} [/api/product/](#get_product){:.custom-attrib}  
Consulta dos produtos existentes no catálogo.  

  
<p style="text-align: right;">
  <a href="#title"><i class="material-icons">expand_less</i></a>
</p>

<a name="post_product"></a>

### **POST** /api/product/  

**DESCRIÇÃO:**  
Adiciona novos produtos no catálogo do vendedor.

**PARÂMETROS JSON:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| Name | Nome do produto | `String` `256` |
| Description | Descrição detalhada do produto | `String` `1024`  |
| Caption | Descrição curta do produto | `String` `128` `opcional` |
| MerchantId | Identificador de loja em formato GUID | `String` `36` |
| Manufacturer.Name | Nome do fabricante | `String` `64` `opcional` |
| Manufacturer.Model | Modelo do produto | `String` `64` `opcional` |
| Manufacturer.Warranty | Garantia do produto (em meses) | `Number` `opcional` |
| Attributes | Array com os nomes dos atributos que descrevem os SKUs do produto. Os nomes são case sensitive. | `Object` `opcional` |
| Metadata | Dados extras relacionados produto | `Object` `opcional` |
| DefaultSku | SKU padrão para exibição do produto | `String` `64` `opcional` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
POST /api/product/ HTTP/1.1
Content-Type: application/json
```

``` json-doc
{
  "MerchantId": "99999999-9999-9999-9999-999999999999",
  "Name": "Nome do produto",
  "Caption": "Uma breve descrição do produto",
  "Description": "Descrição mais detalhada do produto",
  "Manufacturer": {
    "Name": "Fabricante do produto", 
    "Warranty": 12, // meses
    "Model": "MODEL0001"
  },
  "Attributes": ["Cor", "Tamanho"],
  "Metadata": {
    "id-local": "101020",
    "categoria": "utilidades"
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
  

<a name="put_product"></a>

### **PUT** /api/product/*{productId}*  
  
**DESCRIÇÃO:**  
Atualiza atributos de um produto existente no catálogo do vendedor.

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
| Name | Nome do produto | `String` `256` |
| Description | Descrição detalhada do produto | `String` `1024`  |
| Caption | Descrição curta do produto | `String` `128` `opcional` |
| Manufacturer.Name | Nome do fabricante | `String` `64` `opcional` |
| Manufacturer.Model | Modelo do produto | `String` `64` `opcional` |
| Manufacturer.Warranty | Garantia do produto (em meses) | `Number` `opcional` |
| Attributes | Array com nomes (case sensitive) dos atributos que descrevem os SKUs do produto. | `Object` `opcional` |
| Metadata | Dados extras relacionados produto | `Object` `opcional` |
| DefaultSku | SKU padrão para exibição do produto | `String` `64` `opcional` |
|-----------------+------------+----------------|

> **Atenção**: atributos opcionais como *Caption* e *Manufacturer*, se definidos na operação **POST** e não informados na operação **PUT**, serão apagados.

**REQUEST:**  

``` http
PUT /api/product/{productId} HTTP/1.1
Content-Type: application/json
```

``` json
{
  "Name": "Nome do produto",
  "Description": "Descrição do produto",
  "Caption": "Uma breve descrição do produto",
  "DefaultSku": "PROD9999",
  "Metadata": {
    "id-local": "101020",
    "categoria": "utilidades"
  },
  "Manufacturer": {
    "Name": "Fabricante do produto", 
    "Warranty": 12,  
    "Model": "MODEL0001" 
  },
  "Attributes": []
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

<a name="put_publish_product"></a>

### **PUT** /api/product/*{productId}*/publish  
  
**DESCRIÇÃO:**  
Realiza a publicação de um produto existente no catálogo. Para ser publicável, um produto precisa ter ao menos um SKU cadastrado. Quando publicado, o produto se habilita para inclusão em um carrinho de compras.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
PUT /api/product/{productId}/publish HTTP/1.1
Content-Type: application/json
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

<a name="delete_product"></a>

### **DELETE** /api/product/*{productId}*  
  
**DESCRIÇÃO:**  
Remoção (descontinuação) de um produto. Ao remover um produto, o mesmo não pode mais ser adicionado a um carrinho e todos os seus SKUs são marcados como inativos.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
DELETE /api/product/{productId} HTTP/1.1
Content-Type: application/json
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
  
<a name="getbyid_product"></a>

### **GET** /api/product/*{productId}*  

**DESCRIÇÃO:**  
Obtém um produto existente no catálogo através do seu Id. O Id é gerado automaticamente quando o produto é adicionado ao catálogo.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| productId | Identificador do produto | `Number` |
|-----------------+------------+----------------|

**REQUEST:**  

``` http
GET /api/product/{productId} HTTP/1.1
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
  
<a name="get_product"></a>

### **GET** /api/product/  

**DESCRIÇÃO:**  
Lista os produtos existentes no catálogo do vendedor. Permite a aplicação de diversos filtros de pesquisa para maior controle sobre os resultados obtidos.

**PARÂMETROS URL:**  

|-----------------+------------+----------------|
| Nome | Descrição |  |
|-----------------|:-----------|---------------:|
| pageIndex | Índice para paginação (default: 1) | `Number` `opcional` |
| pageSize | Quantidade de registros retornados (default: 25) | `Number` `opcional` |
| merchantId | Filtro para Identificador de loja | `String` `opcional` |
| order | Ordenação ascendente (asc) ou descentende (desc) dos registros retornados (default: asc) | `String` `opcional` |
| startDate | Filtro por data inicial no formato "dd-MM-yyyy" | `String` `opcional` |
| endDate | Filtro por data final no formato "dd-MM-yyyy" | `String` `opcional` |
|-----------------+------------+----------------|


**REQUEST:**  

Sem parâmetros de consulta:

``` http
GET /api/product/ HTTP/1.1
Content-Type: application/json
```

Com parâmetros de consulta:

``` http
GET /api/product/?pageIndex=1&pageSize=25&merchantId=99999999-9999-9999-9999-999999999999&order=desc&startDate=01-06-2017&endDate=10-06-2017 HTTP/1.1
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