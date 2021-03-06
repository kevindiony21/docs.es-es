---
title: "Proceso de contrataci&#243;n | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: d5fcacbb-c884-4b37-a5d6-02b1b8eec7b4
caps.latest.revision: 13
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 13
---
# Proceso de contrataci&#243;n
Este ejemplo muestra cómo implementar un proceso de negocio mediante actividades de mensajería y dos flujos de trabajo hospedados como servicios de flujo de trabajo.Estos flujos de trabajo son parte de la infraestructura de TI de una compañía ficticia denominada Contoso, Inc.  
  
 El proceso de flujo de trabajo `HiringRequest` \(implementado como <xref:System.Activities.Statements.Flowchart>\) solicita autorización a varios administradores de la organización.Para lograr este objetivo, el flujo de trabajo utiliza otros servicios existentes en la organización \(en nuestro caso, un servicio de bandeja de entrada y un servicio de datos de la organización implementados como servicios normales de [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]\).  
  
 El flujo de trabajo `ResumeRequest` \(implementado como <xref:System.Activities.Statements.Sequence>\) publica un anuncio de vacante en el sitio web externo de ofertas de trabajo de Contoso y administra la adquisición de currículos.Un anuncio de vacante está disponible en el sitio web externo durante un período fijo de tiempo \(hasta que expira un tiempo de espera\) o hasta que un empleado en Contoso decide quitarlo.  
  
 Este ejemplo muestra las siguientes características de [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]:  
  
-   Los flujos de trabajo <xref:System.Activities.Statements.Flowchart> y <xref:System.Activities.Statements.Sequence> para modelar los procesos de negocio.  
  
-   Servicios de flujo de trabajo.  
  
-   Actividades de mensajería.  
  
-   Correlación basada en contenido.  
  
-   Actividades personalizadas \(declarativas y basadas en código\).  
  
-   Persistencia del servidor SQL proporcionada por el sistema.  
  
-   <xref:System.Activities.Persistence.PersistenceParticipant> personalizado.  
  
-   Seguimiento personalizado.  
  
-   Seguimiento de eventos para Windows \(ETW\).  
  
-   Composición de actividades.  
  
-   Actividades <xref:System.Activities.Statements.Parallel>.  
  
-   Actividad <xref:System.Activities.Statements.CancellationScope>.  
  
-   Temporizadores duraderos \(actividad<xref:System.Activities.Statements.Delay>\).  
  
-   Transacciones.  
  
-   Más de un flujo de trabajo en la misma solución.  
  
> [!IMPORTANT]
>  Puede que los ejemplos ya estén instalados en su equipo.Compruebe el siguiente directorio \(predeterminado\) antes de continuar.  
>   
>  `<>InstallDrive:\WF_WCF_Samples`  
>   
>  Si no existe este directorio, vaya a la página de [ejemplos de Windows Communication Foundation \(WCF\) y Windows Workflow Foundation \(WF\) Samples para .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) para descargar todos los ejemplos de [!INCLUDE[wf1](../../../../includes/wf1-md.md)] y [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)].Este ejemplo se encuentra en el siguiente directorio.  
>   
>  `<unidadDeInstalación>:\WF_WCF_Samples\WF\Application\HiringProcess`  
  
## Descripción del proceso  
 Contoso, Inc.desea llevar un control detallado del número de personas de cada uno de sus departamentos.Por consiguiente, siempre que un empleado desea iniciar un nuevo proceso de contratación, debe someterse a la aprobación del proceso de solicitud de contratación antes de que la contratación suceda realmente.Este proceso se llama solicitud de proceso de contratación \(se define en el proyecto HiringRequestService\) y consiste de los siguientes pasos:  
  
1.  Un empleado \(el solicitante\) inicia la solicitud del proceso de contratación.  
  
2.  El jefe del solicitante debe aprobar la solicitud:  
  
    1.  El jefe puede rechazar la solicitud.  
  
    2.  El jefe puede devolver la solicitud al solicitante para obtener información adicional:  
  
        1.  El solicitante revisa y devuelve la solicitud al jefe.  
  
    3.  El jefe puede dar su aprobación.  
  
3.  Después de que el jefe del solicitante da su aprobación, el propietario del departamento debe aprobar la solicitud:  
  
    1.  El propietario del departamento puede rechazarla.  
  
    2.  El propietario del departamento puede aprobarla.  
  
