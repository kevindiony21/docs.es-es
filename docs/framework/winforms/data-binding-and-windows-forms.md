---
title: "Enlace de datos y formularios Windows Forms | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "controles enlazados, Windows Forms"
  - "datos [Windows Forms], enlace de datos complejo"
  - "datos [Windows Forms], enlace de datos"
  - "datos [Windows Forms], enlace de datos simple"
  - "enlace de datos, Windows Forms"
  - "controles enlazados a datos, Windows Forms"
  - "tablas de búsqueda, enlace de datos"
  - "listas principal-detalle"
  - "informes, Windows Forms"
  - "controles de Windows Forms, enlace de datos"
ms.assetid: 419aac5e-819b-4aad-88b0-73a2f8c0bd27
caps.latest.revision: 20
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 20
---
# Enlace de datos y formularios Windows Forms
Windows Forms permite enlazar no solo a orígenes de datos tradicionales, sino también a prácticamente cualquier estructura que contenga datos.  Se puede enlazar a una matriz de valores que se calcula en tiempo de ejecución, se lee de un archivo o se deriva de los valores de otros controles.  
  
 Además, se puede enlazar cualquier propiedad de cualquier control al origen de datos.  En el enlace de datos tradicional, habitualmente se enlaza la propiedad de presentación —por ejemplo, la propiedad <xref:System.Windows.Forms.Control.Text%2A> de un control <xref:System.Windows.Forms.TextBox>— al origen de datos.  Con [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], también existe la opción de establecer otras propiedades mediante enlace.  Puede usar el enlace para cualquiera de las siguientes tareas:  
  
-   Establecer el gráfico de un control de imagen.  
  
-   Establecer el color de fondo de uno o más controles.  
  
-   Establecer el tamaño de los controles.  
  
 Esencialmente, el enlace de datos es un modo automático de establecer cualquier propiedad accesible en tiempo de ejecución de cualquier control en un formulario.  
  
## Tipos de enlace de datos  
 Windows Forms puede aprovechar las ventajas de dos tipos de enlace de datos: enlace simple y enlace complejo.  Cada uno ofrece diferentes ventajas.  
  
|Tipo de enlace de datos|Descripción|  
|-----------------------------|-----------------|  
|Enlace de datos simple|Capacidad de un control para enlazarse a un único elemento de datos como, por ejemplo, un valor que existe en una columna de una tabla de conjunto de datos.  Este es el tipo de enlace habitual de controles tales como <xref:System.Windows.Forms.TextBox> o <xref:System.Windows.Forms.Label>, que son controles que suelen mostrar un único valor.  De hecho, se puede enlazar cualquier propiedad de un control a un campo de una base de datos.  Hay una amplia compatibilidad para esta característica en [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].<br /><br /> Para obtener más información, vea:<br /><br /> -   [Interfaces relacionadas con el enlace de datos](../../../docs/framework/winforms/interfaces-related-to-data-binding.md)<br />-   [Cómo: Desplazarse por datos en formularios Windows Forms](../../../docs/framework/winforms/how-to-navigate-data-in-windows-forms.md)<br />-   [Cómo: Crear un control con enlace simple en Windows Forms](../../../docs/framework/winforms/how-to-create-a-simple-bound-control-on-a-windows-form.md)|  
|Enlace de datos complejo|Capacidad de un control para enlazarse a más de un elemento de datos; normalmente, a más de un registro en una base de datos.  El enlace complejo también se denomina enlace basado en lista.  <xref:System.Windows.Forms.DataGridView>, <xref:System.Windows.Forms.ListBox> y <xref:System.Windows.Forms.ComboBox> son ejemplos de controles que admiten el enlace complejo.  Para obtener un ejemplo del enlace de datos complejo, vea [Cómo: Enlazar un control ComboBox o ListBox de formularios Windows Forms a datos](../../../docs/framework/winforms/controls/how-to-bind-a-windows-forms-combobox-or-listbox-control-to-data.md).|  
  
