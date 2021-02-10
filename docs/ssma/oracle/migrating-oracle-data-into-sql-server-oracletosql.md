---
title: Migración de datos de Oracle a SQL Server (OracleToSQL) | Microsoft Docs
description: Obtenga información sobre cómo migrar datos de una base de datos de Oracle a SQL Server, después de sincronizar los objetos convertidos, mediante el uso de la aplicación SSMA para Oracle.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Oracle Data Migration, Client-Side Migration
- Oracle Data Migration,Server-Side Migration
ms.assetid: e23c5268-41ed-4e55-9fe7-a11376202a13
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: a63fcd49d37e47485ed82c3b75514b5c1b7d48cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063330"
---
# <a name="migrating-oracle-data-into-sql-server-oracletosql"></a>Migración de datos de Oracle a SQL Server (OracleToSQL)
Después de sincronizar correctamente los objetos convertidos con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede migrar los datos de Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
> Si el motor utilizado es el motor de migración de datos del lado servidor, antes de poder migrar los datos, debe instalar el paquete de extensión SSMA para Oracle y los proveedores de Oracle en el equipo que ejecuta SSMA. El servicio Agente SQL Server también debe estar en ejecución. Para obtener más información acerca de cómo instalar el paquete de extensión, consulte [instalación de componentes de servidor (OracleToSQL)](./installing-ssma-components-on-sql-server-oracletosql.md) .  
  
## <a name="setting-migration-options"></a>Establecer opciones de migración  
Antes de migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , revise las opciones de migración del proyecto en el cuadro de diálogo **configuración del proyecto** .  
  
-   Mediante este cuadro de diálogo puede establecer opciones como el tamaño del lote de migración, el bloqueo de tablas, la comprobación de restricciones, el control de valores NULL y el control de valores de identidad. Para obtener más información sobre la configuración de migración de proyectos, vea [configuración del proyecto (migración) (OracleToSQL)](./project-settings-migration-oracletosql.md).  
  
-   El **motor de migración** en el cuadro de diálogo **configuración del proyecto** permite que el usuario realice el proceso de migración mediante dos tipos de motores de migración de datos:  
  
    1.  Motor de migración de datos del lado cliente  
  
    2.  Motor de migración de datos del lado servidor  
  
**Migración de datos del lado cliente:**  
  
-   Para iniciar la migración de datos en el lado cliente, seleccione la opción **motor de migración de datos del lado cliente** en el cuadro de diálogo **configuración del proyecto** .  
  
-   En **configuración del proyecto**, se establece la opción **motor de migración de datos del lado cliente** .  
  
    > [!NOTE]  
    > El **motor de migración de datos del lado cliente** reside dentro de la aplicación SSMA y, por lo tanto, no depende de la disponibilidad del paquete de extensión.  
  
**Migración de datos del lado servidor:**  
  
-   Durante la migración de datos del lado servidor, el motor reside en la base de datos de destino. Se instala a través del paquete de extensión. Para obtener más información sobre cómo instalar el paquete de extensión, consulte [instalación de componentes de servidor en SQL Server](installing-ssma-components-on-sql-server-oracletosql.md)  
  
-   Para iniciar la migración en el lado del servidor, seleccione la opción **motor de migración de datos del lado servidor** en el cuadro de diálogo **configuración del proyecto** .  
  
## <a name="migrating-data-to-sql-server"></a>Migrar datos a SQL Server  
La migración de datos es una operación de carga masiva que mueve filas de datos de tablas de Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tablas de transacciones. El número de filas que se cargan en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cada transacción se configura en la configuración del proyecto.  
  
Para ver los mensajes de migración, asegúrese de que el panel de salida esté visible. En caso contrario, en el menú **Ver** , seleccione **salida**.  
  
**Para migrar datos**  
  
1.  Verifique lo siguiente:  
  
    -   Los proveedores de Oracle se instalan en el equipo que ejecuta SSMA.  
  
    -   Ha sincronizado los objetos convertidos con la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de datos.  
  
2.  En el explorador de metadatos de Oracle, seleccione los objetos que contienen los datos que desea migrar:  
  
    -   Para migrar datos de todos los esquemas, active la casilla situada junto a **esquemas**.  
  
    -   Para migrar datos u omitir tablas individuales, expanda primero el esquema, expanda **tablas** y, a continuación, Active o desactive la casilla situada junto a la tabla.  
  
3.  Para migrar datos, surgen dos casos:  
  
    **Migración de datos del lado cliente:**  
  
    -   Para realizar la **migración de datos del lado cliente**, seleccione la opción **motor de migración de datos del lado cliente** en el cuadro de diálogo **configuración del proyecto** .  
  
    **Migración de datos del lado servidor:**  
  
    -   Antes de realizar la migración de datos en el lado servidor, asegúrese de:  
  
        1.  SSMA para Oracle Extension Pack está instalado en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
        2.  El servicio Agente SQL Server se está ejecutando en la instancia de SQL Server.  
  
    -   Para realizar la **migración de datos del lado servidor**, seleccione la opción **motor de migración de datos del lado servidor** en el cuadro de diálogo **configuración del proyecto** .  
  
4.  Haga clic con el botón secundario en **esquemas** en el explorador de metadatos de Oracle y luego haga clic en **migrar datos**. También puede migrar datos para objetos individuales o categorías de objetos: haga clic con el botón secundario en el objeto o en su carpeta primaria; Seleccione la opción **migrar datos** .  
  
    > [!NOTE]  
    > Si el paquete de extensiones de SSMA para Oracle no está instalado en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el **motor de migración de datos del lado servidor** está seleccionado, al migrar los datos a la base de datos de destino, se produce el siguiente error: "no se encontraron los componentes de migración de datos de SSMA en SQL Server, la migración de datos del servidor no será posible. Compruebe si el paquete de extensiones está instalado correctamente. Haga clic en **Cancelar** para finalizar la migración de datos.  
  
5.  En el cuadro de diálogo **conectar con Oracle** , escriba las credenciales de conexión y, a continuación, haga clic en **conectar**. Para obtener más información sobre cómo conectarse a Oracle, vea [conectarse a oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md)  
  
    Para conectarse a la base de datos de destino [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , escriba las credenciales de conexión en el cuadro de diálogo **conectar con el SQL Server** y haga clic en **conectar**. Para obtener más información sobre cómo conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consulte [conexión a SQL Server](../sybase/connecting-to-sql-server-sybasetosql.md)  
  
    Los mensajes aparecerán en el panel de **resultados** . Una vez completada la migración, aparece el **Informe de migración de datos** . Si no se migró ningún dato, haga clic en la fila que contiene los errores y, a continuación, haga clic en **detalles**. Cuando haya terminado con el informe, haga clic en **cerrar**. Para obtener más información sobre el informe de migración de datos, vea [Informe de migración de datos (SSMA Common)](../sybase/data-migration-report-sybasetosql.md)  
  
> [!NOTE]  
> Cuando se utiliza SQL Express Edition como base de datos de destino, solo se permite la migración de datos del lado cliente y no se admite la migración de datos del lado servidor.  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
