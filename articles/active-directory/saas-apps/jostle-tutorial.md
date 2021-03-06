---
title: 'Tutorial: Integración de Azure Active Directory con Jostle | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Jostle.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9ca4ca1f-8f68-4225-81a6-1666b486d6a8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 214cd2ac20207e32d862086f82f6e3c775d88721
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210448"
---
# <a name="tutorial-azure-active-directory-integration-with-jostle"></a>Tutorial: Integración de Azure Active Directory con Jostle

En este tutorial, obtendrá información sobre cómo integrar Jostle con Azure Active Directory (Azure AD).

Integrar Jostle con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Jostle.
- Puede permitir que los usuarios inicien sesión automáticamente en Jostle (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Jostle, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Jostle

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Jostle desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-jostle-from-the-gallery"></a>Adición de Jostle desde la galería
Para configurar la integración de Jostle en Azure AD, deberá agregar Jostle desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Jostle desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]

1. Haga clic en **Agregar** en la parte superior de la ventana.

    ![add_01](./media/jostle-tutorial/add_01.png)

1. En el cuadro de búsqueda en **Agregar una aplicación**, escriba **Jostle**.

    ![add_02](./media/jostle-tutorial/add_02.png)

1. En el panel de resultados, seleccione **Jostle** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/jostle-tutorial/tutorial_jostle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Jostle con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Jostle para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Jostle.

Para establecer la relación de vínculo, en Jostle, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Jostle, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Jostle](#creating-a-jostle-test-user)**: para tener un homólogo de Britta Simon en Jostle que esté vinculado a la representación de ella en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Jostle.

**Para configurar el inicio de sesión único de Azure AD con Jostle, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Jostle**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/jostle-tutorial/tutorial_jostle_samlbase.png)

1. En la sección **Dominio y direcciones URL de Jostle**, lleve a cabo los pasos siguientes:

    ![url_01](./media/jostle-tutorial/url_01.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba: `https://login-prod.jostle.us`.

    b. En el cuadro de texto **Identificador**, escriba: `https://jostle.us`

    c. Haga clic en el cuadro situado junto a **Mostrar configuración avanzada de URL**.

    d. En el cuadro de texto **URL de respuesta**, escriba: `https://login-prod.jostle.us/saml/SSO/alias/newjostle.us`.

1. En la sección **Atributos de usuario**, para el campo **Identificador de usuario**, escriba: `user.userprincipalname`.

    ![url_02](./media/jostle-tutorial/url_02.png)

1. Haga clic en **Guardar** en la parte superior de la ventana.

1. Vaya a **Certificado de firma de SAML** y compruebe que está establecido en **Activo**. Después, haga clic en **XML de metadatos** para descargar el archivo de metadatos.

    ![url_03](./media/jostle-tutorial/url_03.png)

1. Para configurar el inicio de sesión único en Jostle, necesita enviar el archivo XML de metadatos descargado al [equipo de soporte técnico de Jostle](mailto:support@jostle.me). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
>

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/jostle-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.

    ![Creación de un usuario de prueba de Azure AD](./media/jostle-tutorial/create_aaduser_02.png)

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.

    ![Creación de un usuario de prueba de Azure AD](./media/jostle-tutorial/create_aaduser_03.png)

1. En la página de diálogo **Usuario**, realice los siguientes pasos:

    ![Creación de un usuario de prueba de Azure AD](./media/jostle-tutorial/create_aaduser_04.png)

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="creating-a-jostle-test-user"></a>Creación de un usuario de prueba de Jostle

En esta sección, creará un usuario llamado Britta Simon en Jostle. Si no sabe cómo agregar a Britta Simon en Jostle, póngase en contacto con el [equipo de soporte técnico de Jostle](mailto:support@jostle.me) para agregar al usuario y habilitar el inicio de sesión único.

> [!NOTE]
> El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo y seguirá un vínculo para confirmar su cuenta antes de que se active.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Jostle para que use el inicio de sesión único de Azure.

![Asignar usuario][200]

**Para asignar a Britta Simon a Jostle, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

1. En la lista de aplicaciones, seleccione **Jostle**.

    ![Configurar inicio de sesión único](./media/jostle-tutorial/tutorial_jostle_app.png)

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Jostle en el panel de acceso, debería iniciar sesión automáticamente en su aplicación de Jostle.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/jostle-tutorial/tutorial_general_01.png
[2]: ./media/jostle-tutorial/tutorial_general_02.png
[3]: ./media/jostle-tutorial/tutorial_general_03.png
[4]: ./media/jostle-tutorial/tutorial_general_04.png

[100]: ./media/jostle-tutorial/tutorial_general_100.png

[200]: ./media/jostle-tutorial/tutorial_general_200.png
[201]: ./media/jostle-tutorial/tutorial_general_201.png
[202]: ./media/jostle-tutorial/tutorial_general_202.png
[203]: ./media/jostle-tutorial/tutorial_general_203.png