4.  Después de que el propietario del departamento da su aprobación, el proceso requiere la aprobación de dos responsables de Recursos Humanos o el CEO:  
  
    1.  El proceso puede pasar al estado de aceptado o rechazado.  
  
    2.  Si se acepta el proceso, se inicia una nueva instancia del flujo de trabajo `ResumeRequest` \(`ResumeRequest` se vincula a HiringRequest.csproj a través de una referencia de servicio\).  
  
 Cuando los responsables aprueban la contratación de un nuevo empleado, Recursos Humanos debe encontrar el candidato adecuado.El segundo flujo de trabajo \(`ResumeRequest`, definido en ResumeRequestService.csproj\) realiza este proceso.Este flujo de trabajo define el proceso para enviar una publicación de vacante con una oferta de trabajo al sitio web de ofertas de trabajo de Contoso, recibe currículos de los solicitantes y supervisa el estado de la publicación de vacante.Las vacantes están disponibles durante un período de tiempo fijo \(hasta que expire\) o hasta que un empleado en Contoso decida quitarla.El flujo de trabajo `ResumeRequest` se compone de los siguientes pasos:  
  
1.  Un empleado de Contoso escribe la información sobre la vacante y una duración de tiempo de espera.Una vez que el empleado escribe esta información, la vacante se publica en el sitio web de ofertas de trabajo.  
  
2.  Una vez publicada la información, las partes interesadas pueden enviar sus currículos.Cuando se envía un curriculum, se almacena en un registro vinculado al puesto vacante.  
  
3.  Los solicitantes pueden enviar los currículos hasta que expire el tiempo de espera o alguien del departamento de Recursos Humanos de Contoso decida quitar la vacante deteniendo el proceso.  
  
## Proyectos del ejemplo  
 En la siguiente tabla se muestran los proyectos de la solución de ejemplo.  
  
|Proyecto|Descripción|  
|--------------|-----------------|  
|ContosoHR|Contiene contratos de datos, objetos de negocios y clases de repositorio.|  
|HiringRequestService|Contiene la definición del flujo de trabajo del proceso de solicitud de contratación.<br /><br /> Este proyecto se implementa como una aplicación de consola que autohospeda el flujo de trabajo \(archivo xaml\) como servicio.|  
|ResumeRequestService|Servicio de flujo de trabajo que recopila los currículos de los candidatos hasta que expira un tiempo de espera o alguien decide que el proceso tiene que detenerse.<br /><br /> Este proyecto se implementa como un servicio de flujo de trabajo declarativo \(xamlx\).|  
|OrgService|Servicio que expone la información de la organización \(empleados, vacantes, tipos de vacantes y departamentos\).Puede pensar en este servicio como el módulo de organización de la compañía de un plan de recursos empresariales \(ERP\).<br /><br /> Este proyecto se implementa como una aplicación de consola que expone un servicio de [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)].|  
|InboxService|Bandeja de entrada que contiene las tareas procesables para los empleados.<br /><br /> Este proyecto se implementa como una aplicación de consola que expone un servicio WCF.|  
|InternalClient|Aplicación web para interactuar con el proceso.Los usuarios pueden iniciar, participar y ver sus flujos de trabajo de HiringProcess.Con esta aplicación, también pueden iniciar y supervisar los procesos ResumeRequest.<br /><br /> Este sitio se implementa para ser interno a la intranet de Contoso.Este proyecto se implementa como un sitio web de ASP.NET.|  
|CareersWebSite|Sitio web externo que expone las vacantes en Contoso.Cualquier candidato potencial puede navegar a este sitio y enviar un curriculum.|  
  
## Resumen de características  
 La siguiente tabla describe cómo se usa cada característica en este ejemplo.  
  
