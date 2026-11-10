# Administración de identidades y acceso con Microsoft Entra ID

# Objetivos

En este laboratorio aprenderás a administrar usuarios, grupos, roles, licencias, identidades externas y configuraciones del tenant utilizando Microsoft Entra ID.

* Crear y administrar usuarios de Microsoft Entra ID.
* Asignar y eliminar roles administrativos.
* Crear usuarios individualmente y mediante operaciones masivas.
* Administrar usuarios mediante Microsoft Graph PowerShell.
* Eliminar y restaurar usuarios.
* Asignar licencias a usuarios y grupos.
* Crear grupos de seguridad y grupos de Microsoft 365.
* Crear grupos dinámicos mediante reglas de pertenencia.
* Configurar propiedades del tenant.
* Configurar información de privacidad.
* Configurar colaboración externa.
* Invitar usuarios externos individualmente y de forma masiva.
* Configurar Google como proveedor de identidad externo.
* Comprender el proceso de implementación de identidades híbridas mediante Microsoft Entra Connect.

# Requisitos

* Acceso a un tenant de Microsoft 365 E5 proporcionado para el laboratorio.
* Cuenta con permisos de Global Administrator.
* Microsoft Edge.
* Microsoft Authenticator instalado en un dispositivo móvil.
* PowerShell 7.2 o superior para los ejercicios de PowerShell.
* Acceso a Internet.
* Para el ejercicio de federación con Google:
  * Una cuenta de Google.
* Para el laboratorio opcional de identidad híbrida:
  * Acceso a una suscripción de Azure.
  * Permisos para crear recursos.
  * Aproximadamente 120 minutos adicionales.

# Pasos

## Crear un nuevo usuario

* [BROWSER] `https://entra.microsoft.com`
  * Iniciar sesión con la cuenta de administrador del laboratorio.
  * Si se solicita MFA, completar la configuración utilizando Microsoft Authenticator.

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Entra ID
  * [BUTTON] Users
  * [BUTTON] All users
  * [BUTTON] + New user
  * [BUTTON] Create new user

* Completar los siguientes valores:
  * `User principal name`: `ChrisG`
  * `Display name`: `Chris Green`
  * `Password`: mantener `Auto-generate password`

* Copiar la contraseña temporal generada.
  * Guardarla temporalmente porque será necesaria para el primer inicio de sesión.

* [BUTTON] Review + Create
* [BUTTON] Create

* Verificar que `Chris Green` aparezca en `Users > All users`.

---

## Comprobar los permisos iniciales de Chris Green

* [BROWSER] Microsoft Edge
  * Abrir una ventana InPrivate.

* [BROWSER] `https://entra.microsoft.com`
  * Iniciar sesión como:

    * Usuario: `ChrisG@<dominio-del-tenant>`
    * Contraseña: utilizar la contraseña generada anteriormente.

* Cambiar la contraseña cuando se solicite.

* Si se solicita MFA:
  * Configurar Microsoft Authenticator.
  * Escanear el código QR.
  * Aprobar la solicitud de autenticación.

* [BROWSER] Microsoft Entra admin center
  * Buscar `Enterprise applications`.
  * [BUTTON] Enterprise applications
  * [BUTTON] + New application

* Verificar que:
  * `+ Create your own application` no esté disponible.

* Intentar acceder a:
  * `Consent and permissions`
  * `User settings`

* Comprobar que Chris Green todavía no posee permisos administrativos suficientes.

* Cerrar sesión.

---

## Asignar el rol Application Administrator

* [BROWSER] `https://entra.microsoft.com`
  * Iniciar sesión nuevamente con la cuenta de administrador.

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Entra ID
  * [BUTTON] Users
  * [BUTTON] All users
  * Seleccionar `Chris Green`.
  * [BUTTON] Assigned roles
  * [BUTTON] + Add assignments

* Seleccionar:
  * `Application Administrator`

* [BUTTON] Next

* Configurar:
  * `Assignment type`: `Active`
  * `Justification`: `Required for managing app permissions and SSO configurations`

* Completar la asignación.

* [BUTTON] Refresh

* Verificar que el rol `Application Administrator` aparezca en los roles asignados a Chris Green.

> Si Microsoft Entra Privileged Identity Management está habilitado en el tenant, el proceso de asignación puede mostrar opciones adicionales relacionadas con la activación y duración del rol.

---

## Comprobar los nuevos permisos de Chris Green

* [BROWSER] Microsoft Edge
  * Abrir una nueva ventana InPrivate.

* [BROWSER] `https://entra.microsoft.com`
  * Iniciar sesión como `Chris Green`.

* [BROWSER] Microsoft Entra admin center
  * Buscar `Enterprise applications`.
  * [BUTTON] Enterprise applications

* Verificar que ahora esté habilitada la opción:

  * [BUTTON] + New Application

* [BUTTON] + New Application

* Verificar que:

  * [BUTTON] + Create your own application

  ya no esté deshabilitado.

* Seleccionar:

  * [BUTTON] + Create your own application

* Verificar que el botón `Create` esté disponible.

* No es necesario crear la aplicación.

* Cerrar sesión.

---

