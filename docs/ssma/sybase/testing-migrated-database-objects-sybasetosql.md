---
description: Pruebas con objetos de base de datos migrados (SybaseToSQL)
title: Probar objetos de base de datos migrados (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4937f6b4-86bd-4070-88df-3d216306c33a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f889538e5232eedaf3bab8006c54d56e1649ea4c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014795"
---
# <a name="testing-migrated-database-objects-sybasetosql"></a>Pruebas con objetos de base de datos migrados (SybaseToSQL)
Microsoft SQL Server Migration Assistant para Sybase Tester (SSMA Tester) prueba automáticamente la conversión de objetos de base de datos y la migración de datos realizada por SSMA. Una vez finalizados todos los pasos de migración de SSMA, utilice SSMA Tester para comprobar que los objetos convertidos funcionan de la misma manera y que todos los datos se transfirieron correctamente.  
  
> [!NOTE]  
> El componente de prueba está deshabilitado en el caso de la conectividad de Azure.  
  
Puede probar los siguientes tipos de objeto con el evaluador de SSMA:  
  
-   Tablas  
  
-   Procedimientos almacenados  
  
-   Vistas.  
  
-   Instrucciones independientes.  
  
SSMA Tester ejecuta objetos seleccionados para realizar pruebas en Sybase y sus homólogos en SQL Server. Después, compara los resultados según los criterios siguientes:  
  
-   ¿Los cambios en los datos de tabla son idénticos?  
  
-   ¿Son los valores de parámetros de salida para procedimientos y funciones idénticos?  
  
-   ¿Las funciones devuelven los mismos resultados?  
  
-   ¿Son idénticos los conjuntos de resultados?  
  
> [!NOTE]  
> Atención No utilice nunca el evaluador de SSMA en sistemas de producción. Durante la ejecución del evaluador, se modifican los datos y el esquema de origen. Mientras tanto, la restauración completa del estado original puede ser imposible para algunos tipos de código probado.  
  
## <a name="prerequisites"></a>Prerrequisitos  
Si desea usar SSMA Tester, instale SSMA Sybase Extension Pack con la opción **instalar base de datos de prueba** activada.  
  
Además, compruebe lo siguiente:  
  
-   El proveedor de OLE DB de Sybase está instalado en el equipo donde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta.  
  
-   Se ha habilitado la integración de Common Language Runtime (CLR) en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] motor de base de datos.  
  
Tenga en cuenta que la versión actual de SSMA Tester no admite la ejecución en paralelo por parte de usuarios diferentes en el mismo servidor de origen o de destino.  
  
## <a name="getting-started"></a>Introducción  
[Crear casos de prueba &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-test-cases-sybasetosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Instalación de componentes de SSMA en SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
[Configuración del proyecto &#40;conversión&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
