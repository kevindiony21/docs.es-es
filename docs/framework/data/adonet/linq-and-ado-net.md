---
title: "LINQ y ADO.NET | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: bf0c8f93-3ff7-49f3-8aed-f2b7ac938dec
caps.latest.revision: 8
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 8
---
# LINQ y ADO.NET
Actualmente, muchos programadores empresariales deben usar dos \(o más\) lenguajes de programación: un lenguaje de alto nivel para las capas de presentación y lógica empresarial \(como [!INCLUDE[csprcs](../../../../includes/csprcs-md.md)] o [!INCLUDE[vbprvb](../../../../includes/vbprvb-md.md)]\) y un lenguaje de consulta para interactuar con la base de datos \(como [!INCLUDE[tsql](../../../../includes/tsql-md.md)]\).  Esto requiere que el programador tenga conocimientos de varios idiomas para ser efectivo y también causa discrepancias de idiomas en el entorno de desarrollo.  Por ejemplo, una aplicación que utiliza API de acceso a datos para ejecutar una consulta en una base de datos especifica la consulta como un literal de cadena usando comillas.  Esta cadena de consulta es ilegible y no se comprueba si contiene errores, tales como una sintaxis no válida o si existen las columnas o las filas a las que hace referencia.  No hay ninguna comprobación de tipo de los parámetros de consulta y tampoco hay compatibilidad con `IntelliSense`.  
  
 [!INCLUDE[vbteclinqext](../../../../includes/vbteclinqext-md.md)] permite a los programadores formar consultas basadas en conjuntos en el código de su aplicación, sin tener que usar un lenguaje de consulta independiente.  Se puede escribir consultas de [!INCLUDE[vbteclinq](../../../../includes/vbteclinq-md.md)] en varios orígenes de datos enumerables \(es decir, un origen de datos que implementa la interfaz <xref:System.Collections.IEnumerable>\), como estructuras de datos en memoria, documentos XML, bases de datos SQL y objetos <xref:System.Data.DataSet>.  Aunque esos orígenes de datos enumerables se implementan de varias formas, todos revelan las mismas construcciones de lenguaje y sintaxis.  Como las consultas se pueden formar en el lenguaje de programación mismo, no es necesario utilizar otro lenguaje de consultas que esté incrustado como literales de cadena que el compilador no pueda entender o comprobar.  La integración de consultas en el lenguaje de programación también permite a los programadores de [!INCLUDE[vsprvs](../../../../includes/vsprvs-md.md)] ser más productivos proporcionando comprobación de sintaxis y tipo en tiempo de compilación e `IntelliSense`.  Estas características reducen la necesidad de depuración y corrección de errores de consultas.  
  
 La transferencia de datos de las tablas de SQL a objetos de memoria a menudo es una tarea tediosa y propensa a errores.  El proveedor de [!INCLUDE[vbteclinq](../../../../includes/vbteclinq-md.md)] implementado por [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)] y [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] convierte el origen de datos en recopilaciones de objetos basadas en <xref:System.Collections.IEnumerable>.  El programador siempre ve los datos como una recopilación de <xref:System.Collections.IEnumerable> cuando se realiza la consulta y la actualización.  Se proporciona compatibilidad completa con `IntelliSense` para escribir consultas en esas colecciones.  
  
 Existen tres tecnologías [!INCLUDE[vbteclinqext](../../../../includes/vbteclinqext-md.md)] de ADO.NET independientes: [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)], [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] y [!INCLUDE[linq_entities](../../../../includes/linq-entities-md.md)].  [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)] proporciona consultas más ricas y optimizadas de <xref:System.Data.DataSet>, [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] permite consultar directamente los esquemas de base de datos de [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)] y [!INCLUDE[linq_entities](../../../../includes/linq-entities-md.md)] permite consultar un [!INCLUDE[adonet_edm](../../../../includes/adonet-edm-md.md)].  
  
 El siguiente diagrama proporciona una visión general de cómo se relacionan las tecnologías ADO.NET LINQ con lenguajes de programación de alto nivel y orígenes de datos habilitados para LINQ.  
  
 ![Información general sobre LINQ to ADO.NET](../../../../docs/framework/data/adonet/media/dpue-linqtoadonetoverview-bpuedev11.png "DPUE\_LinqToAdoNetOverview\_bpuedev11")  
  
 Para obtener información general acerca de las características del lenguaje LINQ, vea [Introduction to LINQ](../../../../ocs/visual-basic/programming-guide/language-features/linq/introduction-to-linq.md).  Para obtener información sobre cómo usar LINQ en las aplicaciones, vea la [NOT IN BUILD: LINQ General Programming Guide](http://msdn.microsoft.com/es-es/609c7a6b-cbdd-429d-99f3-78d13d3bc049), que contiene información detallada sobre el uso de las tecnologías LINQ.  
  
 En las siguientes secciones se proporciona más información acerca de [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)], [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] y [!INCLUDE[linq_entities](../../../../includes/linq-entities-md.md)].  
  
