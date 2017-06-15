---
title: "Elemento &lt;message&gt; de &lt;wsDualHttpBinding&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 75101744-eed8-4d61-91f4-5fc4473a21f2
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# Elemento &lt;message&gt; de &lt;wsDualHttpBinding&gt;
Define la seguridad de nivel de mensaje para [\<wsDualHttpBinding\>](../../../../../docs/framework/configure-apps/file-schema/wcf/wsdualhttpbinding.md).  
  
## Sintaxis  
  
```  
  
<message   
      clientCredentialType="None/Windows/UserName/Certificate/CardSpace"  
     negotiateServiceCredential="Boolean"  
   algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"/>  
</message>  
```  
  
## Tipo  
 <xref:System.ServiceModel.MessageSecurityOverHttp>  
  
## Atributos y elementos  
 En las siguientes secciones se describen los atributos, los elementos secundarios y los elementos primarios  
  
### Atributos  
  
|Atributo|Descripción|  
|--------------|-----------------|  
|algorithmSuite|Opcional.  Establece el cifrado de mensajes y los algoritmos de encapsulado de claves.  La clase <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> determina los algoritmos y los tamaños de clave.  Estos algoritmos se asignan a los indicados en la especificación Lenguaje de directiva de seguridad \(WS\-SecurityPolicy\).<br /><br /> Consulte a continuación los valores posibles.  El valor predeterminado es `Basic256`.|  
|clientCredentialType|Opcional.  Especifica que el tipo de credenciales que se van a usar al realizar la autenticación del cliente mediante el modo de seguridad es `Message`.  Consulte a continuación los valores posibles.  De manera predeterminada, es `Windows`.<br /><br /> Este atributo es del tipo <xref:System.ServiceModel.MessageCredentialType>.|  
|negotiateServiceCredential|Opcional.  Un valor booleano que especifica si la credencial de servicio se proporciona en el cliente fuera de banda o se obtiene del servicio al cliente a través de un proceso de negociación.  Este tipo de negociación es un precursor del intercambio de mensajes usual.<br /><br /> Si el atributo `clientCredentialType` es None, Username o Certificate, establecer este atributo en `false` implica que el certificado del servicio está disponible en el cliente fuera de la banda y que el cliente necesita especificar el certificado del servicio \(utilizando [\<serviceCertificate\>](../../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md)\) en el comportamiento del servicio [\<serviceCredentials\>](../../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md).  Este modo es interoperable con pilas SOAP que implementan WS\-Trust y WS\-SecureConversation.<br /><br /> Si el atributo `ClientCredentialType` está establecido en `Windows`, establecer este atributo en `false` especifica la autenticación basada en Kerberos.  Esto significa que el cliente y el servicio deben formar parte del mismo dominio Kerberos.  Este modo es interoperable con pilas SOAP que implementan el perfil de token de Kerberos \(tal y como se define en OASIS WSS TC\), así como WS\-Trust y WS\-SecureConversation.  Cuando este atributo es `true`, produce una negociación .NET SOAP que tuneliza el intercambio de SPNego mediante mensajes SOAP.<br /><br /> De manera predeterminada, es `true`.|  
  
## atributo algorithmSuite  
  
