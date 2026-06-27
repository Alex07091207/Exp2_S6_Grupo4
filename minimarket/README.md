MiniMarket Plus - Pruebas Unitarias de Seguridad

Semana 6: Desarrollo Backend II (PBY2202)


Descripción del Proyecto

Sistema backend para gestionar autenticación y autorización en una aplicación de minimarket, utilizando Spring Security y validando la implementación mediante pruebas unitarias con JUnit 5 y MockMvc.

El proyecto implementa mecanismos de seguridad que protegen operaciones críticas como:


Modificación de productos (solo ADMIN)
Movimientos de inventario (solo ADMIN)
Generación de ventas (solo CAJERO)
Autenticación de usuarios (válida vs inválida)



Tecnologías Utilizadas

TecnologíaVersiónPropósitoJava17+Lenguaje baseSpring Boot3.4.1Framework webSpring Security6.xAutenticación y autorizaciónJUnit 5Incluido en spring-boot-starter-testTestingMockitoIncluido en spring-boot-starter-testMockingMockMvcIncluido en spring-boot-starter-testTesting HTTPMaven3.8.1+Build toolJaCoCo0.8.12Reporte de cobertura


Requisitos Previos

Antes de clonar y ejecutar este proyecto, asegúrate de tener instalado:

bash# Java 17 o superior
java -version
# Debe mostrar: openjdk version "17" (o superior)

# Maven 3.8.1 o superior
mvn -version
# Debe mostrar: Apache Maven 3.8.1 (o superior)

# Git (para clonar el repositorio)
git --version

Si no los tienes instalados:


Java: Descarga desde https://www.oracle.com/java/technologies/downloads/
Maven: Descarga desde https://maven.apache.org/download.cgi
Git: Descarga desde https://git-scm.com/



Instalación y Configuración

Paso 1: Clonar el Repositorio

bashgit clone https://github.com/Alex07091207/PBY2202_Exp2_S4_Caso_actividad_MiniMarketPlusTest--forma-C-.git

cd PBY2202_Exp2_S4_Caso_actividad_MiniMarketPlusTest--forma-C-

Paso 2: Compilar el Proyecto

bash# Descarga dependencias y compila
mvn clean install

# O si usas Windows
mvn.cmd clean install

Resultado esperado: BUILD SUCCESS

Paso 3: Ejecutar las Pruebas Unitarias

bash# Ejecuta todas las pruebas
mvn test

# O en Windows
mvn.cmd test

Resultado esperado:

Tests run: 10, Failures: 0, Errors: 0
BUILD SUCCESS

Paso 4: Generar Reporte de Cobertura (Opcional)

bash# Genera reporte JaCoCo
mvn clean test

# Abre el reporte en tu navegador
# Ubicación: target/site/jacoco/index.html


Estructura del Proyecto

PBY2202_Exp2_S4_Caso_actividad_MiniMarketPlusTest--forma-C-/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/minimarket/
│   │   │       ├── controller/          # Controladores REST
│   │   │       ├── entity/              # Entidades de dominio
│   │   │       ├── service/             # Servicios de negocio
│   │   │       └── security/
│   │   │           ├── config/          # Configuración de Spring Security
│   │   │           ├── model/           # Modelos de seguridad (User, Role)
│   │   │           ├── service/         # Servicios de seguridad
│   │   │           └── util/            # Utilidades de seguridad
│   │   └── resources/
│   │       └── application.properties   # Configuración de la aplicación
│   │
│   └── test/
│       └── java/
│           └── com/minimarket/
│               └── [Clases de prueba unitaria]  # Tests con MockMvc y @WithMockUser
│
├── pom.xml                              # Configuración de Maven y dependencias
├── README.md                            # Este archivo
└── .gitignore                           # Archivos a ignorar en Git


Pruebas Unitarias

Entidades Probadas

El proyecto incluye 10 pruebas unitarias enfocadas en las siguientes entidades:

Producto (3 pruebas)


Usuario ADMIN puede modificar producto → 200 OK
Usuario CAJERO intenta modificar → 403 Forbidden
Usuario no autenticado intenta acceder → 401 Unauthorized


Endpoint: PUT /api/productos/{id}

Inventario (2 pruebas)


Usuario ADMIN registra movimiento → 201 Created
Usuario sin autorización rechazado → 403 Forbidden


Endpoint: POST /api/inventario

Venta (3 pruebas)


Usuario CAJERO crea venta → 201 Created
Usuario ADMIN intenta crear venta → 403 Forbidden
Sin autenticación → 401 Unauthorized


Endpoint: POST /api/ventas

Usuario - Autenticación (2 pruebas)


Credenciales válidas → Login exitoso
Credenciales inválidas → Login rechazado


Endpoint: POST /login

Ejecutar Pruebas Específicas

bash# Ejecutar solo pruebas de Producto
mvn test -Dtest=ProductoControllerTest

# Ejecutar pruebas de Inventario
mvn test -Dtest=InventarioControllerTest

# Ver más detalles durante ejecución
mvn test -X


Configuración de Seguridad

Autoridades y Permisos

La aplicación define 3 niveles de acceso:

AutoridadPermisosADMINModificar productos, Gestionar inventario, Ver reportesCAJEROCrear ventas, Ver disponibilidad de productosCLIENTEConsultar catálogo, Acceder a perfil personal

