# Microsoft Entra ID: Identidad, autenticación, autorización y Zero Trust

## Introducción

La identidad es uno de los pilares fundamentales de la seguridad en Microsoft Cloud. Microsoft Entra ID proporciona servicios para administrar identidades, autenticación, autorización, acceso y gobernanza en servicios como Azure y Microsoft 365.

Los principales conceptos relacionados con identidad son:

- **Autenticación (AuthN):** demuestra quién es el usuario, aplicación o dispositivo.
- **Autorización (AuthZ):** determina qué recursos puede utilizar una identidad y qué puede hacer con ellos.
- **Administración:** gestiona las identidades durante todo su ciclo de vida.
- **Auditoría:** permite registrar y analizar las acciones realizadas por las identidades.
- **Gobernanza:** garantiza que los accesos continúen siendo apropiados a lo largo del tiempo.

---

# Zero Trust

Zero Trust parte de la idea de que ninguna solicitud debe considerarse confiable automáticamente, independientemente de dónde se origine.

> **Never trust, always verify.**

Los tres principios fundamentales son:

1. **Verificar explícitamente**
2. **Usar acceso con privilegios mínimos**
3. **Asumir que puede existir una brecha de seguridad**

La verificación puede considerar múltiples señales:

- Identidad del usuario
- Ubicación
- Estado del dispositivo
- Riesgo del inicio de sesión
- Aplicación utilizada
- Sensibilidad de los datos
- Comportamientos anómalos

## Identidad clásica vs. Zero Trust

| Identidad clásica | Zero Trust |
|---|---|
| Confía principalmente en el perímetro de red | No confía automáticamente en ninguna ubicación |
| Los recursos están protegidos detrás del firewall | Los recursos se protegen dondequiera que estén |
| El acceso puede depender de usuario y contraseña | El acceso utiliza múltiples señales |
| Confianza implícita después del acceso | Verificación continua y acceso mínimo |

---

# La identidad como plano de control

En los entornos modernos, los usuarios acceden a recursos desde diferentes:

- Redes
- Dispositivos
- Aplicaciones
- Servicios SaaS
- Entornos locales
- Servicios cloud

El elemento común es la **identidad**.

Por este motivo, la identidad funciona como un **plano de control** desde el cual se pueden tomar decisiones sobre quién puede acceder, a qué recurso y bajo qué condiciones.

---

# Funciones principales de la identidad

La identidad permite responder cuatro preguntas fundamentales:

| Función | Pregunta |
|---|---|
| **Autenticación** | ¿Quién eres? |
| **Autorización** | ¿Qué puedes hacer? |
| **Administración** | ¿Cómo gestionamos tu identidad? |
| **Auditoría** | ¿Qué hiciste? |

---

# Proveedor de identidades (IdP)

Un **Identity Provider (IdP)** es un sistema encargado de crear, administrar y verificar identidades digitales.

Un IdP normalmente proporciona:

- Repositorio de identidades
- Sistema de autenticación
- Protocolos de seguridad
- Emisión de tokens
- Integración con Single Sign-On (SSO)

**Microsoft Entra ID** es un ejemplo de proveedor de identidades.

---

# Protocolos de identidad

## OpenID Connect (OIDC)

**OpenID Connect** es un protocolo de autenticación basado en **OAuth 2.0**.

Permite:

- Autenticar usuarios
- Implementar Single Sign-On
- Obtener información básica del usuario
- Utilizar ID Tokens

## OAuth 2.0

OAuth 2.0 se utiliza principalmente para **autorización**.

Permite que una aplicación obtenga acceso limitado a recursos protegidos mediante tokens.

## SAML

**Security Assertion Markup Language (SAML)** es un estándar basado en XML utilizado para intercambiar información de autenticación y autorización entre:

- Identity Provider (IdP)
- Service Provider (SP)

## WS-Federation

**WS-Federation (WS-Fed)** es un protocolo utilizado para federar identidades y permitir escenarios de Single Sign-On.

---

# Microsoft Entra ID

Microsoft Entra ID es el servicio de administración de identidades y acceso basado en la nube de Microsoft.

Permite administrar identidades para:

- Microsoft 365
- Azure
- Aplicaciones SaaS
- Aplicaciones empresariales
- Aplicaciones desarrolladas internamente
- Entornos híbridos

Microsoft Entra ID proporciona capacidades de **Identity as a Service (IDaaS)**.