## Componente BindingSource  
 Para simplificar el enlace de datos, Windows Forms permite enlazar un origen de datos al componente <xref:System.Windows.Forms.BindingSource> y, a continuación, enlazar controles a <xref:System.Windows.Forms.BindingSource>.  Puede utilizar <xref:System.Windows.Forms.BindingSource> en escenarios de enlace simple o complejo.  En cualquier caso, <xref:System.Windows.Forms.BindingSource> actúa como intermediario entre el origen de datos y los controles enlazados, y proporciona gestión de divisas con notificación de cambios y otros servicios.  
  
## Escenarios comunes que emplean el enlace de datos  
 Casi todas las aplicaciones comerciales utilizan información leída de orígenes de datos de un tipo u otro, normalmente a través del enlace de datos.  La lista siguiente muestra algunos de los escenarios más comunes que utilizan el enlace de datos como método de manipulación y presentación de los datos.  
  
|Escenario|Descripción|  
|---------------|-----------------|  
|Informes|Los informes permiten mostrar y resumir los datos en un documento impreso de modo flexible.  Es muy común crear un informe que imprima contenido seleccionado de un origen de datos en la pantalla o en una impresora.  Entre los informes comunes están las listas, las facturas y los resúmenes.  Los elementos suelen tener formato de columnas de listas, con elementos secundarios organizados bajo cada elemento de la lista; sin embargo, debe elegir el diseño que mejor se adapte a los datos.|  
|Entrada de datos|Es una forma común de introducir grandes cantidades de datos relacionados o de solicitar información a los usuarios a través de un formulario de entrada de datos.  Los usuarios pueden escribir la información o seleccionar opciones mediante cuadros de texto, botones de opción, listas desplegables y casillas.  La información se envía y se almacena a continuación en una base de datos, cuya estructura se basa en la información especificada.|  
|Relación principal\-detalle|Una aplicación principal\-detalle es un formato para examinar datos relacionados.  En concreto, hay dos tablas de datos con una relación que las conecta —en el ejemplo de empresa clásico, una tabla “Clientes” y una tabla “Pedidos” con una relación entre ellos que vincula los clientes y sus respectivos pedidos.  Para obtener más información sobre cómo crear una aplicación principal\-detalle con dos controles <xref:System.Windows.Forms.DataGridView> de Windows Forms, vea [Cómo: Crear un formulario principal\-detalle mediante dos controles DataGridView de formularios Windows Forms](../../../docs/framework/winforms/controls/create-a-master-detail-form-using-two-datagrids.md)|  
|Tabla de búsqueda|Otro escenario común de presentación o manipulación de datos es la tabla de búsqueda.  A menudo, como parte de una presentación de datos mayor, se usa un control <xref:System.Windows.Forms.ComboBox> para mostrar y manipular datos.  La clave es que los datos que se muestran en el control <xref:System.Windows.Forms.ComboBox> son diferentes de los datos escritos en la base de datos.  Por ejemplo, si tiene un control <xref:System.Windows.Forms.ComboBox> que muestra los elementos disponibles en una tienda de comestibles, probablemente desee ver los nombres de los productos \(pan, leche, huevos\).  Sin embargo, para facilitar la recuperación de la información en la base de datos y para lograr la normalización de la base de datos, probablemente almacene la información de los elementos específicos de un determinado pedido como códigos de artículo \(501, 603, etc.\).  Por lo tanto, existe una conexión implícita entre el “nombre descriptivo” del artículo de alimentación en el control <xref:System.Windows.Forms.ComboBox> de su formulario y el número de artículo relacionado presente en un pedido.  Esta es la esencia de una tabla de búsqueda.  Para obtener más información, vea [Cómo: Crear una tabla de búsqueda con el componente BindingSource de formularios Windows Forms](../../../docs/framework/winforms/controls/how-to-create-a-lookup-table-with-the-windows-forms-bindingsource-component.md).|  
  
## Vea también  
 <xref:System.Windows.Forms.Binding>   
 [Enlace de datos en Windows Forms](../../../docs/framework/winforms/windows-forms-data-binding.md)   
 [Cómo: Enlazar el control DataGrid de formularios Windows Forms a un origen de datos](../../../docs/framework/winforms/controls/how-to-bind-the-windows-forms-datagrid-control-to-a-data-source.md)   
 [BindingSource \(Componente\)](../../../docs/framework/winforms/controls/bindingsource-component.md)