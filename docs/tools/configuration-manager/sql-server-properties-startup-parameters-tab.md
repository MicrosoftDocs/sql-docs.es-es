---
title: Propiedades de SQL Server (pestaña Parámetros de inicio)
description: Use la pestaña Parámetros de inicio del cuadro de diálogo Propiedades de SQL Server para agregar o quitar parámetros de inicio, lo que puede afectar al rendimiento del Motor de base de datos.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 16942624-5374-446c-8de4-ee6ed34d6e94
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a80683b2e2092cf6a0eff2519edbcd893c2357ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349594"
---
# <a name="sql-server-properties-startup-parameters-tab"></a>Propiedades de SQL Server (pestaña Parámetros de inicio)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilice este cuadro de diálogo para agregar o quitar parámetros de inicio para el [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Los parámetros de inicio pueden tener una gran influencia en el rendimiento del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Antes de agregar o cambiar los parámetros de inicio, vea el tema "Usar las opciones de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] " en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="options"></a>Opciones  
 **Especifique un parámetro de inicio**  
 Para agregar un parámetro, escríbalo y haga clic en **Agregar**.  
  
 Para modificar uno de los parámetros necesarios, seleccione el parámetro en el cuadro **Parámetros existentes** , cambie los valores en el cuadro **Especifique un parámetro de inicio** y, a continuación, haga clic en **Actualizar**.  
  
 **Parámetros existentes**  
 Para quitar un parámetro, selecciónelo y haga clic en **Quitar**.  
  
## <a name="parameter-format"></a>Formato de parámetros  
 No incluya un separador entre parámetros. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo agrega automáticamente. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aplica los requisitos de parámetro siguientes.  
  
-   Los espacios iniciales y finales se recortan de los parámetros de inicio.  
  
-   Todos los parámetros de inicio empiezan con un guion (–) y el segundo valor es una letra.  
  
## <a name="required-parameters"></a>Parámetros obligatorios  
 Los parámetros siguientes son necesarios. Se pueden cambiar pero no quitar.  
  
-   -d es la ruta de acceso al archivo **master.mdf** (el archivo de datos de la base de datos maestra).  
  
-   -l es la ruta de acceso al archivo **master.ldf** (el archivo de registro de la base de datos maestra).  
  
-   -e es la ruta de acceso de los archivos de registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!CAUTION]  
>  Si los parámetros de ruta de acceso al archivo son incorrectos, es posible que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se inicie.  
  
 Para obtener más información sobre cómo mover la base de datos maestra, vea el tema "Mover bases de datos del sistema" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="optional-parameters"></a>Parámetros opcionales  
 Todos los parámetros de inicio admitidos se describen en el tema "Usar las opciones de inicio del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un parámetro de inicio de -T *n.º de seguimiento* indica que una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha de iniciarse con una marca de seguimiento especificada (*n.º de seguimiento*) activa. Las marcas de seguimiento se utilizan para iniciar el servidor con un comportamiento distinto del habitual. Para más información sobre las marcas de seguimiento, consulte el tema "Marcas de seguimiento ([!INCLUDE[tsql](../../includes/tsql-md.md)])" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!CAUTION]  
>  Puede ver parámetros de inicio y marcas de seguimiento sin documentar adicionales en Internet. Los parámetros de inicio y marcas de seguimiento sin documentar se crean para resolver problemas poco habituales o forzar determinadas condiciones requeridas para las pruebas. El uso de parámetros de inicio sin documentar puede proporcionar resultados inesperados. No utilice parámetros sin documentar a menos que estén dirigidos por los servicios de soporte al cliente de Microsoft.  
  
 La lista siguiente describe algunos parámetros opcionales comunes.  
  
|Parámetro|Descripción breve|  
|---------------|-----------------------|  
|-M|Inicia una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de usuario único.|  
|-T1204|Devuelve los recursos y los tipos de bloqueos que participan en un interbloqueo, además del comando actual afectado.|  
|-T1224|Deshabilita la extensión de bloqueo en función del número de bloqueos.|  
|-T3608|Evita que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inicie automáticamente y recupere bases de datos excepto la base de datos maestra.|  
|-T7806|Habilita una conexión de administrador dedicada (DAC) en [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].|  
  
> [!CAUTION]  
>  Algunos parámetros opcionales pueden cambiar el comportamiento del servidor y afectar al rendimiento.  
  
## <a name="permissions"></a>Permisos  
 El uso de esta página está restringido a los usuarios que pueden cambiar las entradas relacionadas en el Registro. Esto incluye a los usuarios siguientes.  
  
-   Miembros del grupo local de administradores.  
  
-   La cuenta de dominio utilizada por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el [!INCLUDE[ssDE](../../includes/ssde-md.md)] está configurado para ejecutarse bajo una cuenta de dominio.  
  
## <a name="books-online-references"></a>Referencias de los Libros en pantalla  
 Para obtener más información sobre los parámetros de inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea "Cómo configurar opciones de inicio del servidor (Administrador de configuración de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] )" en los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