|Característica|Descripción|Proyecto|  
|--------------------|-----------------|--------------|  
|Diagrama de flujo|El proceso de negocio se representa como un diagrama de flujo.Esta descripción de diagrama de flujo representa el proceso de la misma manera en la que una empresa lo habría dibujado en una pizarra.|HiringRequestService|  
|Servicios de flujo de trabajo|El diagrama de flujo con la definición del proceso se hospeda en un servicio \(en este ejemplo, el servicio se hospeda en una aplicación de consola\).|HiringRequestService|  
|Actividades de mensajería|El diagrama de flujo utiliza actividades de mensajería de dos maneras:<br /><br /> -   Para obtener información del usuario \(para recibir las decisiones e información relacionada en cada paso de aprobación\).<br />-   Para interactuar con otros servicios existentes \(InboxService y OrgDataService, que se usan a través de las referencias del servicio\).|HiringRequestService|  
|Correlación basada en contenido|Los mensajes de aprobación se correlacionan por la propiedad ID de la solicitud de contratación:<br /><br /> -   Cuando se inicia un proceso, el identificador de correlación se inicializa con el identificador de la solicitud.<br />-   Los mensajes de la aprobación de entrada se correlacionan por su identificador \(el primer parámetro de cada mensaje de aprobación es el identificador de la solicitud\).|HiringRequestService \/ ResumeRequestService|  
|Actividades personalizadas \(declarativas y basadas en código\).|Hay varias actividades personalizadas en este ejemplo:<br /><br /> -   `SaveActionTracking`: esta actividad emite un <xref:System.Activities.Tracking.TrackingRecord> personalizado \(mediante <xref:System.Activities.NativeActivityContext.Track%2A>\).Esta actividad se ha creado utilizando código imperativo que extiende <xref:System.Activities.NativeActivity>.<br />-   `GetEmployeesByPositionTypes`: esta actividad recibe una lista de identificadores de tipos de puestos de trabajo y devuelve una lista de personas que ocupan ese puesto en Contoso.Esta actividad se ha creado de forma declarativa \(con el diseñador de actividades\).<br />-   `SaveHiringRequestInfo`: esta actividad guarda la información de `HiringRequest` \(mediante `HiringRequestRepository.Save`\).Esta actividad se ha creado utilizando código imperativo que extiende <xref:System.Activities.CodeActivity>.|HiringRequestService|  
|Persistencia de SQL Server proporcionada por el sistema.|La instancia de <xref:System.ServiceModel.Activities.WorkflowServiceHost> que hospeda la definición del proceso de Diagrama de flujo se configura para utilizar la persistencia de SQL Server proporcionada por el sistema.|HiringRequestService \/ ResumeRequestService|  
|Seguimiento personalizado|En el ejemplo se incluye un participante de seguimiento personalizado que guarda el historial de `HiringRequestProcess` \(registra qué acción se ha realizado, quién la ha realizado y cuándo se ha realizado\).El código fuente está en la carpeta Tracking de HiringRequestService.|HiringRequestService|  
|Seguimiento ETW|El seguimiento de ETW proporcionado por el sistema se configura en el archivo App.config en el servicio HiringRequestService.|HiringRequestService|  
|Composición de actividades|La definición del proceso utiliza la composición libre de <xref:System.Activities.Activity>.El Diagrama de flujo contiene varias actividades Sequence y Parallel que al mismo tiempo contienen otras actividades \(y así sucesivamente\).|HiringRequestService|  
|Actividades paralelas|-   <xref:System.Activities.Statements.ParallelForEach%601> se utiliza para registrarse en la Bandeja de entrada del CEO y los responsables de Recursos Humanos en paralelo \(a la espera del paso de aprobación de los dos responsables de Recursos Humanos\).<br />-   <xref:System.Activities.Statements.Parallel> se utiliza para realizar algunas tareas de limpieza en los pasos Completed y Rejected.|HiringRequestService|  
|Cancelación de modelo|El Diagrama de flujo utiliza <xref:System.Activities.Statements.CancellationScope> para crear el comportamiento de cancelación \(en este caso, se lleva a cabo alguna limpieza\).|HiringRequestService|  
|Participante de persistencia del cliente|`HiringRequestPersistenceParticipant` guarda los datos de una variable de flujo de trabajo en una tabla almacenada en la base de datos de Recursos Humanos de Contoso.|HiringRequestService|  
|Servicios de flujo de trabajo|`ResumeRequestService` se implementa utilizando los servicios de flujo de trabajo.La definición del Flujo de trabajo y la información del servicio se encuentran en ResumeRequestService.xamlx.El servicio se configura para utilizar la persistencia y el seguimiento.|ResumeRequestService|  
|Temporizadores duraderos|`ResumeRequestService` utiliza temporizadores duraderos para definir la duración de la publicación de vacante \(una vez que expira el tiempo de espera, se cierra la publicación de vacante\).|ResumeRequestService|  
|Transacciones|<xref:System.Activities.Statements.TransactionScope> se utiliza para garantizar la coherencia de los datos dentro de la ejecución de varias actividades \(cuando se recibe un nuevo curriculum vitae\).|ResumeRequestService|  
|Transacciones|El participante de persistencia personalizado \(`HiringRequestPersistenceParticipant`\) y el participante de seguimiento personalizado \(`HistoryFileTrackingParticipant`\) usan la misma transacción.|HiringRequestService|  
|Utilizar [!INCLUDE[wf1](../../../../includes/wf1-md.md)] en aplicaciones [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)].|Los flujos de trabajo están accesibles desde dos aplicaciones [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)].|InternalClient \/ CareersWebSite|  
  