## LINQ to DataSet  
 <xref:System.Data.DataSet> es un elemento fundamental del modelo de programación desconectada sobre el que se basa [!INCLUDE[vstecado](../../../../includes/vstecado-md.md)] y se usa ampliamente.  [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)] permite a los programadores crear capacidades de consulta más complejas en <xref:System.Data.DataSet> utilizando el mismo mecanismo de formulación de consultas que está disponible para muchos otros orígenes de datos.  Para obtener más información, consulta [LINQ to DataSet](../../../../docs/framework/data/adonet/linq-to-dataset.md).  
  
## LINQ to SQL  
 [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] es una herramienta útil para programadores que no requieren la asignación a un modelo conceptual.  Si utiliza [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)], puede usar el modelo de programación de [!INCLUDE[vbteclinq](../../../../includes/vbteclinq-md.md)] directamente en un esquema de base de datos existente.  [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] permite a los programadores generar clases de [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] que representan datos.  En lugar de la asignación a un modelo de datos conceptual, esas clases generadas se asignan directamente a tablas de bases de datos, vistas, procedimientos almacenados y funciones definidas por el usuario.  
  
 Con [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)], los programadores pueden escribir código directamente en el esquema de almacenamiento usando el mismo patrón de programación de [!INCLUDE[vbteclinq](../../../../includes/vbteclinq-md.md)] que las recopilaciones en memoria y <xref:System.Data.DataSet>, además de otros orígenes de datos como XML.  Para obtener más información, consulta [LINQ a SQL](../../../../docs/framework/data/adonet/sql/linq/index.md).  
  
## LINQ to Entities  
 La mayor parte de las aplicaciones se escriben actualmente utilizando bases de datos relacionales.  En algún punto, estas aplicaciones tendrán que interactuar con los datos representados en un formato relacional.  Los esquemas de base de datos no siempre son ideales para crear aplicaciones y los modelos conceptuales de aplicación no son iguales que los modelos lógicos de bases de datos.  [!INCLUDE[adonet_edm](../../../../includes/adonet-edm-md.md)] es un modelo de datos conceptual que se puede usar para crear un modelo de los datos de un dominio en particular para que las aplicaciones puedan interactuar con datos como objetos.  Vea [ADO.NET Entity Framework](../../../../docs/framework/data/adonet/ef/index.md) para obtener más información.  
  
 A través de [!INCLUDE[adonet_edm](../../../../includes/adonet-edm-md.md)], los datos relacionales se exponen como objetos en el entorno .NET.  Esto hace de la capa de objetos un objetivo idóneo para la compatibilidad con [!INCLUDE[vbteclinq](../../../../includes/vbteclinq-md.md)], ya que permite a los programadores formular consultas en la base de datos con el lenguaje usado para compilar la lógica empresarial.  Esta función se conoce como [!INCLUDE[linq_entities](../../../../includes/linq-entities-md.md)].  Vea [LINQ to Entities](../../../../docs/framework/data/adonet/ef/language-reference/linq-to-entities.md) para obtener más información.  
  
## Vea también  
 [LINQ to DataSet](../../../../docs/framework/data/adonet/linq-to-dataset.md)   
 [LINQ a SQL](../../../../docs/framework/data/adonet/sql/linq/index.md)   
 [LINQ to Entities](../../../../docs/framework/data/adonet/ef/language-reference/linq-to-entities.md)   
 [LINQ \(Language\-Integrated Query\)](../Topic/LINQ%20\(Language-Integrated%20Query\).md)   
 [Proveedores administrados de ADO.NET y centro de desarrolladores de conjuntos de datos](http://go.microsoft.com/fwlink/?LinkId=217917)