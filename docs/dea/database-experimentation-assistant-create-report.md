---
title: Crear informes de análisis
description: Generar un informe de análisis en Asistente para experimentación con bases de datos (DEA). Los informes de análisis proporcionan información sobre las implicaciones de rendimiento de los cambios propuestos.
ms.date: 01/24/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 931e013a84cd808d75a50edafdcf73d105f51827
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045115"
---
# <a name="create-analysis-reports-in-database-experimentation-assistant-sql-server"></a>Crear informes de análisis en Asistente para experimentación con bases de datos (SQL Server)

Después de reproducir el seguimiento de origen en los dos servidores de destino, puede generar un informe de análisis en Asistente para experimentación con bases de datos (DEA). Los informes de análisis le ayudan a obtener información sobre las implicaciones de rendimiento de los cambios propuestos.

## <a name="create-an-analysis-report"></a>Crear un informe de análisis

1. En DEA, seleccione el icono de lista, especifique el nombre del servidor y el tipo de autenticación, Active o desactive las casillas **cifrar conexión** y **confiar en certificado de servidor** según corresponda para su escenario y, a continuación, seleccione **conectar**.

   ![Conectarse al servidor con archivos de seguimiento](./media/database-experimentation-assistant-create-report/dea-connect-to-server-with-trace-files.png)

2. En la pantalla **informes de análisis** , seleccione **nuevo informe de análisis**.

   ![Crear nuevo informe de análisis](./media/database-experimentation-assistant-create-report/dea-create-an-analysis-report.png)

3. En la pantalla **nuevo informe de análisis** , especifique un nombre para el informe, la ubicación de almacenamiento y la ruta de acceso a los archivos de seguimiento de destino 1 y 2 y, a continuación, seleccione **iniciar**.

   ![Especificar nuevos detalles del informe de análisis](./media/database-experimentation-assistant-create-report/dea-new-analysis-report-details.png)

   Si la información especificada es válida, se crea el informe de análisis.

   ![Informe de análisis recién creado](./media/database-experimentation-assistant-create-report/dea-newly-created-analysis-report.png)

      > [!NOTE]
      > Si alguno de los datos introducidos no es válido, los cuadros de texto que contienen la información incorrecta se resaltan en rojo. Realice las correcciones necesarias y, a continuación, seleccione **iniciar** de nuevo.

## <a name="frequently-asked-questions-about-analysis-reports"></a>Preguntas más frecuentes sobre los informes de análisis

**P: ¿qué me informa mi informe de análisis?**

DEA usa pruebas estadísticas para analizar la carga de trabajo y determinar cómo se ejecutó cada consulta desde el destino 1 hasta el destino 2. Proporciona detalles de rendimiento para cada consulta. Obtenga más información sobre DEA [en Introducción](database-experimentation-assistant-get-started.md).

**P: ¿puedo crear un nuevo informe de análisis mientras se genera otro informe?**

No.  Actualmente, solo se puede generar un informe a la vez para evitar conflictos. Sin embargo, puede ejecutar más de una captura y reproducir al mismo tiempo.

**P: ¿se puede generar un informe de análisis mediante el símbolo del sistema?**

Sí. Puede generar un informe de análisis en el símbolo del sistema. Después, puede ver el informe en la interfaz de usuario. Para obtener más información, vea [ejecutar en el símbolo del sistema](database-experimentation-assistant-run-command-prompt.md).

## <a name="troubleshoot-analysis-reports"></a>Solucionar problemas de informes de análisis

**P: ¿Qué permisos de seguridad necesito para generar y ver un informe de análisis en mi servidor?**

El usuario que ha iniciado sesión en DEA debe tener derechos de administrador del servidor en el servidor de análisis. Si el usuario forma parte de un grupo, asegúrese de que el grupo tiene derechos sysadmin.

|Posibles errores|Solución|  
|---|---|  
|No se puede conectar con la base de datos. Asegúrese de que dispone de derechos de administrador del usuario para analizar y ver los informes.|Es posible que no tenga derechos de acceso o de administrador del servidor en el servidor o la base de datos. Confirme los derechos de inicio de sesión e inténtelo de nuevo.|  
|No se puede generar el **nombre del informe** en el **nombre del servidor**. Para obtener más información, compruebe el informe **nombre del informe** .|Es posible que no tenga los derechos de sysadmin necesarios para generar un nuevo informe. Para ver los errores detallados, seleccione el informe con errores y compruebe los registros en% Temp% \\ DEA.|  
|El usuario actual no tiene los permisos necesarios para ejecutar la operación. Asegúrese de que dispone de derechos de administrador del usuario para realizar el seguimiento y analizar los informes.|No tiene los derechos de sysadmin necesarios para generar un nuevo informe.|  

**P: no puedo conectarme al equipo que ejecuta SQL Server**

