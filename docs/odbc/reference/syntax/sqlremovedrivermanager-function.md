---
description: Función SQLRemoveDriverManager
title: Función SQLRemoveDriverManager | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDriverManager
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDriverManager
helpviewer_keywords:
- SQLRemoveDriverManager function function [ODBC]
ms.assetid: 3a41511f-6603-4b81-a815-7883874023c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 92359b7df67535f8bee5a5a3dfb4b7a7348f54f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192494"
---
# <a name="sqlremovedrivermanager-function"></a>Función SQLRemoveDriverManager
**Conformidad**  
 Versión introducida: ODBC 3,0: desusado en Windows XP Service Pack 2, Windows Server 2003 Service Pack 1 y sistemas operativos posteriores.  
  
 **Resumen**  
 **SQLRemoveDriverManager** cambia o quita información sobre los componentes principales de ODBC de la entrada Odbcinst.ini en la información del sistema.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
BOOL SQLRemoveDriverManager(  
     LPDWORD     pdwUsageCount);  
```  
  
## <a name="arguments"></a>Argumentos  
 *pdwUsageCount*  
 Genere El recuento de uso del administrador de controladores después de llamar a esta función.  
  
## <a name="returns"></a>Devoluciones  
 La función devuelve TRUE si es correcto, FALSE si se produce un error. Si no existe ninguna entrada en la información del sistema cuando se llama a esta función, la función devuelve FALSE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLRemoveDriverManager** devuelve false, se puede obtener un valor de *\* pfErrorCode* asociado mediante una llamada a **SQLInstallerError**. En la tabla siguiente se enumeran los valores de *\* pfErrorCode* que puede devolver **SQLInstallerError** y se explica cada uno de ellos en el contexto de esta función.  
  
|*\*pfErrorCode*|Error|Descripción|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Error general del instalador|Se produjo un error en el que no había ningún error específico del instalador.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|No se encontró el componente en el registro|El instalador no pudo quitar la información del administrador de controladores porque no existía en el registro o no se encontró en el registro.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|No se pudo aumentar o reducir el recuento de uso de componentes|El instalador no pudo reducir el recuento de uso del administrador de controladores.|  
|ODBC_ERROR_OUT_OF_MEM|No hay memoria suficiente|El instalador no pudo realizar la función debido a una falta de memoria.|  
  
## <a name="comments"></a>Comentarios  
 **SQLRemoveDriverManager** complementa la función **SQLInstallDriverManager** y actualiza el recuento de uso de componentes en la información del sistema. Solo se debe llamar a esta función desde una aplicación de instalación.  
  
 **SQLRemoveDriverManager** disminuirá el recuento de uso de componentes principales en 1. Si el recuento de uso de componentes va a 0, se quitará la información del sistema de entrada. La entrada del componente principal se encuentra en la siguiente ubicación de la información del sistema, en el título "ODBC Core":  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
> [!CAUTION]  
>  Una aplicación no debe quitar físicamente los archivos del administrador de controladores cuando el recuento de uso de componentes y el recuento de uso de archivos llega a cero.  
  
 **SQLRemoveDriverManager** realmente no quita ningún archivo. El programa de llamada es responsable de la eliminación de archivos y el mantenimiento de los recuentos de uso de archivos. No obstante, los archivos del administrador de controladores no deben quitarse cuando el recuento de uso de componentes y el recuento de uso de archivos alcanzan cero, ya que estos archivos pueden ser usados por otras aplicaciones que no han incrementado el recuento de uso de archivos.  
  
 Se llama a **SQLRemoveDriverManager** como parte del proceso de desinstalación. Los componentes principales de ODBC (que incluyen el administrador de controladores, la biblioteca de cursores, el instalador, la biblioteca de lenguajes, el administrador, los archivos de thunk, etc.) se desinstalan en su totalidad. Los siguientes archivos no se quitan cuando se llama a **SQLRemoveDriverManager** como parte del proceso de desinstalación:  

:::row:::
    :::column:::
        ODBC32DLL  
        ODBCCR32.DLL  
        ODBCCU32.DLL  
        ODBCINT.DLL  
        ODBCTRAC.DLL  
        MSVCRT40.DLL  
        ODBCCP32.CPL  
    :::column-end:::
    :::column:::
        ODBCCP32.DLL  
        ODBC16GT.DLL  
        ODBC32GT.DLL  
        DS16GT.DLL  
        DS32GT.DLL  
        ODBCAD32.EXE  
    :::column-end:::
:::row-end:::

 También se llama a **SQLRemoveDriverManager** como parte de un proceso de actualización. Si una aplicación detecta que tiene que realizar una actualización y ha instalado previamente el controlador, el controlador se debe quitar y volver a instalar.  
  
 Primero se debe llamar a **SQLRemoveDriverManager** para reducir el recuento de uso de componentes. A continuación, se debe llamar a **SQLInstallDriverEx** para incrementar el recuento de uso de componentes. El programa de instalación de la aplicación debe reemplazar los archivos de componentes principales antiguos por los nuevos archivos. Los recuentos de uso de archivos seguirán siendo los mismos y otras aplicaciones que usen los archivos de componentes principales de la versión anterior utilizarán los archivos de la versión más reciente.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Instalación de un administrador de controladores|[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|