---

# Terminología de Microsoft Entra ID

## Identidad

Objeto que puede autenticarse.

Puede representar:

- Usuario
- Aplicación
- Servicio
- Dispositivo

## Cuenta

Una identidad que tiene información asociada.

## Usuario

Identidad personal verificable dentro de Microsoft Entra ID.

## Grupo

Colección de identidades utilizada para simplificar la asignación de:

- Permisos
- Aplicaciones
- Licencias
- Políticas

## Tenant

Un **tenant de Microsoft Entra** representa una instancia dedicada de Microsoft Entra ID asociada normalmente con una organización.

Los términos **tenant** y **directorio** suelen utilizarse de forma relacionada.

## Unidad administrativa

Permite establecer límites administrativos dentro de un tenant.

---

# Microsoft Entra ID vs. AD DS vs. Entra Domain Services

| Servicio | Uso principal |
|---|---|
| **Microsoft Entra ID** | Identidad y autenticación cloud |
| **Active Directory Domain Services (AD DS)** | Identidad y administración tradicional de dominios |
| **Microsoft Entra Domain Services** | Servicios de dominio administrados en Azure |

### Active Directory Domain Services

Proporciona funcionalidades tradicionales como:

- LDAP
- Kerberos
- NTLM
- Group Policy
- Domain Join
- Administración de equipos

### Microsoft Entra ID

Está diseñado principalmente para escenarios cloud como:

- Microsoft 365
- Azure
- Aplicaciones SaaS
- Aplicaciones modernas
- Dispositivos móviles

### Microsoft Entra Domain Services

Proporciona servicios administrados compatibles con tecnologías tradicionales como:

- Domain Join
- Group Policy
- LDAP
- Kerberos
- NTLM

Es especialmente útil para aplicaciones heredadas migradas a Azure que todavía necesitan servicios tradicionales de dominio.

---

# Autenticación

La **autenticación (AuthN)** verifica que una identidad es realmente quien afirma ser.

Puede utilizar:

- Contraseña
- Certificados
- Claves
- Biométricos
- MFA
- Métodos passwordless
- Señales basadas en riesgo

Una organización debe buscar un equilibrio entre:

- Seguridad
- Experiencia del usuario
- Protocolos modernos
- Nivel de confianza requerido

---

# Autenticación multifactor

La **Multi-Factor Authentication (MFA)** aumenta la seguridad utilizando diferentes factores de autenticación.

Ejemplos:

- Algo que sabes: contraseña o PIN
- Algo que tienes: teléfono o clave de seguridad
- Algo que eres: huella digital o reconocimiento biométrico

---

# Federación de identidades

La federación permite establecer relaciones de confianza entre diferentes sistemas de identidad.

Esto permite reutilizar identidades existentes para acceder a recursos externos.

Ejemplo:

Un usuario autenticado mediante el proveedor de identidad de su organización puede acceder a una aplicación externa sin crear otra identidad independiente.

---

# Claims

Una **claim** es una afirmación sobre una identidad incluida dentro de un token.

Ejemplos:

- Nombre
- Identificador
- Email
- Tenant
- Roles
- Audiencia del token

Las aplicaciones pueden utilizar estas claims para tomar decisiones de autorización.

Flujo simplificado:

1. El usuario se autentica.
2. El Identity Provider genera un token.
3. El token contiene claims.
4. La aplicación valida el token.
5. La aplicación utiliza las claims para determinar el acceso.

---

# Tokens de seguridad

La plataforma de identidad de Microsoft utiliza diferentes tipos de tokens.

## Access Token

Permite acceder a un recurso protegido o API.

Ejemplo:

Una aplicación solicita acceso a Microsoft Graph y utiliza un Access Token para realizar llamadas.

## Refresh Token

Permite obtener nuevos Access Tokens sin solicitar nuevamente las credenciales al usuario.

## ID Token

Se utiliza con OpenID Connect y contiene información sobre la identidad autenticada.

### Resumen

| Token | Propósito |
|---|---|
| **Access Token** | Acceder a APIs y recursos |
| **Refresh Token** | Obtener nuevos Access Tokens |
| **ID Token** | Identificar al usuario |

---

# JSON Web Token (JWT)

**JWT** es un estándar para representar información de forma compacta mediante JSON.

Normalmente contiene:

- Header
- Payload
- Signature

Los JWT firmados permiten verificar:

- Integridad del token
- Emisor
- Audiencia
- Claims
- Validez temporal

---

# Autorización

La **autorización (AuthZ)** determina qué puede hacer una identidad después de autenticarse.

Ejemplo:

Un usuario puede autenticarse correctamente, pero eso no significa que pueda:

- Leer todos los archivos
- Modificar configuraciones
- Administrar usuarios
- Eliminar recursos

La autorización determina esos permisos.

---

# Modelos de autorización

## ACL

**Access Control List**

Define explícitamente qué identidades pueden acceder a un recurso.

Ventaja:

- Control granular

Desventaja:

- Difícil administración a gran escala

## RBAC

**Role-Based Access Control**

Los permisos se asignan a roles y los usuarios reciben esos roles.

Ejemplo:

```text
Usuario -> Rol -> Permisos
```

Es uno de los modelos más utilizados.

## ABAC

**Attribute-Based Access Control**

Utiliza atributos para tomar decisiones.

Puede considerar:

- Usuario
- Recurso
- Dispositivo
- Ubicación
- Horario
- Sensibilidad

Ejemplo:

```text
Permitir acceso si:

Departamento = Finanzas
AND
Dispositivo = Administrado
AND
Horario = Laboral
```

## PBAC

**Policy-Based Access Control**

Utiliza políticas centralizadas para determinar el acceso a los recursos.

---

# Administración de identidades

La administración de identidades controla los objetos de identidad durante todo su ciclo de vida.

Entre las tareas habituales se encuentran:

- Crear usuarios
- Actualizar usuarios
- Eliminar usuarios
- Administrar grupos
- Administrar contraseñas
- Asignar aplicaciones
- Administrar permisos
- Sincronizar identidades
- Automatizar procesos

---

# Provisionamiento y desprovisionamiento

## Provisionamiento

Creación y configuración automática de identidades.

Ejemplo:

```text
Nuevo empleado
      ↓
Sistema de RR. HH.
      ↓
Microsoft Entra ID
      ↓
Cuenta + grupos + aplicaciones
```

## Desprovisionamiento

Elimina o deshabilita el acceso cuando una identidad ya no debería utilizar los recursos.

Es fundamental para evitar cuentas abandonadas con permisos activos.

---

# Ciclo de vida de las identidades

La estrategia suele resumirse como:

### Joiner

Una persona entra en la organización.

Acciones:

- Crear identidad
- Asignar grupos
- Asignar aplicaciones
- Asignar permisos

### Mover

Una persona cambia de función.

Ejemplo:

```text
Ventas -> Marketing
```

Se deben eliminar permisos anteriores y proporcionar los nuevos.

### Leaver

Una persona abandona la organización.

Se debe:

- Bloquear acceso
- Revocar sesiones
- Eliminar permisos
- Retirar aplicaciones
- Deshabilitar o eliminar la cuenta según las políticas de la organización

---

# Gobernanza de identidades

La gobernanza garantiza que los usuarios mantengan solamente los accesos necesarios.

Permite responder preguntas como:

- ¿Esta persona todavía trabaja en la empresa?
- ¿Necesita estos permisos?
- ¿Cuándo utilizó la cuenta por última vez?
- ¿Tiene privilegios administrativos innecesarios?
- ¿Quién aprobó este acceso?

La gobernanza debe funcionar durante todo el ciclo de vida de la identidad.

---

# Auditoría

La auditoría permite registrar y analizar las actividades relacionadas con identidad.

Ejemplos:

- Inicios de sesión
- Cambios de contraseña
- Cambios administrativos
- Uso de MFA
- Provisionamiento de usuarios
- Modificaciones de configuración

Entre los registros importantes de Microsoft Entra se encuentran:

- Sign-in logs
- Audit logs
- Provisioning logs

Estos datos pueden utilizarse con servicios como:

- Azure Monitor
- Microsoft Sentinel

---

# Microsoft Entra External ID

Microsoft Entra permite trabajar con identidades externas a la organización.

Los usuarios externos pueden utilizar identidades existentes para acceder a los recursos autorizados.

Esto permite escenarios como:

- Partners
- Proveedores
- Contratistas
- Clientes
- Usuarios externos

---

# B2B Collaboration

**Business-to-Business (B2B)** permite colaborar con usuarios externos sin tener que administrar completamente sus credenciales.

Los usuarios externos pueden utilizar su propia identidad para acceder a recursos compartidos.