- Confirme que el nombre del equipo que ejecuta SQL Server es válido. Para confirmar, intente conectarse al servidor mediante SQL Server Management Studio (SSMS).
- Confirme que la configuración del firewall no bloquea las conexiones con el equipo que ejecuta SQL Server.
- Confirme que el usuario tiene los derechos de usuario necesarios.

Puede ver más detalles en los registros en% Temp% \\ DEA. Si el problema persiste, póngase en contacto con el equipo del producto.

**P: veo un error al generar un informe de análisis**

El acceso a Internet es necesario la primera vez que se genera un informe de análisis después de instalar DEA. Se requiere acceso a Internet para descargar los paquetes necesarios para el análisis estadístico.

Si se produce un error mientras se crea el informe, la página progreso muestra el paso específico en el que se produjo un error en la generación del análisis. Puede ver más detalles en los registros en% Temp% \\ DEA. Compruebe que tiene una conexión válida con el servidor con los derechos de usuario necesarios y, a continuación, vuelva a intentarlo. Si el problema persiste, póngase en contacto con el equipo del producto.

|Posibles errores|Solución|  
|---|---|  
|RInterop encontró un error al iniciarse. Compruebe los registros de RInterop e inténtelo de nuevo.|DEA requiere acceso a Internet para descargar paquetes de R dependientes. Compruebe los registros de RInterop en% Temp% \\ RInterop y los registros de DEA en% Temp% \\ DEA. Si RInterop se inicializó incorrectamente o si se inicializó sin los paquetes de R correctos, es posible que vea la excepción "no se pudo generar el nuevo informe de análisis" después del paso InitializeRInterop en los registros de DEA.<br><br>Los registros de RInterop también pueden mostrar un error similar a "no hay ningún paquete de jsonlite disponible". Si el equipo no tiene acceso a Internet, puede descargar manualmente el paquete de jsonlite R necesario:<br><br><li>Vaya a la carpeta% userprofile% \\ DEARPackages en el sistema de archivos de la máquina. Esta carpeta se compone de los paquetes usados por R para DEA.</li><br><li>Si falta la carpeta jsonlite en la lista de paquetes instalados, necesita una máquina con acceso a Internet para descargar la versión de lanzamiento de jsonlite \_1.4.zip de [https://cran.r-project.org/web/packages/jsonlite/index.html](https://cran.r-project.org/web/packages/jsonlite/index.html) .</li><br><li>Copie el archivo. zip en la máquina en la que está ejecutando DEA.  Extraiga la carpeta jsonlite y cópiela en% userprofile% \\ DEARPackages. Este paso instala automáticamente el paquete jsonlite en R. La carpeta debe denominarse **jsonlite** y el contenido debe estar directamente dentro de la carpeta, no un nivel por debajo.</li><br><li>Cierre DEA, vuelva a abrir y vuelva a intentar el análisis.</li><br>También puede usar RGUI. Vaya a **paquetes**  >  **instalar desde zip**. Vaya al paquete que descargó anteriormente e instálelo.<br><br>Si RInterop se ha inicializado y configurado correctamente, debería ver "instalación del paquete de R dependiente jsonlite" en los registros de RInterop.|  
|No se puede conectar con la instancia de SQL Server, asegúrese de que el nombre del servidor es correcto y compruebe el acceso necesario para el usuario que ha iniciado sesión.|Es posible que no tenga derechos de acceso o de usuario en el servidor o que el nombre del servidor no sea correcto.|
|Se agotó el tiempo de espera del proceso RInterop. Compruebe los registros de DEA y RInterop, detenga el proceso de RInterop en el administrador de tareas e inténtelo de nuevo.<br><br>or<br><br>RInterop tiene un estado de error. Detenga el proceso RInterop en el administrador de tareas e inténtelo de nuevo.|Compruebe los registros en% Temp% \\ RInterop para confirmar el error. Quite el proceso RInterop del administrador de tareas antes de intentarlo de nuevo. Si el problema persiste, póngase en contacto con el equipo del producto.|

**P: el informe se genera, pero parece que faltan datos**

Compruebe la base de datos en el equipo de análisis que ejecuta SQL Server para confirmar que existen datos. Compruebe que la base de datos de análisis existe y compruebe sus tablas. Por ejemplo, compruebe estas tablas: TblBatchesA, TblBatchesB y TblSummaryStats.

Si los datos no existen, es posible que los datos no se hayan copiado correctamente o que la base de datos esté dañada. Si solo faltan algunos datos, es posible que los archivos de seguimiento creados en la captura o la reproducción no hayan capturado la carga de trabajo con precisión. Si los datos están allí, compruebe los archivos de registro en% Temp% \\ DEA para ver si se registró algún error. A continuación, intente generar de nuevo el informe de análisis.

¿Tiene más preguntas o comentarios? Envíe sus comentarios a través de la herramienta de DEA eligiendo el icono de Smiley en la esquina inferior izquierda.

## <a name="see-also"></a>Consulte también

- Para obtener información sobre cómo ver el informe de análisis, vea [ver informes](database-experimentation-assistant-view-report.md).
