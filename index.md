---
title: Visão Geral  
category: Documentação
---


**BlackBox** é uma plataforma desenvolvida pelo laboratório de inovação da [Braspag](http://www.braspag.com.br){:target="_blank} que combina virtualização de pagamentos, catálogo de produtos e carrinho de compras, tendo como propósito proporcionar uma experiência de compra simplificada, segura e ubíqua. 

> Release atual:  **2.13** (BETA)

A plataforma é composta por um conjunto de APIs Web baseadas em arquitetura REST, que trocam dados em formato JSON seguindo fluxos de autorização definidos pelo protocolo OAuth 2, todos padrões amplamente utilizados pelo mercado e suportado pelas comunidades técnicas. 

Internamente, BlackBox foi construido utilizando alguns dos principais produtos da Braspag, como o Cartão Protegido, para tokenização de cartões, e o Pagador, para processamento de pagamentos. 
  
> A plataforma Braspag é certificada PCI DSS 3.2 e ISO 22301
  
As APIs REST permitem integrar a sua aplicação com a plataforma BlackBox. 

### Lista completa das APIs

APIs | Recurso
------------ | ------------- 
[Product API](/catalog/product/), [SKU API](/catalog/sku/) | Catálogo de Produtos  
[Cart API](/cart/cart/), [Cart Item API](/cart/cartitem/) | Carrinho de Compras  
[Order API](/order/order/) | Pedidos  
[OAuth2 API](/auth/oauth/) | Servidor de Autorização OAuth2  


