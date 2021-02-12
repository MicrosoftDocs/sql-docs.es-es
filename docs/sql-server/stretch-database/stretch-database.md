---
description: Stretch Database
title: Stretch Database
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database
ms.assetid: ce6db775-21a5-40bc-95a1-f560376d4ee2
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: c2d2acc7ba0ac0f57038b3a72f62e6b67383b1bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077456"
---
# <a name="stretch-database"></a>Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Stretch Database migra los datos inactivos de forma clara y segura a la nube de Microsoft Azure.  
  
 Consulte [Introducción mediante la ejecución del Asistente para Habilitar base de datos para Stretch](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)si su intención es comenzar a usar inmediatamente Stretch Database.  
  
## <a name="what-are-the-benefits-of-stretch-database"></a>¿Cuáles son las ventajas de Stretch Database?  
 Stretch Database ofrece las siguientes ventajas:  
  
 **Permite disponer de los datos inactivos de forma provechosa**  
 Use SQL Server Stretch Database para ampliar dinámicamente los datos transaccionales tanto activos como inactivos desde SQL Server a Microsoft Azure. A diferencia de lo que suele ocurrir en el almacenamiento de datos inactivos, los datos siempre estarán en línea y disponibles para consultarlos. Además, se pueden establecer escalas de tiempo de retención de datos más prolongadas, sin que ello interrumpa el banco de tablas grandes como el historial de pedidos de cliente. Aproveche el bajo costo de Azure en lugar de escalar a un almacenamiento local prohibitivo. Será usted quien elija el plan de tarifa y configure las opciones que quiera en el Portal de Azure para tener los precios y costos bajo control. Escalado o reducción vertical según sea necesario. Visite la página de [precios de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) para obtener información pormenorizada.  
  
 **No hay que cambiar las consultas ni las aplicaciones**  
 Acceda a los datos de SQL Server sin problemas, independientemente de si están almacenados de forma local o extendidos a la nube.  Establezca la directiva que determina dónde se almacenan los datos y SQL Server controlará el movimiento de datos en segundo plano. La tabla entera estará siempre en línea y lista para consultarse. Además, Stretch Database no requiere ningún cambio en las consultas o aplicaciones existentes: la ubicación de los datos es completamente transparente para la aplicación.  
  
 **Agiliza el mantenimiento de los datos locales**  
 Reduzca el almacenamiento y mantenimiento local de los datos. Las copias de seguridad de los datos locales se ejecutan más rápidamente y terminan dentro de la ventana de tiempo de mantenimiento. Las copias de seguridad de los datos almacenados en la nube se hacen automáticamente, Sus necesidades de almacenamiento local se reducirán considerablemente. El almacenamiento de Azure puede ser hasta un 80 % más asequible que recurrir a una SSD local.  
  
 **Los datos se mantienen seguros incluso durante la migración**  
 Relájese mientras extiende las aplicaciones más importantes a la nube sin correr ningún tipo de riesgo. Always Encrypted de SQL Server proporciona cifrado para los datos en movimiento. La seguridad de nivel de fila y otras características de seguridad avanzadas de SQL Server funcionan también con Stretch Database para proteger los datos.  
  
## <a name="what-does-stretch-database-do"></a>¿Qué hace Stretch Database?  
 Después de habilitar Stretch Database en al menos una tabla y una base de datos y seleccionar una instancia de SQL Server, comenzará a migrar silenciosamente los datos inactivos a Azure.  
  
-   Si los datos inactivos están almacenados en otra tabla, puede migrarla entera.  
  
-   Si la tabla contiene datos activos e inactivos, puede especificar una función de filtro para seleccionar las filas que se migrarán.

**No es necesario cambiar las consultas ni las aplicaciones cliente existentes.** Seguirá disfrutando de un acceso sin fisuras a los datos locales y remotos, incluso durante la migración de datos. En las consultas remotas es posible percibir una cierta latencia, pero solamente al consultar datos inactivos.

