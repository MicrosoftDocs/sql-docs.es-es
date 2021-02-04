---
description: Exportar información de servidores registrados (SQL Server Management Studio)
title: Exportar información de servidores registrados
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.exportregisteredservers.f1
helpviewer_keywords:
- Registered Servers [SQL Server], exporting
- exporting registered server information
- transferring registered server information
ms.assetid: b65e168f-b6bf-489c-b8ad-3b8644acf0b6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: a71bebde2525a18e062832aba064a4b9aecf7f3a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250485"
---
# <a name="export-registered-server-information-sql-server-management-studio"></a>Exportar información de servidores registrados (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este tema se describe cómo guardar y exportar información de servidores registrados en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]para distribuirla a otros empleados o servidores. Puede utilizar esta característica de exportación para crear una interfaz de usuario coherente en varios equipos.  
  
 La exportación y posterior importación de archivos de servidores registrados permite configurar fácilmente varios equipos con los mismos servidores en Servidores registrados. Esto resulta útil cuando se administra un gran número de servidores desde equipos en varias ubicaciones, o cuando se desea configurar un usuario menos experto con una configuración de conexión básica.  
  
> [!NOTE]  
>  Los registros de servidor que usan la autenticación de SQL Server almacenan las contraseñas por usuario. Después de exportar e importar los registros de servidor, los usuarios deben escribir la contraseña para cada servidor la primera vez que se conectan, y almacenar así las contraseñas en las listas de los servidores registrados. Esto no es necesario en los servidores registrados mediante la autenticación de Windows.  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-export-registered-server-information"></a>Para exportar información del servidor registrado  
  
1.  En Servidores registrados, haga clic con el botón derecho en un grupo de servidores y, luego, haga clic en **Exportar**.  
  
    > [!NOTE]  
    >  Se puede exportar un solo servidor, todo el árbol de servidores registrados o un subconjunto del árbol.  
  
2.  En el cuadro de diálogo **Exportar servidores registrados** , realice las selecciones siguientes.  
  
     **Grupo de servidores**  
     Especifique el grupo de servidores que se va a exportar. Puede exportar todos los servidores registrados, los servidores registrados de un grupo de servidores determinado o un solo servidor registrado. La función de exportación es recursiva; por ejemplo, si el grupo de servidores A contiene el grupo de servidores B y éste contiene los grupos C y D, la exportación del grupo A exporta todas las entradas de A, B, C y D.  
  
     El grupo de servidores muestra solo los grupos del actual árbol de servidores registrados.  
  
     **Archivo de exportación**  
     Escriba el nombre del archivo de exportación en el cuadro de texto o use el botón Examinar (**...**) para buscar un archivo de exportación en el equipo cliente. Si selecciona un archivo existente, la información del servidor registrado se anexa al archivo. Utilice la extensión .regsrvr. Si desea que la información del servidor registrado esté disponible para otros usuarios u otro equipo, puede guardar el archivo en la red. De este modo, otros usuarios podrán obtener acceso al archivo e importar parte o la totalidad de la información del servidor registrado. Si selecciona un archivo existente como archivo de exportación, el contenido del archivo se sobrescribirá con la información del servidor registrado.  
  
     **No incluir nombres de usuario ni contraseñas en el archivo de exportación**  
     Excluye los nombres de usuario en la exportación del archivo.  
  
    > [!IMPORTANT]  
    >  Los archivos de exportación están cifrados; sin embargo, si se incluyen nombres de usuario y contraseñas para la autenticación de SQL Server, el acceso al archivo debe controlarse cuidadosamente. De manera predeterminada, los nombres de usuario y las contraseñas se excluyen del archivo de exportación.  
  
## <a name="see-also"></a>Consulte también  
 [Importar información de servidores registrados &#40;SQL Server Management Studio&#41;](./import-registered-server-information-sql-server-management-studio.md)   
 [Crear un servidor registrado &#40;SQL Server Management Studio&#41;](./create-a-new-registered-server-sql-server-management-studio.md)  
  
