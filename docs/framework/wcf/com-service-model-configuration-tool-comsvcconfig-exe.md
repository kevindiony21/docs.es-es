---
title: "Herramienta de configuraci&#243;n del modelo de servicio COM+ (ComSvcConfig.exe) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
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
  - "WCF, Integración de COM+"
  - "Windows Communication Foundation, Integración de COM+"
ms.assetid: 7717c6c2-85fc-418b-a8ed-bad8e61cec5c
caps.latest.revision: 15
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 15
---
# Herramienta de configuraci&#243;n del modelo de servicio COM+ (ComSvcConfig.exe)
La herramienta de línea de comandos de configuración del modelo de servicios COM\+ \(ComSvcConfig.exe\) le permite configurar interfaces de COM\+ que se van a exponer como servicio Web.  
  
## Sintaxis  
  
```  
  
ComSvcConfig.exe /install | /uninstall | /list [/application:<ApplicationID | ApplicationName>] [/contract:<ClassID | ProgID | *,InterfaceID | InterfaceName | *>] [/hosting:<complus | was>] [/webSite:<WebsiteName>] [/webDirectory:<WebDirectoryName>] [/mex] [/id] [/nologo] [/verbose] [/help] [/partial]  
```  
  
## Comentarios  
  
> [!NOTE]
>  Debe ser un administrador en el equipo local para utilizar ComSvcConfig.exe.  
  
 La herramienta se puede encontrar en la ubicación siguiente  
  
 %SystemRoot%\\Microsoft.Net\\Framework\\v3.0\\Windows Communication Foundation\\  
  
 Para obtener más información sobre ComSvcConfig.exe, vea [Cómo: Utilizar la herramienta configuración de modelos de servicio COM\+](../../../docs/framework/wcf/feature-details/how-to-use-the-com-service-model-configuration-tool.md).  
  
 La tabla siguiente describe los modos que se pueden utilizar con ComSvcConfig.exe.  
  
|Opción|Descripción|  
|------------|-----------------|  
|`install`|Instala una configuración para una interfaz de COM\+ para la integración del modelo de servicio.<br /><br /> Forma abreviada: `/i`|  
|`uninstall`|Desinstala una configuración para una interfaz de COM\+ de la integración del modelo de servicio.<br /><br /> Forma abreviada: `/u`|  
|`list`|Hace una lista de información sobre las aplicaciones y componentes COM\+ que tienen interfaces configuradas para la integración del modelo de servicio.<br /><br /> Forma abreviada: `/l`|  
  
 La tabla siguiente describe los marcadores que se pueden utilizar con ComSvcConfig.exe.  
  