## Eliminar el rol Application Administrator

* [BROWSER] `https://entra.microsoft.com`
  * Iniciar sesión como administrador.

* Buscar:
  * `Roles`

* [BUTTON] Microsoft Entra roles and administrators

* [BROWSER] Roles and administrators
  * Buscar `Application Administrator`.
  * Seleccionar el rol.

* En la lista de asignaciones:
  * Localizar `Chris Green`.
  * Seleccionarlo.

* [BUTTON] Remove
* [BUTTON] Yes

* Verificar que Chris Green ya no aparezca entre las asignaciones del rol.

---

# Administración masiva de usuarios

## Crear usuarios mediante un archivo CSV

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Entra ID
  * [BUTTON] Users
  * [BUTTON] All users
  * [BUTTON] Bulk operations
  * [BUTTON] Bulk create

* En `Bulk create users`:
  * [BUTTON] Download

* El laboratorio proporciona además el archivo:

  `Allfiles/Labs/Lab1/SC300BulkUser.csv`

* [FILE] `SC300BulkUser.csv`
  * Abrir utilizando Notepad.
  * No editar el archivo con Excel.
  * Reemplazar el dominio de ejemplo con el dominio real del tenant.
  * Guardar el archivo.

* [BROWSER] Bulk create users
  * Seleccionar el icono para cargar un archivo.
  * Seleccionar `SC300BulkUser.csv`.
  * [BUTTON] Open

* Esperar la validación.

* Cuando aparezca que el archivo se cargó correctamente:
  * [BUTTON] Submit

* Volver a:
  * `Entra ID > Users > All users`

* Verificar que los nuevos usuarios aparezcan en la lista.

---

## Administrar usuarios mediante Microsoft Graph PowerShell

* [CONSOLE] Abrir PowerShell 7 como administrador.

* Verificar que la versión sea 7.2 o superior.

* [CONSOLE] Instalar Microsoft Graph PowerShell si todavía no está instalado:

```powershell
Install-Module Microsoft.Graph -Scope CurrentUser -Verbose
```

* Si PowerShell solicita confirmación:
  * Ingresar `Y`.

* [CONSOLE] Verificar la instalación:

```powershell
Get-InstalledModule Microsoft.Graph
```

* [CONSOLE] Conectarse a Microsoft Graph:

```powershell
Connect-MgGraph -Scopes "User.ReadWrite.All"
```

* Iniciar sesión utilizando la cuenta administrativa proporcionada para el laboratorio.

* Aceptar los permisos solicitados.

* [CONSOLE] Mostrar los usuarios existentes:

```powershell
Get-MgUser
```

### Crear el perfil de contraseña

* [CONSOLE]

```powershell
$PWProfile = @{
    Password = '<contraseña-segura>'
    ForceChangePasswordNextSignIn = $false
}
```

* Reemplazar `<contraseña-segura>` por una contraseña válida para el laboratorio.

### Crear un usuario

* [CONSOLE]

```powershell
New-MgUser `
    -DisplayName "New PW User" `
    -GivenName "New" `
    -Surname "User" `
    -MailNickname "newuser" `
    -UsageLocation "US" `
    -UserPrincipalName "newuser@<tenant>.onmicrosoft.com" `
    -PasswordProfile $PWProfile `
    -AccountEnabled `
    -Department "Research" `
    -JobTitle "Trainer"
```

* Reemplazar `<tenant>.onmicrosoft.com` por el dominio real del tenant.

* [CONSOLE]

```powershell
Get-MgUser
```

* Confirmar que el usuario aparezca en Microsoft Entra ID.

---

# Eliminar y restaurar usuarios

## Eliminar a Chris Green

* [BROWSER] `https://entra.microsoft.com`

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Entra ID
  * [BUTTON] Users
  * [BUTTON] All users

* Seleccionar la casilla correspondiente a:
  * `Chris Green`

* [BUTTON] Delete
* [BUTTON] Yes

---

## Restaurar a Chris Green

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Users
  * [BUTTON] Deleted users

* Seleccionar:
  * `Chris Green`

* [BUTTON] Restore users
* [BUTTON] OK

* Volver a:
  * `Users > All users`

* Verificar que Chris Green aparezca nuevamente.

> Los usuarios eliminados permanecen temporalmente disponibles para restauración antes de su eliminación permanente.

---

# Administración de licencias

## Verificar que Raul Razo no tenga licencia

* [BROWSER] `https://entra.microsoft.com`

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Entra ID
  * [BUTTON] Users
  * [BUTTON] All users

* Buscar:
  * `Raul`

* Seleccionar:
  * `Raul Razo`

* Revisar que tenga configurado `Usage location`.

* Si no está configurado:
  * [BUTTON] Edit properties
  * [BUTTON] Settings
  * `Usage Location`: `United States`
  * [BUTTON] Save

* [BUTTON] Licenses

* Verificar que inicialmente aparezca:

  `No license assignments found.`

---

## Asignar Microsoft 365 E5 a Raul Razo

* [BROWSER] `https://admin.microsoft.com`

* [BROWSER] Microsoft 365 admin center
  * [BUTTON] Billing
  * [BUTTON] Licenses
  * Seleccionar `Microsoft 365 E5 (no Teams)`.

