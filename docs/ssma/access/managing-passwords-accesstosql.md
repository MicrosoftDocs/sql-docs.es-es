---
description: Administrar contraseñas (AccessToSQL)
title: Administración de contraseñas (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 07/01/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: b099d0f9-dd37-4c87-8b6f-ed0177881ea4
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1d6fa64888898221ced921a2306278b7c4503fc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058920"
---
# <a name="managing-passwords-accesstosql"></a>Administrar contraseñas (AccessToSQL)
Esta sección trata sobre la protección de contraseñas de base de datos y el procedimiento para importarlas o exportarlas entre servidores:  
  
1.  Protección de contraseñas  
  
2.  Exportar o importar contraseña cifrada  
  
## <a name="securing-password"></a>Protección de contraseñas  
SSMA le permite proteger la contraseña de una base de datos.  
  
Use el procedimiento siguiente para implementar una conexión segura:  
  
Especifique una contraseña válida con uno de los tres métodos siguientes:  
  
1.  **Texto no cifrado:** Escriba la contraseña de la base de datos en el atributo de valor del nodo ' contraseña '. Se encuentra en el nodo definición del servidor en la sección servidor del archivo de script o el archivo de conexión de servidor.  
  
    Las contraseñas en texto no cifrado no son seguras. Por lo tanto, se mostrará el siguiente mensaje de advertencia en la salida de la consola: *"la &lt; contraseña del ID. del servidor del servidor &gt; se proporciona en forma de texto no cifrado no seguro, la aplicación de consola SSMA proporciona una opción para proteger la contraseña a través del cifrado, consulte la opción-securepassword en el archivo de ayuda de SSMA para obtener más información".*  
  
    **Contraseñas cifradas:** La contraseña especificada, en este caso, se almacena en un formato cifrado en el equipo local en ProtectedStorage. SSMA.  
  
    -   **Protección de contraseñas**  
  
        -   Ejecute `SSMAforAccessConsole.exe` con la `-securepassword` línea de comandos y Add switch at pasando la conexión de servidor o el archivo de script que contiene el nodo de contraseña en la sección de definición de servidor.  
  
        -   En el símbolo del sistema, se pide al usuario que escriba la contraseña de la base de datos y la confirme.  
  
            Los identificadores de definición de servidor y sus contraseñas cifradas correspondientes se almacenan en un archivo en el equipo local.  

            &nbsp;

            _Ejemplo 1:_
            
            Especificar contraseña

            ```console
            C:\SSMA\SSMAforAccessConsole.EXE -securepassword -add all -s "D:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "D:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ VariableValueFileSample.xml"
            ```

            Escriba la contraseña de server_id ' XXX_1 ': xxxxxxx
                
            Vuelva a escribir la contraseña de server_id ' XXX_1 ': xxxxxxx  

            &nbsp;

            _Ejemplo 2:_

            ```console
            C:\SSMA\SSMAforAccessConsole.EXE -securepassword -add "source_1,target_1" -c "D:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ServersConnectionFileSample.xml" - v "D:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ VariableValueFileSample.xml" -o
            ```

            Escriba la contraseña de server_id ' source_1 ': xxxxxxx
                
            Vuelva a escribir la contraseña de server_id ' source_1 ': xxxxxxx
                
            Escriba la contraseña de server_id ' target_1 ': xxxxxxx
                
            Vuelva a escribir la contraseña para server_id ' Target _ 1 ': xxxxxxx  
  
    -   **Quitar contraseñas cifradas**  
  
        Ejecute `SSMAforAccessConsole.exe` con el `-securepassword` `-remove` modificador y en la línea de comandos pasando los identificadores de servidor para quitar las contraseñas cifradas del archivo de almacenamiento protegido presente en el equipo local.  

        ```console
        C:\SSMA\SSMAforAccessConsole.EXE -securepassword -remove all
        C:\SSMA\SSMAforAccessConsole.EXE -securepassword -remove "source_1,target_1"
        ```
  
    -   **Enumerar los identificadores de servidor cuyas contraseñas están cifradas**  
  
        Ejecute el SSMAforAccessConsole.exe con la `-securepassword` `-list` línea de comandos y en la línea de comandos para enumerar todos los identificadores de servidor cuyas contraseñas se han cifrado.  

        ```console
        C:\SSMA\SSMAforAccessConsole.EXE -securepassword -list
        ```
  
    > [!NOTE]  
    > 1.  La contraseña en texto no cifrado mencionada en el archivo de conexión de script o servidor tiene prioridad sobre la contraseña cifrada en un archivo protegido.  
    > 2.  Cuando no existe ninguna contraseña en la sección servidor del archivo de conexión del servidor o el archivo de script o si no se ha protegido en el equipo local, la consola de le pide que escriba la contraseña.  
  
## <a name="exporting-or-importing-encrypted-passwords"></a>Exportar o importar contraseñas cifradas  
La aplicación de consola SSMA permite exportar contraseñas de base de datos cifradas presentes en un archivo del equipo local a un archivo protegido y viceversa. Ayuda a que las contraseñas cifradas sean independientes de la máquina. La funcionalidad de exportación lee el identificador y la contraseña del servidor desde el almacenamiento protegido local y guarda la información en un archivo cifrado. Se solicita al usuario que escriba la contraseña del archivo protegido. Asegúrese de que la contraseña especificada tiene una longitud de 8 caracteres o más. Este archivo protegido es portátil entre diferentes equipos. La funcionalidad de importación lee el ID. de servidor y la información de contraseña del archivo protegido. Se solicita al usuario que escriba la contraseña del archivo protegido y anexa la información al almacenamiento protegido local.  

### <a name="export-password"></a>Exportar contraseña

1. Escriba la contraseña para proteger el archivo exportado

2. `C:\SSMA\SSMAforAccessConsole.EXE -securepassword -export all "machine1passwords.file"`

3. Escriba la contraseña para proteger el archivo exportado: xxxxxxxx

4. Confirme la contraseña: xxxxxxxx

5. `C:\SSMA\SSMAforAccessConsole.EXE -p -e "AccessDB_1_1,Sql_1" "machine2passwords.file"`

6. Escriba la contraseña para proteger el archivo exportado: xxxxxxxx

7. Confirme la contraseña: xxxxxxxx  

### <a name="import-an-encrypted-password"></a>Importar una contraseña cifrada

1. Escriba la contraseña para proteger el archivo importado.

2. `C:\SSMA\SSMAforAccessConsole.EXE -securepassword -import all "machine1passwords.file"`

3. Escriba la contraseña para importar los servidores del archivo cifrado: xxxxxxxx

4. Confirme la contraseña: xxxxxxxx

5. `C:\SSMA\SSMAforAccessConsole.EXE -p -i "AccessDB_1,Sql_1" "machine2passwords.file"`

6. Escriba la contraseña para importar los servidores del archivo cifrado: xxxxxxxx

7. Confirme la contraseña: xxxxxxxx  

## <a name="see-also"></a>Consulte también  
[Ejecutar la consola SSMA (acceso)](./executing-the-ssma-console-accesstosql.md)  
