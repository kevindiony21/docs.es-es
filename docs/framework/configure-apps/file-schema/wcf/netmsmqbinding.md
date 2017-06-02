---
title: "&lt;netMsmqBinding&gt; | Microsoft Docs"
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
ms.assetid: a68b44d7-7799-43a3-9e63-f07c782810a6
caps.latest.revision: 35
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 35
---
# &lt;netMsmqBinding&gt;
Define un enlace en cola adecuado para la comunicación del equipo de cruce.  
  
## Sintaxis  
  
```  
  
<netMsmqBinding>  
    <binding   
       closeTimeout="TimeSpan"   
       customDeadLetterQueue="Uri"  
       deadLetterQueue="Uri"  
       durable="Boolean"  
       exactlyOnce="Boolean"   
       maxBufferPoolSize="Integer"  
       maxReceivedMessageSize"Integer"  
       maxRetryCycles="Integer"   
       name="string"   
       openTimeout="TimeSpan"   
       poisonMessageHandling="Disabled/EnabledIfSupported"   
       queueTransferProtocol="Native/Srmp/SrmpSecure"  
       receiveErrorHandling="Drop/Fault/Move/Reject"  
       receiveTimeout="TimeSpan"   
       receiveRetryCount="Integer"  
       rejectAfterLastRetry="Boolean"   
       retryCycleDelay="TimeSpan"    
       sendTimeout="TimeSpan"   
       timeToLive="TimeSpan"    
       useActiveDirectory="Boolean"  
       useMsmqTracing="Boolean  
       useSourceJournal="Boolean"  
          <security>  
                  <message    
                        algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"  
            clientCredentialType="None/Windows/UserName/Certificate/InfoCard "/>  
                  <transport msmqAuthenticationMode="None/WindowsDomain/Certificate"  
            msmqEncryptionAlgorithm="RC4Stream/AES"  
            msmqProtectionLevel="None/Sign/EncryptAndSign"  
            msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />  
          </security>  
       <readerQuotas   
            maxArrayLength="Integer"  
            maxBytesPerRead="Integer"  
            maxDepth="Integer"   
            maxNameTableCharCount="Integer"           
            maxStringContentLength="Integer" />  
   </binding>  
</netMsmqBinding>  
```  
  
## Atributos y elementos  
 En las siguientes secciones se describen los atributos, los elementos secundarios y los elementos primarios.  
  
### Atributos  
  
