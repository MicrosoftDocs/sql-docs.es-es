---
title: Ejecutar paquetes en la escalabilidad horizontal de SQL Server Integration Services (SSIS) | Microsoft Docs
description: Aprenda a ejecutar paquetes de SQL Server Integration Services (SSIS) en Escalabilidad horizontal con una variedad de métodos.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.reviewer: maghan
f1_keywords:
- sql13.ssis.ssms.ispackageexecuteinscaleout.f1
ms.openlocfilehash: 13f9a386b6ab60280edf2d8b494721dd233b042a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347412"
---
# <a name="run-packages-in-integration-services-ssis-scale-out"></a>Ejecutar paquetes en la escalabilidad horizontal de Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Tras implementar los paquetes en el servidor de Integration Services, puede ejecutarlos en escalabilidad horizontal con uno de los siguientes métodos:

-   [Cuadro de diálogo Ejecutar paquete en escalabilidad horizontal](#scale_out_dialog)

-   [procedimientos almacenados](#stored_proc)

-   [trabajos del Agente SQL Server](#sql_agent)

## <a name="run-packages-with-the-execute-package-in-scale-out-dialog-box"></a><a name="scale_out_dialog"></a> Ejecutar paquetes con el cuadro de diálogo Ejecutar paquete en escalabilidad horizontal

1. Abra el cuadro de diálogo Ejecutar paquete en escalabilidad horizontal.

    En [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)], conecte con el servidor de Integration Services. En el Explorador de objetos, expanda el árbol para ver los nodos de **Catálogos de Integration Services**. Haga clic en el nodo **SSISDB** o el proyecto o el paquete que quiera ejecutar y, luego, haga clic en **Ejecutar en escalado horizontal**.

2. Seleccione los paquetes y establezca las opciones.

    En la página **Selección de paquetes**, seleccione uno o varios paquetes para ejecutarlos. Establezca el entorno, los parámetros, los administradores de conexiones y las opciones avanzadas de cada paquete. Haga clic en un paquete para establecer estas opciones.
    
    En la pestaña **Avanzadas**, establezca la opción de escalabilidad horizontal **Número de reintentos** para especificar el número de veces que se reintentará ejecutar el paquete en caso de error.

    > [!NOTE]
    > La opción **Volcado de errores** solo funciona cuando la cuenta que ejecuta el servicio Trabajo de escalabilidad horizontal es un administrador del equipo local.

3. Seleccione los equipos de trabajo.

    En la página **Selección de equipo**, seleccione los equipos de trabajo de escalabilidad horizontal para ejecutar los paquetes. De forma predeterminada, los paquetes se pueden ejecutar en cualquier equipo. 

   > [!NOTE] 
   > Los paquetes se ejecutan con las credenciales de las cuentas de usuario de los servicios de trabajo de escalabilidad horizontal. Revise estas credenciales en la página **Selección de equipo**. De forma predeterminada, la cuenta es `NT Service\SSISScaleOutWorker140`.

   > [!WARNING]
   > Las ejecuciones de paquetes que desencadenan usuarios diferentes en el mismo trabajo se ejecutan con las mismas credenciales. No presentan ningún límite de seguridad entre sí. 

4. Ejecute los paquetes y vea los informes.

    Haga clic en **Aceptar** para iniciar las ejecuciones de paquetes. Para ver el informe de ejecución de un paquete, haga clic con el botón derecho en el paquete en el Explorador de objetos, haga clic en **Informes**, en **Todas las ejecuciones** y busque la ejecución.
    
## <a name="run-packages-with-stored-procedures"></a><a name="stored_proc"></a> Ejecución de paquetes con procedimientos almacenados

1.  Cree las ejecuciones.

    Llame a `[catalog].[create_execution]` para cada paquete. Establezca el parámetro **\@runinscaleout** en `True`. Si no se permite a todos los equipos de trabajo de Escalabilidad horizontal ejecutar el paquete, establezca el parámetro **\@useanyworker** en `False`. Para obtener más información sobre este procedimiento almacenado y el parámetro **\@useanyworker**, consulte [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md). 

2. Establezca los parámetros de ejecución.

    Llame a `[catalog].[set_execution_parameter_value]` para cada ejecución.

3. Establezca los trabajos de escalabilidad horizontal.

    Llame a `[catalog].[add_execution_worker]`. Si se permite ejecutar el paquete en cualquier equipo, no es necesario llamar a este procedimiento almacenado. 

4. Inicie las ejecuciones.

    Llame a `[catalog].[start_execution]`. Establezca el parámetro **\@retry_count** para definir el número de veces que se volverá a intentar ejecutar un paquete en caso de error.
    
### <a name="example"></a>Ejemplo
En el ejemplo siguiente se ejecutan dos paquetes, `package1.dtsx` y `package2.dtsx`, en escalabilidad horizontal con un trabajo de escalabilidad horizontal.  

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder1', @project_name=N'project1', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO

Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package2.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder2', @project_name=N'project2', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO
```

### <a name="permissions"></a>Permisos
Para ejecutar paquetes en escalabilidad horizontal, es necesario disponer de uno de los siguientes permisos:

-   Pertenencia al rol de base de datos **ssis_admin**  

-   Pertenencia al rol de base de datos **ssis_cluster_executor**  
  
-   Pertenencia al rol de servidor **sysadmin**  

## <a name="set-default-execution-mode"></a>Establecer el modo de ejecución predeterminado
Para establecer el modo de ejecución predeterminado para los paquetes en **Escalabilidad horizontal**, haga lo siguiente:

1.  En SSMS, en el Explorador de objetos, haga clic con el botón derecho en el nodo **SSISDB** y seleccione **Propiedades**.

2.  En el cuadro de diálogo **Propiedades del catálogo**, establezca **Modo de ejecución predeterminado de todo el servidor** en **Escalabilidad horizontal**.

Una vez que haya establecido este modo de ejecución predeterminado, ya no tendrá que especificar el parámetro **\@runinscaleout** al llamar al procedimiento almacenado `[catalog].[create_execution]`. Los paquetes se ejecutan en escalabilidad horizontal de forma automática. 

![Modo de ejecución](media/exe-mode.PNG)

Para volver a cambiar el modo de ejecución predeterminado de modo que los paquetes no se ejecuten de forma automática en el modo de escalabilidad horizontal, establezca **Modo de ejecución predeterminado de todo el servidor** en **Servidor**.

## <a name="run-package-in-sql-server-agent-job"></a><a name="sql_agent"></a> Ejecución del paquete en el trabajo del Agente SQL Server
En un trabajo del Agente SQL Server, puede ejecutar un paquete SSIS como paso del trabajo. Para ejecutar el paquete en escalabilidad horizontal, establezca el modo de ejecución en **Escalabilidad horizontal**. Una vez configurado el modo de ejecución predeterminado en **Escalabilidad horizontal**, los trabajos del Agente SQL Server se ejecutarán en dicho modo.

## <a name="next-steps"></a>Pasos siguientes
-   [Solución de problemas de escalabilidad horizontal](troubleshooting-scale-out.md)
