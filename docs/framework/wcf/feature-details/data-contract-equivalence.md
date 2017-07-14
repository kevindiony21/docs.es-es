---
title: "Equivalencia del contrato de datos | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
helpviewer_keywords: 
  - "contratos de datos [WCF], equivalencia"
ms.assetid: f06f3c7e-c235-4ec1-b200-68142edf1ed1
caps.latest.revision: 16
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 16
---
# Equivalencia del contrato de datos
Para que un cliente envíe correctamente datos de un cierto tipo a un servicio, o para que un servicio envíe correctamente datos a un cliente, el tipo enviado no tiene que existir necesariamente en el extremo receptor.  El único requisito es que los contratos de datos de ambos tipos sean equivalentes.  \(En ocasiones, no se requiere la equivalencia estricta, como se explica en [Versiones de contratos de datos](../../../../docs/framework/wcf/feature-details/data-contract-versioning.md).\)  
  
 Para que los contratos de datos sean equivalentes, deben incluir el mismo espacio de nombres y nombre.  Además, cada miembro de datos en un lado debe tener un miembro de datos equivalente en el otro.  
  
 Para que los miembros de datos sean equivalentes, deben tener el mismo nombre.  De manera adicional, deben representar el mismo tipo de datos; es decir, sus contratos de datos deben ser equivalentes.  
  
> [!NOTE]
>  Tenga en cuenta que los nombres y espacios de nombres de contrato de datos, así como los nombres del miembros de datos, distinguen entre mayúsculas y minúsculas.  
  
 [!INCLUDE[crabout](../../../../includes/crabout-md.md)] nombres y espacios de nombres de contratos de datos, así como los nombres del miembro de datos, vea [Nombres de contratos de datos](../../../../docs/framework/wcf/feature-details/data-contract-names.md).  
  
 Si dos tipos existen en el mismo lado \(remitente o receptor\) y sus contratos de datos no son equivalentes \(por ejemplo, tienen diferentes miembros de datos\), no debería darles el mismo nombre y espacio de nombres.  Si lo hace, pueden iniciarse excepciones.  
  
 Los contratos de datos de los tipos siguientes son equivalentes:  
  
 [!code-csharp[C_DataContractNames#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#5)]
 [!code-vb[C_DataContractNames#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#5)]  
  
## Orden del miembro de datos y equivalencia del contrato de datos  
 Utilizar la propiedad <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A> de la clase <xref:System.Runtime.Serialization.DataMemberAttribute> puede afectar a la equivalencia del contrato de datos.  Los contratos de datos deben tener miembros que aparecen en el mismo orden para ser equivalentes.  El orden predeterminado es alfabético.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Orden de los miembros de datos](../../../../docs/framework/wcf/feature-details/data-member-order.md).  
  
 Por ejemplo, el código siguiente produce contratos de datos equivalentes.  
  
 [!code-csharp[C_DataContractNames#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#6)]
 [!code-vb[C_DataContractNames#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#6)]  
  
 Sin embargo, el siguiente código no produce un contrato de datos equivalente.  
  
 [!code-csharp[C_DataContractNames#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#7)]
 [!code-vb[C_DataContractNames#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#7)]  
  
## Herencia, interfaces y equivalencia del contrato de datos  
 Al determinar la equivalencia, un contrato de datos que hereda de otro contrato de datos se trata como si fuese, simplemente, un contrato de datos que incluye todos los miembros de datos del tipo base.  Tenga presente que el orden de los miembros de datos debe coincidir y que los miembros de tipo base preceden, en el orden, a los miembros de tipo derivado.  Además, si, como en el ejemplo de código siguiente, dos miembros de datos tienen el mismo valor de orden, la clasificación de esos miembros de datos será alfabética.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Orden de los miembros de datos](../../../../docs/framework/wcf/feature-details/data-member-order.md).  
  
 En el ejemplo siguiente, el contrato de datos para el tipo `Employee` es equivalente al contrato de datos para el tipo `Worker`.  
  
 [!code-csharp[C_DataContractNames#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#8)]
 [!code-vb[C_DataContractNames#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#8)]  
  
 Cuando se pasan parámetros y valores devueltos entre un cliente y un servicio, no se puede enviar un contrato de datos de una clase base si el extremo receptor espera un contrato de datos de una clase derivada.  Esto es conforme con los principios de la programación orientada a objetos.  En el ejemplo anterior, no se puede enviar un objeto de tipo `Person` si se espera `Employee` .  
  
 Se puede enviar un contrato de datos de una clase derivada si se espera un contrato de datos de una clase base, pero solo si el extremo receptor "conoce" el tipo derivado mediante <xref:System.Runtime.Serialization.KnownTypeAttribute>.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Tipos conocidos de contratos de datos](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md).  En el ejemplo anterior, se puede enviar un objeto de tipo `Employee` si se espera un `Person`, aunque solo si el código del receptor utiliza <xref:System.Runtime.Serialization.KnownTypeAttribute> para incluirlo en la lista de tipos conocidos.  
  
 Al pasar parámetros y valores devueltos entre las aplicaciones, si el tipo esperado es una interfaz, es equivalente a cuando el tipo esperado es <xref:System.Object>.  Dado que, en última instancia, cada tipo deriva de <xref:System.Object>, todos los contratos de datos derivan, a la larga, del contrato de datos para <xref:System.Object>.  De este modo, se puede pasar cualquier tipo de contrato de datos cuando se espera una interfaz.  Son necesarios algunos pasos adicionales para trabajar correctamente con interfaces; para más información, vea [Tipos conocidos de contratos de datos](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md).  
  
## Vea también  
 <xref:System.Runtime.Serialization.DataContractAttribute>   
 <xref:System.Runtime.Serialization.DataMemberAttribute>   
 [Orden de los miembros de datos](../../../../docs/framework/wcf/feature-details/data-member-order.md)   
 [Tipos conocidos de contratos de datos](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md)   
 [Nombres de contratos de datos](../../../../docs/framework/wcf/feature-details/data-contract-names.md)