**Stretch Database garantiza que no se perderán datos** si se produce un error durante la migración. También tiene una lógica de reintento para tratar los problemas de conexión que se pueden producir durante la migración. Una vista de administración dinámica proporciona el estado de la migración.

**Puede pausar la migración de datos** tanto para solucionar problemas en el servidor local como para maximizar el ancho de banda de red disponible.  
  
 ![Información general sobre la base de datos de extensión](../../sql-server/stretch-database/media/stretch-overview.png "Información general sobre la base de datos de extensión")  
  
## <a name="is-stretch-database-for-you"></a>¿Cómo sé si Stretch Database se ajusta a mis necesidades?  
 Si se siente identificado con alguna de las siguientes afirmaciones, Stretch Database puede ayudarle a satisfacer sus requisitos y a solucionar problemas.  
  
|Soy un responsable de la toma de decisiones y...|Soy un administrador de base de datos y...|  
|--------------------------------|---------------------|  
|Tengo que conservar los datos transaccionales durante mucho tiempo.|El tamaño de las tablas empieza a estar fuera de control.|  
|A veces tengo que consultar los datos inactivos.|Mis usuarios afirman que quieren tener acceso a los datos inactivos, pero en realidad apenas si los usan.|  
|Tengo aplicaciones, incluidas aplicaciones antiguas, que no quiero actualizar.|Estoy constantemente adquiriendo y agregando más espacio de almacenamiento.|  
|Quiero encontrar una manera de ahorrar dinero en almacenamiento.|No puedo restaurar unas tablas tan grandes ni hacer copias de seguridad dentro de los términos del contrato de nivel de servicio.|  
  
## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>¿Qué tipos de bases de datos y tablas son candidatas a Stretch Database?  
 Stretch Database está pensado para bases de datos transaccionales con grandes cantidades de datos inactivos que suelen almacenarse en un número reducido de tablas. Estas tablas pueden contener millones y millones de filas.  
  
 Si usa la característica de tabla temporal de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], use Stretch Database para migrar total o parcialmente la tabla de historial asociada a un almacenamiento rentable en Azure. Para obtener más información, vea [Administración de la retención de datos históricos en las tablas temporales con versiones del sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md).  
  
 Use Stretch Database Advisor, una característica del Asesor de actualizaciones de SQL Server 2016, para identificar las bases de datos y tablas para Stretch Database. Para obtener más información, vea [Identificar bases de datos y tablas para Stretch Database al ejecutar el Asesor de Stretch Database](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md). Para obtener más información sobre los posibles problemas de bloqueo, vea [Limitaciones de Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).  

## <a name="test-drive-stretch-database"></a>Versión de prueba de Stretch Database  
 **Use la versión de prueba de Stretch Database con la base de datos de ejemplo de AdventureWorks.** Para obtener la base de datos de ejemplo de AdventureWorks, descargue al menos el archivo de base de datos y el archivo de ejemplos y scripts [aquí](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks). Después de restaurar la base de datos de ejemplo en una instancia de SQL Server 2016, descomprima el archivo de ejemplos y abra el archivo Stretch DB Samples de la carpeta Stretch DB. Ejecute los scripts de este archivo para comprobar el espacio que ocupan los datos antes y después de habilitar Stretch Database, para realizar un seguimiento del progreso de la migración de datos y para confirmar que puede seguir consultando los datos existentes e insertar otros nuevos durante y tras la migración de datos.  
  
## <a name="next-step"></a>Paso siguiente  
 **Identificación de las bases de datos y las tablas que son candidatas a Stretch Database.** Descargue Data Migration Assistant y ejecute una evaluación para identificar las bases de datos y las tablas que son candidatas para Stretch Database. Para obtener más información, vea [Identificar bases de datos y tablas para Stretch Database al ejecutar el Asesor de Stretch Database](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md).  
  
  
