---
title: "Personalizar la experiencia de dise&#241;o del flujo de trabajo | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "ampliar [WF], Diseñador de flujo de trabajo"
ms.assetid: 98135077-0f5d-4d16-9337-01094e843537
caps.latest.revision: 13
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 13
---
# Personalizar la experiencia de dise&#241;o del flujo de trabajo
Los escenarios para diseñar las actividades personalizadas y para re\-hospedar [!INCLUDE[wfd1](../../../includes/wfd1-md.md)] se han simplificado enormemente en [!INCLUDE[netfx40_short](../../../includes/netfx40-short-md.md)].El desarrollo y la implementación resultan ahora más fáciles y más flexibles.El cambio infraestructural clave es que el nuevo modelo de programación del diseñador de actividad se compila en [!INCLUDE[avalon1](../../../includes/avalon1-md.md)].Esto proporciona la capacidad de definir diseñadores de actividad mediante declaración y re\-hospedar [!INCLUDE[wfd2](../../../includes/wfd2-md.md)] en otras aplicaciones con relativa facilidad.Al realizar el rehospedaje, se puede desarrollar un editor de expresiones personalizado para admitir IntelliSense o un dominio de expresión simplificado.La integración con [!INCLUDE[indigo1](../../../includes/indigo1-md.md)] se ha vuelto más uniforme con el uso de los servicios del flujo de trabajo.Se pueden utilizar diseñadores de actividad personalizados y el árbol de elementos de modelo para mejorar las experiencias en tiempo de diseño en los diseñadores de flujo de trabajo rehospedados.  
  
## En esta sección  
 [Usar plantillas y diseñadores de actividad personalizados](../../../docs/framework/windows-workflow-foundation//using-custom-activity-designers-and-templates.md)  
 Describe cómo crear diseñadores de actividad personalizados y plantillas.  
  
 [Rehospedar el Diseñador de flujo de trabajo](../../../docs/framework/windows-workflow-foundation//rehosting-the-workflow-designer.md)  
 Describe cómo re\-hospedar [!INCLUDE[wfd1](../../../includes/wfd1-md.md)] fuera de [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] y cómo mostrar errores de validación.  
  
 [Usar un editor de expresiones personalizado](../../../docs/framework/windows-workflow-foundation//using-a-custom-expression-editor.md)  
 Describe cómo implementar un editor de expresiones personalizado para utilizar con los diseñadores de flujo de trabajo rehospedados fuera de [!INCLUDE[vs2010](../../../includes/vs2010-md.md)].  
  
## Referencia  
 <xref:System.Activities.Presentation.ActivityDesigner>  
  
## Vea también  
 [Ampliar Windows Workflow Foundation](../../../docs/framework/windows-workflow-foundation//extend.md)   
 [Diseñador](../../../docs/framework/windows-workflow-foundation/samples/designer.md)   
 [Diseñadores de actividad personalizados](../../../docs/framework/windows-workflow-foundation/samples/custom-activity-designers.md)   
 [Rehospedaje del diseñador](../../../docs/framework/windows-workflow-foundation/samples/designer-rehosting.md)