|Valor|Descripción|  
|-----------|-----------------|  
|Basic128|Utilice el cifrado Aes128, Sha1 para la síntesis del mensaje y Rsa\-oaep\-mgf1p para el encapsulado de claves.|  
|Basic192|Utilice el cifrado Aes192, Sha1 para la síntesis del mensaje y Rsa\-oaep\-mgf1p para el encapsulado de claves.|  
|Basic256|Utilice el cifrado Aes256, Sha1 para la síntesis del mensaje y Rsa\-oaep\-mgf1p para el cifrado de claves.|  
|Basic256Rsa15|Utilice Aes256 para el cifrado de mensajes, Sha1 para la síntesis del mensaje y Rsa15 para el encapsulado de claves.|  
|Basic192Rsa15|Utilice Aes192 para el cifrado de mensajes, Sha1 para la síntesis del mensaje y Rsa15 para el encapsulado de claves.|  
|TripleDes|Utilice el cifrado TripleDes, Sha1 para la síntesis del mensaje y Rsa\-oaep\-mgf1p para el encapsulado de claves.|  
|Basic128Rsa15|Utilice Aes128 para el cifrado de mensajes, Sha1 para la síntesis del mensaje y Rsa15 para el encapsulado de claves.|  
|TripleDesRsa15|Utilice el cifrado TripleDes, Sha1 para la síntesis del mensaje y Rsa15 para el encapsulado de claves.|  
|Basic128Sha256|Utilice Aes256 para el cifrado de mensajes, Sha256 para la síntesis del mensaje y Rsa\-oaep\-mgf1p para el encapsulado de claves.|  
|Basic192Sha256|Utilice Aes192 para el cifrado de mensajes, Sha256 para la síntesis del mensaje y Rsa\-oaep\-mgf1p para el encapsulado de claves.|  
|Basic256Sha256|Utilice Aes256 para el cifrado de mensajes, Sha256 para la síntesis del mensaje y Rsa\-oaep\-mgf1p para el encapsulado de claves.|  
|TripleDesSha256|Utilice TripleDes para el cifrado de mensajes, Sha256 para la síntesis del mensaje y Rsa\-oaep\-mgf1p para el encapsulado de claves.|  
|Basic128Sha256Rsa15|Utilice Aes128 para el cifrado de mensajes, Sha256 para la síntesis del mensaje y Rsa15 para el encapsulado de claves.|  
|Basic192Sha256Rsa15|Utilice Aes192 para el cifrado de mensajes, Sha256 para la síntesis del mensaje y Rsa15 para el encapsulado de claves.|  
|Basic256Sha256Rsa15|Utilice Aes256 para el cifrado de mensajes, Sha256 para la síntesis del mensaje y Rsa15 para el encapsulado de claves.|  
|TripleDesSha256Rsa15|Utilice TripleDes para el cifrado de mensajes, Sha256 para la síntesis del mensaje y Rsa15 para el encapsulado de claves.|  
  
## Atributo clientCredentialType  
  
|Valor|Descripción|  
|-----------|-----------------|  
|Ninguna|Esto permite al servicio interactuar con clientes anónimos.  En el lado del servicio, esto indica que el servicio no requiere ninguna credencial del cliente.  En el cliente, esto indica que el cliente no proporciona ninguna credencial del cliente.|  
|Windows|Permite a los intercambios de SOAP estar bajo el contexto autenticado de una credencial de Windows.  Si el atributo `negotiateServiceCredential` está establecido en `true`, esto realiza una Negociación de la SSPI o Kerberos \(una norma interoperable\).|  
|UserName|Permite que el servicio requiera que el cliente se autentique con una credencial UserName.  [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] no permite enviar un resumen de contraseña ni derivar claves mediante una contraseña, así como tampoco usar dichas claves para seguridad de mensajes.  Como tal, [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] garantiza que el transporte sea seguro al usar credenciales UserName.  Este modo de credencial produce o bien un intercambio interoperable o bien una negociación no interoperable basada en el atributo `negotiateServiceCredential`.|  
|Certificado|Permite al servicio exigir la autenticación del cliente mediante un certificado.  Si se utiliza el modo de seguridad del mensaje y el atributo `negotiateServiceCredential` está establecido como `false`, se necesita proporcionar al cliente el certificado de servicio.|  
|IssuedToken|Especifica un token personalizado, normalmente emitido por un servicio de token de seguridad.|  
  
### Elementos secundarios  
 Ninguno.  
  
### Elementos primarios  
  
|Elemento|Descripción|  
|--------------|-----------------|  
|[\<seguridad\>](../../../../../docs/framework/configure-apps/file-schema/wcf/security-of-wsdualhttpbinding.md)|Define todas las funciones de seguridad de [\<wsDualHttpBinding\>](../../../../../docs/framework/configure-apps/file-schema/wcf/wsdualhttpbinding.md).|  
  
## Vea también  
 <xref:System.ServiceModel.Configuration.WSDualHttpSecurityElement.Message%2A>   
 <xref:System.ServiceModel.WSDualHttpSecurity.Message%2A>   
 <xref:System.ServiceModel.Configuration.MessageSecurityOverTcpElement>   
 <xref:System.ServiceModel.MessageSecurityOverHttp>   
 [Protección de servicios y clientes](../../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)   
 [Enlaces](../../../../../docs/framework/wcf/bindings.md)   
 [Configuración de enlaces proporcionados por el sistema](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)   
 [Using Bindings to Configure Windows Communication Foundation Services and Clients](http://msdn.microsoft.com/es-es/bd8b277b-932f-472f-a42a-b02bb5257dfb)   
 [\<enlace\>](../../../../../docs/framework/misc/binding.md)