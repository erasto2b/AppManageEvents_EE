# AppManageEvents
## Aplicación de EventosManager para talento digital
## numero intento 12


- ##### Introducción *Presentación de equipo*  :
 - Cristopher Vergara
 - Erasto Borthomierth 
 - Gabriel Muñoz  
 - Sixto Felipe   
 - Jesús Seiler 




- ##### CVS
 - Manejo de versión y trabajo en equipo : github  - git




- ##### Herramientas para aplicar DevOps
  - Webhooks
  - Jenkins


- ##### Stack
 - Java Spring Boot + Thymeleaf + PostgreSQL


- ##### Pruebas
  - Junit


- ## Requerimientos Entidades / Funciones Clave de la aplicación
 * CRUD para gestión de eventos :
   * Entidades :
     * (EVENTO)(CRUD)
       * Id
       * Nombre
       * Fecha
       * Invitados

     * (SALÓN)(CRUD) 
       * Id 
       * Descripción 
       * Capacidad

- ## Funcionalidades:
    - Crear, Leer, Actualizar y Borrar eventos (CRUD básico) 
    - Listar todos los eventos
    - Crear, Leer, Actualizar y Borrar salones (CRUD básico) 
    - Asignar salones a eventos
    

- ## Funcionalidades Adicionales
 - Registrar evento:
   - Asignar un nombre, fecha y lista de invitados al evento.
   - Asignar salón para el evento:
   - Seleccionar un salón disponible que cumpla con la capacidad requerida.
 - Editar evento:
   - Modificar detalles del evento como nombre, fecha y lista de invitados.
   - Eliminar evento:
   - Borrar un evento existente.

- ## Duración Estimada del Desarrollo
 - Desarrollo de las funcionalidades básicas (CRUD): 2 semanas
 - Implementación de la interfaz de usuario (Thymeleaf): 1 semana
 - Integración con PostgreSQL: 1 semana
 - Implementación de pruebas (JUnit): 1 semana
 - Total estimado: 5 semanas

- ## Despliegue
 - Para el despliegue de la aplicación, se considera utilizar un enfoque de despliegue continuo (CI/CD) con Jenkins:
   - Integración continua (CI): Automatización de pruebas y construcción de la aplicación cada vez que se realiza un commit en la rama principal del repositorio.
   - Despliegue continuo (CD): Automatización del despliegue de la aplicación en un entorno de producción una vez que las pruebas hayan sido satisfactorias.

  ## Comandos
    $ java -jar AppManageEvents-0.0.1-RELEASE.jar
    $ mvn clean install
    $ mvn spring-boot:run
    


- ## Ambiente de Desarrollo
 - Desarrollo local en máquinas de desarrolladores.

- ## Ambiente de Producción
 - Despliegue en un servidor en la nube (por ejemplo, AWS, Azure, Google Cloud) con PostgreSQL como base de datos y Java Spring Boot para el backend.


