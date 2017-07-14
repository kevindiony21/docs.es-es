---
title: "Distribuidor de canal personalizado | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 813acf03-9661-4d57-a3c7-eeab497321c6
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# Distribuidor de canal personalizado
Este ejemplo muestra cómo crear la pila del canal de manera personalizada implementando <xref:System.ServiceModel.ServiceHostBase> directamente y cómo crear un distribuidor de canal personalizado en un entorno de host web.El distribuidor del canal interactúa con <xref:System.ServiceModel.Channels.IChannelListener> para aceptar los canales y recupera los mensajes de la pila del canal.Este ejemplo también proporciona un ejemplo básico para mostrar cómo integrar una pila del canal en un entorno de host web con <xref:System.ServiceModel.Activation.VirtualPathExtension>.  
  
## ServiceHostBase personalizado  
 En este ejemplo se implementa el tipo base <xref:System.ServiceModel.ServiceHostBase> en lugar de <xref:System.ServiceModel.ServiceHost> para mostrar cómo reemplazar la implementación de la pila de [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] con un nivel de control de mensajes personalizado encima de la pila del canal.Se invalida el método virtual <xref:System.ServiceModel.ServiceHostBase.InitializeRuntime%2A> para compilar los agentes de escucha del canal y el distribuidor de canal.  
  
 Para implementar un servicio hospedado en web, obtenga la extensión de servicio <xref:System.ServiceModel.Activation.VirtualPathExtension> de la colección de la propiedad <xref:System.ServiceModel.ServiceHostBase.Extensions%2A> y agréguela al objeto <xref:System.ServiceModel.Channels.BindingParameterCollection> para que el nivel de transporte sepa cómo configurar el agente de escucha del canal según los valores de entorno de hospedaje, es decir, la configuración de Internet Information Services \(IIS\)\/Servicio de activación de procesos de Windows \(WAS\).  
  
## Distribuidor de canal personalizado  
 El distribuidor de canal personalizado extiende el tipo <xref:System.ServiceModel.Dispatcher.ChannelDispatcherBase>.Este tipo implementa la lógica de programación del nivel del canal.En este ejemplo, solo se admite <xref:System.ServiceModel.Channels.IReplyChannel> para el modelo de intercambio de mensajes de solicitud\-respuesta, pero el distribuidor del canal personalizado se puede extender con facilidad a otros tipos de canal.  
  
 El distribuidor abre el agente de escucha del canal primero y, a continuación, acepta el canal de respuesta singleton.Con el canal, empieza a enviar los mensajes \(solicitudes\) en un bucle infinito.Para cada solicitud, crea un mensaje de respuesta y lo envía de nuevo al cliente.  
  
## Crear un mensaje de respuesta  
 El procesamiento de mensajes se implementa en el tipo `MyServiceManager`.En el método `HandleRequest`, el encabezado de mensaje `Action` se comprueba primero para ver si se admite la solicitud.Se define una acción "http:\/\/tempuri.org\/HelloWorld\/Hello" de SOAP predefinida para permitir el filtrado de mensajes.Esto es similar al concepto de contrato de servicios en la implementación de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] de <xref:System.ServiceModel.ServiceHost>.  
  
 En el caso de la acción SOAP correcta, el ejemplo recupera los datos de mensaje solicitados y genera una respuesta correspondiente a la solicitud similar a lo que se ve en el caso de <xref:System.ServiceModel.ServiceHost>.  
  
 Para administrar el verbo HTTP\-GET en especial, devuelve un mensaje HTML personalizado, en este caso, para que pueda examinar el servicio desde un explorador para ver que está compilado correctamente.Si la acción SOAP no coincide, envíe a un mensaje de error para indicar que no se admite la solicitud.  
  
 El cliente de este ejemplo es un cliente de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] normal que no supone nada del servicio.De este modo, el servicio está diseñado especialmente para coincidir con lo que se obtiene de una implementación de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]<xref:System.ServiceModel.ServiceHost> normal.Como resultado, solo se requiere un contrato de servicio en el cliente.  
  
## Utilizar el ejemplo  
 Al ejecutar la aplicación cliente directamente, se genera el siguiente resultado.  
  
```Output  
El cliente está hablando con un servicio WCF de solicitud/respuesta.   
Escriba lo que desea indicar al servidor: Howdy  
El Servidor respondió: Usted dijo: Howdy.Identificador de mensaje: 1  
El Servidor respondió: Usted dijo: Howdy.Identificador de mensaje: 2  
El Servidor respondió: Usted dijo: Howdy.Identificador de mensaje: 3  
El Servidor respondió: Usted dijo: Howdy.Identificador de mensaje: 4  
El Servidor respondió: Usted dijo: Howdy.Identificador de mensaje: 5  
  
```  
  
 También puede examinar el servicio desde un explorador para que se procese un mensaje HTTP\-GET en el servidor.De esta forma obtiene de vuelta texto HTLM con formato correcto.  
  
> [!IMPORTANT]
>  Puede que los ejemplos ya estén instalados en su equipo.Compruebe el siguiente directorio \(valor predeterminado\) antes de continuar.  
>   
>  `<>InstallDrive:\WF_WCF_Samples`  
>   
>  Si no existe este directorio, vaya a la página de [ejemplos de Windows Communication Foundation \(WCF\) y Windows Workflow Foundation \(WF\) Samples para .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) para descargar todos los ejemplos de [!INCLUDE[wf1](../../../../includes/wf1-md.md)] y [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)].Este ejemplo se encuentra en el siguiente directorio.  
>   
>  `<unidadDeInstalación>:\WF_WCF_Samples\WCF\Extensibility\Channels\CustomChannelDispatcher`