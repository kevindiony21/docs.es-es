---
title: "How to: Cancel a Parallel.For or ForEach Loop | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "parallel foreach loop, how to cancel"
  - "parallel for loops, how to cancel"
ms.assetid: 9d19b591-ea95-4418-8ea7-b6266af9905b
caps.latest.revision: 12
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 12
---
# How to: Cancel a Parallel.For or ForEach Loop
Los métodos <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=fullName> y <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=fullName> admiten la cancelación a través del uso de tokens de cancelación.  Para obtener más información sobre la cancelación en general, vea [Cancelación](../../../docs/standard/threading/cancellation-in-managed-threads.md).  En un bucle paralelo, se proporciona <xref:System.Threading.CancellationToken> al método en el parámetro <xref:System.Threading.Tasks.ParallelOptions> y después se agrega la llamada paralela en un bloque try\-catch.  
  
## Ejemplo  
 En el ejemplo siguiente se muestra cómo cancelar una llamada a <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=fullName>.  Puede aplicar el mismo enfoque a una llamada <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=fullName>.  
  
 [!code-csharp[TPL_Parallel#29](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/parallel_cancel.cs#29)]
 [!code-vb[TPL_Parallel#29](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/cancelloop.vb#29)]  
  
 Si el token que señala la cancelación es el mismo que se especifica en la instancia de <xref:System.Threading.Tasks.ParallelOptions>, el bucle paralelo producirá una <xref:System.OperationCanceledException> única en la cancelación.  Si algún otro token produce la cancelación, el bucle producirá una <xref:System.AggregateException> con <xref:System.OperationCanceledException> como InnerException.  
  
## Vea también  
 [Data Parallelism](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)   
 [Lambda Expressions in PLINQ and TPL](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)