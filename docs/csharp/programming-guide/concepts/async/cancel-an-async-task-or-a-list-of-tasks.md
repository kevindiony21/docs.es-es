---
title: "Cancelar una tarea asincrónica o una lista de tareas (C#) | Microsoft Docs"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: eec32dbb-70ea-4c88-bd27-fa2e34546914
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 61f1ac22923ad637ba145b448f75e0f1a7e142b6
ms.lasthandoff: 03/13/2017

---
# <a name="cancel-an-async-task-or-a-list-of-tasks-c"></a>Cancelar una tarea asincrónica o una lista de tareas (C#)
Puede configurar un botón para cancelar una aplicación asincrónica si no quiere esperar a que termine. Mediante los ejemplos de este tema, puede agregar un botón de cancelación a una aplicación que descargue el contenido de un sitio web o una lista de sitios web.  
  
 En los ejemplos se usa la interfaz de usuario que se describe en [Ajustar una aplicación asincrónica (C#)](../../../../csharp/programming-guide/concepts/async/fine-tuning-your-async-application.md).  
  
> [!NOTE]
>  Para ejecutar los ejemplos, debe tener Visual Studio 2012 o posterior y .NET Framework 4.5 o posterior instalados en el equipo.  
  
##  <a name="BKMK_CancelaTask"></a> Cancelar una tarea  
 En el primer ejemplo se asocia el botón **Cancelar** a una sola tarea de descarga. Si elige el botón mientras la aplicación descarga contenido, se cancela la descarga.  
  
### <a name="downloading-the-example"></a>Descargar el ejemplo  
 Puede descargar el proyecto completo de Windows Presentation Foundation (WPF) en [Async Sample: Fine Tuning Your Application](http://go.microsoft.com/fwlink/?LinkId=255046) (Ejemplo asincrónico: Ajustar la aplicación [C# y Visual Basic]) y después seguir estos pasos.  
  
1.  Descomprima el archivo descargado y, a continuación, inicie Visual Studio.  
  
2.  En la barra de menús, elija **Archivo**, **Abrir**, **Proyecto o solución**.  
  
3.  En el cuadro de diálogo **Abrir proyecto**, abra la carpeta que contiene el código de ejemplo que ha descomprimido y, después, abra el archivo de solución (.sln) para AsyncFineTuningCS.  
  
4.  En el **Explorador de soluciones**, abra el menú contextual del proyecto **CancelATask** y, después, elija **Establecer como proyecto de inicio**.  
  
5.  Pulse la tecla F5 para ejecutar el proyecto.  
  
     Pulse las teclas Ctrl+F5 para ejecutar el proyecto sin depurarlo.  
  
 Si no quiere descargar el proyecto, puede revisar los archivos MainWindow.xaml.cs al final de este tema.  
  
### <a name="building-the-example"></a>Compilación del ejemplo  
 Los cambios siguientes agregan un botón **Cancelar** a una aplicación que descarga un sitio web. Si no quiere descargar ni generar el ejemplo, puede revisar el producto final en la sección "Ejemplos completos" al final de este tema. Los cambios en el código se marcan con asteriscos.  
  
 Para generar su propio ejemplo, paso a paso, siga las instrucciones de la sección "Descargar el ejemplo", pero elija **StarterCode** como **Proyecto de inicio** en lugar de **CancelATask**.  
  
 Luego, realice los cambios siguientes en el archivo MainWindow.xaml.cs del proyecto.  
  
1.  Declare una variable de `CancellationTokenSource`, `cts`, que esté en el ámbito de todos los métodos que acceden a ella.  
  
    ```cs  
    public partial class MainWindow : Window  
    {  
        // ***Declare a System.Threading.CancellationTokenSource.  
        CancellationTokenSource cts;  
    ```  
  
2.  Agregue el controlador de eventos siguiente para el botón **Cancelar**. El controlador de eventos usa el método <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=fullName> para informar a `cts` cuando el usuario solicita la cancelación.  
  
    ```cs  
    // ***Add an event handler for the Cancel button.  
    private void cancelButton_Click(object sender, RoutedEventArgs e)  
    {  
        if (cts != null)  
        {  
            cts.Cancel();  
        }  
    }  
    ```  
  
3.  Realice los siguientes cambios en el controlador de eventos para el botón **Iniciar**, `startButton_Click`.  
  
    -   Cree una instancia de `CancellationTokenSource`, `cts`.  
  
        ```cs  
        // ***Instantiate the CancellationTokenSource.  
        cts = new CancellationTokenSource();  
        ```  
  
    -   En la llamada a `AccessTheWebAsync`, que descarga el contenido de un sitio web especificado, envíe la propiedad <xref:System.Threading.CancellationTokenSource.Token%2A?displayProperty=fullName> de `cts` como argumento. La propiedad `Token` propaga el mensaje si se solicita la cancelación. Agregue un bloque catch que muestre un mensaje si el usuario decide cancelar la operación de descarga. En el código siguiente se muestran los cambios.  
  
        ```cs  
        try  
        {  
            // ***Send a token to carry the message if cancellation is requested.  
            int contentLength = await AccessTheWebAsync(cts.Token);  
            resultsTextBox.Text +=  
                String.Format("\r\nLength of the downloaded string: {0}.\r\n", contentLength);  
        }  
        // *** If cancellation is requested, an OperationCanceledException results.  
        catch (OperationCanceledException)  
        {  
            resultsTextBox.Text += "\r\nDownload canceled.\r\n";  
        }  
        catch (Exception)  
        {  
            resultsTextBox.Text += "\r\nDownload failed.\r\n";  
        }  
        ```  
  
4.  En `AccessTheWebAsync`, use la sobrecarga <xref:System.Net.Http.HttpClient.GetAsync%28System.String%2CSystem.Threading.CancellationToken%29?displayProperty=fullName> del método `GetAsync` en el tipo <xref:System.Net.Http.HttpClient> para descargar el contenido de un sitio web. Pase `ct`, el parámetro <xref:System.Threading.CancellationToken> de `AccessTheWebAsync`, como segundo argumento. El token lleva el mensaje si el usuario elige el botón **Cancelar**.  
  
     En el código siguiente se muestran los cambios en `AccessTheWebAsync`.  
  
    ```cs  
    // ***Provide a parameter for the CancellationToken.  
    async Task<int> AccessTheWebAsync(CancellationToken ct)  
    {  
        HttpClient client = new HttpClient();  
  
        resultsTextBox.Text +=  
            String.Format("\r\nReady to download.\r\n");  
  
        // You might need to slow things down to have a chance to cancel.  
        await Task.Delay(250);  
  
        // GetAsync returns a Task<HttpResponseMessage>.   
        // ***The ct argument carries the message if the Cancel button is chosen.  
        HttpResponseMessage response = await client.GetAsync("http://msdn.microsoft.com/library/dd470362.aspx", ct);  
  
        // Retrieve the website contents from the HttpResponseMessage.  
        byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
        // The result of the method is the length of the downloaded website.  
        return urlContents.Length;  
    }  
    ```  
  
5.  Si no lo cancela, el programa produce el resultado siguiente.  
  
    ```  
    Ready to download.  
    Length of the downloaded string: 158125.  
    ```  
  
     Si elige el botón **Cancelar** antes de que el programa termine de descargar el contenido, el programa produce el resultado siguiente.  
  
    ```  
    Ready to download.  
    Download canceled.  
    ```  
  
##  <a name="BKMK_CancelaListofTasks"></a> Cancelar una lista de tareas  
 Puede ampliar el ejemplo anterior para cancelar muchas tareas asociando la misma instancia de `CancellationTokenSource` a cada tarea. Si elige el botón **Cancelar**, cancela todas las tareas que aún no se han completado.  
  
### <a name="downloading-the-example"></a>Descargar el ejemplo  
 Puede descargar el proyecto completo de Windows Presentation Foundation (WPF) en [Async Sample: Fine Tuning Your Application](http://go.microsoft.com/fwlink/?LinkId=255046) (Ejemplo asincrónico: Ajustar la aplicación [C# y Visual Basic]) y después seguir estos pasos.  
  
1.  Descomprima el archivo descargado y, a continuación, inicie Visual Studio.  
  
2.  En la barra de menús, elija **Archivo**, **Abrir**, **Proyecto o solución**.  
  
3.  En el cuadro de diálogo **Abrir proyecto**, abra la carpeta que contiene el código de ejemplo que ha descomprimido y, después, abra el archivo de solución (.sln) para AsyncFineTuningCS.  
  
4.  En el **Explorador de soluciones**, abra el menú contextual del proyecto **CancelAListOfTasks** y, después, elija **Establecer como proyecto de inicio**.  
  
5.  Pulse la tecla F5 para ejecutar el proyecto.  
  
     Pulse las teclas Ctrl+F5 para ejecutar el proyecto sin depurarlo.  
  
 Si no quiere descargar el proyecto, puede revisar los archivos MainWindow.xaml.cs al final de este tema.  
  
### <a name="building-the-example"></a>Compilación del ejemplo  
 Para ampliar el ejemplo personalmente, paso a paso, siga las instrucciones de la sección "Descargar el ejemplo", pero elija **CancelATask** como el **Proyecto de inicio**. Agregue los siguientes cambios a ese proyecto. Los cambios en el programa se marcan con asteriscos.  
  
1.  Agregue un método para crear una lista de direcciones web.  
  
    ```cs  
    // ***Add a method that creates a list of web addresses.  
    private List<string> SetUpURLList()  
    {  
        List<string> urls = new List<string>   
        {   
            "http://msdn.microsoft.com",  
            "http://msdn.microsoft.com/library/hh290138.aspx",  
            "http://msdn.microsoft.com/library/hh290140.aspx",  
            "http://msdn.microsoft.com/library/dd470362.aspx",  
            "http://msdn.microsoft.com/library/aa578028.aspx",  
            "http://msdn.microsoft.com/library/ms404677.aspx",  
            "http://msdn.microsoft.com/library/ff730837.aspx"  
        };  
        return urls;  
    }  
    ```  
  
2.  Llame al método en `AccessTheWebAsync`.  
  
    ```cs  
    // ***Call SetUpURLList to make a list of web addresses.  
    List<string> urlList = SetUpURLList();  
    ```  
  
3.  Agregue el siguiente bucle en `AccessTheWebAsync` para procesar cada dirección web de la lista.  
  
    ```cs  
    // ***Add a loop to process the list of web addresses.  
    foreach (var url in urlList)  
    {  
        // GetAsync returns a Task<HttpResponseMessage>.   
        // Argument ct carries the message if the Cancel button is chosen.   
        // ***Note that the Cancel button can cancel all remaining downloads.  
        HttpResponseMessage response = await client.GetAsync(url, ct);  
  
        // Retrieve the website contents from the HttpResponseMessage.  
        byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
        resultsTextBox.Text +=  
            String.Format("\r\nLength of the downloaded string: {0}.\r\n", urlContents.Length);  
    }  
    ```  
  
4.  Como `AccessTheWebAsync` muestra las duraciones, el método no tiene que devolver nada. Quite la instrucción return y cambie el tipo de valor devuelto del método a <xref:System.Threading.Tasks.Task> en lugar de <xref:System.Threading.Tasks.Task%601>.  
  
<CodeContentPlaceHolder>10</CodeContentPlaceHolder>  
     Llame al método desde `startButton_Click` mediante una instrucción en lugar de una expresión.  
  
    ```cs  
    await AccessTheWebAsync(cts.Token);  
    ```  
  
5.  Si no lo cancela, el programa produce el resultado siguiente.  
  
    ```  
  
    Length of the downloaded string: 35939.  
  
    Length of the downloaded string: 237682.  
  
    Length of the downloaded string: 128607.  
  
    Length of the downloaded string: 158124.  
  
    Length of the downloaded string: 204890.  
  
    Length of the downloaded string: 175488.  
  
    Length of the downloaded string: 145790.  
  
    Downloads complete.  
  
    ```  
  
     Si elige el botón **Cancelar** antes de que se completen las descargas, la salida contiene las duraciones de las descargas completadas antes de la cancelación.  
  
    ```  
    Length of the downloaded string: 35939.  
  
    Length of the downloaded string: 237682.  
  
    Length of the downloaded string: 128607.  
  
    Downloads canceled.  
  
    ```  
  
##  <a name="BKMK_CompleteExamples"></a> Ejemplos completos  
 Las secciones siguientes contienen el código para cada uno de los ejemplos anteriores. Observe que debe agregar una referencia para <xref:System.Net.Http>.  
  
 Puede descargar los proyectos en [Async Sample: Fine Tuning Your Application](http://go.microsoft.com/fwlink/?LinkId=255046) (Ejemplo asincrónico: Ajustar la aplicación [C# y Visual Basic]).  
  
### <a name="cancel-a-task-example"></a>Ejemplo de cancelación de una tarea  
 El código siguiente es el archivo MainWindow.xaml.cs completo del ejemplo que cancela una sola tarea.  
  
```cs  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Threading.Tasks;  
using System.Windows;  
using System.Windows.Controls;  
using System.Windows.Data;  
using System.Windows.Documents;  
using System.Windows.Input;  
using System.Windows.Media;  
using System.Windows.Media.Imaging;  
using System.Windows.Navigation;  
using System.Windows.Shapes;  
  
// Add a using directive and a reference for System.Net.Http.  
using System.Net.Http;  
  
// Add the following using directive for System.Threading.  
  
using System.Threading;  
namespace CancelATask  
{  
    public partial class MainWindow : Window  
    {  
        // ***Declare a System.Threading.CancellationTokenSource.  
        CancellationTokenSource cts;  
  
        public MainWindow()  
        {  
            InitializeComponent();  
        }  
  
        private async void startButton_Click(object sender, RoutedEventArgs e)  
        {  
            // ***Instantiate the CancellationTokenSource.  
            cts = new CancellationTokenSource();  
  
            resultsTextBox.Clear();  
  
            try  
            {  
                // ***Send a token to carry the message if cancellation is requested.  
                int contentLength = await AccessTheWebAsync(cts.Token);  
                resultsTextBox.Text +=  
                    String.Format("\r\nLength of the downloaded string: {0}.\r\n", contentLength);  
            }  
            // *** If cancellation is requested, an OperationCanceledException results.  
            catch (OperationCanceledException)  
            {  
                resultsTextBox.Text += "\r\nDownload canceled.\r\n";  
            }  
            catch (Exception)  
            {  
                resultsTextBox.Text += "\r\nDownload failed.\r\n";  
            }  
  
            // ***Set the CancellationTokenSource to null when the download is complete.  
            cts = null;   
        }  
  
        // ***Add an event handler for the Cancel button.  
        private void cancelButton_Click(object sender, RoutedEventArgs e)  
        {  
            if (cts != null)  
            {  
                cts.Cancel();  
            }  
        }  
  
        // ***Provide a parameter for the CancellationToken.  
        async Task<int> AccessTheWebAsync(CancellationToken ct)  
        {  
            HttpClient client = new HttpClient();  
  
            resultsTextBox.Text +=  
                String.Format("\r\nReady to download.\r\n");  
  
            // You might need to slow things down to have a chance to cancel.  
            await Task.Delay(250);  
  
            // GetAsync returns a Task<HttpResponseMessage>.   
            // ***The ct argument carries the message if the Cancel button is chosen.  
            HttpResponseMessage response = await client.GetAsync("http://msdn.microsoft.com/library/dd470362.aspx", ct);  
  
            // Retrieve the website contents from the HttpResponseMessage.  
            byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
            // The result of the method is the length of the downloaded website.  
            return urlContents.Length;  
        }  
    }  
  
    // Output for a successful download:  
  
    // Ready to download.  
  
    // Length of the downloaded string: 158125.  
  
    // Or, if you cancel:  
  
    // Ready to download.  
  
    // Download canceled.  
}  
```  
  
### <a name="cancel-a-list-of-tasks-example"></a>Ejemplo de cancelación de una lista de tareas  
 El código siguiente es el archivo MainWindow.xaml.cs completo del ejemplo que cancela una lista de tareas.  
  
```cs  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Threading.Tasks;  
using System.Windows;  
using System.Windows.Controls;  
using System.Windows.Data;  
using System.Windows.Documents;  
using System.Windows.Input;  
using System.Windows.Media;  
using System.Windows.Media.Imaging;  
using System.Windows.Navigation;  
using System.Windows.Shapes;  
  
// Add a using directive and a reference for System.Net.Http.  
using System.Net.Http;  
  
// Add the following using directive for System.Threading.  
using System.Threading;  
  
namespace CancelAListOfTasks  
{  
    public partial class MainWindow : Window  
    {  
        // Declare a System.Threading.CancellationTokenSource.  
        CancellationTokenSource cts;  
  
        public MainWindow()  
        {  
            InitializeComponent();  
        }  
  
        private async void startButton_Click(object sender, RoutedEventArgs e)  
        {  
            // Instantiate the CancellationTokenSource.  
            cts = new CancellationTokenSource();  
  
            resultsTextBox.Clear();  
  
            try  
            {  
                await AccessTheWebAsync(cts.Token);  
                // ***Small change in the display lines.  
                resultsTextBox.Text += "\r\nDownloads complete.";  
            }  
            catch (OperationCanceledException)  
            {  
                resultsTextBox.Text += "\r\nDownloads canceled.";  
            }  
            catch (Exception)  
            {  
                resultsTextBox.Text += "\r\nDownloads failed.";  
            }  
  
            // Set the CancellationTokenSource to null when the download is complete.  
            cts = null;  
        }  
  
        // Add an event handler for the Cancel button.  
        private void cancelButton_Click(object sender, RoutedEventArgs e)  
        {  
            if (cts != null)  
            {  
                cts.Cancel();  
            }  
        }  
  
        // Provide a parameter for the CancellationToken.  
        // ***Change the return type to Task because the method has no return statement.  
        async Task AccessTheWebAsync(CancellationToken ct)  
        {  
            // Declare an HttpClient object.  
            HttpClient client = new HttpClient();  
  
            // ***Call SetUpURLList to make a list of web addresses.  
            List<string> urlList = SetUpURLList();  
  
            // ***Add a loop to process the list of web addresses.  
            foreach (var url in urlList)  
            {  
                // GetAsync returns a Task<HttpResponseMessage>.   
                // Argument ct carries the message if the Cancel button is chosen.   
                // ***Note that the Cancel button can cancel all remaining downloads.  
                HttpResponseMessage response = await client.GetAsync(url, ct);  
  
                // Retrieve the website contents from the HttpResponseMessage.  
                byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
                resultsTextBox.Text +=  
                    String.Format("\r\nLength of the downloaded string: {0}.\r\n", urlContents.Length);  
            }  
        }  
  
        // ***Add a method that creates a list of web addresses.  
        private List<string> SetUpURLList()  
        {  
            List<string> urls = new List<string>   
            {   
                "http://msdn.microsoft.com",  
                "http://msdn.microsoft.com/library/hh290138.aspx",  
                "http://msdn.microsoft.com/library/hh290140.aspx",  
                "http://msdn.microsoft.com/library/dd470362.aspx",  
                "http://msdn.microsoft.com/library/aa578028.aspx",  
                "http://msdn.microsoft.com/library/ms404677.aspx",  
                "http://msdn.microsoft.com/library/ff730837.aspx"  
            };  
            return urls;  
        }  
    }  
  
    // Output if you do not choose to cancel:  
  
    //Length of the downloaded string: 35939.  
  
    //Length of the downloaded string: 237682.  
  
    //Length of the downloaded string: 128607.  
  
    //Length of the downloaded string: 158124.  
  
    //Length of the downloaded string: 204890.  
  
    //Length of the downloaded string: 175488.  
  
    //Length of the downloaded string: 145790.  
  
    //Downloads complete.  
  
    // Sample output if you choose to cancel:  
  
    //Length of the downloaded string: 35939.  
  
    //Length of the downloaded string: 237682.  
  
    //Length of the downloaded string: 128607.  
  
    //Downloads canceled.  
}  
```  
  
## <a name="see-also"></a>Vea también  
 <xref:System.Threading.CancellationTokenSource>   
 <xref:System.Threading.CancellationToken>   
 [Programación asincrónica con Async y Await (C#)](../../../../csharp/programming-guide/concepts/async/index.md)   
 [Ajustar una aplicación asincrónica (C#)](../../../../csharp/programming-guide/concepts/async/fine-tuning-your-async-application.md)   
 [Async Sample: Fine Tuning Your Application](http://go.microsoft.com/fwlink/?LinkId=255046) (Ejemplo asincrónico: Ajustar la aplicación [C# y Visual Basic])