* Si no quedan licencias disponibles:
  * Buscar `Irvin Sayers`.
  * [BUTTON] More options
  * [BUTTON] Unassign license

* [BUTTON] + Assign licenses

* Buscar:
  * `Raul Razo`

* Seleccionar al usuario.

* [BUTTON] Assign licenses

* Volver a Microsoft Entra admin center.

* Navegar a:
  * `Entra ID > Users > All users > Raul Razo > Licenses`

* Verificar que la licencia aparezca asignada.

---

# Configuración del tenant

## Crear un subdominio

* [BROWSER] `https://entra.microsoft.com`

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Entra ID
  * [BUTTON] Domain names
  * [BUTTON] + Add custom domain

* Utilizar el formato:

  `sales.<tenant>.onmicrosoft.com`

* Si aparece el mensaje indicando que los dominios `onmicrosoft.com` deben administrarse desde Microsoft 365:
  * Abrir Microsoft 365 Admin Center.

* [BROWSER] Microsoft 365 admin center
  * Navegar a `Domains`.
  * [BUTTON] Add domain

* Ingresar:

  `sales.<tenant>.onmicrosoft.com`

* [BUTTON] Use this domain
* [BUTTON] Close

* No configurar DNS adicional para este laboratorio.

---

## Cambiar el nombre del tenant

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Entra ID
  * [BUTTON] Overview
  * [BUTTON] Properties

* Modificar:
  * `Name`: `Contoso Marketing`
  * `Technical contact`: utilizar la cuenta Global Administrator.

* [BUTTON] Save

* Actualizar el navegador si el nombre no cambia inmediatamente.

---

## Revisar el país o región

* Navegar a:

  `Entra ID > Overview > Properties`

* Buscar:
  * `Country or region`

* Revisar el valor configurado.

> El país o región se establece durante la creación del tenant y no puede modificarse posteriormente.

---

## Encontrar el Tenant ID

* Navegar a:

  `Entra ID > Overview > Properties`

* Localizar:
  * `Tenant ID`

* Copiar el identificador.

* [FILE] Notepad
  * Guardar temporalmente el Tenant ID porque será utilizado posteriormente.

---

# Configurar información de privacidad

## Agregar contacto y declaración de privacidad

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Entra ID
  * [BUTTON] Overview
  * [BUTTON] Properties

* Configurar:

  * `Global privacy contact`: utilizar la cuenta de Allan Deyoung proporcionada en el tenant.
  * `Privacy statement URL`:

  `https://github.com/MicrosoftLearning/SC-300-Identity-and-Access-Administrator/blob/master/Allfiles/Labs/Lab2/SC-300-Lab_ContosoPrivacySample.pdf`

* [BUTTON] Save

---

## Comprobar la declaración de privacidad

* [BROWSER] Microsoft Entra admin center
  * Seleccionar el usuario en la esquina superior derecha.
  * [BUTTON] View account

* En la nueva pestaña:
  * [BUTTON] My Account
  * [BUTTON] Settings & Privacy
  * [BUTTON] Privacy

* En `Organization's notice`:
  * Buscar `Contoso Marketing organizational privacy statement`.
  * [BUTTON] View

* Revisar el documento de privacidad.

---

# Asignación de licencias mediante grupos

## Comprobar el acceso de Delia Dennis

* [BROWSER] Microsoft Edge
  * Abrir una ventana InPrivate.

* [BROWSER] `https://www.office.com`
  * Iniciar sesión como `Delia Dennis`.

* Utilizar:
  * Usuario: `DeliaD@<dominio-del-tenant>`
  * Contraseña: utilizar la contraseña proporcionada por el entorno del laboratorio.

* Abrir el App Launcher.

* Intentar abrir:
  * Outlook
  * Word
  * Excel

* Verificar que Delia todavía no posea una licencia válida para utilizar las aplicaciones.

* Cerrar la ventana InPrivate.

---

## Crear un grupo de seguridad

* [BROWSER] `https://entra.microsoft.com`

* Navegar a:
  * `Entra ID > Groups > All groups`

* [BUTTON] New group

* Configurar:
  * `Group type`: `Security`
  * `Group name`: `sg-SC300-O365`
  * `Membership type`: `Assigned`
  * `Owners`: seleccionar la cuenta administrativa utilizada para el laboratorio.

* En `Members`:
  * [BUTTON] No members selected
  * Seleccionar `Delia Dennis`.
  * [BUTTON] Select

* [BUTTON] Create

* Verificar que aparezca:
  * `sg-SC300-O365`

---

## Asignar una licencia al grupo

* [BROWSER] `https://admin.microsoft.com`

* Navegar a:
  * `Billing > Licenses`

* Seleccionar:
  * `Microsoft 365 E5 (No Teams)`

* Si no quedan licencias:
  * Buscar `Lidia Holloway`.
  * Liberar temporalmente su licencia.

* [BUTTON] + Assign licenses

* Buscar:
  * `sg-SC300-O365`

* Seleccionar el grupo.

* [BUTTON] Assign licenses

* Volver a Microsoft Entra admin center.