## Almacenamiento de datos  
 Los datos están almacenados en una base de datos de SQL Server llamada `ContosoHR` \(el script para crear esta base de datos se encuentra en la carpeta `DbSetup` \).Las instancias del flujo de trabajo se almacenan en una base de datos de SQL Server llamada `InstanceStore` \(los scripts para crear el almacén de instancias forman parte de la distribución de [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)]\).  
  
 Ambas bases de datos se crean ejecutando el script Setup.cmd desde un símbolo del sistema de [!INCLUDE[vs_current_short](../../../../includes/vs-current-short-md.md)].  
  
## Ejecutar el ejemplo  
  
#### Para crear las bases de datos  
  
1.  Abra un símbolo del sistema de [!INCLUDE[vs_current_short](../../../../includes/vs-current-short-md.md)].  
  
2.  Navegue hasta la carpeta del ejemplo.  
  
3.  Ejecute Setup.cmd.  
  
4.  Compruebe que las dos bases de datos, `ContosoHR` e `InstanceStore`, se crearon en SQL Express.  
  
#### Para configurar la solución para su ejecución  
  
1.  Ejecute [!INCLUDE[vs_current_short](../../../../includes/vs-current-short-md.md)] como administrador.Abra HiringRequest.sln.  
  
2.  Haga clic con el botón secundario en la solución en el **Explorador de soluciones** y seleccione **Propiedades**.  
  
3.  Seleccione la opción **Proyectos de inicio múltiples** y establezca **CareersWebSite**, **InternalClient**, **HiringRequestService** y **ResumeRequestService** en **Iniciar**.Deje **ContosoHR**, **InboxService** y **OrgService** como Ninguno.  
  
4.  Compile la solución presionando CTRL\+MAYÚS\+B.Compruebe que la compilación se ha realizado correctamente.  
  
#### Para ejecutar la solución  
  
1.  Cuando se compile la solución, presione CTRL\+F5 para ejecutarla sin depuración.Compruebe que todos los servicios se han iniciado.  
  
2.  Haga clic con el botón secundario en **InternalClient** en la solución y, a continuación, seleccione **Ver en el explorador**.Se muestra la página predeterminada para `InternalClient`.Asegúrese de que los servicios se están ejecutando y haga clic en el vínculo.  
  
3.  Se muestra el módulo **HiringRequest**.Puede seguir el escenario que aquí se detalla.  
  
4.  Cuando `HiringRequest` se haya completado, puede iniciar `ResumeRequest`.Puede seguir el escenario que aquí se detalla.  
  
5.  Cuando se expone `ResumeRequest`, está disponible en el sitio web público \(sitio web de ofertas de trabajo de Contoso\).Para ver la publicación de vacante \(y solicitar el puesto\), navegue hasta el sitio web de ofertas de trabajo \(Careers\).  
  
6.  Haga clic con el botón secundario en **CareersWebSite** en la solución y seleccione **Ver en el explorador**.  
  
7.  Vuelva a `InternalClient` haciendo clic con el botón secundario en **InternalClient** en la solución y seleccionando **Ver en el explorador**.  
  
8.  Vaya a la sección **JobPostings** haciendo clic en el vínculo **Job Postings** en el menú superior de la bandeja de entrada.Puede seguir el escenario que aquí se detalla.  
  
## Escenarios  
  
