---
title: "How to: Verify the Digital Signatures of XML Documents | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "System.Security.Cryptography.SignedXml class"
  - "signatures, cryptographic"
  - "System.Security.Cryptography.RSACryptoServiceProvider class"
  - "verifying signatures"
  - "checking signatures"
  - "XML digital signatures"
  - "digital signatures, verifying"
ms.assetid: a4d5ceb1-b9f5-47e8-9e4a-a2b39110002f
caps.latest.revision: 8
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 8
---
# How to: Verify the Digital Signatures of XML Documents
Puede usar las clases del espacio de nombres <xref:System.Security.Cryptography.Xml> para comprobar datos XML firmados con una firma digital.  Las firmas XML digitales \(XMLDSIG\) permiten comprobar que los datos no se modificaron después de firmarlos.  Para obtener más información sobre el estándar XMLDSIG, consulte la especificación de World Wide Web Consortium \(W3C\) en http:\/\/www.w3.org\/TR\/xmldsig\-core\/.  
  
 En el ejemplo de código de este procedimiento se muestra cómo comprobar una firma digital XML que se encuentra en un elemento \<`Signature`\>.  En el ejemplo se recupera una clave pública RSA de un contenedor de claves y se usa la clave para comprobar la firma.  
  
 Para obtener información sobre cómo crear una firma digital que se pueda comprobar mediante esta técnica, consulte [How to: Sign XML Documents with Digital Signatures](../../../docs/standard/security/how-to-sign-xml-documents-with-digital-signatures.md).  
  
### Para comprobar la firma digital de un documento XML  
  
1.  Para comprobar el documento, debe usar la misma clave asimétrica que se empleó para la firma.  Cree un objeto <xref:System.Security.Cryptography.CspParameters> y especifique el nombre del contenedor de claves usado para la firma.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#2)]
     [!code-vb[HowToVerifyXMLDocumentRSA#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#2)]  
  
2.  Recupere la clave pública mediante la clase <xref:System.Security.Cryptography.RSACryptoServiceProvider>.  La clave se carga automáticamente desde el contenedor de claves por nombre al pasar el objeto <xref:System.Security.Cryptography.CspParameters> al constructor de la clase <xref:System.Security.Cryptography.RSACryptoServiceProvider>.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#3)]
     [!code-vb[HowToVerifyXMLDocumentRSA#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#3)]  
  
3.  Cree un objeto <xref:System.Xml.XmlDocument> cargando un archivo XML del disco.  El objeto <xref:System.Xml.XmlDocument> contiene el documento XML firmado que se va a comprobar.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#4)]
     [!code-vb[HowToVerifyXMLDocumentRSA#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#4)]  
  
4.  Cree un nuevo objeto <xref:System.Security.Cryptography.Xml.SignedXml> y pásele el objeto <xref:System.Xml.XmlDocument>.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#5)]
     [!code-vb[HowToVerifyXMLDocumentRSA#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#5)]  
  
5.  Busque el elemento \<`signature`\> y cree un nuevo objeto <xref:System.Xml.XmlNodeList>.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#6)]
     [!code-vb[HowToVerifyXMLDocumentRSA#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#6)]  
  
6.  Cargue el archivo XML del primer elemento \<`signature`\> en el objeto <xref:System.Security.Cryptography.Xml.SignedXml>.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#7)]
     [!code-vb[HowToVerifyXMLDocumentRSA#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#7)]  
  
7.  Compruebe la firma con el método <xref:System.Security.Cryptography.Xml.SignedXml.CheckSignature%2A> y la clave pública RSA.  Este método devuelve un valor booleano que indica éxito o error.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#8)]
     [!code-vb[HowToVerifyXMLDocumentRSA#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#8)]  
  
## Ejemplo  
 En este ejemplo se supone que un archivo llamado `"test.xml"` se encuentra en el mismo directorio que el programa compilado.  El archivo `"test.xml"` debe firmarse mediante las técnicas descritas en [How to: Sign XML Documents with Digital Signatures](../../../docs/standard/security/how-to-sign-xml-documents-with-digital-signatures.md).  
  
 [!code-csharp[HowToVerifyXMLDocumentRSA#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#1)]
 [!code-vb[HowToVerifyXMLDocumentRSA#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#1)]  
  
## Compilar el código  
  
-   Para compilar este ejemplo, debe incluir una referencia a `System.Security.dll`.  
  
-   Incluya los siguientes espacios de nombres: <xref:System.Xml>, <xref:System.Security.Cryptography> y <xref:System.Security.Cryptography.Xml>.  
  
## Seguridad de .NET Framework  
 Nunca almacene ni transfiera la clave privada de un par de claves asimétricas en texto sin formato.  Para obtener más información acerca de las claves criptográficas simétricas y asimétricas, consulte [Generating Keys for Encryption and Decryption](../../../docs/standard/security/generating-keys-for-encryption-and-decryption.md).  
  
 No inserte nunca una clave privada directamente en el código fuente.  Las claves insertadas se pueden leer fácilmente desde un ensamblado con el [Ildasm.exe \(IL Disassembler\)](../../../docs/framework/tools/ildasm-exe-il-disassembler.md) o abriendo el ensamblado en un editor de texto, como el Bloc de notas.  
  
## Vea también  
 <xref:System.Security.Cryptography.Xml>   
 [How to: Sign XML Documents with Digital Signatures](../../../docs/standard/security/how-to-sign-xml-documents-with-digital-signatures.md)