* Navegar a:

  `Entra ID > Groups > All groups > sg-SC300-O365 > Licenses`

* Verificar que Microsoft 365 E5 aparezca asociado al grupo.

---

## Comprobar la licencia de Delia

* [BROWSER] Microsoft Edge
  * Abrir una ventana InPrivate.

* [BROWSER] `https://www.office.com`
  * Iniciar sesión como `Delia Dennis`.

* Abrir el App Launcher.

* Verificar que aparezcan las aplicaciones correspondientes, por ejemplo:
  * Outlook
  * Word
  * Excel
  * PowerPoint
  * OneNote
  * OneDrive
  * SharePoint

* Comprobar que ya no aparezca el mensaje indicando que Delia no posee licencia.

---

# Crear un grupo de Microsoft 365

* [BROWSER] `https://entra.microsoft.com`

* Navegar a:
  * `Entra ID > Groups > All groups`

* [BUTTON] New group

* Configurar:
  * `Group type`: `Microsoft 365`
  * `Group name`: `Northwest Sales`
  * `Membership type`: `Assigned`
  * `Owner`: cuenta administrativa del laboratorio.

* Agregar como miembros:
  * `Alex Wilber`
  * `Bianca Pisani`

* [BUTTON] Create

* Verificar que `Northwest Sales` aparezca en `All groups`.

---

# Crear un grupo dinámico

## Crear SC300-myDynamicGroup

* [BROWSER] `https://entra.microsoft.com`

* Navegar a:
  * `Entra ID > Groups > All groups`

* [BUTTON] New group

* Configurar:
  * `Group type`: `Security`
  * `Group name`: `SC300-myDynamicGroup`
  * `Membership type`: `Dynamic User`
  * `Owner`: seleccionar un propietario.

* En `Dynamic user members`:
  * [BUTTON] Add dynamic query
  * [BUTTON] Edit

* Utilizar la regla:

```text
user.objectId -ne null
```

* [BUTTON] OK
* [BUTTON] Save
* [BUTTON] Create

> La evaluación de pertenencia del grupo dinámico puede tardar varios minutos.

---

## Verificar los miembros dinámicos

* Navegar a:

  `Entra ID > Groups > All groups`

* Buscar:
  * `SC300`

* Abrir:
  * `SC300-myDynamicGroup`

* [BUTTON] Members

* Verificar que los usuarios hayan sido agregados automáticamente.

---

## Probar reglas alternativas

### Incluir únicamente invitados

```text
(user.objectId -ne null) and (user.userType -eq "Guest")
```

### Incluir únicamente miembros

```text
(user.objectId -ne null) and (user.userType -eq "Member")
```

* Recordar que los nombres de propiedades son sensibles a mayúsculas y minúsculas.

---

# Configurar colaboración externa

## Habilitar self-service sign-up para invitados

* [BROWSER] `https://entra.microsoft.com`

* Navegar a:
  * `Entra ID > Users > All users > User Settings`

* [BUTTON] Manage external collaboration settings

* Configurar:
  * `Enable guest self-service sign up via user flows`: `Yes`

* [BUTTON] Save

---

## Configurar Email One-Time Passcode

* Navegar a:
  * `Entra ID > External Identities > All identity providers`

* Seleccionar:
  * `Email one-time passcode`

* Verificar que esté configurado como:
  * `Yes`

* [BUTTON] Save, si se realizó algún cambio.

---

## Restringir el acceso de usuarios invitados

* Navegar a:
  * `External Identities > External Collaboration Settings`

* En `Guest user access`, seleccionar:

  `Guest user access is restricted to properties and memberships of their own directory objects (most restrictive)`

* En `Guest invite settings`, seleccionar:

  `Member users and users assigned to specific admin roles can invite guest users including guests with member permissions`

* En `Collaboration restrictions`:
  * Mantener la configuración predeterminada.

* [BUTTON] Save

---

# Invitar usuarios externos

## Invitar un usuario individual

* [BROWSER] `https://entra.microsoft.com`

* Navegar a:
  * `Entra ID > Users > All users`

* [BUTTON] + New user
* [BUTTON] Invite external user

* Ingresar una dirección de correo externa válida.

* Ejemplo utilizado por el laboratorio:

  `sc300externaluser1@sc300email.com`

* [BUTTON] Properties

* Verificar:
  * `User type`: `Guest`

* [BUTTON] Review + Invite
* [BUTTON] Invite

* Verificar que el usuario aparezca automáticamente en Microsoft Entra ID como invitado.

---

# Invitar usuarios externos en forma masiva

* [BROWSER] Microsoft Entra admin center
  * `Entra ID > Users > All users`

* [BUTTON] Bulk operations
* [BUTTON] Bulk invite

* [BUTTON] Download

* [FILE] Plantilla CSV
  * Abrir con Notepad.
  * Agregar una línea para cada invitado.

* Los valores principales son:
  * `Email address to invite`
  * `Redirection url`

* Ejemplo conceptual:

```text
testing@test.com,https://myapplications.microsoft.com/,,
```

* Guardar el archivo.

* [BROWSER] Bulk invite users
  * Cargar el archivo CSV.