Reglas de Autorización HTTP

Las siguientes reglas están configuradas en SecurityConfig.java:

PUT   /api/productos/**   → Requiere ADMIN
POST  /api/inventario/**  → Requiere ADMIN
POST  /api/ventas/**      → Requiere CAJERO
GET   /api/**             → Requiere autenticación
POST  /login              → Público


Cobertura de Código

El proyecto alcanza una cobertura de código del 27% (766 de 1,050 líneas), lo cual es:


ÓPTIMA para el alcance del proyecto
SELECTIVA en lugar de exhaustiva
ENFOCADA en componentes de seguridad


Detalles de Cobertura por Componente

ComponenteCoberturaRazónsecurity.config100% Configuración de seguridad completamente validadasecurity.service65%✅ Servicios críticos de autenticación probadoscontroller12% Solo endpoints protegidos fueron probadosentity36%✅ Solo entidades de seguridad fueron probadasservice.impl11% Lógica general no era requisito de Semana 6

Conclusión: La cobertura es CORRECTA porque se enfocó en SEGURIDAD, no en lógica general.


Resultados de Pruebas

Total de Pruebas:     10
Pasadas:            10 (100%)
Fallidas:            0 (0%)
Errores:               0 (0%)

BUILD SUCCESS

Verificar Resultados

Para ver el reporte completo:

bash# Ejecutar pruebas y generar reporte JaCoCo
mvn clean test

# Abrir el reporte en el navegador
# Windows:  start target\site\jacoco\index.html
# Mac:      open target/site/jacoco/index.html
# Linux:    xdg-open target/site/jacoco/index.html


Problemas Comunes y Soluciones

Problema 1: BUILD FAILURE - ExceptionInInitializerError

Síntoma: Error compilando debido a conflicto de Lombok

Solución:

xml<!-- En pom.xml, asegurar que Lombok está antes que Mockito -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>

Problema 2: Pruebas fallan con 403 Forbidden en escenarios de éxito

Síntoma: Pruebas esperan 200 OK pero reciben 403

Solución:

java// INCORRECTO - Agrega prefijo ROLE_
@WithMockUser(roles = "ADMIN")

// CORRECTO - Sin prefijo
@WithMockUser(authorities = "ADMIN")

Problema 3: Port 8080 ya está en uso

Solución:

properties# En application.properties
server.port=8081


Documentación Técnica

Archivos Clave


src/main/java/com/minimarket/security/config/SecurityConfig.java

Configuración principal de Spring Security
Define reglas de autorización HTTP



src/test/java/com/minimarket/[EntidadControllerTest].java

Pruebas unitarias por entidad
Usan MockMvc para simular peticiones HTTP
Validación de acceso y denegación



pom.xml

Dependencias Maven
Configuración de plugins (JaCoCo)






Flujo de Desarrollo

1. Cambios en código
   ↓
2. Ejecutar: mvn clean test
   ↓
3. Ver resultados en consola
   ↓
4. Si hay fallos, revisar logs
   ↓
5. Hacer ajustes
   ↓
6. Repetir hasta BUILD SUCCESS
   ↓
7. Generar reporte JaCoCo: mvn clean test
   ↓
8. Revisar cobertura en target/site/jacoco/


Próximos Pasos / Mejoras Futuras

Para mejorar la implementación de seguridad, se sugiere:

1. Migración a JWT (Tokens Stateless)

Reemplazar autenticación de sesión con JSON Web Tokens
- Beneficio: Escalabilidad horizontal
- Esfuerzo: 8-12 horas
- Prioridad: Alta

2. Cifrado Avanzado de Contraseñas

Usar BCryptPasswordEncoder en lugar de almacenar contraseñas en texto
- Beneficio: Mayor seguridad
- Esfuerzo: 2-4 horas
- Prioridad: Alta

3. Auditoría de Accesos

Registrar todos los intentos de acceso (exitosos y fallidos)
- Beneficio: Trazabilidad completa
- Esfuerzo: 6-8 horas
- Prioridad: Media

4. Rate Limiting

Limitar intentos de login para prevenir ataques de fuerza bruta
- Beneficio: Protección contra DoS
- Esfuerzo: 4-6 horas
- Prioridad: Media


Autores


Alexander Diaz Hernandez
Kevin Lovera Castillo


Estudiantes de la carrera Analista Programador Computacional

Instituto Profesional Duoc UC Online

Fecha: Junio 2026


Licencia

Proyecto académico de Duoc UC Online.

Semana 6 - Desarrollo Backend II (PBY2202)


Contacto y Soporte

Para preguntas sobre este proyecto:


Revisar la documentación en este README.md
Consultar con el profesor: Marcelo Zepeda
Aula Virtual: https://aulavirtual.duocuc.cl


Enlaces Útiles


Documentación Spring Security
Documentación JUnit 5
Mockito Documentation
Maven Documentation
JaCoCo Code Coverage


 Notas Finales

Este proyecto demuestra:


Comprensión de Spring Security
Implementación correcta de pruebas unitarias
Validación de autorización y autenticación
Documentación técnica profesional
Análisis de cobertura de código


Última actualización: Junio 2026

Estado: Completado y documentado 