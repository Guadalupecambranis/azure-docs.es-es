---
title: Solución de problemas de inicio y detención de máquinas virtuales con Azure Automation
description: En este artículo se proporciona información sobre la solución de problemas de inicio y detención de máquinas virtuales en Azure Automation.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 01/30/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ef2a782a19dd319de346f14d6189759d0a26686c
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665776"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Solución de problemas de la solución Start/Stop VMs during off-hours

## <a name="all-vms-fail-to-startstop"></a>Escenario: Error al iniciar o detener todas las máquinas virtuales

### <a name="issue"></a>Problema

Ha configurado la solución Start/Stop VMs, pero esta no inicia o detiene todas las máquinas virtuales configuradas.

### <a name="cause"></a>Causa

Este error puede deberse a alguna de las siguientes razones:

1. Una programación no está configurada correctamente.
2. La cuenta de ejecución puede no estar configurada correctamente.
3. Un runbook puede presentar errores.
4. Las máquinas virtuales pueden haberse excluido.

### <a name="resolution"></a>Resolución

Revise la siguiente lista de posibles soluciones para su problema o para saber dónde buscar:

* Compruebe que ha configurado correctamente una programación para la solución Start/Stop VMs. Para más información sobre cómo configurar una programación, vea el artículo sobre [programación](../automation-schedules.md).

* Compruebe los flujos de trabajo de los runbooks para detectar posibles errores. En Azure Portal, vaya a la cuenta de Automation y seleccione **Trabajos** en **Automatización de procesos**. En la página **Trabajos**, busque los trabajos de alguno de los siguientes runbooks:

  * AutoStop_CreateAlert_Child
  * AutoStop_CreateAlert_Parent
  * AutoStop_Disable
  * AutoStop_VM_Child
  * ScheduledStartStop_Base_Classic
  * ScheduledStartStop_Child_Classic
  * ScheduledStartStop_Child
  * ScheduledStartStop_Parent
  * SequencedStartStop_Parent

* Verifique que su [cuenta de ejecución](../manage-runas-account.md) tiene los permisos adecuados para las máquinas virtuales que intenta iniciar o detener. Para obtener información sobre cómo comprobar los permisos para un recurso, vea [Guía de inicio rápido: Visualización de los roles asignados a un usuario mediante Azure Portal](../../role-based-access-control/check-access.md). Deberá proporcionar el identificador de la aplicación para la entidad de servicio utilizada por la cuenta de ejecución. Para obtener este valor, vaya a la cuenta de Automation en Azure Portal, seleccione **Cuentas de ejecución** en **Configuración de cuenta** y haga clic en la cuenta de ejecución apropiada.

* Las máquinas virtuales no pueden iniciarse o detenerse en caso de que se hayan excluido de forma explícita. Las máquinas virtuales excluidas se definen en la variable **External_ExcludeVMNames** en la cuenta de Automation en la que está implementada la solución. El ejemplo siguiente, se muestra cómo se puede consultar ese valor con PowerShell.

  ```powershell-interactive
  Get-AzureRmAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="some-vms-fail-to-startstop"></a>Escenario: Algunas máquinas virtuales no se pueden iniciar o detener

### <a name="issue"></a>Problema

Ha configurado la solución Start/Stop VMs, pero esta no inicia o detiene algunas de las máquinas virtuales configuradas.

### <a name="cause"></a>Causa

Este error puede deberse a alguna de las siguientes razones:

1. Si usa el escenario en secuencia, podría faltar una etiqueta o ser incorrecta.
2. La máquina virtual puede estar excluida.
3. La cuenta de ejecución puede no tener suficientes permisos en la máquina virtual.
4. La máquina virtual puede tener alguna configuración que impide su inicio o detención.

### <a name="resolution"></a>Resolución

Revise la siguiente lista de posibles soluciones para su problema o para saber dónde buscar:

* Si usa el [escenario en secuencia](../automation-solution-vm-management.md#scenario-2-startstop-vms-in-sequence-by-using-tags) de la solución Start/Stop VMs during off-hours, debe asegurarse de que cada máquina virtual que desea iniciar o detener tiene la etiqueta correcta. Asegúrese de que las máquinas virtuales que desea iniciar tienen la etiqueta `sequencestart` y que aquellas que desea detener tienen la etiqueta `sequencestop`. Ambas etiquetas requieren un valor entero positivo. Puede usar una consulta similar a la del ejemplo siguiente para buscar todas las máquinas virtuales con las etiquetas y sus valores.

  ```powershell-interactive
  Get-AzureRmResource | ? {$_.Tags.Keys -contains "SequenceStart" -or $_.Tags.Keys -contains "SequenceStop"} | ft Name,Tags
  ```

* Las máquinas virtuales no pueden iniciarse o detenerse en caso de que se hayan excluido de forma explícita. Las máquinas virtuales excluidas se definen en la variable **External_ExcludeVMNames** en la cuenta de Automation en la que está implementada la solución. El ejemplo siguiente, se muestra cómo se puede consultar ese valor con PowerShell.

  ```powershell-interactive
  Get-AzureRmAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

* Para iniciar y detener máquinas virtuales, la cuenta de ejecución de la cuenta de Automation debe tener los permisos apropiados para la máquina virtual. Para obtener información sobre cómo comprobar los permisos para un recurso, vea [Guía de inicio rápido: Visualización de los roles asignados a un usuario mediante Azure Portal](../../role-based-access-control/check-access.md). Deberá proporcionar el identificador de la aplicación para la entidad de servicio utilizada por la cuenta de ejecución. Para obtener este valor, vaya a la cuenta de Automation en Azure Portal, seleccione **Cuentas de ejecución** en **Configuración de cuenta** y haga clic en la cuenta de ejecución apropiada.