* Esperar hasta que aparezca:
  * `File uploaded successfully`

* [BUTTON] Submit

* Revisar:
  * `# Success`
  * `# Failure`
  * `Total Requests`

* Corregir el CSV si aparecen errores.

---

# Invitar un usuario mediante Microsoft Graph PowerShell

* [CONSOLE] Abrir PowerShell 7 como administrador.

* [CONSOLE]

```powershell
Install-Module Microsoft.Graph -Scope CurrentUser -Verbose
```

* [CONSOLE]

```powershell
Get-InstalledModule Microsoft.Graph
```

* [CONSOLE]

```powershell
Connect-MgGraph -Scopes "User.ReadWrite.All"
```

* Iniciar sesión con la cuenta administrativa del laboratorio.

* [CONSOLE]

```powershell
Import-Module Microsoft.Graph.Identity.SignIns

$params = @{
    invitedUserEmailAddress = "admin@fabrikam.com"
    inviteRedirectUrl = "https://myapp.contoso.com"
}
```

* [CONSOLE]

```powershell
New-MgInvitation -BodyParameter $params
```

* Volver a:
  * `Entra ID > Users > All users`

* Comprobar la aparición del usuario externo.

---

# Configurar Google como proveedor de identidad

## Crear el proyecto en Google

* [BROWSER] `https://console.developers.google.com`

* Iniciar sesión con una cuenta de Google.

* [BUTTON] New Project

* Configurar:
  * `Project name`: `MyB2BApp`

* [BUTTON] Create

* Abrir el proyecto.

* Navegar a:
  * `APIs & Services`
  * `OAuth consent screen`

* [BUTTON] Get Started

* Configurar:

  * `App name`: `Microsoft Entra ID`
  * `User support email`: seleccionar la cuenta de Google.
  * `Audience`: `External`
  * `Contact Information`: utilizar la misma dirección de correo.
  * Aceptar el acuerdo solicitado.

* [BUTTON] Create

---

## Crear el cliente OAuth

* [BUTTON] Create OAuth client

* Configurar:
  * `Application type`: `Web Application`

* En `Authorized JavaScript origins` agregar:

  `https://microsoftonline.com`

* En `Authorized redirect URIs` agregar:

```text
https://login.microsoftonline.com
```

```text
https://login.microsoftonline.com/te/<tenant-ID>/oauth2/authresp
```

```text
https://login.microsoftonline.com/te/<tenant-name>.onmicrosoft.com/oauth2/authresp
```

* Reemplazar:
  * `<tenant-ID>` por el Tenant ID obtenido anteriormente.
  * `<tenant-name>` por el nombre real del tenant.

* [BUTTON] Create

* Copiar:
  * `Client ID`
  * `Client Secret`

* [FILE] Notepad
  * Guardar temporalmente ambos valores.

---

## Agregar el usuario de prueba en Google

* [BROWSER] Google Cloud
  * [BUTTON] Audience

* En `Test Users`:
  * [BUTTON] + Add Users

* Agregar la cuenta de Gmail utilizada en el laboratorio.

* [BUTTON] Save

---

## Configurar el dominio autorizado

* [BROWSER] Google Cloud
  * [BUTTON] Branding

* Buscar:
  * `Authorized domains`

* [BUTTON] Add domain

* Agregar:

  `microsoftonline.com`

* En `Developer contact information`:
  * Agregar la cuenta de Google utilizada para el laboratorio.

* [BUTTON] Save

---

# Configurar Google Federation en Microsoft Entra ID

* [BROWSER] `https://entra.microsoft.com`

* Navegar a:
  * `Entra ID > External Identities > All identity providers`

* Seleccionar Google como proveedor.

* [BUTTON] Configure

* Ingresar:
  * `Client ID`
  * `Client secret`

* Utilizar los valores obtenidos desde Google Cloud.

* [BUTTON] Save

---

# Probar Google Federation

## Invitar la cuenta de Gmail

* Navegar a:
  * `Entra ID > Users > All users`

* [BUTTON] + New User
* [BUTTON] Invite external user

* Ingresar la cuenta Gmail utilizada anteriormente.

* Agregar un mensaje personal si se desea.

* [BUTTON] Review & Invite
* [BUTTON] Invite

---

## Aceptar la invitación

* [BROWSER] Abrir una ventana InPrivate.

* Iniciar sesión en Gmail.

* Abrir el correo de invitación de Microsoft.

* [BUTTON] Accept invitation

* Autenticarse utilizando las credenciales de Google.

* Revisar la pantalla:
  * `Permissions requested by`

* [BUTTON] Accept

* Esperar hasta acceder a My Apps.

---

## Probar el inicio de sesión directo

* [BROWSER] `https://login.microsoftonline.com`

* [BUTTON] Sign-in options
* [BUTTON] Sign in to an organization

* Ingresar el dominio del tenant.

* [BUTTON] Next

* Ingresar la cuenta de Google.

* Verificar que Microsoft redirija la autenticación hacia Google.

---

# Laboratorio opcional: Identidad híbrida con Microsoft Entra Connect

> Este ejercicio es opcional y requiere aproximadamente 120 minutos. También requiere permisos sobre una suscripción de Azure.