### Solicitud de contratación  
  
1.  Michael Alejandro \(Ingeniero de software\) desea solicitar una nueva vacante para contratar a un ingeniero de software en Pruebas \(SDET\), en el departamento de Ingeniería, que tenga 3 años de experiencia en C\# por lo menos.  
  
2.  Después de crearla, la solicitud aparece en la bandeja de entrada de Michael \(haga clic en **Actualizar** si no ve la solicitud\), que espera la aprobación de Peter Brehm, el jefe de Michael.  
  
3.  Peter desea actuar sobre la solicitud de Michael.Piensa que el puesto exige 5 años de experiencia en C\# en lugar de 3, de modo que devuelve sus comentarios para su revisión.  
  
4.  Michael ve un mensaje de su jefe en la bandeja de entrada y desea tomar parte.Michael ve el historial de la solicitud de vacante y está de acuerdo con Peter.Michael modifica la descripción para exigir 5 años de experiencia en C\# y acepta la modificación.  
  
5.  Peter actúa sobre la solicitud modificada de Michael y la acepta.Ahora, el director de Ingeniería, Tsvi Reiter, debe aprobar la solicitud.  
  
6.  Tsvi Reiter desea agilizar la solicitud, de modo que incluye un comentario para decir que la solicitud es urgente y la acepta.  
  
7.  Ahora, la solicitud tiene que ser aprobada por dos responsables de Recursos Humanos o el CEO.El CEO, Brian Richard Goldstein, ve la solicitud urgente de Tsvi.Actúa sobre la solicitud, aceptándola, evitando así la aprobación de los dos responsables de Recursos Humanos.  
  
8.  La solicitud se quita de la bandeja de entrada de Michael y comienza ahora el proceso de contratación de un SDET.  
  
### Iniciar la solicitud de curriculum vitae  
  
1.  Ahora, el puesto de trabajo está a la espera de su publicación en un sitio web externo donde las personas puedan solicitarlo \(puede verlo al hacer clic en el vínculo **Job Postings** \).Actualmente, el puesto de trabajo está en manos de un representante de Recursos Humanos, que es el responsable de terminarlo y publicarlo.  
  
2.  Recursos Humanos desea modificar este puesto de trabajo \(haciendo clic en el vínculo **Editar** \) para establecer un tiempo de espera de 60 minutos \(en la vida real, podrían ser días o semanas\).El tiempo de espera permite retirar el puesto de trabajo del sitio web externo según el tiempo especificado.  
  
3.  Después de guardar el puesto de trabajo editado, aparece en la pestaña **Receiving Resumes** \(actualice la página web para ver el nuevo puesto de trabajo\).  
  
### Recopilar currículos  
  
1.  El puesto de trabajo debería aparecer en el sitio web externo.Si una persona está interesada en solicitar el puesto, puede hacerlo y enviar su curriculum vitae.  
  
2.  Si regresa al servicio Job Postings List, puede "ver los currículos" que se han recopilado hasta ahora.  
  
3.  Recursos humanos también puede dejar de recopilar currículos \(una vez que se ha identificado el candidato correcto\).  
  
## Solución de problemas  
  
1.  Asegúrese de que está ejecutando [!INCLUDE[vs_current_short](../../../../includes/vs-current-short-md.md)] con privilegios de administrador.  
  
2.  Si la solución no se compila, compruebe lo siguiente:  
  
    -   La referencia a `ContosoHR` se encuentra en los proyectos `InternalClient` o `CareersWebSite`.  
  
3.  Si la solución no se ejecuta, compruebe lo siguiente:  
  
    1.  Todos los servicios se están ejecutando.  
  
    2.  Las referencias del servicio están actualizadas.  
  
        1.  Abra la carpeta App\_WebReferences.  
  
        2.  Haga clic con el botón secundario en **Contoso** y seleccione **Update Web\/Service References**.  
  
        3.  Recompile la solución presionando CTRL\+MAYÚS\+B en [!INCLUDE[vs_current_short](../../../../includes/vs-current-short-md.md)].  
  
## Desinstalación  
  
1.  Para eliminar el almacén de instancias de SQL Server, ejecute el archivo Cleanup.bat, situado en la carpeta DbSetup.  
  
2.  Elimine el código fuente de su disco duro.  
  
## Vea también