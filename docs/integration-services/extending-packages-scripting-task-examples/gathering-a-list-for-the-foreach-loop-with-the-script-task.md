---
description: Recopilar una lista para el bucle Foreach con la tarea Script
title: Recopilar una lista para el bucle Para cada uno con la tarea Script | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Foreach Loop containers
- Script task [Integration Services], Foreach loops
- Script task [Integration Services], examples
- SSIS Script task, Foreach loops
ms.assetid: 694f0462-d0c5-4191-b64e-821b1bdef055
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e8ff797225d3a97673bf0f1b108ce15eb6e8bd52
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92193105"
---
# <a name="gathering-a-list-for-the-foreach-loop-with-the-script-task"></a>Recopilar una lista para el bucle Foreach con la tarea Script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El enumerador de variable para Foreach enumera los elementos de una lista que le se pasa en una variable y realiza las mismas tareas en cada elemento. Puede utilizar código personalizado en una tarea Script para rellenar una lista con este fin. Para obtener más información sobre el enumerador, vea [Contenedor de bucles Para cada uno](../../integration-services/control-flow/foreach-loop-container.md).  
  
> [!NOTE]  
>  Si desea crear una tarea que pueda reutilizar más fácilmente en varios paquetes, considere la posibilidad de utilizar el código de este ejemplo de tarea Script como punto inicial de una tarea personalizada. Para más información, vea [Desarrollar una tarea personalizada](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="description"></a>Descripción  
 En el ejemplo siguiente se utilizan métodos del espacio de nombres **System.IO** para recopilar una lista de libros de Excel del equipo que son posteriores o anteriores a un número de días especificado por el usuario en una variable. Busca de forma recursiva en los directorios de la unidad C los archivos que tienen la extensión .xls y examina la fecha en la que cada archivo se modificó por última vez para determinar si el archivo pertenece a la lista. Agrega los archivos correspondientes a un objeto **ArrayList** y guarda **ArrayList** en una variable para el uso posterior en un contenedor de bucles Para cada uno. El contenedor de bucles Foreach se configura para utilizar el enumerador de variable para Foreach.  
  
> [!NOTE]  
>  La variable que utiliza con el enumerador de variable para Para cada uno debe ser del tipo **Object**. El objeto que se coloca en la variable debe implementar una de las interfaces siguientes: **System.Collections.IEnumerable**, **System.Runtime.InteropServices.ComTypes.IEnumVARIANT**, **System.ComponentModel IListSource** o **Microsoft.SqlServer.Dts.Runtime.Wrapper.ForEachEnumeratorHost**. En general, se usa **Array** o **ArrayList**. **ArrayList** requiere una referencia y una instrucción **Imports** para el espacio de nombres **System.Collections**.  
  
 Puede experimentar con esta tarea utilizando diferentes valores positivos y negativos en la variable de paquete `FileAge`. Por ejemplo, puede escribir 5 para buscar archivos creados en los cinco últimos días o escribir -3 para buscar archivos creados hace más de tres días. Esta tarea puede tardar un minuto o dos en una unidad de disco con un gran número de carpetas donde buscar.  
  
#### <a name="to-configure-this-script-task-example"></a>Para configurar este ejemplo de tarea Script  
  
1.  Cree una variable de paquete denominada `FileAge` del tipo entero y escriba un valor entero positivo o negativo. Cuando el valor es positivo, el código busca archivos posteriores al número especificado de días; cuando es negativo, archivos anteriores al número especificado de días.  
  
2.  Cree una variable de paquete denominada `FileList` de tipo **Object** para recibir la lista de archivos recopilada por la tarea Script para su uso posterior en el enumerador de variable para Para cada uno.  
  
3.  Agregue la variable `FileAge` a la propiedad **ReadOnlyVariables** de la tarea Script y la variable `FileList` a la propiedad **ReadWriteVariables**.  
  
4.  En el código, importe los espacios de nombres **System.Collections** y **System.IO**.  
  
### <a name="code"></a>Código  
  
```vb  
Imports System  
Imports System.Data  
Imports System.Math  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports System.Collections  
Imports System.IO  
  
Public Class ScriptMain  
  
  Private Const FILE_AGE As Integer = -50  
  
  Private Const FILE_ROOT As String = "C:\"  
  Private Const FILE_FILTER As String = "*.xls"  
  
  Private isCheckForNewer As Boolean = True  
  Dim fileAgeLimit As Integer  
  Private listForEnumerator As ArrayList  
  
  Public Sub Main()  
  
    fileAgeLimit = DirectCast(Dts.Variables("FileAge").Value, Integer)  
  
    ' If value provided is positive, we want files NEWER THAN n days.  
    '  If negative, we want files OLDER THAN n days.  
    If fileAgeLimit < 0 Then  
      isCheckForNewer = False  
    End If  
    ' Extract number of days as positive integer.  
    fileAgeLimit = Math.Abs(fileAgeLimit)  
  
    listForEnumerator = New ArrayList  
  
    GetFilesInFolder(FILE_ROOT)  
  
    ' Return the list of files to the variable  
    '  for later use by the Foreach from Variable enumerator.  
    System.Windows.Forms.MessageBox.Show("Matching files: " & listForEnumerator.Count.ToString, "Results", Windows.Forms.MessageBoxButtons.OK, Windows.Forms.MessageBoxIcon.Information)  
    Dts.Variables("FileList").Value = listForEnumerator  
  
    Dts.TaskResult = ScriptResults.Success  
  
  End Sub  
  
  Private Sub GetFilesInFolder(ByVal folderPath As String)  
  
    Dim localFiles() As String  
    Dim localFile As String  
    Dim fileChangeDate As Date  
    Dim fileAge As TimeSpan  
    Dim fileAgeInDays As Integer  
    Dim childFolder As String  
  
    Try  
      localFiles = Directory.GetFiles(folderPath, FILE_FILTER)  
      For Each localFile In localFiles  
        fileChangeDate = File.GetLastWriteTime(localFile)  
        fileAge = DateTime.Now.Subtract(fileChangeDate)  
        fileAgeInDays = fileAge.Days  
        CheckAgeOfFile(localFile, fileAgeInDays)  
      Next  
  
      If Directory.GetDirectories(folderPath).Length > 0 Then  
        For Each childFolder In Directory.GetDirectories(folderPath)  
          GetFilesInFolder(childFolder)  
        Next  
      End If  
  
    Catch  
      ' Ignore exceptions on special folders such as System Volume Information.  
    End Try  
  
  End Sub  
  
  Private Sub CheckAgeOfFile(ByVal localFile As String, ByVal fileAgeInDays As Integer)  
  
    If isCheckForNewer Then  
      If fileAgeInDays <= fileAgeLimit Then  
        listForEnumerator.Add(localFile)  
      End If  
    Else  
      If fileAgeInDays > fileAgeLimit Then  
        listForEnumerator.Add(localFile)  
      End If  
    End If  
  
  End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Data;  
using System.Math;  
using Microsoft.SqlServer.Dts.Runtime;  
using System.Collections;  
using System.IO;  
  
public partial class ScriptMain : Microsoft.SqlServer.Dts.Tasks.ScriptTask.VSTARTScriptObjectModelBase  
    {  
  
        private const int FILE_AGE = -50;  
  
        private const string FILE_ROOT = "C:\\";  
        private const string FILE_FILTER = "*.xls";  
  
        private bool isCheckForNewer = true;  
        int fileAgeLimit;  
        private ArrayList listForEnumerator;  
  
        public void Main()  
  {  
  
    fileAgeLimit = (int)(Dts.Variables["FileAge"].Value);  
  
    // If value provided is positive, we want files NEWER THAN n days.  
    // If negative, we want files OLDER THAN n days.  
    if (fileAgeLimit<0)  
    {  
      isCheckForNewer = false;  
    }  
    // Extract number of days as positive integer.  
    fileAgeLimit = Math.Abs(fileAgeLimit);  
  
    listForEnumerator = new ArrayList();  
  
    GetFilesInFolder(FILE_ROOT);  
  
    // Return the list of files to the variable  
    // for later use by the Foreach from Variable enumerator.  
    System.Windows.Forms.MessageBox.Show("Matching files: "+ listForEnumerator.Count, "Results",   
MessageBoxButtons.OK, MessageBoxIcon.Information);  
    Dts.Variables["FileList"].Value = listForEnumerator;  
  
    Dts.TaskResult = (int)ScriptResults.Success;  
  
  }  
  
        private void GetFilesInFolder(string folderPath)  
        {  
  
            string[] localFiles;  
            DateTime fileChangeDate;  
            TimeSpan fileAge;  
            int fileAgeInDays;  
  
            try  
            {  
                localFiles = Directory.GetFiles(folderPath, FILE_FILTER);  
                foreach (string localFile in localFiles)  
                {  
                    fileChangeDate = File.GetLastWriteTime(localFile);  
                    fileAge = DateTime.Now.Subtract(fileChangeDate);  
                    fileAgeInDays = fileAge.Days;  
                    CheckAgeOfFile(localFile, fileAgeInDays);  
                }  
  
                if (Directory.GetDirectories(folderPath).Length > 0)  
                {  
                    foreach (string childFolder in Directory.GetDirectories(folderPath))  
                    {  
                        GetFilesInFolder(childFolder);  
                    }  
                }  
  
            }  
            catch  
            {  
                // Ignore exceptions on special folders, such as System Volume Information.  
            }  
  
        }  
  
        private void CheckAgeOfFile(string localFile, int fileAgeInDays)  
        {  
  
            if (isCheckForNewer)  
            {  
                if (fileAgeInDays <= fileAgeLimit)  
                {  
                    listForEnumerator.Add(localFile);  
                }  
            }  
            else  
            {  
                if (fileAgeInDays > fileAgeLimit)  
                {  
                    listForEnumerator.Add(localFile);  
                }  
            }  
  
        }  
  
    }  
```  
  
## <a name="see-also"></a>Consulte también  
 [Contenedor de bucles Para cada uno](../../integration-services/control-flow/foreach-loop-container.md)   
 [Configurar un contenedor de bucles Foreach](../control-flow/foreach-loop-container.md)  
  