## Arquitectura del laboratorio

Se utilizarán:

* `DC1`
  * Windows Server.
  * Active Directory Domain Services.
  * Dominio `corp.contoso.com`.

* `APP1`
  * Servidor miembro del dominio.

* Microsoft Entra ID.
* Microsoft Entra Connect Sync.
* Pass-through Authentication.
* Seamless Single Sign-On.
* Microsoft Entra hybrid join.

---

## Implementar la infraestructura

* [BROWSER] Azure Portal

* Abrir la plantilla proporcionada por el laboratorio:

  `https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmaxskunkworks%2Ftlg%2Fmaster%2Ftlg-base-config_3-vm%2Fazuredeploy.json`

* Iniciar sesión con las credenciales proporcionadas por el entorno.

* [BUTTON] Edit template

* Buscar:

```text
"kind": "Storage"
```

* Modificar:

```text
"apiVersion": "2017-06-01"
```

por:

```text
"apiVersion": "2023-01-01"
```

* Modificar:

```text
"kind": "Storage"
```

por:

```text
"kind": "StorageV2"
```

* [BUTTON] Save

* Configurar:
  * `Resource group`: `hybrididentity-RG`
  * `Region`: `East US`
  * `Config Name`: `TlgBaseConfig-01`
  * `Domain Name`: `corp.contoso.com`
  * `Server OS`: `2016-Datacenter`
  * `Admin Username`: `demouser`
  * `Admin Password`: `demo@pass123`
  * `Deploy Client VM`: `No`
  * `VM Size`: `Standard_D2s_v3`
  * `DNS Label Prefix`: utilizar un nombre único.

* [BUTTON] Review + Create
* [BUTTON] Create

* Esperar aproximadamente entre 40 y 60 minutos.

---

## Conectarse a DC1

* [BROWSER] Azure Portal
  * Abrir `hybrididentity-RG`.
  * Seleccionar `DC1`.
  * [BUTTON] Connect
  * [BUTTON] Download RDP file

* Abrir `DC1.rdp`.

* Utilizar:
  * Usuario: `CORP\demouser`
  * Contraseña: `demo@pass123`

---

## Preparar DC1 y APP1

* [CONSOLE] Windows PowerShell ISE como administrador.

* Deshabilitar IE Enhanced Security Configuration y los prompts de UAC utilizados por el laboratorio:

```powershell
$vmNames = @('dc1','app1')

Invoke-Command -ComputerName $vmNames {
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
}

Invoke-Command -ComputerName $vmNames {
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
}

Invoke-Command -ComputerName $vmNames {
    Set-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorAdmin" -Value 0
}
```

* Instalar RSAT:

```powershell
$vmNames = @('dc1','app1')

Invoke-Command -ComputerName $vmNames {
    Install-WindowsFeature RSAT -IncludeAllSubFeature
}
```

* Esperar a que finalice completamente antes de continuar.

---

## Crear la estructura de Active Directory

* [CONSOLE] Windows PowerShell ISE en DC1.

```powershell
New-ADOrganizationalUnit -Name 'Demo Accounts' -Path 'DC=corp,DC=contoso,DC=com'
New-ADOrganizationalUnit -Name 'Users' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
New-ADOrganizationalUnit -Name 'Groups' -Path 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
New-ADOrganizationalUnit -Name 'US' -Path 'OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'

foreach ($state in 'NJ','VT','NY','WA','TX') {
    New-ADOrganizationalUnit -Name $state -Path 'OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
}
```

---

## Crear los grupos

* [CONSOLE]

```powershell
foreach ($group in 'Engineering','Sales','Marketing','Finance') {
    New-ADGroup `
        -Name $group `
        -GroupScope Global `
        -GroupCategory Security `
        -Path 'OU=Groups,OU=Demo Accounts,DC=corp,DC=contoso,DC=com'
}
```

---

## Crear los usuarios del laboratorio

* [CONSOLE]

```powershell
$password = ConvertTo-SecureString 'demo@pass123' -AsPlainText -Force

$users = @(
    @('Ann','G','Ayers','NY','Sales'),
    @('Teresa','F','Bell','WA','Marketing'),
    @('Liam','R','Carter','NJ','Engineering'),
    @('Mia','K','Dawson','NJ','Engineering'),
    @('Noah','J','Ellis','VT','Engineering'),
    @('Olivia','M','Foster','NY','Engineering'),
    @('Ethan','P','Grant','WA','Engineering'),
    @('Ava','L','Hughes','TX','Engineering'),
    @('Lucas','T','Irwin','TX','Sales'),
    @('Emma','S','Jensen','NY','Sales'),
    @('Mason','D','Keller','WA','Marketing'),
    @('Sophia','B','Lawson','VT','Finance'),
    @('Logan','C','Morris','NJ','Finance'),
    @('Isla','N','Norris','TX','Marketing')
)

foreach ($u in $users) {
    $first, $middle, $last, $state, $group = $u
    $sam = $first.Substring(0,1) + $middle + $last

    New-ADUser `
        -Name "$last, $first" `
        -GivenName $first `
        -Surname $last `
        -DisplayName "$first $last" `
        -SamAccountName $sam `
        -UserPrincipalName "$sam@corp.contoso.com" `
        -Department $group `
        -Path "OU=$state,OU=US,OU=Users,OU=Demo Accounts,DC=corp,DC=contoso,DC=com" `
        -AccountPassword $password `
        -Enabled $true `
        -PasswordNeverExpires $true

    Add-ADGroupMember -Identity $group -Members $sam
}
```

* Verificar la cantidad de usuarios:

```powershell
(Get-ADUser -Filter * -SearchBase 'OU=Demo Accounts,DC=corp,DC=contoso,DC=com').Count
```

* El resultado esperado es:

```text
14
```

---

# Crear el tenant para identidad híbrida

* [BROWSER] `https://portal.azure.com`

