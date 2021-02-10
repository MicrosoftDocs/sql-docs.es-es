---
description: No compatible de Visual FoxPro comandos y funciones (controlador ODBC de Visual FoxPro)
title: Funciones y comandos de Visual FoxPro no admitidos | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- FoxPro ODBC driver [ODBC], commands and functions
- functions [ODBC], Visual FoxPro
- Visual FoxPro ODBC driver [ODBC], commands and functions
- Visual FoxPro commands and functions
- FoxPro ODBC driver
ms.assetid: afdb6b7e-738d-42ca-8053-67ae50873ca6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4ac7f25709cf91e03f9ad8b15f9fb0026ee5e98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015395"
---
# <a name="unsupported-visual-foxpro-commands-and-functions-visual-foxpro-odbc-driver"></a>No compatible de Visual FoxPro comandos y funciones (controlador ODBC de Visual FoxPro)
En la tabla siguiente se enumeran los comandos y las funciones de FoxPro que no son compatibles con el controlador ODBC de Visual FoxPro pero que son compatibles con Microsoft® Visual FoxPro®.  
  
 Si la aplicación interactúa con datos cuyas reglas, desencadenadores, valores predeterminados o procedimientos almacenados llaman a estos comandos o funciones de Visual FoxPro, el controlador puede generar un error.  
  
## <a name="unsupported-visual-foxpro-commands-and-functions"></a>Funciones y comandos de Visual FoxPro no admitidos  

