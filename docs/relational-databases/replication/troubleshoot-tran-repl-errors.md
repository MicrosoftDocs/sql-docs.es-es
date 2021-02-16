---
title: Detección de errores con replicación transaccional
description: Describe cómo localizar e identificar errores con la replicación transaccional, así como la metodología de solución de problemas para abordar problemas con la replicación.
ms.custom: seo-lt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], tutorials
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d39286c1bc228a9da76a163a39e76dbb2022d807
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348641"
---
# <a name="troubleshooter-find-errors-with-sql-server-transactional-replication"></a>Solucionador de problemas: Búsqueda de errores con la replicación transaccional de SQL Server 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

La solución de problemas de errores de replicación puede resultar frustrante sin un conocimiento básico de cómo funciona la replicación transaccional. El primer paso para crear una publicación es hacer que el Agente de instantáneas cree la instantánea y la guarde en la carpeta de instantáneas. Después, el Agente de distribución aplica la instantánea al suscriptor. 

Este proceso crea la publicación y la coloca en el estado de *sincronización*. La sincronización funciona en tres fases:
1. Las transacciones se producen en los objetos que se replican y se marcan como "para replicación" en el registro de transacciones. 
2. El Agente de registro del LOG examina el registro de transacciones y busca las transacciones marcadas "para replicación". Después, estas transacciones se guardan en la base de datos de distribución. 
3. El Agente de distribución examina la base de datos de distribución mediante el subproceso de lectura. Después, con el subproceso de escritura, este agente se conecta al suscriptor para aplicar los cambios en él.

En cualquier paso de este proceso se pueden producir errores. La búsqueda de esos errores puede ser el aspecto más complejo de la solución de problemas de sincronización. Afortunadamente, el uso del Monitor de replicación facilita este proceso. 

>[!NOTE]
> - El propósito de esta guía de solución de problemas es enseñar la metodología de solución de problemas. No se ha diseñado para resolver errores específicos, sino para ofrecer instrucciones generales para buscar errores con la replicación. Se proporcionan algunos ejemplos específicos, pero su resolución puede variar en función del entorno. 
> - Los errores que se proporcionan esta guía como ejemplos están basados en el tutorial [Configurar la replicación entre dos servidores conectados completamente (transaccional)](../../relational-databases/replication/tutorial-replicating-data-between-continuously-connected-servers.md).



## <a name="troubleshooting-methodology"></a>Metodología de solución de problemas 

### <a name="questions-to-ask"></a>Preguntas que se deben formular
1. ¿En qué parte del proceso de sincronización se produce un error en la replicación?
2. ¿Qué agente está experimentando un error?
1. ¿Cuándo fue la última vez que la replicación funcionó correctamente? ¿Qué ha cambiado desde entonces?  

### <a name="steps-to-take"></a>Pasos a seguir
1. Use el Monitor de replicación para identificar en qué punto de la replicación se encuentra el error (en qué agente):
   - Si los errores se producen en la sección **Publicador a distribuidor**, el problema está relacionado con el Agente de registro del LOG. 
   - Si los errores se producen en la sección **Distribuidor a publicador**, el problema está relacionado con el Agente de distribución.  
