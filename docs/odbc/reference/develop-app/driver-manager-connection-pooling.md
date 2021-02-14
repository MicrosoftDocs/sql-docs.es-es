---
description: Agrupación de conexiones de administrador de controladores
title: Agrupación de conexiones del administrador de controladores | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connection pooling [ODBC]
- pooled connections [ODBC]
- connecting to driver [ODBC], connection pooling
- connecting to data source [ODBC], connection pooling
ms.assetid: ee95ffdb-5aa1-49a3-beb2-7695b27c3df9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8876696412360275fcf443a5543634c9ac537df6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070340"
---
# <a name="driver-manager-connection-pooling"></a>Agrupación de conexiones de administrador de controladores
La agrupación de conexiones permite a una aplicación utilizar una conexión de un grupo de conexiones que no es necesario volver a establecer para cada uso. Una vez que una conexión se ha creado y colocado en un grupo, una aplicación puede volver a usar esa conexión sin realizar el proceso de conexión completo.  
  
 El uso de una conexión agrupada puede producir mejoras significativas en el rendimiento, ya que las aplicaciones pueden ahorrar la sobrecarga que implica la creación de una conexión. Esto puede ser especialmente importante para las aplicaciones de nivel intermedio que se conectan a través de una red o para las aplicaciones que se conectan y desconectan repetidamente, como las aplicaciones de Internet.  
  
 Además de las mejoras de rendimiento, la arquitectura de agrupación de conexiones habilita un entorno y sus conexiones asociadas para que los usen varios componentes en un único proceso. Esto significa que los componentes independientes en el mismo proceso pueden interactuar entre sí sin tener en cuenta entre sí. Varios componentes pueden usar varias veces una conexión en un grupo de conexiones.  
  
> [!NOTE]
>  La agrupación de conexiones se puede usar en una aplicación ODBC que muestre ODBC 2. comportamiento *x* , siempre y cuando la aplicación pueda llamar a *SQLSetEnvAttr*. Al usar la agrupación de conexiones, la aplicación no debe ejecutar instrucciones SQL que cambien la base de datos o el contexto de la base de datos, como cambiar \<*database name*> , que cambia el catálogo utilizado por un origen de datos.  


 Un controlador ODBC debe ser totalmente seguro para subprocesos y las conexiones no deben tener afinidad de subprocesos para admitir la agrupación de conexiones. Esto significa que el controlador puede controlar una llamada en cualquier subproceso en cualquier momento y puede conectarse en un subproceso, usar la conexión en otro subproceso y desconectarse en un tercer subproceso.  
  
 El administrador de controladores mantiene el grupo de conexiones. Las conexiones se extraen del grupo cuando la aplicación llama a **SQLConnect** o **SQLDriverConnect** y se devuelven al grupo cuando la aplicación llama a **SQLDisconnect**. El tamaño del grupo aumenta de forma dinámica, en función de las asignaciones de recursos solicitadas. Se reduce según el tiempo de espera de inactividad: Si una conexión está inactiva durante un período de tiempo (no se ha usado en una conexión), se quita del grupo. El tamaño del grupo solo está limitado por las restricciones de memoria y los límites en el servidor.  
  
 El administrador de controladores determina si se debe usar una conexión específica de un grupo de acuerdo con los argumentos pasados en **SQLConnect** o **SQLDriverConnect**, y de acuerdo con los atributos de conexión establecidos después de asignar la conexión.  
  
 Cuando el administrador de controladores agrupa las conexiones, debe ser capaz de determinar si una conexión sigue funcionando antes de entregar la conexión. De lo contrario, el administrador de controladores mantiene la conexión inactiva a la aplicación cada vez que se produce un error de red transitorio. Se ha definido un nuevo atributo de conexión en ODBC 3 *. x*: SQL_ATTR_CONNECTION_DEAD. Este es un atributo de conexión de solo lectura que devuelve SQL_CD_TRUE o SQL_CD_FALSE. El valor SQL_CD_TRUE significa que se ha perdido la conexión, mientras que el valor SQL_CD_FALSE significa que la conexión todavía está activa. (Los controladores que se ajustan a las versiones anteriores de ODBC también pueden admitir este atributo).  
  
 Un controlador debe implementar esta opción de forma eficaz o afectará al rendimiento de la agrupación de conexiones. En concreto, una llamada a get this Connection Attribute no debe producir un recorrido de ida y vuelta al servidor. En su lugar, un controlador solo debe devolver el último estado conocido de la conexión. La conexión está muerta si se produjo un error en el último recorrido al servidor y no está inactivo si el último recorrido se realizó correctamente.  
  
## <a name="remarks"></a>Observaciones  
 Si se ha perdido una conexión (se ha comunicado mediante SQL_ATTR_CONNECTION_DEAD), el administrador de controladores ODBC destruirá la conexión llamando a SQLDisconnect en el controlador. Es posible que las nuevas solicitudes de conexión no encuentren una conexión utilizable en el grupo. Finalmente, el administrador de controladores puede crear una nueva conexión, suponiendo que el grupo está vacío.  
  
 Para usar un grupo de conexiones, una aplicación realiza los siguientes pasos:  
  