Normalmente aparecen en el directorio como usuarios invitados.

---

# B2B Direct Connect

Permite establecer confianza entre organizaciones de Microsoft Entra.

Un escenario importante es la colaboración mediante **canales compartidos de Microsoft Teams**.

El usuario externo puede acceder utilizando la identidad de su propia organización.

---

# Identidad centralizada vs. descentralizada

## Identidad centralizada

Una autoridad central administra las identidades.

Ejemplo:

**Microsoft Entra ID**

Características:

- Administración centralizada
- Repositorio central de identidades
- Autenticación
- Autorización
- Políticas centralizadas

## Identidad descentralizada

El usuario tiene mayor control sobre su propia identidad digital y sus credenciales.

Entre los conceptos relacionados se encuentran:

- Decentralized Identifiers (DID)
- Verifiable Credentials
- Public Key Infrastructure
- Wallets
- Identidad autosoberana

Los DID utilizan estándares desarrollados por **W3C**.

---

# IAM

**Identity and Access Management (IAM)** engloba los procesos y tecnologías utilizados para administrar identidades y controlar el acceso a recursos.

IAM combina principalmente:

```text
Identidad
   ↓
Autenticación
   ↓
Autorización
   ↓
Acceso
   ↓
Auditoría
   ↓
Gobernanza
```

---

# Automatización de identidades

La administración de Microsoft Entra puede automatizarse utilizando diferentes herramientas.

## PowerShell

PowerShell proporciona cmdlets para gestionar los recursos de Microsoft.

Para Microsoft Graph se utiliza el **Microsoft Graph PowerShell SDK**.

## Azure CLI

Azure CLI proporciona una interfaz de línea de comandos multiplataforma.

Puede utilizarse desde:

- Windows
- Linux
- macOS
- Bash
- PowerShell

## Microsoft Graph

Microsoft Graph proporciona APIs para acceder y administrar información de servicios Microsoft Cloud.

Su endpoint principal es:

`https://graph.microsoft.com`

Permite trabajar con información relacionada con:

- Usuarios
- Grupos
- Aplicaciones
- Dispositivos
- Microsoft 365
- Seguridad
- Identidad

---

# Microsoft Graph

Microsoft Graph constituye una interfaz unificada para acceder a datos y servicios de Microsoft Cloud.

Sus principales componentes incluyen:

### Microsoft Graph API

Permite que aplicaciones y scripts interactúen con los servicios Microsoft mediante APIs REST y SDK.

### Microsoft Graph Connectors

Permiten incorporar información externa en experiencias Microsoft, incluyendo información procedente de plataformas de terceros.

### Microsoft Graph Data Connect

Permite trabajar con grandes volúmenes de datos de Microsoft 365 para escenarios analíticos mediante servicios de datos de Azure.

---

# Licencias

Microsoft Entra dispone de diferentes niveles de licenciamiento.

Entre ellos:

- Microsoft Entra ID Free
- Microsoft Entra ID P1
- Microsoft Entra ID P2

Las características avanzadas pueden requerir licencias específicas.

Por ejemplo, **Privileged Identity Management (PIM)** y determinadas capacidades avanzadas de Identity Governance requieren licenciamiento adicional.

---

# Suscripción vs. licencia

## Suscripción

Contrato con Microsoft que permite consumir uno o varios servicios cloud.

Ejemplos:

- Azure
- Microsoft 365
- Dynamics 365

## Licencia

Permite que un usuario concreto utilice determinadas funcionalidades incluidas dentro de una suscripción.

Ejemplo:

```text
Organización
    ↓
Microsoft 365 Subscription
    ↓
100 licencias
    ↓
Hasta 100 usuarios licenciados
```

---

# En resumen

La seguridad moderna de identidades puede representarse como:

```text
Usuario / Aplicación / Dispositivo
              ↓
        Microsoft Entra ID
              ↓
         Autenticación
              ↓
          Autorización
              ↓
      Políticas de acceso
              ↓
           Recurso
              ↓
     Auditoría y Gobernanza
```

Todo el modelo debe estar basado en los principios de **Zero Trust**:

1. **Verificar explícitamente**
2. **Aplicar privilegios mínimos**
3. **Asumir que existe una brecha**

La identidad se convierte así en uno de los principales **planos de control de seguridad** para Azure, Microsoft 365 y el resto de los servicios de Microsoft Cloud.