2. Examine el historial de trabajos de ese agente en el Monitor de actividad de trabajo para identificar los detalles del error. Si en el historial de trabajos no se muestran detalles suficientes, puede [habilitar el registro detallado](#enable-verbose-logging-on-any-agent) en ese agente específico.
3. Intente determinar una solución para el error.


## <a name="find-errors-with-the-snapshot-agent"></a>Buscar errores con el Agente de instantáneas
El Agente de instantáneas genera la instantánea y la escribe en la carpeta de instantáneas especificada. 

1. Vea el estado del Agente de instantáneas:

    a. En el Explorador de objetos, expanda el nodo **Publicación local** bajo **Replicación**.

    b. Haga clic con el botón derecho en la publicación **AdvWorksProductTrans** > **Ver estado del Agente de instantáneas**. 

    ![Comando "Ver estado del Agente de instantáneas" en el menú contextual](media/troubleshooting-tran-repl-errors/view-snapshot-agent-status.png)

1. Si se notifica un error en el estado del Agente de instantáneas, puede encontrar más detalles en el historial de trabajos del Agente de instantáneas:

    a. Expanda **Agente SQL Server** en el Explorador de objetos y abra el Monitor de actividad de trabajo. 

    b. Ordene por **Categoría** e identifique el Agente de instantáneas por la categoría **REPL-Instantánea**.

    c. Haga clic con el botón derecho en el Agente de instantáneas y después seleccione **Ver historial**. 

   ![Selecciones para abrir el historial del Agente de instantáneas](media/troubleshooting-tran-repl-errors/snapshot-agent-history.png)
    
1. En el historial del Agente de instantáneas, seleccione la entrada de registro correspondiente. Suele ser una línea o dos *antes* de la entrada en la que se informa del error. (Una X de color rojo indica errores). Revise el texto del mensaje en el cuadro situado debajo de los registros: 

    ![Error del Agente de instantáneas para acceso denegado](media/troubleshooting-tran-repl-errors/snapshot-access-denied.png)

    ```console
    The replication agent had encountered an exception.
    Exception Message: Access to path '\\node1\repldata.....' is denied.
    ```

Si los permisos de Windows no están configurados correctamente para la carpeta de instantáneas, verá un error de "acceso denegado" para el Agente de instantáneas. Tendrá que comprobar los permisos para la carpeta donde se almacena la instantánea y asegurarse de que la cuenta que se usa para ejecutar al Agente de instantáneas tiene permisos para acceder al recurso compartido.  

## <a name="find-errors-with-the-log-reader-agent"></a>Buscar errores con el Agente de registro del LOG
El Agente de registro del LOG se conecta a la base de datos del publicador y examina el registro de transacciones para las transacciones marcadas como "para replicación". Después, agrega esas transacciones a la base de datos de distribución. 

1.  Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Expanda el nodo del servidor, haga clic con el botón derecho en la carpeta **Replicación** y luego seleccione **Iniciar Monitor de replicación**:  

    ![Comando "Iniciar el Monitor de replicación" en el menú contextual](media/troubleshooting-tran-repl-errors/launch-repl-monitor.png)
  
    Se abre el monitor de replicación: ![Monitor de replicación](media/troubleshooting-tran-repl-errors/repl-monitor.png) 
   
2. La X de color rojo indica que la publicación no se está sincronizando. Expanda **Mis publicadores** en el lado izquierdo y, después, expanda el servidor del publicador relevante.  
  
3.  Seleccione la publicación **AdvWorksProductTrans** de la izquierda y, después, busque la X de color rojo en una de las pestañas para identificar dónde está el problema. En este caso, la X de color rojo se encuentra en la pestaña **Agentes**, por lo que uno de los agentes ha encontrado un error: 

    ![X de color rojo en la pestaña "Agentes"](media/troubleshooting-tran-repl-errors/agent-error.png)

4. Haga clic en la pestaña **Agentes** para identificar el agente que tiene el error: 

    ![X de color rojo en el Agente de registro del LOG en el que se produce el error](media/troubleshooting-tran-repl-errors/log-reader-agent-failure.png)


5. En esta vista se muestran dos agentes, el Agente de instantáneas y el Agente de registro del LOG. En el que se produce el error tiene la X de color rojo. En este caso, es el Agente de registro del LOG. 

    Haga doble clic en la línea en la que se informa del error, para abrir el historial del Agente de registro del LOG. En este historial se proporciona información más detallada sobre el error: 
    
    ![Detalles del error para el Agente de registro del LOG](media/troubleshooting-tran-repl-errors/log-reader-error.png)

    ```console
    Status: 0, code: 20011, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.
    The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.
    Status: 0, code: 15517, text: 'Cannot execute as the database principal because the principal "dbo" does not exist, this type of principal cannot be impersonated, or you do not have permission.'.
    Status: 0, code: 22037, text: 'The process could not execute 'sp_replcmds' on 'NODE1\SQL2016'.'.        
    ```

6. Normalmente, el error se produce cuando el propietario de la base de datos del publicador no se ha establecido correctamente. Esto puede ocurrir cuando se restaura una base de datos. Para comprobarlo:

    a. Expanda **Bases de datos** en el Explorador de objetos.

    b. Haga clic con el botón derecho en **AdventureWorks2012** > **Propiedades**. 

    c. Compruebe la existencia de un propietario en la página **Archivos**. Si este cuadro está en blanco, esta es la causa probable del problema. 

   ![Página "Archivos" en las propiedades de la base de datos, con un cuadro "Propietario" en blanco](media/troubleshooting-tran-repl-errors/db-properties.png)

7. Si el propietario está en blanco en la página **Archivos**, abra una **Nueva ventana de consulta** dentro del contexto de la base de datos AdventureWorks2012. Ejecute el código de T-SQL siguiente:

    ```sql
    -- set the owner of the database to 'sa' or a specific user account, without the brackets. 
    EXECUTE sp_changedbowner '<useraccount>'
    -- example for sa: exec sp_changedbowner 'sa'
    -- example for user account: exec sp_changedbowner 'sqlrepro\administrator' 
    ```

8. Es posible que tenga que reiniciar el Agente de registro del LOG:

    a. Expanda el nodo **Agente SQL Server** en el Explorador de objetos y abra el Monitor de actividad de trabajo.

    b. Ordene por **Categoría** e identifique el Agente de registro del LOG por la categoría **REPL-Lector del registro**. 

    c. Haga clic con el botón derecho en el trabajo del **Agente de registro del LOG** y seleccione **Iniciar trabajo en el paso**. 

    ![Selecciones para reiniciar el Agente de registro del LOG](media/troubleshooting-tran-repl-errors/start-job-at-step.png)

9. Vuelva a abrir el Monitor de replicación para comprobar que ahora la publicación se está sincronizando. Si no está abierto, puede encontrarlo si hace clic con el botón derecho en **Replicación** en el Explorador de objetos. 
10. Seleccione la publicación **AdvWorksProductTrans**, haga clic en la pestaña **Agentes** y haga doble clic en el Agente de registro del LOG para abrir el historial del agente. Ahora debería ver que el Agente de registro del LOG se está ejecutando y, o bien está replicando comandos, o muestra "No hay transacciones replicadas":

    ![Agente de registro del LOG en ejecución sin transacciones replicadas](media/troubleshooting-tran-repl-errors/log-reader-running.png)

## <a name="find-errors-with-the-distribution-agent"></a>Buscar errores con el Agente de distribución
El Agente de distribución busca los datos en la base de datos de distribución y después los aplica al suscriptor. 

1. Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Expanda el nodo del servidor, haga clic con el botón derecho en la carpeta **Replicación** y luego seleccione **Iniciar Monitor de replicación**.  
2. En **Monitor de replicación**, seleccione la publicación **AdvWorksProductTrans** y seleccione la pestaña **Todas las suscripciones**. Haga clic con el botón derecho en la suscripción y seleccione **Ver detalles**:

    ![Comando "Ver detalles" en el menú contextual](media/troubleshooting-tran-repl-errors/view-details.png)

2. Se abre el cuadro de diálogo **Historial de Distribuidor a suscriptor** y aclara qué tipo de error está detectando el agente: 

     ![Detalles del error para el Agente de distribución](media/troubleshooting-tran-repl-errors/dist-history-error.png)

    ```console
    Error messages:
    Agent 'NODE1\SQL2016-AdventureWorks2012-AdvWorksProductTrans-NODE2\SQL2016-7' is retrying after an error. 89 retries attempted. See agent job history in the Jobs folder for more details.
    ```

3. El error indica que el Agente de distribución está volviendo a intentarlo. Para buscar más información, compruebe el historial de trabajos del Agente de distribución: 

    a. Expanda **Agente SQL Server** en el Explorador de objetos > **Monitor de actividad de trabajo**. 
    
    b. Ordene los trabajos por **Categoría**. 

    c. Identifique el Agente de distribución por la categoría **REPL-Distribución**. Haga clic con el botón derecho en el agente y seleccione **Ver historial**.

    ![Selecciones para ver el historial del Agente de distribución](media/troubleshooting-tran-repl-errors/view-dist-agent-history.png)

5. Seleccione una de las entradas de error y vea el texto del error en la parte inferior de la ventana:  

    ![Texto de error que indica una contraseña incorrecta para el agente de distribución](media/troubleshooting-tran-repl-errors/dist-pw-wrong.png)

    ```console
    Message:
    Unable to start execution of step 2 (reason: Error authenticating proxy NODE1\repl_distribution, system error: The user name or password is incorrect.)
    ```

6. Este error indica que la contraseña usada por el Agente de distribución es incorrecta. Para resolverlo:

    a. Expanda el nodo **Replicación** en el Explorador de objetos.
    
    b. Haga clic con el botón derecho en la suscripción > **Propiedades**.
    
    c. Haga clic en el botón de puntos suspensivos (...) situado junto a **Cuenta de proceso del agente** y modifique la contraseña.

    ![Selecciones para modificar la contraseña para el Agente de distribución](media/troubleshooting-tran-repl-errors/dist-agent-pw-change.png)

7. Vuelva a comprobar el Monitor de replicación haciendo clic con el botón derecho en **Replicación** en el Explorador de objetos. Una X de color rojo debajo de **Todas las suscripciones** indica que el Agente de distribución todavía detecta un error. 

    Abra el historial de **Distribución al suscriptor** haciendo clic con el botón derecho en la suscripción en **Monitor de replicación** > **Ver detalles**. En este caso, el error ahora es diferente: 

    ![Error que indica que el Agente de distribución no se puede conectar](media/troubleshooting-tran-repl-errors/dist-agent-cant-connect.png)

    ```console
    Connecting to Subscriber 'NODE2\SQL2016'        
    Agent message code 20084. The process could not connect to Subscriber 'NODE2\SQL2016'.
    Number:  18456
    Message: Login failed for user 'NODE2\repl_distribution'.
    ```

8. Este error indica que el Agente de distribución no se pudo conectar al suscriptor, ya que se produjo un error al iniciar sesión con el usuario **NODE2\repl_distribution**. Para investigar en profundidad, conéctese al suscriptor y abra el registro de errores de SQL Server *actual* en el nodo **Administración** del Explorador de objetos: 

    ![Error que indica el error de inicio de sesión para el suscriptor](media/troubleshooting-tran-repl-errors/login-failed.png)
    
    Si está viendo este error, falta el inicio de sesión en el suscriptor. Para resolver este error, vea [Permisos para la replicación](../../relational-databases/replication/security/security-role-requirements-for-replication.md).

9. Una vez resuelto el error de inicio de sesión, vuelva a comprobar el Monitor de replicación. Si se han solucionado todos los problemas, debería ver una flecha de color verde junto al **Nombre de la publicación** y un estado de **En ejecución** en **Todas las suscripciones**. 

    Haga clic con el botón derecho en la suscripción para volver a abrir el historial de **Distribuidor a suscriptor** para comprobar que se realizó correctamente. Si es la primera vez que se ejecuta el Agente de distribución, verá que la instantánea se ha copiado de forma masiva en el suscriptor: 

     ![Agente de distribución con un estado "En ejecución" y un mensaje sobre la copia masiva](media/troubleshooting-tran-repl-errors/dist-agent-success.png)   


## <a name="enable-verbose-logging-on-any-agent"></a>Habilitar el registro detallado en todos los agentes
Puede usar el registro detallado para ver información más detallada sobre los errores que se produzcan en cualquier agente en la topología de replicación. Los pasos son los mismos para cada agente. Asegúrese de seleccionar el agente correcto en el Monitor de actividad de trabajo. 

   >[!NOTE]   
   > Los agentes pueden estar en el publicador o el suscriptor, en función de si se trata de una suscripción de inserción o de extracción. Si no puede encontrar al agente que está buscando en el servidor que está examinando, compruebe el otro servidor.  

1. Decida dónde quiere guardar el registro detallado y asegúrese de que la carpeta exista. En este ejemplo se usa c:\temp. 
2. Expanda el nodo **Agente SQL Server** en el Explorador de objetos y abra el Monitor de actividad de trabajo. 

    ![Comando "Ver la actividad de trabajo" en el menú contextual para el Monitor de actividad](media/troubleshooting-tran-repl-errors/job-activity-monitor.png)    

1. Ordene por **Categoría** e identifique el agente que le interese. Este ejemplo se usa el Agente de registro del LOG. Haga clic con el botón derecho en el agente que le interese > **Propiedades**.

    ![Selecciones para abrir las propiedades del agente](media/troubleshooting-tran-repl-errors/log-agent-properties.png)

1. Seleccione la página **Pasos** y, después, resalte el paso **Ejecutar agente**. Seleccione **Editar**. 

    ![Selecciones para editar el paso "Ejecutar agente"](media/troubleshooting-tran-repl-errors/edit-steps.png)

1. En el cuadro **Comando**, inicie una línea nueva, escriba el texto siguiente y haga clic en **Aceptar**: 

    ```console
    -Output C:\Temp\OUTPUTFILE.txt -Outputverboselevel 3
    ```

    Puede modificar la ubicación y el nivel de detalle según sus preferencias.

    ![Resultados detallados en las propiedades del paso del trabajo](media/troubleshooting-tran-repl-errors/verbose.png)

   > [!NOTE]
   > Es posible que todo esto provoque un error en el agente, o bien que falte el archivo de salida, cuando se va a agregar el parámetro de salida detallada:
   > - Hay un problema de formato en el que un guión largo se convierte en un guión. 
   > - La ubicación no existe en el disco, o bien la cuenta que está ejecutando al agente no tiene permiso para escribir en la ubicación especificada. 
   > - Falta un espacio entre el último parámetro y el parámetro `-Output`. 
   > - Cada agente admite diferentes niveles de detalle. Si habilita el registro detallado, pero no se puede iniciar el agente, pruebe a reducir el nivel de detalle especificado en 1. 

1. Reinicie el Agente de registro del LOG haciendo clic con el botón derecho en el agente > **Detener trabajo en el paso**. Para actualizar, haga clic en el icono **Actualizar** de la barra de herramientas. Haga clic con el botón derecho en el agente > **Iniciar trabajo en el paso**.
2. Revise la salida en el disco. 

    ![Archivo de texto de salida](media/troubleshooting-tran-repl-errors/output.png)

    
1. Para deshabilitar el registro detallado, siga los mismos pasos anteriores para quitar toda la línea `-Output` que agregó anteriormente. 

Para obtener más información, vea [Cómo habilitar a los agentes de duplicación para el registro de archivos de salida en SQL Server](https://support.microsoft.com/help/312292/how-to-enable-replication-agents-for-logging-to-output-files-in-sql-se). 


## <a name="see-also"></a>Consulte también
<br>[Replicación transaccional](../../relational-databases/replication/transactional/transactional-replication.md)
<br>[Tutoriales de replicación](../../relational-databases/replication/replication-tutorials.md)
<br>[Blog de ReplTalk](https://blogs.msdn.microsoft.com/repltalk)

[!INCLUDE[get-help-options](../../includes/paragraph-content/get-help-options.md)]

