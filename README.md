# ![GO](docs/logo-indigo.png) - Custom Policy AD_B2C

---

## Overview

Esta es un repositorio que cuenta con la directiva personalizada para integrarse con un proveedor de identidades externo. En este caso, estamos usando Azure Active Directory B2C.

## Prerequisites

* Suscripción a Azure.
* Rol de Colaborador dentro de la suscripción o un grupo de recursos dentro de la suscripción.

* Visual Studio Code
* [XML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-xml) - Extension vscode
* [XML Tools](https://marketplace.visualstudio.com/items?itemName=DotJoshJohnson.xml) - Extension vscode
* [Azure Policy](https://marketplace.visualstudio.com/items?itemName=AzurePolicy.azurepolicyextension) - Extension vscode
* [Azure AD B2C](https://marketplace.visualstudio.com/items?itemName=AzureADB2CTools.aadb2c) - Extension vscode

### Notas

* Se pueden crear hasta 20 inquilinos por suscripción.
* De manera predeterminada, cada inquilino puede alojar un total de 1,25 millones de objetos (cuentas de usuario y aplicaciones), pero puede aumentar este límite a 5,25 millones de objetos al agregar y comprobar un dominio personalizado.

## Setup

### Step 1: Clonar o descargar este repositorio

From your shell or command line:

`git clone https://Indira-HealthTech@dev.azure.com/Indira-HealthTech/ADB2C-Policy/_git/ADB2C-Policy`

### Step 2: Creación de un inquilino de Azure AD B2C

1. Inicie sesión en[Azure portal](https://portal.azure.com).
2. Asegúrese de que usa el inquilino de Azure Active Directory (Azure AD) que contiene su suscripción:
   1. Seleccione el icono Directorios y suscripciones en la barra de herramientas de Azure Portal.
   2. En la página Configuración del portal | Directorios y suscripciones, busque el directorio de Azure AD que contiene su suscripción en la lista Nombre de directorio y, después, seleccione el botón Cambiar junto a ella.
3. Agregue Microsoft.AzureActiveDirectory como proveedor de recursos para la suscripción de Azure que utiliza
   1. En Azure Portal, busque y seleccione Suscripciones.
   2. Seleccione la suscripción y luego Proveedores de recursos en el menú izquierdo. Si no ve el menú izquierdo, seleccione el icono Mostrar el menú para "nombre de la suscripción" en la parte superior izquierda de la página para expandirla.
   3. Asegúrese de que la fila Microsoft.AzureActiveDirectory muestre el estado Registrado. Si no es así, seleccione la fila y, a continuación, seleccione Registrar.
4. En el menú de Azure Portal o en la página principal, seleccione Crear un recurso.
5. Busque Azure Active Directory B2C y, a continuación, seleccione Crear.
6. Seleccione Crear un nuevo inquilino de Azure AD B2C.
7. Diligencie la informacion necesaria y Seleccione Revisar + crear.

Nota: Cuando se crea un directorio de Azure AD B2C, se crea en él automáticamente una aplicación denominada b2c-extensions-app. No la modifique ni la elimine. Azure AD B2C usa esta aplicación para almacenar datos de usuario. Obtenga más información sobre la aplicación de extensiones de Azure AD B2C.

## Step 3: Registro de una aplicación web en Azure Active Directory B2C

1. Inicie sesión en[Azure portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el icono Directorios y suscripciones en la barra de herramientas del portal.
3. En la página Configuración del portal | Directorios y suscripciones, busque el directorio de Azure AD B2C en la lista Nombre de directorio y seleccione Cambiar.
4. En Azure Portal, busque y seleccione Azure AD B2C.
5. Seleccione Registros de aplicaciones y luego Nuevo registro.
6. Escriba un Nombre para la aplicación.
7. En Tipos de cuenta compatibles, seleccione Cuentas en cualquier proveedor de identidades o directorio de la organización (para autenticar usuarios con flujos de usuario) .
8. En URI de redirección, seleccione Web y escriba [https://jwt.ms](https://jwt.ms) en el cuadro de texto. (este con fin de realizar pruebas)
9. En Permisos, active la casilla Conceda consentimiento del administrador a los permisos _openid_ y _offline_access_.
10. Seleccione Registrar.

## Step 4: Creación de un secreto de cliente en la aplicacion

1. En la página Azure AD B2C: Registros de aplicaciones, seleccione la aplicación que ha creado.
2. En el menú de la izquierda, en Administrar, seleccione Certificados y secretos.
3. Seleccione Nuevo secreto de cliente.
4. Escriba una descripción para el secreto de cliente en el cuadro Descripción.
5. En Expira, seleccione el tiempo durante el cual el secreto es válido y, a continuación, seleccione Agregar.
6. Registre el valor del secreto para usarlo en el código de la aplicación cliente. Este valor secreto no se volverá a mostrar una vez que abandone esta página. Use este valor como secreto de aplicación en el código de la aplicación.

### Habilitación de la concesión implícita de tokens de identificador

1. En el menú izquierdo, en Administrar, seleccione Autenticación.
2. En Flujos híbridos y de concesión implícita, seleccione las casillas Tokens de acceso (para flujos implícitos) y Tokens de id. (para flujos híbridos e implícitos).
3. Seleccione Guardar.

## Step 5: Creación de una política de inicio de sesión personalizada

### 5.1 Agregar claves de firma y cifrado para aplicaciones de Identity Experience Framework

1. Inicie sesión en Azure Portal.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el icono Directorios y suscripciones en la barra de herramientas del portal.
3. En la página Configuración del portal | Directorios y suscripciones, busque el directorio de Azure AD B2C en la lista Nombre de directorio y seleccione Cambiar.
4. En Azure Portal, busque y seleccione Azure AD B2C.
5. En la página de información general, en Directivas, seleccione Identity Experience Framework.

#### 5.1.1 Crear la clave de firma

1. Seleccione Claves de directiva y luego Agregar.
2. En Opciones, elija Generate.
3. En Nombre, escriba TokenSigningKeyContainer. Es posible que se agregue automáticamente el prefijo B2C_1A_.
4. En Tipo de clave, seleccione RSA.
5. En Uso de claves, seleccione Firma.
6. Seleccione Crear.

#### 5.1.2 Crear la clave de cifrado

1. Seleccione Claves de directiva y luego Agregar.
2. En Opciones, elija Generate.
3. En Nombre, escriba TokenEncryptionKeyContainer. Es posible que se agregue automáticamente el prefijo B2C_1A_.
4. En Tipo de clave, seleccione RSA.
5. En Uso de la clave, seleccione Cifrado.
6. Seleccione Crear.

### 5.2 egistro de las aplicaciones del marco de experiencia de identidad

Azure AD B2C requiere que registre dos aplicaciones que se usen para registrar usuarios e iniciar su sesión con cuentas locales: IdentityExperienceFramework (una API web) y ProxyIdentityExperienceFramework (una aplicación nativa) con permiso delegado de la aplicación IdentityExperienceFramework. Los usuarios pueden registrarse con una dirección de correo electrónico o un nombre de usuario y una contraseña para acceder a las aplicaciones registradas en el inquilino, lo que crea una "cuenta local". Las cuentas locales existen solo en el inquilino de Azure AD B2C.

_Nota: Solo tiene que registrar estas dos aplicaciones en el inquilino de Azure AD B2C una vez._

#### 5.2.1 Registro de la aplicación IdentityExperienceFramework

Para registrar una aplicación en el inquilino de Azure AD B2C, puede usar la experiencia Registros de aplicaciones.

1. Seleccione Registros de aplicaciones y luego Nuevo registro.
2. En Nombre, escriba _"IdentityExperienceFramework"_.
3. En Tipos de cuenta admitidos, seleccione Solo las cuentas de este directorio organizativo.
4. En URI de redirección, seleccione Web y, a continuación, escriba _"https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft"_ com, donde _"your-tenant-name"_ es el nombre de dominio del inquilino de Azure AD B2C.
5. En Permisos, active la casilla Conceda consentimiento del administrador a los permisos _"openid"_ y _"offline_access"_.
6. Seleccione Registrar.
7. Anote el Id. de aplicación (cliente) para usarlo en un paso posterior.

##### A continuación, exponga la API agregando un ámbito

1. En el menú de la izquierda, en Administrar, seleccione Exponer una API.
2. Seleccione agregar un ámbito y, a continuación, Guardar y continuar para aceptar el URI de identificador de aplicación predeterminado.
3. Escriba los valores siguientes para crear un ámbito que permita la ejecución de la directiva personalizada en el inquilino de Azure AD B2C:
   1. Nombre de ámbito: _"user_impersonation"_
   2. Nombre para mostrar del consentimiento del administrador: _"Access IdentityExperienceFramework"_
   3. Descripción del consentimiento del administrador: _"Allow the application to access IdentityExperienceFramework on behalf of the signed-in user."_
4. Seleccione Agregar ámbito.

#### 5.2.2 Registro de la aplicación ProxyIdentityExperienceFramework

1. Seleccione Registros de aplicaciones y luego Nuevo registro.
2. En Nombre, escriba _"ProxyIdentityExperienceFramework"_.
3. En Tipos de cuenta admitidos, seleccione Solo las cuentas de este directorio organizativo.
4. En URI de redirección, use la lista desplegable para seleccionar Cliente público o nativo (móvil & escritorio).
5. En URI de redirección, escriba myapp://auth.
6. En Permisos, active la casilla Conceda consentimiento del administrador a los permisos _"openid"_ y _"offline_access"_.
7. Seleccione Registrar.
8. Anote el Id. de aplicación (cliente) para usarlo en un paso posterior.

##### A continuación, especifique que la aplicación se debe tratar como un cliente público

1. En el menú izquierdo, en Administrar, seleccione Autenticación.
2. En Configuración avanzada, en la sección Permitir flujos de clientes públicos, establezca Habilite los siguientes flujos móviles y de escritorio en Sí.
3. Seleccione Guardar.
4. Asegúrese de que "allowPublicClient": true esté establecido en el manifiesto de aplicación:
   1. En el menú izquierdo, en Administrar, seleccione Manifiesto para abrir el manifiesto de aplicación.
   2. Busque la clave allowPublicClient y asegúrese de que su valor esté establecido en true.

##### Ahora, conceda permisos al ámbito de la API que expuso anteriormente en el registro de IdentityExperienceFramework

1. En el menú de la izquierda, en Administrar, seleccione Permisos de API.
2. En Permisos configurados, seleccione Agregar un permiso.
3. Seleccione la pestaña Mis API y, después, seleccione la aplicación IdentityExperienceFramework.
4. En Permiso, seleccione el ámbito user_impersonation que definió anteriormente.
5. Seleccione Agregar permisos. Como se indicó, espere unos minutos antes de continuar con el paso siguiente.
6. Seleccione Conceder consentimiento de administrador para "el nombre del inquilino".
7. Seleccione Sí.
8. Seleccione Actualizar y compruebe que aparece "Granted for..." (Concedido para...) en Estado para el ámbito.

#### 5.3 Paquete de inicio de directivas personalizadas

Las directivas personalizadas son un conjunto de archivos XML que se cargan en el inquilino de Azure AD B2C para definir perfiles técnicos y recorridos de usuario. Proporcionamos paquetes de inicio con varias directivas predefinidas para que pueda empezar a trabajar rápidamente. Cada uno de estos paquetes de inicio contiene el menor número de perfiles técnicos y recorridos del usuario necesarios para lograr los escenarios descritos:

* LocalAccounts: habilita el uso solo de cuentas locales.
* SocialAccounts: habilita el uso solo de cuentas sociales (o federadas).
* SocialAndLocalAccounts: habilita el uso de cuentas locales y sociales.
* SocialAndLocalAccountsWithMFA: habilita opciones sociales, locales y de autenticación multifactor.

Cada paquete de inicio contiene lo siguiente:

* Archivo base: se requieren algunas modificaciones en el archivo base. Ejemplo: TrustFrameworkBase.xml
* Archivo de localización: este archivo es donde se realizan los cambios de localización. Por ejemplo: TrustFrameworkLocalization.xml
* Archivo de extensión: este archivo es donde se hace la mayoría de los cambios de configuración. Ejemplo: TrustFrameworkExtensions.xml
* Archivos de usuario de confianza: archivos específicos de la tarea a los que llama la aplicación. Ejemplos: SignUpOrSignin.xml, ProfileEdit.xml, PasswordReset.xml.

todos estos archivos se encuentran en la carpeta _"templates"_, pero tambien existe la posibilidad de descargarlos directamente del repositorio de github de microsoft

``` GIT
git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
```

##### Configurar los archivos base

1. En todos los archivos del directorio SocialAndLocalAccounts o SocialAndLocalAccountsWithMFA, reemplace la cadena _yourtenant_ por el nombre de su inquilino de Azure AD B2C. el nombre de la politica si utiliza otro nombre, y el id del Tenant.

<details open >
  <summary> TrustFrameworkBase.xml </summary>

  ``` XML
  <TrustFrameworkPolicy
    ...
    TenantId="yourtenant.onmicrosoft.com"
    PolicyId="B2C_1A_signup_signin"
    PublicPolicyUri="http://yourtenant.onmicrosoft.com/B2C_1A_signup_signin"
    TenantObjectId="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ...
    >

    <BasePolicy>
      <TenantId>yourtenant.onmicrosoft.com</TenantId>
      ...
    </BasePolicy>

    ...
  ```

</details>

2. Abra el archivo TrustFrameworkExtensions.xml y busque el elemento _"TechnicalProfile Id="login-NonInteractive"_
3. Reemplace ambas instancias de _IdentityExperienceFrameworkAppId_ por el identificador de la aplicación _IdentityExperienceFramework_ que creó antes.
4. Reemplace ambas instancias de _ProxyIdentityExperienceFrameworkAppId_ por el identificador de la aplicación _ProxyIdentityExperienceFramework_ que creó antes.

<details open>

  <summary> TrustFrameworkBase.xml </summary>

``` XML
...

<ClaimsProvider>
  <DisplayName>Local Account SignIn</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="login-NonInteractive">
      <Metadata>
        <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
        <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppId" />
        <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppId" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>

...
```

</details>

5. Guarde el archivo.

#### 5.3.1 Cargar las directivas en Azure

1. Seleccione el elemento de menú Identity Experience Framework en el inquilino de B2C en Azure Portal.
2. Seleccione Cargar directiva personalizada.
3. En este orden, cargue los archivos de directiva:
   1. TrustFrameworkBase.xml
   2. TrustFrameworkLocalization.xml
   3. TrustFrameworkExtensions.xml
   4. SignUpOrSignin.xml
   5. ProfileEdit.xml
   6. PasswordReset.xml

_"Nota: A medida que cargue los archivos, Azure agregará el prefijo B2C_1A_ a cada uno"_.

#### 6 Prueba de la directiva personalizada

1. En Directivas personalizadas, seleccione B2C_1A_signup_signin.
2. En Seleccionar aplicación en la página de información general de la directiva personalizada, seleccione la aplicación web que quiere probar, como la denominada webapp1.
3. Asegúrese de que la URL de respuesta sea _"https://jwt.ms"_.
4. Seleccione Ejecutar ahora.
5. Regístrese con una dirección de correo electrónico.
6. Vuelva a seleccionar Ejecutar ahora.
7. Inicie sesión con la misma cuenta para confirmar que tiene una configuración correcta.

---

---

---

---

## Contribute

If you would like to contribute to this sample, see [CONTRIBUTING.MD](CONTRIBUTING.md).

## Questions and comments

We'd love to get your feedback about this sample. You can send your questions and suggestions to us in the Issues section of this repository.

Questions about Azure Active Directory B2C should be posted to [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-ad-b2c). Make sure that your questions or comments are tagged with [azure-ad-b2c].

## Additional resources

* [Azure Active Directory B2C](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview)