* Si la máquina virtual presenta algún problema para iniciarse o desasignarse, este comportamiento puede deberse a algún problema en la propia máquina virtual. Entre algunos ejemplos o posibles problemas destacan la aplicación en curso de alguna actualización al intentar apagar, el bloqueo de un servicio, etc. Vaya al recurso de máquina virtual y consulte los **registros de actividad** para ver si en ellos se ha identificado algún error. También puede intentar iniciar sesión en la máquina virtual para ver si existe algún error en los registros de eventos.

## <a name="custom-runbook"></a>Escenario: Mi runbook personalizado no puede iniciar o detener mis máquinas virtuales

### <a name="issue"></a>Problema

Ha creado un runbook personalizado o ha descargado uno de la Galería de PowerShell, pero no funciona correctamente.

### <a name="cause"></a>Causa

La causa del error podría ser una entre muchas. Vaya a la cuenta de Automation en Azure Portal y seleccione **Trabajos** en **Automatización de procesos**. En la página **Trabajos**, busque los trabajos del runbook para ver cualquier error que pueda haber en ellos.

### <a name="resolution"></a>Resolución

Se recomienda usar la [solución Start/Stop VMs during off-hours](../automation-solution-vm-management.md) para iniciar y detener máquinas virtuales en Azure Automation. Esta solución la creó Microsoft. Los runbooks personalizados no son compatibles con Microsoft. Puede encontrar una solución para su runbook personalizado en el artículo [Solución de problemas relativos a errores con runbooks](runbooks.md). En este artículo se proporcionan instrucciones generales y solución de problemas para runbooks de todos los tipos.

## <a name="dont-start-stop-in-sequence"></a>Escenario: Las máquinas virtuales no se inician o detienen en la secuencia correcta

### <a name="issue"></a>Problema

Las máquinas virtuales configuradas en la solución no se inician o detienen en la secuencia correcta.

### <a name="cause"></a>Causa

Esto se debe a un etiquetado incorrecto de las máquinas virtuales.

### <a name="resolution"></a>Resolución

Realice los pasos siguientes para asegurarse de que la solución está configurada correctamente.

1. Asegúrese de que todas las máquinas virtuales que se van a iniciar o detener tienen una etiqueta `sequencestart` o `sequencestop`, en función de su situación. Estas etiquetas necesitan un número entero positivo como valor. Las máquinas virtuales se procesan en orden ascendente según este valor.
2. Asegúrese de que los grupos de recursos de las máquinas virtuales que se van a iniciar o detener están en las variables `External_Start_ResourceGroupNames` o `External_Stop_ResourceGroupNames`, en función de su situación.
3. Pruebe los cambios mediante la ejecución del runbook `SequencedStartStop_Parent` con el parámetro WHATIF establecido en True para obtener una vista previa de dichos cambios.

Para obtener información detallada e instrucciones adicionales sobre cómo usar la solución para iniciar y detener máquinas virtuales en secuencia, vea [Iniciar o detener las máquinas virtuales en secuencia ](../automation-solution-vm-management.md#scenario-2-startstop-vms-in-sequence-by-using-tags).

## <a name="403"></a>Escenario: El trabajo de inicio y detención de máquinas virtuales genera un estado de error 403 Prohibido 

### <a name="issue"></a>Problema

Puede encontrar trabajos con el error `403 forbidden` para los runbooks de la solución Start/Stop VMs during off-hours.

### <a name="cause"></a>Causa

Este problema puede deberse a una cuenta de ejecución mal configurada o expirada. La causa también puede ser que la cuenta de ejecución de Cuentas de Automation tenga permisos inadecuados para los recursos de máquina virtual.

### <a name="resolution"></a>Resolución

Para comprobar si la cuenta de ejecución está configurada correctamente, vaya a la cuenta de Automation en Azure Portal y seleccione **Cuentas de ejecución** en **Configuración de la cuenta**. Aquí verá el estado de las cuentas de ejecución; si alguna de ellas está configurada incorrectamente o expirada, el estado lo indicará.

Si la cuenta de ejecución está [mal configurada](../manage-runas-account.md#misconfiguration), debe eliminarla y volver a crearla.

Si el certificado de la cuenta de ejecución ha expirado, siga los pasos de [Renovación de certificado autofirmado](../manage-runas-account.md#cert-renewal) para renovarlo.

El problema puede deberse a la falta de permisos. Para obtener información sobre cómo comprobar los permisos para un recurso, vea [Guía de inicio rápido: Visualización de los roles asignados a un usuario mediante Azure Portal](../../role-based-access-control/check-access.md). Deberá proporcionar el identificador de la aplicación para la entidad de servicio utilizada por la cuenta de ejecución. Para obtener este valor, vaya a la cuenta de Automation en Azure Portal, seleccione **Cuentas de ejecución** en **Configuración de cuenta** y haga clic en la cuenta de ejecución apropiada.

## <a name="other"></a>Escenario: Mi problema no se ha indicado anteriormente

### <a name="issue"></a>Problema

Experimenta un problema u obtiene un resultado inesperado al utilizar la solución Start/Stop VMs during off-hours que no se indica en esta página.

### <a name="cause"></a>Causa

Muchas veces los errores pueden deberse a que se usa una versión anterior y obsoleta de la solución.

### <a name="resolution"></a>Resolución

Para resolver muchos errores, se recomienda quitar y actualizar la solución. Para obtener información sobre cómo actualizar la solución, vea [Actualización de la solución](../automation-solution-vm-management.md#update-the-solution).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
