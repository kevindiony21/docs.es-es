---
title: object (Referencia de C#) | Microsoft Docs
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- object_CSharpKeyword
- object
dev_langs:
- CSharp
helpviewer_keywords:
- object keyword [C#]
ms.assetid: 93f60c0b-e17a-40a9-9362-cca5fb77b0e7
caps.latest.revision: 16
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: cf1ec249c928f4bc23827ef0b831a8a64659ab8c
ms.contentlocale: es-es
ms.lasthandoff: 03/13/2017

---
# <a name="object-c-reference"></a>object (Referencia de C#)
El tipo `object` es un alias de <xref:System.Object> en .NET Framework. En el sistema de tipos unificado de C#, todos los tipos, los predefinidos y los definidos por el usuario, los tipos de referencia y los tipos de valores, heredan directa o indirectamente de <xref:System.Object>. Puede asignar valores de cualquier tipo a las variables de tipo `object`. Cuando una variable de un tipo de valor se convierte en objeto, se dice que se aplica la *conversión boxing*. Cuando una variable de tipo de objeto se convierte en un tipo de valor, se dice que se aplica la *conversión unboxing*. Para obtener más información, vea [Boxing and Unboxing](../../../csharp/programming-guide/types/boxing-and-unboxing.md) (Conversión boxing y conversión unboxing [Guía de programación de C#]).  
  
## <a name="example"></a>Ejemplo  
 El siguiente ejemplo muestra cómo las variables de tipo `object` pueden aceptar valores de cualquier tipo de datos y cómo las variables de tipo `object` pueden usar métodos en <xref:System.Object> desde .NET Framework.  
  
 [!code-cs[csrefKeywordsTypes#16](../../../csharp/language-reference/keywords/codesnippet/CSharp/object_1.cs)]  
  
## <a name="c-language-specification"></a>Especificación del lenguaje C#  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>Vea también  
 [Referencia de C#](../../../csharp/language-reference/index.md)   
 [Guía de programación de C#](../../../csharp/programming-guide/index.md)   
 [Palabras clave de C#](../../../csharp/language-reference/keywords/index.md)   
 [Reference Types](../../../csharp/language-reference/keywords/reference-types.md)  (Tipos de referencia [Referencia de C#])  
 [Tipos de valor](../../../csharp/language-reference/keywords/value-types.md)
