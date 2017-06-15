---
title: "C&#243;mo crear un servicio que emplee un validador de certificado personalizado | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "WCF, autenticación"
ms.assetid: bb0190ff-0738-4e54-8d22-c97d343708bf
caps.latest.revision: 14
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 14
---
# C&#243;mo crear un servicio que emplee un validador de certificado personalizado
En este tema se muestra cómo implementar un validador del certificado personalizado y cómo configurar el cliente o credenciales del servicio para reemplazar la lógica de validación de certificado predeterminada por el validador del certificado personalizado.  
  
 Si se utiliza el certificado X.509 para autenticar un cliente o servicio, [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] utiliza de forma predeterminada el almacén de certificados de Windows y Crypto API para validar el certificado y asegurarse que es de confianza.  La funcionalidad integrada de validación del certificado no es suficiente y a veces se debe cambiar.  [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] proporciona una manera sencilla de cambiar la lógica de validación permitiendo a los usuarios agregar un validador personalizado de certificado.  Si se especifica un validador de certificado personalizado, [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] no utiliza la lógica de validación de certificado integrada, pero se basa  en el validador personalizado en su lugar.  
  
## Procedimientos  
  
#### Crear un validador de certificado personalizado  
  
1.  Defina una clase nueva derivada a partir de <xref:System.IdentityModel.Selectors.X509CertificateValidator>.  
  
2.  Implemente el método <xref:System.IdentityModel.Selectors.X509CertificateValidator.Validate%2A> abstracto.  El certificado que se debe validar se pasa como un argumento al método.  Si el certificado pasado no es válido según la lógica de la validación, este método inicia <xref:System.IdentityModel.Tokens.SecurityTokenValidationException>.  Si el certificado es válido, el método vuelve al autor de la llamada.  
  
    > [!NOTE]
    >  Para devolver errores de autenticación al cliente, genere una <xref:System.ServiceModel.FaultException> en el método <xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A>.  
  
 [!code-csharp[c_CustomCertificateValidator#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcertificatevalidator/cs/source.cs#2)]
 [!code-vb[c_CustomCertificateValidator#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcertificatevalidator/vb/source.vb#2)]  
  
#### Especificar un validador de certificado personalizado en configuración de servicio  
  
1.  Agregue un elemento [\<comportamientos\>](../../../../docs/framework/configure-apps/file-schema/wcf/behaviors.md)[\<serviceBehaviors\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicebehaviors.md)[\<system.serviceModel\>](../../../../docs/framework/configure-apps/file-schema/wcf/system-servicemodel.md).  
  
2.  Agregue [\<comportamiento\>](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) y defina el atributo `name` en un valor adecuado.  
  
3.  Agregue [\<serviceCredentials\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md)`<behavior>` al elemento .  
  
4.  Agregue un elemento `<clientCertificate>` al elemento `<serviceCredentials>`.  
  
5.  Agregue [\<authentication\>](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-clientcertificate-element.md)`<clientCertificate>` al elemento .  
  
6.  Defina el atributo `customCertificateValidatorType` en el tipo de validador.  El ejemplo siguiente define el atributo en el espacio de nombres y nombre del tipo.  
  
7.  Defina el atributo `certificateValidationMode` a `Custom`.  
  
    ```  
    <configuration>  
     <system.serviceModel>  
      <behaviors>  
       <serviceBehaviors>  
        <behavior name="ServiceBehavior">  
         <serviceCredentials>  
          <clientCertificate>  
          <authentication certificateValidationMode="Custom" customCertificateValidatorType="Samples.MyValidator, service" />  
          </clientCertificate>  
         </serviceCredentials>  
        </behavior>  
       </serviceBehaviors>  
      </behaviors>  
    </system.serviceModel>  
    </configuration>  
    ```  
  
#### Especificar un validador de certificado personalizado mediante la configuración del cliente  
  
1.  Agregue un elemento [\<comportamientos\>](../../../../docs/framework/configure-apps/file-schema/wcf/behaviors.md)[\<serviceBehaviors\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicebehaviors.md)[\<system.serviceModel\>](../../../../docs/framework/configure-apps/file-schema/wcf/system-servicemodel.md).  
  
2.  Agregue un elemento [\<endpointBehaviors\>](../../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md).  
  
3.  Agregue un elemento `<behavior>``name y establezca el atributo`  en un valor adecuado.  
  
4.  Agregue un elemento [\<clientCredentials\>](../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md).  
  
5.  Agregue [\<serviceCertificate\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md).  
  
6.  Agregue [\<autenticación\>](../../../../docs/framework/configure-apps/file-schema/wcf/authentication-of-servicecertificate-element.md) como se muestra en el ejemplo siguiente.  
  
7.  Defina el atributo `customCertificateValidatorType` en el tipo de validador.  
  
8.  Defina el atributo `certificateValidationMode` a `Custom`.  El ejemplo siguiente define el atributo en el espacio de nombres y nombre del tipo.  
  
    ```  
    <configuration>  
     <system.serviceModel>  
      <behaviors>  
       <endpointBehaviors>  
        <behavior name="clientBehavior">  
         <clientCredentials>  
          <serviceCertificate>  
           <authentication certificateValidationMode="Custom"   
                  customCertificateValidatorType=  
             "Samples.CustomX509CertificateValidator, client"/>  
          </serviceCertificate>  
         </clientCredentials>  
        </behavior>  
       </endpointBehaviors>  
      </behaviors>  
     </system.serviceModel>  
    </configuration>  
    ```  
  
#### Especificar un validador de certificado personalizado mediante el código en el servicio  
  
1.  Especifique el validador de certificado personalizado en la propiedad <xref:System.ServiceModel.Description.ServiceCredentials.ClientCertificate%2A>.  Puede obtener acceso a las credenciales de servicio mediante la propiedad <xref:System.ServiceModel.ServiceHostBase.Credentials%2A>.  
  
2.  Establezca la propiedad <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication.CertificateValidationMode%2A> en <xref:System.ServiceModel.Security.X509CertificateValidationMode>.  
  
 [!code-csharp[c_CustomCertificateValidator#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcertificatevalidator/cs/source.cs#1)]
 [!code-vb[c_CustomCertificateValidator#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcertificatevalidator/vb/source.vb#1)]  
  
#### Especificar un validador de certificado personalizado mediante el código en el cliente  
  
1.  Especifique el validador de certificado personalizado mediante la propiedad <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CustomCertificateValidator%2A>.  Puede obtener acceso a las credenciales de cliente mediante la propiedad <xref:System.ServiceModel.ServiceHostBase.Credentials%2A>.  \(La clase de cliente generada por [Herramienta de utilidad de metadatos de ServiceModel \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) siempre deriva a partir de la clase <xref:System.ServiceModel.ClientBase%601>.\)  
  
2.  Establezca la propiedad <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A> en <xref:System.ServiceModel.Security.X509CertificateValidationMode>.  
  
## Ejemplo  
  
### Descripción  
 El ejemplo siguiente muestra una implementación de un validador de certificado personalizado y su uso en el servicio.  
  
### Código  
 [!code-csharp[c_CustomCertificateValidator#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customcertificatevalidator/cs/source.cs#3)]
 [!code-vb[c_CustomCertificateValidator#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customcertificatevalidator/vb/source.vb#3)]  
  
## Vea también  
 <xref:System.IdentityModel.Selectors.X509CertificateValidator>