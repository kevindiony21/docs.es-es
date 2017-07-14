---
title: "C&#243;mo registrar y configurar un moniker de servicio | Microsoft Docs"
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
helpviewer_keywords: 
  - "COM [WCF], configura monikers de servicio"
  - "COM [WCF], registra monikers de servicio"
ms.assetid: e5e16c80-8a8e-4eef-af53-564933b651ef
caps.latest.revision: 20
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 20
---
# C&#243;mo registrar y configurar un moniker de servicio
Antes de utilizar el moniker de servicio [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] en una aplicación COM que posee un contrato con tipo, debe registrar los tipos con atributos necesarios en COM y configurar la aplicación COM y el moniker con la configuración de enlace necesaria.  
  
### Para registrar los tipos con atributos necesarios en COM  
  
1.  Utilice la herramienta [Herramienta de utilidad de metadatos de ServiceModel \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) para recuperar el contrato de metadatos del servicio [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  Con esto se genera el código fuente para un ensamblado de cliente [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] y un archivo de configuración de la aplicación cliente.  
  
2.  Asegúrese de que los tipos del ensamblado se marcan como `ComVisible`.  Para hacerlo, agregue el siguiente atributo al archivo AssemblyInfo.cs de su proyecto Visual Studio.  
  
    ```  
    [assembly: ComVisible(true)]  
    ```  
  
3.  Compile el cliente [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] administrado como un ensamblado con nombre seguro.  Para ello se es necesario firmar con un par de claves criptográficas.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Firmar un ensamblado con un nombre seguro](http://go.microsoft.com/fwlink/?LinkId=94874) en la Guía del desarrollador de .NET Framework.  
  
4.  Utilice la herramienta de registro de ensamblados \(Regasm.exe\) con la opción `/tlb` para registrar los tipos del ensamblado en COM.  
  
5.  Use la herramienta de la caché global de ensamblados \(Gacutil.exe\) para agregar el ensamblado a la caché global de ensamblados.  
  
    > [!NOTE]
    >  Firmar y agregar el ensamblado a la caché global de ensamblados son pasos opcionales, pero permiten simplificar el proceso de carga del ensamblado desde la ubicación correcta en el tiempo de ejecución.  
  
### Para configurar la aplicación COM y el moniker con la configuración de enlace necesaria  
  
-   Coloque las definiciones de enlace \(generadas por [Herramienta de utilidad de metadatos de ServiceModel \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) en el archivo de configuración de la aplicación cliente generado\), en el archivo de configuración de la aplicación cliente.  Por ejemplo, para un ejecutable de Visual Basic 6.0 denominado CallCenterClient.exe, la configuración debe encontrarse en un archivo denominado CallCenterConfig.exe.config en el mismo directorio que la aplicación ejecutable.  De este modo la aplicación cliente puede utilizar el moniker.  Tenga en cuenta que la configuración de enlace no es necesaria si utiliza uno de los tipos de enlace estándar proporcionados por [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].  
  
     El siguiente tipo está registrado:  
  
    ```  
    using System.ServiceModel;  
  
    ...  
  
    [ServiceContract]   
    public interface IMathService   
    {  
    [OperationContract]  
    public int Add(int x, int y);  
    [OperationContract]  
    public int Subtract(int x, int y);  
    }  
    ```  
  
     La aplicación se expone mediante un enlace `wsHttpBinding`.  Para el tipo y configuración de aplicación especificados, se utilizan las siguientes cadenas moniker de ejemplo.  
  
    ```  
    service4:address=http://localhost/MathService, binding=wsHttpBinding, bindingConfiguration=Binding1  
  
    ```  
  
     `or`  
  
    ```  
    service4:address=http://localhost/MathService, binding=wsHttpBinding, bindingConfiguration=Binding1, contract={36ADAD5A-A944-4d5c-9B7C-967E4F00A090}  
  
    ```  
  
     Puede utilizar cualquiera de estas cadenas moniker desde una aplicación de Visual Basic 6.0, siempre que agregue una referencia al ensamblado que contiene los tipos `IMathService`, como se muestra en el siguiente código de ejemplo.  
  
    ```  
    Dim MathProxy As IMathService  
    Dim result As Integer  
  
    Set MathProxy = GetObject( _  
            "service4:address=http://localhost/MathService, _  
            binding=wsHttpBinding, _  
            bindingConfiguration=Binding1")  
  
    result = MathProxy.Add(3, 5)  
  
    ```  
  
     En este ejemplo, la definición de la configuración del enlace `Binding1` se almacena en un archivo de configuración con un nombre adecuado, por ejemplo vb6appname.exe.config.  
  
    > [!NOTE]
    >  Puede utilizar un código similar en una aplicación C\#, C\+\+, o cualquier otra aplicación de lenguaje .NET.  
  
    > [!NOTE]
    >  : si el moniker es incorrecto o el servicio no está disponible, la llamada a `GetObject` devolverá el error "Sintaxis no válida."  Si recibe este error, asegúrese de que el moniker que está utilizando es correcto y el servicio está disponible.  
  
     Aunque este tema se centra en la utilización del moniker de servicio del código de VB 6.0, puede utilizar un moniker de servicio de otros lenguajes.  Si utiliza un moniker del código de C\+\+, deberá importar el ensamblado generado por Svcutil.exe con "no\_namespace named\_guids raw\_interfaces\_only", como se muestra en el siguiente código.  
  
    ```  
    #import "ComTestProxy.tlb" no_namespace named_guids  
    ```  
  
     De este modo se modifican las definiciones de la interfaz importada para que todos los métodos devuelvan `HResult`.  Cualquier otro valor devuelto se convierte en parámetros de salida.  La ejecución global de los métodos sigue siendo la misma.  Esto permite determinar la causa de una excepción al llamar a un método en el proxy.  Esta funcionalidad sólo está disponible en el código de C\+\+.  
  
## Vea también  
 [Herramienta de utilidad de metadatos de ServiceModel \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)