* Buscar:
  * `Microsoft Entra ID`

* [BUTTON] Manage tenants
* [BUTTON] + Create

* Seleccionar:
  * `Governed Workforce`

* [BUTTON] Continue

* Configurar:
  * `Tenant name`: `Contoso`
  * `Domain name`: utilizar un nombre único.
  * `Country/Region`: `United States`
  * `Resource Group`: `hybrididentity-RG`

* [BUTTON] Review + Create
* [BUTTON] Create

* Copiar el dominio generado:

  `<tenant>.onmicrosoft.com`

* [FILE] Notepad
  * Guardar el dominio porque será utilizado en los siguientes pasos.

---

# Crear un administrador para Microsoft Entra Connect

* [BROWSER] Microsoft Entra ID
  * [BUTTON] Users
  * [BUTTON] All users
  * [BUTTON] + New user
  * [BUTTON] Create new user

* Configurar:
  * `User principal name`: `john.doe`
  * `Display name`: `John Doe`
  * `First name`: `John`
  * `Last name`: `Doe`
  * `Usage location`: `United States`
  * `Password`: `Auto-generate password`

* Copiar la contraseña.

* En `Assignments`:
  * [BUTTON] + Add role
  * Seleccionar `Global Administrator`

* [BUTTON] Review + create
* [BUTTON] Create

* [FILE] Notepad
  * Guardar:
    * `john.doe@<tenant>.onmicrosoft.com`
    * Contraseña temporal.

---

# Configurar el sufijo UPN

* [RDP] DC1

* [BROWSER] Server Manager
  * [BUTTON] Tools
  * [BUTTON] Active Directory Domains and Trusts

* Hacer clic derecho sobre:
  * `Active Directory Domains and Trusts`

* [BUTTON] Properties

* En `Alternative UPN suffixes` agregar:

  `<tenant>.onmicrosoft.com`

* [BUTTON] Add
* [BUTTON] OK

---

# Instalar Microsoft Entra Connect

* [RDP] DC1

* Descargar e iniciar:
  * `AzureADConnect.msi`

* En el asistente:
  * Aceptar los términos.
  * [BUTTON] Continue
  * [BUTTON] Customize

* En `Install required components`:
  * Mantener las opciones predeterminadas.
  * [BUTTON] Install

* En `User sign-in`:
  * Seleccionar `Pass-through authentication`.
  * Habilitar `Enable single sign-on`.

* [BUTTON] Next

* En `Connect to Microsoft Entra ID`:
  * Utilizar:

  `john.doe@<tenant>.onmicrosoft.com`

* Autenticarse y completar MFA si se solicita.

* En `Connect your directories`:
  * Seleccionar `corp.contoso.com`.
  * [BUTTON] Add Directory

* Utilizar:
  * Usuario: `CORP.CONTOSO.COM\demouser`
  * Contraseña: `demo@pass123`

* Continuar hasta `Domain and OU filtering`.

* Seleccionar:
  * `Sync selected domains and OUs`
  * Únicamente `Demo Accounts`

* Continuar con los valores predeterminados.

* En `Enable single sign-on`:
  * [BUTTON] Enter credentials

* Utilizar:
  * Usuario: `CORP\demouser`
  * Contraseña: `demo@pass123`

* En `Ready to configure`:
  * Desmarcar `Start the synchronization process when configuration completes`.

* [BUTTON] Install
* [BUTTON] Exit

---

# Configurar filtrado de sincronización

## Crear la regla UPN Filter

* [RDP] DC1

* Abrir:
  * `Synchronization Rules Editor`

* Seleccionar:
  * `Direction`: `Inbound`

* [BUTTON] Add new rule

* Configurar:
  * `Name`: `Custom In from AD - UPN Filter`
  * `Connected System`: `corp.contoso.com`
  * `Connected System Object Type`: `user`
  * `Metaverse Object Type`: `person`
  * `Link Type`: `join`
  * `Precedence`: `50`

* En `Scoping filter`:
  * [BUTTON] Add Group
  * [BUTTON] Add clause

* Configurar:
  * `Attribute`: `userPrincipalName`
  * `Operator`: `ENDSWITH`
  * `Value`: `@<tenant>.onmicrosoft.com`

* En `Transformations`:
  * `FlowType`: `Constant`
  * `Target Attribute`: `cloudFiltered`
  * `Source`: `False`

* [BUTTON] Add

---

## Crear la regla Catch-all Filter