:::row:::
    :::column:::
        ! Comando (vea ejecutar &#124;! Command  
        #<a name="define--undef"></a>DEFINIR... #UNDEF  
        #<a name="if--endif-preprocessor-directive"></a>IF... #ENDIF Directiva de preprocesador  
        #<a name="ifdef-124-ifndef"></a>IFDEF &#124; #IFNDEF  
        #<a name="include-preprocessor-directive"></a>INCLUIR Directiva de preprocesador  
        :: Operador de resolución de ámbito  
        ? ¿&#124;?? Get-Help  
    :::column-end:::
    :::column:::
        ??? Get-Help  
        @ ... BOX (comando)  
        @ ... CLASS (comando)  
        @ ... CLEAR (comando)  
        @ ... Comando Editar cuadros de edición  
        @ ... FILL (comando)  
        @ ... Obtener  
    :::column-end:::
    :::column:::
        @ ... Comando de menú  
        @ ... PROMPT (comando)  
        @ ... Comando SAY  
        @ ... SCROLL (comando)  
        @ ... Comando TO  
        \ &#124; \\ \ comando  
    :::column-end:::
:::row-end:::

## <a name="a"></a>A  

:::row:::
    :::column:::
        ACCEPT (comando)  
        ACLASS () (función)  
        Comando de menú activar  
        ACTIVAR el comando POPUP  
        Activar pantalla (comando)  
        ACTIVAR ventana (comando)  
    :::column-end:::
    :::column:::
        Método ActivateCell  
        Agregar clase (comando)  
        ADIR () (función)  
        AFONT () (función)  
        AINSTANCE () (función)  
        _ALIGNMENT variable de memoria del sistema  
    :::column-end:::
    :::column:::
        AMEMBERS () (función)  
        ANSITOOEM () (función)  
        APRINTERS () (función)  
        ASELOBJ () (función)  
        AYUDA (comando)  
    :::column-end:::
:::row-end:::

## <a name="b"></a>B  

:::row:::
    :::column:::
        BAR () (función)  
        BARCOUNT () (función)  
        BARPROMPT () (función)  
        _BEAUTIFY variable de memoria del sistema  
    :::column-end:::
    :::column:::
        _BOX variable de memoria del sistema  
        EXAMINAR (comando)  
        _BROWSER variable de memoria del sistema  
        Comando de compilación de la aplicación  
    :::column-end:::
    :::column:::
        BUILD EXE (comando)  
        Comando compilar proyecto  
        _BUILDER variable de memoria del sistema  
    :::column-end:::
:::row-end:::

## <a name="c"></a>C  

:::row:::
    :::column:::
        _CALCVALUE variable de memoria del sistema  
        CALL (comando)  
        Cancelar (comando)  
        CAPSLOCK () (función)  
        CD (comando)  
        CAMBIAR comando  
        CHDIR (comando)  
        CHRSAW () (función)  
        _CLIPTEXT variable de memoria del sistema  
        CERRAR el comando de memorando  
        CNTBAR () (función)  
        CNTPAD () (función)  
        COL () (función)  
        Comando compilar  
    :::column-end:::
    :::column:::
        Comando compilar base de datos  
        Compilar comando de formulario  
        COMPOBJ () (función)  
        Container (objeto)  
        Objeto de control  
        _CONVERTER variable de memoria del sistema  
        COPIAR archivo (comando)  
        COPIAR memorando (comando)  
        CREAR comando  
        Comando crear clase  
        CREAR comando CLASSLIB  
        CREAR conjunto de colores (comando)  
        CREAR conexión (comando)  
        CREAR base de datos (comando)  
    :::column-end:::
    :::column:::
        CREAR formulario (comando)  
        CREAR desde comando  
        Comando crear etiqueta  
        Comando de menú crear  
        Comando crear proyecto  
        CREAR consulta (comando)  
        Comando crear informe  
        CREAR pantalla (comando)  
        Comando crear vista SQL  
        CREAR DESENCADENAdor (comando)  
        CREAR vista (comando)  
        CREATEOBJECT () (función)  
        CURDIR () (función)  
        _CUROBJ variable de memoria del sistema  
    :::column-end:::
:::row-end:::

## <a name="d"></a>D  

:::row:::
    :::column:::
        _DBLCLICK variable de memoria del sistema  
        DBSETPROP () (función)  
        Funciones DDE  
        Comando de menú desactivar  
        DESACTIVAR comando emergente  
        DESACTIVAR ventana (comando)  
        Declare (comando)  
        Declare-DLL (comando)  
        Comando de barra de definición  
        Comando definir cuadro  
        Comando definir clase  
        DEFINIR el comando de menú  
    :::column-end:::
    :::column:::
        Comando definir panel  
        DEFINIR el comando POPUP  
        Comando definir ventana  
        Comando eliminar conexión  
        ELIMINAR base de datos (comando)  
        Comando eliminar archivo  
        Comando eliminar DESENCADENAdor  
        ELIMINAR Vista (comando)  
        _DIARYDATE variable de memoria del sistema  
        DIR (comando)  
        Comando de directorio  
        Mostrar comando  
    :::column-end:::
    :::column:::
        Mostrar conexiones (comando)  
        Mostrar base de datos (comando)  
        Comando Mostrar archivos dll  
        Mostrar archivos (comando)  
        Mostrar memoria (comando)  
        MOSTRAR objetos (comando)  
        Comando Mostrar procedimientos  
        Mostrar estado (comando)  
        Mostrar estructura (comando)  
        Comando Mostrar tablas  
        Comando mostrar vistas  
        HACER formulario (comando)  
    :::column-end:::
:::row-end:::

## <a name="e"></a>E  

:::row:::
    :::column:::
        Editar comando  
        EJECT (comando)  
        Comando de la página EJECT  
    :::column-end:::
    :::column:::
        Comando ERASe  
        ERROR (comando)  
        Comando de exportación  
    :::column-end:::
    :::column:::
        Comando externo  
    :::column-end:::
:::row-end:::

## <a name="f"></a>F  

:::row:::
    :::column:::
        FCHSIZE () (función)  
        FCLOSE () (función)  
        FCREATE () (función)  
        FEOF () (función)  
        FERROR () (función)  
        FFLUSH () (función)  
        FGETS () (función)  
    :::column-end:::
    :::column:::
        Comando de filtro  
        BUSCAR (comando)  
        FKLABEL () (función)  
        FKMAX () (función)  
        FONTMETRIC () (función)  
        FOPEN () (función)  
        _FOXDOC variable de memoria del sistema  
    :::column-end:::
    :::column:::
        _FOXGRAPH variable de memoria del sistema  
        FPUTS () (función)  
        FREAD () (función)  
        FSEEK () (función)  
        FWRITE () (función)  
    :::column-end:::
:::row-end:::

## <a name="g"></a>G  

:::row:::
    :::column:::
        _GENGRAPH variable de memoria del sistema  
        _GENMENU variable de memoria del sistema  
        _GENPD variable de memoria del sistema  
        _GENSCRN variable de memoria del sistema  
        _GENXTAB variable de memoria del sistema  
    :::column-end:::
    :::column:::
        GETBAR () (función)  
        GETCOLOR () (función)  
        GETDIR () (función)  
        Comando GETEXPR  
        GETFILE () (función)  
    :::column-end:::
    :::column:::
        GETFONT () (función)  
        GETOBJECT () (función)  
        GETPAD () (función)  
        GETPICT () (función)  
        GETPRINTER () (función)  
    :::column-end:::
:::row-end:::

## <a name="h"></a>H  

:::row:::
    :::column:::
        Comando ayuda  
        OCULTAR comando de menú  
    :::column-end:::
    :::column:::
        OCULTAR comando emergente  
        OCULTAR ventana (comando)  
    :::column-end:::
    :::column:::
        Función HOME ()  
    :::column-end:::
:::row-end:::

## <a name="i"></a>I  

:::row:::
    :::column:::
        IMESTATUS () (función)  
        Comando importar  
        _INDENT variable de memoria del sistema  
        INDEX ON (comando)  
    :::column-end:::
    :::column:::
        INKEY () (función)  
        Comando de entrada  
        INSERTAR comando  
        INSMODE () (función)  
    :::column-end:::
    :::column:::
        ISCOLOR () (función)  
        ISMOUSE () (función)  
    :::column-end:::
:::row-end:::

## <a name="j"></a>J  

JOIN (comando)

## <a name="k"></a>K  

Comando de teclado

## <a name="l"></a>L  

:::row:::
    :::column:::
        ETIQUETA (comando)  
        LASTKEY () (función)  
        Función de ropa ()  
    :::column-end:::
    :::column:::
        Mostrar comandos  
        Comando ENUMERAr conexiones  
        _LMARGIN variable de memoria del sistema  
    :::column-end:::
    :::column:::
        Comando LOAD  
        ARCHIVO LOCFILE () (función)  
    :::column-end:::
:::row-end:::

## <a name="m"></a>M  

:::row:::
    :::column:::
        MCOL () (función)  
        MD (comando)  
        MDOWN () (función)  
        MEMORY () (función)  
        Comando de menú  
        MENU () (función)  
        MENÚ a comando  
        MESSAGEBOX () (función)  
        MKDIR (comando)  
        Comando modificar clase  
        Comando MODIFY COMMAND  
        Comando modificar conexión  
    :::column-end:::
    :::column:::
        Comando MODIFY DATABASE  
        Comando MODIFY FILE  
        Comando modificar formulario  
        Comando modificar GENERAL  
        Comando modificar etiqueta  
        MODIFICAR el comando MEMO  
        Comando de menú modificar  
        Comando MODIFY PROCEDURE  
        Comando modificar proyecto  
        Comando modificar consulta  
        Comando modificar informe  
        Comando modificar pantalla  
    :::column-end:::
    :::column:::
        Comando modificar estructura  
        Comando modificar vista  
        Comando modificar ventana  
        Comando del MOUSE  
        MOVE (comando emergente)  
        Comando MOVE WINDOW  
        MRKBAR () (función)  
        MRKPAD () (función)  
        MROW () (función)  
        MWINDOW () (función)  
    :::column-end:::
:::row-end:::

## <a name="n"></a>N  

NUMLOCK () (función)

## <a name="o"></a>O  

:::row:::
    :::column:::
        OBJNUM () (función)  
        OBJTOCLIENT () (función)  
        OBJVAR () (función)  
        OEMTOANSI () (función)  
        EN el comando APLABOUT  
        Comando ON BAR  
        EN el comando ESCAPE  
        Comando al salir de la barra  
    :::column-end:::
    :::column:::
        EN el comando de menú salir  
        Comando al salir del panel  
        EN salir (comando emergente)  
        EN la clave = comando  
        EN el comando etiqueta de clave  
        EN el comando MACHELP  
        Comando ON PAD  
        Comando ON PAGE  
    :::column-end:::
    :::column:::
        EN el comando READERROR  
        Comando en la barra de selección  
        Comando del menú de selección  
        Comando en el panel de selección  
        Comando emergente en la selección  
        Comando ON SHUTDOWN  
    :::column-end:::
:::row-end:::

## <a name="p"></a>P  

:::row:::
    :::column:::
        PAQUETE de base de datos (comando)  
        PAD () (función)  
        _PADVANCE variable de memoria del sistema  
        _PAGENO variable de memoria del sistema  
        _PBPAGE variable de memoria del sistema  
        PCOL () (función)  
        _PCOLNO variable de memoria del sistema  
        _PCOPIES variable de memoria del sistema  
        _PDRIVER variable de memoria del sistema  
        _PDSETUP variable de memoria del sistema  
        _PECODE variable de memoria del sistema  
        _PEJECT variable de memoria del sistema  
        PEMSTATUS () (función)  
    :::column-end:::
    :::column:::
        _PEPAGE variable de memoria del sistema  
        Comando reproducir MACRO  
        _PLENGTH variable de memoria del sistema  
        _PLINENO variable de memoria del sistema  
        _PLOFFSET variable de memoria del sistema  
        Comando de tecla de POP  
        Comando de menú emergente  
        POP (comando emergente)  
        POPUP () (función)  
        _PPITCH variable de memoria del sistema  
        _PQUALITY variable de memoria del sistema  
        _PRETEXT variable de memoria del sistema  
        PRINTJOB... Comando ENDPRINTJOB  
    :::column-end:::
    :::column:::
        PRINTSTATUS () (función)  
        PRMBAR () (función)  
        PRMPAD () (función)  
        PROMPT () (función)  
        PROW () (función)  
        PRTINFO () (función)  
        _PSCODE variable de memoria del sistema  
        _PSPACING variable de memoria del sistema  
        Comando de tecla de comando de extracción  
        Comando de menú de extracción  
        Comando emergente de extracción  
        PUTFILE () (función)  
        _PWAIT variable de memoria del sistema  
    :::column-end:::
:::row-end:::

## <a name="q"></a>Q  

Comando salir

## <a name="r"></a>R  

:::row:::
    :::column:::
        RD (comando)  
        RDLEVEL () (función)  
        READ (comando)  
        Comando de menú leer  
        READKEY () (función)  
        REFRESH () (función)  
        REINDEX (comando)  
        VERSIÓN (comando)  
        Comando de barra de versión  
        Comando RELEASE CLASSLIB  
        Comando de la biblioteca de versión  
        Comando de menú de versión  
        Comando de módulo de versión  
    :::column-end:::
    :::column:::
        Comando del panel de versión  
        Comando Mostrar elementos emergentes  
        Comando de procedimiento de versión  
        Comando de versión de WINDOWS  
        QUITAR clase (comando)  
        Rename (comando)  
        Cambiar nombre de clase (comando)  
        Comando cambiar nombre de conexión  
        Comando cambiar nombre de tabla  
        Cambiar nombre de vista (comando)  
        Comando de informe  
        Requery () (función)  
        Comando restaurar desde  
    :::column-end:::
    :::column:::
        Comando RESTOre MACROs  
        Comando RESTOre SCREEN  
        Comando restaurar ventana  
        Comando resume  
        RGB () (función)  
        RGBSCHEME () (función)  
        _RMARGIN variable de memoria del sistema  
        RMDIR (comando)  
        ROW () (función)  
        EJECUTE &#124;. Get-Help  
        RUNSCRIPT (comando)  
    :::column-end:::
:::row-end:::

## <a name="s"></a>S  

:::row:::
    :::column:::
        Comando Guardar MACROs  
        Comando Guardar pantalla  
        GUARDAR en (comando)  
        GUARDAR comando de WINDOWS  
        SCHEME () (función)  
        SCOLS () (función)  
        _SCREEN variable de memoria del sistema  
        SCROLL (comando)  
        SET (comando)  
        ESTABLECER comando alternativo  
        Comando de ANSI SET  
        Comando SET APLABOUT  
        ESTABLECER el comando autosave  
        Comando SET BELL  
        ESTABLECER parpadeo (comando)  
        Comando establecer borde  
        Comando SET BRSTATUS  
        Comando SET CLASSLIB  
        Comando SET CLEAR  
        ESTABLECER reloj (comando)  
        ESTABLECER el COLOR del comando  
        ESTABLECER COLOR del esquema (comando)  
        ESTABLECER conjunto de colores (comando)  
        ESTABLECER el COLOR en el comando  
        ESTABLECER comando COMPATIBLE  
        ESTABLECER comando CONFIRM  
        ESTABLECER comando de consola  
        ESTABLECER CPCOMPILE  
        ESTABLECER CPDIALOG  
        ESTABLECER moneda (comando)  
        Comando SET CURSOR  
        ESTABLECER el comando de la sesión  
        ESTABLECER comando de depuración  
        Comando SET DECIMALs  
        Comando establecer delimitadores  
        ESTABLECER el comando de desarrollo  
        ESTABLECER dispositivo (comando)  
    :::column-end:::
    :::column:::
        ESTABLECER comando para mostrar  
        SET-HISTOry (comando)  
        ESTABLECER Eco (comando)  
        ESTABLECER ESCAPE (comando)  
        Comando SET FORMAT  
        Comando de la función SET  
        Comando establecer encabezados  
        ESTABLECER comando de ayuda  
        Comando SET HELPFILTER  
        ESTABLECER intensidad (comando)  
        Comando SET KEY  
        Comando SET KEYCOMP  
        Comando SET LOGERRORS  
        Comando SET MACDESKTOP  
        Comando SET MACHELP  
        Comando SET MACKEY  
        ESTABLECER margen (comando)  
        ESTABLECER marca de comando  
        ESTABLECER marcar en comando  
        Comando SET MEMOWIDTH  
        Comando SET MESSAGE  
        ESTABLECER comando del MOUSE  
        Comando SET odómetro  
        SET OLEOBJECT (comando)  
        Comando SET PALETte  
        Comando SET PDSETUP  
        Comando SET POINT  
        Comando establecer impresora  
        Comando SET READBORDER  
        ESTABLECER comando de actualización  
        ESTABLECER recurso (comando)  
        ESTABLECER comando de seguridad  
        ESTABLECER el comando del panel de resultados  
        SET SECONDs (comando)  
        ESTABLECER separador (comando)  
        Comando SET SHADOWs  
        Comando SET SKIP OF  
    :::column-end:::
    :::column:::
        Comando SET SPACE  
        Comando SET STATUs  
        ESTABLECER la barra de estado (comando)  
        Comando SET STEP  
        ESTABLECER comando permanente  
        Comando SET SYSFORMATS  
        Comando SET SYSMENU  
        Comando SET TALK  
        Comando SET TEXTMERGE  
        Comando SET TEXTMERGE Delimiters  
        Comando establecer tema  
        Comando establecer ID. de tema  
        Comando SET TRBETWEEN  
        Comando SET escritura anticipada  
        Comando establecer vista  
        ESTABLECER ventana de comando de memorando  
        Comando SET XCMDFILE  
        _SHELL variable de memoria del sistema  
        Mostrar comando GET  
        Mostrar obtiene el comando  
        Mostrar comando de menú  
        Mostrar objeto (comando)  
        Mostrar comando emergente  
        Mostrar ventana (comando)  
        SIZE (comando emergente)  
        TAMAÑO de la ventana (comando)  
        SKPBAR () (función)  
        SKPPAD () (función)  
        SOUNDEX () (función)  
        _SPELLCHK variable de memoria del sistema  
        Funciones SQL  
        SROWS () (función)  
        _STARTUP variable de memoria del sistema  
        SUSPENDEr (comando)  
        Funciones SYS () excepto SYS (2011)  
        SYSMETRIC () (función)  
    :::column-end:::
:::row-end:::

## <a name="t"></a>T  

:::row:::
    :::column:::
        _TABS variable de memoria del sistema  
        TEXTO... Comando ENDTEXT  
        _THROTTLE variable de memoria del sistema  
    :::column-end:::
    :::column:::
        TRANSFORM () (función)  
        _TRANSPORT variable de memoria del sistema  
        TXTWIDTH () (función)  
    :::column-end:::
    :::column:::
        Comando TYPE  
    :::column-end:::
:::row-end:::

## <a name="u"></a>U  

:::row:::
    :::column:::
        Función UPDATEd ()  
    :::column-end:::
    :::column:::
        USAR comando  
    :::column-end:::
:::row-end:::

## <a name="v"></a>V  

:::row:::
    :::column:::
        Comando validar base de datos  
    :::column-end:::
    :::column:::
        VARREAD () (función)  
    :::column-end:::
    :::column:::
        VERSION () (función)  
    :::column-end:::
:::row-end:::

## <a name="w"></a>W  

:::row:::
    :::column:::
        Comando WAIT  
        WBORDER () (función)  
        WCHILD () (función)  
        WCOLS () (función)  
        WEXIST () (función)  
        WFONT () (función)  
        _WINDOWS variable de memoria del sistema  
        CON... Comando ENDWITH  
    :::column-end:::
    :::column:::
        _WIZARD variable de memoria del sistema  
        WLAST () (función)  
        WLCOL () (función)  
        WLROW () (función)  
        WMAXIMUM () (función)  
        WMINIMUM () (función)  
        WONTOP () (función)  
        WOUTPUT () (función)  
    :::column-end:::
    :::column:::
        WPARENT () (función)  
        _WRAP variable de memoria del sistema  
        WREAD () (función)  
        WROWS () (función)  
        WTITLE () (función)  
        WVISIBLE () (función)  
    :::column-end:::
:::row-end:::

## <a name="z"></a>Z  

Comando de la ventana ZOOM
