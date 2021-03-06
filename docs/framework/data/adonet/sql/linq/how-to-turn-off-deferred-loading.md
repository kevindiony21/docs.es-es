---
title: "C&#243;mo: Desactivar la carga diferida | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 1b84b852-3cad-41a7-8077-149a70d50c8b
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# C&#243;mo: Desactivar la carga diferida
Puede desactivar la carga aplazada al establecer <xref:System.Data.Linq.DataContext.DeferredLoadingEnabled%2A> en `false`.  Para obtener más información, consulta [Comparación entre carga aplazada y carga inmediata](../../../../../../docs/framework/data/adonet/sql/linq/deferred-versus-immediate-loading.md).  
  
> [!NOTE]
>  La carga aplazada se desactiva al desactivar el seguimiento de los objetos.  Para obtener más información, consulta [Cómo: Recuperar información como de solo lectura](../../../../../../docs/framework/data/adonet/sql/linq/how-to-retrieve-information-as-read-only.md).  
  
## Ejemplo  
 En el ejemplo siguiente se muestra cómo desactivar la carga aplazada al establecer <xref:System.Data.Linq.DataContext.DeferredLoadingEnabled%2A> en `false`.  
  
 [!code-csharp[DLinqQuerying#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQuerying/cs/Program.cs#3)]
 [!code-vb[DLinqQuerying#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQuerying/vb/Module1.vb#3)]  
  
## Vea también  
 [Conceptos de consulta](../../../../../../docs/framework/data/adonet/sql/linq/query-concepts.md)   
 [Consultar la base de datos](../../../../../../docs/framework/data/adonet/sql/linq/querying-the-database.md)