* [BUTTON] Add new rule

* Configurar:
  * `Name`: `Custom In from AD - Catch-all Filter`
  * `Connected System`: `corp.contoso.com`
  * `Connected System Object Type`: `user`
  * `Metaverse Object Type`: `person`
  * `Link Type`: `join`
  * `Precedence`: `51`

* No configurar ningún `Scoping filter`.

* En `Transformations`:
  * `FlowType`: `Constant`
  * `Target Attribute`: `cloudFiltered`
  * `Source`: `True`

* [BUTTON] Add

---

# Iniciar la sincronización

* [RDP] DC1

* Abrir:
  * `Azure AD Connect`

* [BUTTON] Configure

* Seleccionar:
  * `Customize synchronization options`

* [BUTTON] Next

* Autenticarse utilizando:
  * `john.doe@<tenant>.onmicrosoft.com`

* Continuar utilizando la configuración existente.

* En `Ready to configure`:
  * Seleccionar `Start the synchronization process when configuration completes`.

* [BUTTON] Configure
* [BUTTON] Exit

---

## Verificar la sincronización

* [BROWSER] Microsoft Entra admin center
  * [BUTTON] Users
  * [BUTTON] All users

* Verificar que aparezcan los usuarios sincronizados.

* [BUTTON] Groups
* [BUTTON] All groups

* Verificar que aparezcan:
  * `Engineering`
  * `Finance`
  * `Marketing`
  * `Sales`

* Navegar a Microsoft Entra Connect.

* Verificar:
  * `Sync status`: `Enabled`
  * `Password Hash Sync`: `Disabled`
  * `Federation`: `Disabled`
  * `Seamless single sign-on`: `Enabled`
  * `Pass-through authentication`: `Enabled`

---

# Configurar Microsoft Entra hybrid join

* [RDP] DC1
  * Abrir `Azure AD Connect`.
  * [BUTTON] Configure

* Seleccionar:
  * `Configure device options`

* [BUTTON] Next

* Seleccionar:
  * `Configure Hybrid Microsoft Entra ID join`

* [BUTTON] Next

* Seleccionar:
  * `Windows 10 or later domain-joined devices`
  * `Supported Windows down-level domain-joined devices`

* [BUTTON] Next

* En `SCP configuration`:
  * Seleccionar `corp.contoso.com`.
  * `Authentication Service`: `Microsoft Entra ID`
  * [BUTTON] Add

* Utilizar:
  * Usuario: `CORP\demouser`
  * Contraseña: `demo@pass123`

* [BUTTON] Next
* [BUTTON] Configure
* [BUTTON] Exit

---

# Comprobar Microsoft Entra hybrid join

* [BROWSER] Azure Portal
  * Abrir la VM `APP1`.
  * [BUTTON] Connect
  * [BUTTON] Download RDP file

* [RDP] APP1

* Utilizar:
  * Usuario: `AGAyers@<tenant>.onmicrosoft.com`
  * Contraseña: `demo@pass123`

* Abrir:
  * `Task Scheduler`

* Navegar a:

  `Task Scheduler Library > Microsoft > Windows > Workplace Join`

* Habilitar y ejecutar:
  * `Automatic-Device-Join`

* [RDP] DC1

* [CONSOLE]

```powershell
Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'
Start-ADSyncSyncCycle -PolicyType Delta
```

* [RDP] APP1

* [CONSOLE]

```cmd
dsregcmd /status
```

* Verificar:

```text
AzureAdJoined : YES
DomainJoined  : YES
```

* [BROWSER] Microsoft Entra admin center
  * Navegar a `Entra ID > Devices > All devices`.

* Buscar:
  * `APP1`

* Verificar que el tipo de unión sea:

  `Microsoft Entra hybrid joined`

# Conclusiones

En este laboratorio aprendimos a:

* Crear y administrar usuarios en Microsoft Entra ID.
* Comprobar los permisos de un usuario sin privilegios administrativos.
* Asignar y eliminar el rol Application Administrator.
* Crear usuarios mediante operaciones masivas con CSV.
* Administrar usuarios mediante Microsoft Graph PowerShell.
* Eliminar y restaurar usuarios.
* Asignar licencias desde Microsoft 365 admin center.
* Utilizar grupos de seguridad para administrar licencias.
* Crear grupos de seguridad y grupos de Microsoft 365.
* Crear grupos dinámicos mediante reglas.
* Configurar propiedades y privacidad del tenant.
* Configurar opciones de colaboración externa.
* Invitar usuarios B2B individualmente y mediante operaciones masivas.
* Invitar usuarios mediante Microsoft Graph PowerShell.
* Configurar Google como proveedor de identidad federado.
* Validar el inicio de sesión de usuarios externos utilizando sus credenciales de Google.
* Preparar una infraestructura local de Active Directory para una implementación híbrida.
* Instalar y configurar Microsoft Entra Connect.
* Configurar Pass-through Authentication y Seamless Single Sign-On.
* Filtrar los objetos sincronizados hacia Microsoft Entra ID.
* Sincronizar usuarios y grupos desde Active Directory.
* Configurar y verificar Microsoft Entra hybrid join.