|Atributo|Descripción|  
|--------------|-----------------|  
|`closeTimeout`|Un valor <xref:System.TimeSpan> que especifica el intervalo de tiempo del que dispone una operación de cierre para completarse.  Este valor debe ser mayor o igual que <xref:System.TimeSpan.Zero>.  El valor predeterminado es 00:01:00.|  
|`customDeadLetterQueue`|Un URI que contiene la ubicación de la cola de componentes con problemas de entrega por aplicación, donde se colocan los mensajes que han expirado o cuya transferencia o envío han fallado.<br /><br /> La cola de componentes con problemas de entrega es una cola en el administrador de colas de la aplicación de envío para los mensajes caducados que no se hayan entregado.<br /><br /> El URI que es especificado por <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> debe utilizar el esquema de net.msmq.|  
|`deadLetterQueue`|Un valor <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A> que especifica qué tipo de cola de mensajes no enviados hay que usar, si hay alguna.<br /><br /> La cola de componentes con problemas de entrega es el lugar al que se transfieren los mensajes que no han podido ser entregados a la aplicación.<br /><br /> Para los mensajes que requieren la convicción `exactlyOnce` \(es decir, el atributo `exactlyOnce` está establecido en `true`\), este atributo tiene como valor predeterminado la cola de mensajes transaccionales no enviados para todo el sistema en MSMQ.<br /><br /> Para los mensajes que no requieren convicciones, este atributo tiene como valor predeterminado `null`.|  
|`durable`|Valor de tipo booleano que indica si el mensaje es duradero o volátil en la cola.  Un mensaje duradero sobrevive un bloqueo de administrador de cola, mientras que un mensaje volátil no.  Los mensajes volátiles son útiles cuando las aplicaciones requieren la menor latencia y toleran la pérdida de mensajes ocasional.  Si el atributo `exactlyOnce` está establecido en `true`, los mensajes deben ser duraderos.  De manera predeterminada, es `true`.|  
|`exactlyOnce`|Valor de tipo booleano que indica si cada mensaje procesado por este enlace sólo se entrega una vez.  Se notificará al remitente a continuación de los errores de la entrega.  Cuando `durable` es `false`, se omite este atributo y los mensajes se transfieren sin convicción de la entrega.  De manera predeterminada, es `true`.  Para obtener más información, consulta <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>.|  
|`maxBufferPoolSize`|Entero que especifica el tamaño máximo del grupo de búferes para este enlace.  El valor predeterminado es 8.|  
|`maxReceivedMessageSize`|Un entero positivo que define el tamaño del mensaje máximo, en bytes, incluyendo encabezados, que está procesado por este enlace.  El remitente de un mensaje que supere este límite recibirá un error SOAP.  El destinatario quita el mensaje y crea una entrada del evento en el registro de seguimiento.  El valor predeterminado es 65536.  Este límite en el tamaño del mensaje tiene como objetivo limitar la exposición a ataques de denegación de servicio \(DoS\).|  
|`maxRetryCycles`|Un entero que indica el número de ciclos de reintento utilizado por la característica de detección de mensaje dudoso.  Un mensaje se vuelve un mensaje dudoso cuando produce un error en todos los intentos de entrega de todos los ciclos.  El valor predeterminado es 3.  Para obtener más información, consulta <xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A>.|  
|`name`|Atributo necesario.  Cadena que contiene el nombre de configuración del enlace.  Este valor debe ser único porque se usa como identificación del enlace.  A partir de [!INCLUDE[netfx40_short](../../../../../includes/netfx40-short-md.md)], no es necesario que los enlaces y los comportamientos tengan nombre.  Para obtener más información sobre la configuración predeterminada, así como sobre enlaces y comportamientos sin nombre, vea [Configuración simplificada](../../../../../docs/framework/wcf/simplified-configuration.md) y [Configuración simplificada de los servicios de WCF](../../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md).|  
|`openTimeout`|Valor de la estructura <xref:System.TimeSpan> que especifica el intervalo de tiempo del que dispone una operación de apertura para completarse.  Este valor debe ser mayor o igual que <xref:System.TimeSpan.Zero>.  El valor predeterminado es 00:01:00.|  
|`QueueTransferProtocol`|Un valor <xref:System.ServiceModel.QueueTransferProtocol> válido que especifica el transporte del canal de comunicación en cola que este enlace utiliza.  MSMQ no admite el direccionamiento de Active Directory al utilizar el protocolo de mensajería de confianza SOAP.  Por consiguiente, no debe establecer este atributo como `Srmp` o `Srmps` cuando el atributo `u``seActiveDirectory` está establecido en `true`.|  
|`receiveErrorHandling`|Un valor <xref:System.ServiceModel.ReceiveErrorHandling> que especifica cómo se administran mensajes dudosos y que no se pueden enviar.|  
|`receiveRetryCount`|Un entero que especifica el número máximo de veces que el administrador de cola debería intentar enviar un mensaje antes de transferirlo a la cola de reintento.|  
|`receiveTimeout`|Un valor <xref:System.TimeSpan> que especifica el intervalo de tiempo del que dispone una operación de recepción para completarse.  Este valor debe ser mayor o igual que <xref:System.TimeSpan.Zero>.  El valor predeterminado es 00:10:00.|  
|`retryCycleDelay`|Un valor TimeSpan que especifica el tiempo de retardo entre los ciclos de reintento al intentar entregar un mensaje que no se pudo entregar inmediatamente.  El valor define sólo el tiempo de espera mínimo porque el tiempo de espera real puede ser más largo.  El valor predeterminado es 00:10:00.  Para obtener más información, consulta <xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A>.|  
|`sendTimeout`|Un valor <xref:System.TimeSpan> que especifica el intervalo de tiempo del que dispone una operación de envío para completarse.  Este valor debe ser mayor o igual que <xref:System.TimeSpan.Zero>.  El valor predeterminado es 00:01:00.|  
|`timeToLive`|Un valor TimeSpan que especifica cuánto tiempo son válidos los mensajes antes de expirar y ser colocados en la cola de mensajes no enviados.  El valor predeterminado es 1.00:00:00.<br /><br /> Este atributo se establece para asegurarse de que los mensajes que dependen del tiempo no se vuelvan obsoletos antes de ser procesados por las aplicaciones receptoras.  Un mensaje en una cola expira si no es consumido por la aplicación receptora dentro del intervalo de tiempo especificado.  Los mensajes caducados se envían a la cola especial llamada cola de mensajes no enviados.  La ubicación de la cola de mensajes no enviados se establece con el atributo `DeadLetterQueue` o con el valor predeterminado adecuado, basado en garantías.|  
|`usingActiveDirectory`|Valor de tipo booleano que especifica si las direcciones de la cola se deben convertir utilizando Active Directory.<br /><br /> Las direcciones de la cola de MSMQ pueden estar compuestas de nombres de ruta o nombres de formato directos.  Con un nombre de formato directo, MSMQ resuelve el nombre del equipo mediante DNS, NetBIOS o IP.  Con un nombre de ruta, MSMQ resuelve el nombre del equipo mediante Active Directory.<br /><br /> De forma predeterminada, el transporte de cola [!INCLUDE[indigo1](../../../../../includes/indigo1-md.md)] convierte el URI de una cola de mensajes en un nombre de formato directo.  Al establecer la propiedad `UseActiveDirectory` como true, una aplicación puede especificar que el transporte en cola debe resolver el nombre del equipo mediante Active Directory en lugar de DNS, NetBIOS o IP.|  
|`useMsmqTracing`|Valor de tipo booleano que especifica si los mensajes procesados por este enlace se deberían seguir paso a paso.  De manera predeterminada, es `false`.  Si se habilita la traza, los mensajes de informe se crean y envían a la cola de informes cada vez que el mensaje sale o llega a un equipo Message Queuing.|  
|`useSourceJournal`|Valor de tipo booleano que especifica las copias de mensajes procesadas por este enlace debería estar almacenado en el diario de origen.  De manera predeterminada, es `false`.<br /><br /> Las aplicaciones en cola que quieran mantener un registro de mensajes que han dejado la cola de salida del equipo pueden copiar los mensajes a una cola del diario.  Cuando un mensaje deja la cola de salida y se recibe una confirmación de que se ha recibido el mensaje en el equipo de destino, se mantiene una copia del mensaje en la cola del diario de sistema del equipo remitente.|  
  