1.  Habilita la agrupación de conexiones mediante una llamada a **SQLSetEnvAttr** para establecer el atributo de entorno SQL_ATTR_CONNECTION_POOLING en SQL_CP_ONE_PER_DRIVER o SQL_CP_ONE_PER_HENV. Esta llamada debe realizarse antes de que la aplicación asigne el entorno compartido para el que se va a habilitar la agrupación de conexiones. El identificador de entorno en la llamada a **SQLSetEnvAttr** debe establecerse en null, lo que hace que SQL_ATTR_CONNECTION_POOLING un atributo de nivel de proceso. Si el atributo se establece en SQL_CP_ONE_PER_DRIVER, se admite un único grupo de conexiones para cada controlador. Si una aplicación funciona con muchos controladores y pocos entornos, esto podría ser más eficaz porque es posible que se requieran menos comparaciones. Si se establece en SQL_CP_ONE_PER_HENV, se admite un único grupo de conexiones para cada entorno. Si una aplicación funciona con muchos entornos y con pocos controladores, podría ser más eficaz porque es posible que se requieran menos comparaciones. La agrupación de conexiones está deshabilitada al establecer SQL_ATTR_CONNECTION_POOLING en SQL_CP_OFF.  
  
2.  Asigna un entorno mediante una llamada a **SQLAllocHandle** con el argumento *HandleType* establecido en SQL_HANDLE_ENV. El entorno asignado por esta llamada será un entorno compartido implícito porque se ha habilitado la agrupación de conexiones. Sin embargo, no se determina el entorno que se va a usar hasta que se llama a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_DBC en este entorno.  
  
3.  Asigna una conexión mediante una llamada a **SQLAllocHandle** con *InputHandle* establecido en SQL_HANDLE_DBC y el *InputHandle* establecido en el identificador de entorno asignado para la agrupación de conexiones. El administrador de controladores intenta encontrar un entorno existente que coincida con los atributos de entorno establecidos por la aplicación. Si no existe ningún entorno de este tipo, se crea uno, con un recuento de referencias (mantenido por el administrador de controladores) de 1. Si se encuentra un entorno compartido coincidente, el entorno se devuelve a la aplicación y se incrementa su recuento de referencias. (El administrador de controladores no determina la conexión real que se va a usar hasta que se llame a **SQLConnect** o **SQLDriverConnect** ).  
  
4.  Llama a **SQLConnect** o **SQLDriverConnect** para realizar la conexión. El administrador de controladores usa las opciones de conexión en la llamada a **SQLConnect** (o las palabras clave de conexión en la llamada a **SQLDriverConnect**) y los atributos de conexión establecidos después de la asignación de conexión para determinar qué conexión del grupo se debe usar.  
  
    > [!NOTE]  
    >  La coincidencia de una conexión solicitada con una conexión agrupada viene determinada por el atributo de entorno SQL_ATTR_CP_MATCH. Para obtener más información, vea [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md).  
  
     Las aplicaciones ODBC que utilizan la agrupación de conexiones deben llamar a [CoInitializeEx](/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex) durante la inicialización de la aplicación y [CoUninitialize](/windows/win32/api/combaseapi/nf-combaseapi-couninitialize) cuando se cierra la aplicación.  
  
5.  Llama a **SQLDisconnect** cuando se realiza con la conexión. La conexión se devuelve al grupo de conexiones y pasa a estar disponible para su reutilización.  
  
 Para obtener una explicación detallada, consulte [agrupación en Microsoft Data Access Components](/previous-versions/ms810829(v=msdn.10)).  
  
## <a name="connection-pooling-considerations"></a>Consideraciones sobre la agrupación de conexiones  
 La realización de cualquiera de las siguientes acciones mediante un comando SQL (en lugar de a través de la API de ODBC) puede afectar al estado de la conexión y provocar problemas inesperados cuando la agrupación de conexiones está activa:  
  
-   Abrir una conexión y cambiar la base de datos predeterminada.  
  
-   Usar la instrucción SET para cambiar las opciones configurables (incluidas SET ROWCOUNT, ANSI_NULL, IMPLICIT_TRANSACTIONS, SHOWPLAN, STATISTICs, TEXTSIZE y DATEFORMAT).  
  
-   Crear tablas temporales y procedimientos almacenados.  
  
 Si alguna de estas acciones se realiza fuera de la API de ODBC, la siguiente persona que use la conexión heredará automáticamente la configuración, las tablas o los procedimientos anteriores.  
  
> [!NOTE]  
>  No espere que haya determinada configuración en el estado de la conexión. Siempre debe establecer el estado de conexión en la aplicación y asegurarse de que la aplicación quita los valores de agrupación de conexiones no usados.  
  
## <a name="driver-aware-connection-pooling"></a>Agrupación de conexiones dependientes del controlador  
 A partir de Windows 8, un controlador ODBC puede usar las conexiones del grupo de forma más eficaz. Para obtener más información, consulte [agrupación de conexiones compatible con controladores](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md).  
  
## <a name="see-also"></a>Consulte también  
 [Conexión a un origen de datos o un controlador](../../../odbc/reference/develop-app/connecting-to-a-data-source-or-driver.md)   
 [Desarrollar un controlador ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Agrupación en Microsoft Data Access Components](/previous-versions/ms810829(v=msdn.10))