|Opción|Descripción|  
|------------|-----------------|  
|`/application:<` *ApplicationID*  `&#124;`  *ApplicationName* `>`|Especifica la aplicación COM\+ que se va a configurar.<br /><br /> Forma abreviada `/a`.|  
|`/contract:<` *ClassID*  `&#124;`  *ProgID*  `&#124; *,` *InterfaceID*  `&#124;`  *InterfaceName*  `&#124; *` `>`|Especifica el componente y la interfaz COM\+ que se configurarán como contrato para el servicio.<br /><br /> Forma abreviada: `/c`<br /><br /> Aunque se puede utilizar el carácter comodín \(\*\) al especificar los nombres de interfaz y componente, se recomienda no utilizarlo, porque podría exponer interfaces en las que no pensaba.|  
|`/hosting:<` *complus*  `&#124;`  *was* `>`|Especifica si utilizar el modo de alojamiento COM\+ o el modo del alojamiento Web.<br /><br /> Forma abreviada: `/h`<br /><br /> Para utilizar el modo del alojamiento de COM\+ se requiere activación explícita de la aplicación COM\+.Utilizar el modo del alojamiento Web permite activar la aplicación COM\+ automáticamente tal y como se requiere.Si la aplicación COM\+ es una aplicación de biblioteca, se ejecuta en el proceso de Internet Information Services \(IIS\).Si la aplicación COM\+ es una aplicación de servidor, se ejecuta en el proceso de Dllhost.exe.|  
|`/webSite:<` *WebsiteName* `>`|Especifica el sitio web para alojamiento cuando se utiliza el modo de alojamiento Web \(vea el marcador `/hosting`\).<br /><br /> Forma abreviada `/w`.<br /><br /> Si no se especifica ningún sitio web, se utiliza el sitio web predeterminado.|  
|`/webDirectory:<` *WebDirectoryName* `>`|Especifica el directorio virtual para alojamiento cuando se utiliza el alojamiento Web \(vea el marcador `/hosting`\).<br /><br /> Forma abreviada: `/d`|  
|`/mex`|Agrega un extremo de servicio de intercambio de metadatos \(MEX\) a la configuración de servicio predeterminada para admitir clientes que desean recuperar del servicio una definición del contrato.<br /><br /> Forma abreviada: `/x`|  
|`/id`|Muestra la información de aplicación, componente e interfaz como id.<br /><br /> Forma abreviada: `/k`|  
|`/nologo`|Evita que ComSvcConfig.exe muestre su logotipo.<br /><br /> Forma abreviada: `/n`|  
|`/verbose`|Genera todas las advertencias o el texto informativo además de los errores encontrados.<br /><br /> Forma abreviada: `/v`|  
|`/help`|Muestra el mensaje de uso.<br /><br /> Forma abreviada: `/?`|  
|`/partial`|Genera una configuración de servicio cuando la interfaz especificada incluye una o más firmas de método que se pueden exponer.En el momento de inicialización del servicio, aparecen métodos compatibles como operaciones en el contrato de servicios y los métodos no compatibles se omiten y no están presentes en el contrato de servicios.<br /><br /> Si falta este marcador, la herramienta no generará una configuración de servicio cuando la interfaz especificada incluye uno o más métodos incompatibles.|  
  
## Ejemplos  
  
### Descripción  
 El ejemplo siguiente agrega la interfaz `IFinances` del componente `ItemOrders.IFinancial` \(de la aplicación COM\+ de OnlineStore\) al conjunto de interfaces que se exponen como servicios web utilizando el modo del alojamiento COM\+.Se generarán todas las advertencias además de cualquier error encontrado.  
  
### Código  
  
```  
ComSvcConfig.exe /install /application:OnlineStore /contract:ItemOrders.Financial,IFinances /hosting:complus /verbose  
```  
  
### Descripción  
 El ejemplo siguiente agrega la interfaz `IStockLevels` del componente `ItemInventory.Warehouse` \(de la aplicación COM\+ de OnlineWarehouse\) al conjunto de interfaces que se exponen como servicios web utilizando el modo de alojamiento Web.El servicio Web está alojado en Web en el directorio virtual de OnlineWarehouse de IIS.  
  
### Código  
  
```  
ComSvcConfig.exe /install /application:OnlineWarehouse /contract:ItemInventory.Warehouse,IStockLevels /hosting:was /webDirectory:root/OnlineWarehouse  
```  
  
### Descripción  
 El ejemplo siguiente quita la interfaz `IFinances` del componente `ItemOrders.Financial` \(de la aplicación COM\+ de OnlineStore\) del conjunto de interfaces que se exponen como servicios web utilizando el modo del alojamiento COM\+.  
  
### Código  
  
```  
ComSvcConfig.exe /uninstall /application:OnlineStore /interface:ItemOrders.Financial,IFinances /hosting:complus  
```  
  
### Descripción  
 El ejemplo siguiente enumera las interfaces hospedadas en COM\+ expuestas actualmente, junto con la dirección correspondiente y los detalles del enlace, para la aplicación OnlineStore COM\+ en el equipo local.  
  
### Código  
  
```  
ComSvcConfig.exe /list /application:OnlineStore /hosting:complus  
```  
  
## Vea también  
 [Cómo: Utilizar la herramienta configuración de modelos de servicio COM\+](../../../docs/framework/wcf/feature-details/how-to-use-the-com-service-model-configuration-tool.md)