### Elementos secundarios  
  
|Elemento|Descripción|  
|--------------|-----------------|  
|[\<readerQuotas\>](../Topic/%3CreaderQuotas%3E.md)|Define las restricciones en la complejidad de los mensajes SOAP que pueden ser procesados por los extremos configurados con este enlace.  Este elemento es del tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>.|  
|[\<seguridad\>](../../../../../docs/framework/configure-apps/file-schema/wcf/security-of-netmsmqbinding.md)|Define la configuración de seguridad del enlace.  Este elemento es del tipo <xref:System.ServiceModel.Configuration.NetMsmqSecurityElement>.|  
  
### Elementos primarios  
  
|Elemento|Descripción|  
|--------------|-----------------|  
|[\<enlaces\>](../../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)|Este elemento contiene una colección de enlaces estándar y personalizados.|  
  
## Comentarios  
 El enlace `netMsmqBinding` proporciona compatibilidad para poner en cola aprovechando Microsoft Message Queuing \(MSMQ\) como transporte y proporcionando compatibilidad para aplicaciones acopladas flexiblemente, aislamiento de errores, equilibrio de carga y operaciones desconectadas.  Para ver una descripción de estas características, vea [Colas en WCF](../../../../../docs/framework/wcf/feature-details/queues-in-wcf.md).  
  
## Ejemplo  
  
```  
<configuration>  
<system.ServiceModel>  
    <bindings>  
           <netMsmqBinding>  
                <binding   
                         closeTimeout="00:00:10"   
                         openTimeout="00:00:20"   
                         receiveTimeout="00:00:30"  
                         sendTimeout="00:00:40"  
                         deadLetterQueue="net.msmq://localhost/blah"   
                         durable="true"   
                         exactlyOnce="true"   
                         maxReceivedMessageSize="1000"  
                         maxRetries="11"  
                         maxRetryCycles="12"   
                         poisonMessageHandling="Disabled"   
                         rejectAfterLastRetry="false"   
                         retryCycleDelay="00:05:55"   
                         timeToLive="00:11:11"   
                         sourceJournal="true"  
                         useMsmqTracing="true"  
                         useActiveDirectory="true">  
                         <security>  
                             <message clientCredentialType="Windows" />  
                         </security>  
            </netMsmqBinding>  
    </bindings>  
</system.ServiceModel>  
</configuration>  
```  
  
## Vea también  
 <xref:System.ServiceModel.NetMsmqBinding>   
 <xref:System.ServiceModel.Configuration.NetMsmqBindingElement>   
 [\<enlace\>](../../../../../docs/framework/misc/binding.md)   
 [Enlaces](../../../../../docs/framework/wcf/bindings.md)   
 [Configuración de enlaces proporcionados por el sistema](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)   
 [Using Bindings to Configure Windows Communication Foundation Services and Clients](http://msdn.microsoft.com/es-es/bd8b277b-932f-472f-a42a-b02bb5257dfb)   
 [Colas en WCF](../../../../../docs/framework/wcf/feature-details/queues-in-wcf.md)