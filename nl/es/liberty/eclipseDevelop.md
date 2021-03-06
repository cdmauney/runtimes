---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}

# Despliegue aplicaciones con IBM Eclipse Tools para {{site.data.keyword.cloud_notm}}

También puede utilizar {{site.data.keyword.eclipsetoolsfull}} como una forma alternativa para desarrollar y desplegar aplicaciones en {{site.data.keyword.Bluemix}}. {{site.data.keyword.eclipsetoolsfull}} proporciona plug-ins que se pueden instalar en un entorno Eclipse existente para ayudarle a integrar el entorno de desarrollo integrado (IDE) con {{site.data.keyword.Bluemix_notm}}.

Este procedimiento sigue los mismos pasos generales que la [Guía de aprendizaje de iniciación](getting-started.html) para Liberty. Si utiliza Eclipse, configurará una app localmente y en la nube, e integrará un servicio de base de datos de {{site.data.keyword.Bluemix_notm}} en la app.

## Antes de empezar
{: #prereqs}

Necesitará las siguientes cuentas y herramientas:
* [IBM Eclipse Tools for IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/wasdev/downloads/#asset/tools-IBM_Eclipse_Tools_for_Bluemix){: new_window}
* [Eclipse IDE for Java EE Developers ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/neon2){: new_window}

Si ha completado la [Guía de aprendizaje de iniciación](getting-started.md), es posible que ya tenga estas herramientas y cuentas. Antes de empezar, asegúrese de que también ha instalado y registrado lo siguiente:
* [Cuenta de {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/registration)
* [CLI de {{site.data.keyword.Bluemix_notm}}](../../cli/reference/bluemix_cli/download_cli.html)
* [Git ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://git-scm.com/downloads){: new_window}
* [Maven ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://maven.apache.org/download.cgi){: new_window}

## Paso 1: Clone la app de ejemplo
{: #clone}

En primer lugar, clone el repositorio GitHub de la app de ejemplo.

  ```
git clone https://github.com/IBM-Cloud/get-started-java
  ```
  {: codeblock}

## Paso 2: Cree el código fuente de la aplicación
{: #build_app}

Utilice Maven para compilar el código fuente y ejecute la app resultante.

1. En la línea de mandatos, vaya al directorio en el que se encuentra la app de ejemplo.

  ```
cd get-started-java
  ```
  {: codeblock}

1. Utilice Maven para instalar dependencias y compilar el archivo .war.

  ```
mvn clean install
  ```
  {: codeblock}

## Paso 3: Prepare la app para el despliegue
{: #prepare}

Para desplegar en {{site.data.keyword.Bluemix_notm}}, puede resultarle útil configurar un archivo manifest.yml. El archivo manifest.yml incluye información básica sobre la app, como por ejemplo el nombre, la cantidad de memoria a asignar para cada instancia y la ruta. Encontrará un archivo manifest.yml de ejemplo en el directorio `get-started-java`.

Abra el archivo manifest.yml y cambie el valor de `name` de `GetStartedJava` por el nombre de su app, <var class="keyword varname" data-hd-keyref="app_name">app_name</var>.
{: download}

  ```
  applications:
   - name: GetStartedJava
     random-route: true
     path: target/GetStartedJava.war
     memory: 512M
     instances: 1
  ```
  {: codeblock}

En este archivo manifest.yml, **random-rout: true** genera una ruta aleatoria para la app a fin de evitar que su ruta entre en conflicto con otras.  Si lo desea, puede sustituir **random-route: true** por **host: myChosenHostName**, especificando el nombre de host que elija.
{: tip}

## Paso 4: Inicie la sesión en {{site.data.keyword.Bluemix_notm}}
{: #deploy}

1. Inicie sesión en su cuenta de {{site.data.keyword.Bluemix_notm}} y seleccione un punto final de API.

  ```
ibmcloud login
  ```
  {: codeblock}

  Si no puede iniciar sesión utilizando los mandatos `ibmcloud login` porque tiene un ID de usuario federado, utilice lo siguiente para iniciar sesión con el ID de inicio de sesión único. Consulte [Inicio de sesión con un ID federado](https://cloud.ibm.com/docs/iam?topic=iam-federated_id#federated_id) para obtener más información.

  ```
ibmcloud login --sso
  ```
  {: codeblock}

  A continuación, establezca la CLI de Cloud Foundry:

  ```	  
ibmcloud target --cf
  ```
  {: codeblock}

  Si no tiene una org o un espacio configurado, consulte [Adición de organizaciones y espacios](https://cloud.ibm.com/docs/account?topic=account-orgsspacesusers#orgsspacesusers).
  {: tip}

## Paso 5: Desarrollo mediante Eclipse
{: #eclipse}

1. Asegúrese de tener [IBM Eclipse Tools for Bluemix](https://developer.ibm.com/wasdev/downloads/#asset/tools-IBM_Eclipse_Tools_for_Bluemix).

2. Importe el ejemplo `get-started-java` en Eclipse; para ello vaya a **Archivo > Importar > Maven > Proyectos Maven existentes**.

3. Cree una definición de servidor Liberty. Los pasos siguientes descargan un nuevo servidor Liberty.
  - En la vista **Ventana > Mostrar vista > Servidores**, pulse con el botón derecho y seleccione **Nuevo > Servidor**.
  - Seleccione **IBM > WebSphere Application Server Liberty**.
  - Seleccione **Instalar desde un archivo o repositorio**.
  - Cuando se le solicite, escriba una vía de acceso de destino a una carpeta nueva (/Users/username/liberty) donde desee instalar Liberty.
  - Seleccione **Descargar e instalar un nuevo entorno de ejecución desde ibm.com**.
  - Seleccione **WAS Liberty con Java EE 7 Web Profile**.
  - Continúe el asistente con las opciones predeterminadas para finalizar.

4. Ejecute la aplicación localmente en Liberty:
  - Cambie el navegador web por el valor predeterminado del sistema; para ello vaya a **Ventana > Navegador Web > Navegador web predeterminado del sistema**.
  - Pulse con el botón derecho en el ejemplo `GetStartedJava` y seleccione **Ejecutar como > Ejecutar en servidor**.
  - Busque y seleccione el servidor Liberty localhost y pulse **Finalizar**.

  En unos segundos, la aplicación se estará ejecutando en http://localhost:9080/GetStartedJava.

5. Actualice el código:
  - Abra `src/main/webapp/index.html` y cambie la cabecera `<h1>Welcome.</h1>` to `<h1>Welcome Jane.</h1>`.
  - Guardar los cambios. Liberty tomará los cambios automáticamente.
  - Renueve el navegador (http://localhost:9080/GetStartedJava) para ver los cambios.

6. Envíe por push los cambios a {{site.data.keyword.Bluemix_notm}}:
  - En el directorio `get-started-java` en la línea de mandatos, vuelva a compilar el archivo .war.

    ```
mvn clean install
    ```
    {: codeblock}

  - Envíe por push la aplicación a {{site.data.keyword.Bluemix_notm}}.

    ```
ibmcloud cf push
    ```
    {: codeblock}

Ahora ha ejecutado el código tanto localmente como en la nube.

## Paso 6: Añada una base de datos
{: #add_database}

A continuación, añadiremos una base de datos NoSQL a esta aplicación y configuraremos la aplicación para que se pueda ejecutar localmente y en {{site.data.keyword.Bluemix_notm}}.

1. En el navegador, inicie una sesión en {{site.data.keyword.Bluemix_notm}} y vaya al panel de control. Seleccione **Crear recurso**.
2. Elija la sección **Datos y análisis** y, a continuación, seleccione **{{site.data.keyword.cloudant_short_notm}}** y cree el servicio.
3. Vaya a la vista de **Conexiones** y seleccione su aplicación y, a continuación a **Crear conexión**.
4. Seleccione **Volver a transferir** cuando se le solicite. {{site.data.keyword.Bluemix_notm}} reiniciará la aplicación y proporcionará las credenciales de base de datos para la aplicación mediante la variable de entorno `VCAP_SERVICES`. Esta variable de entorno sólo está disponible para la aplicación cuando se ejecuta en {{site.data.keyword.Bluemix_notm}}.

Las variables de entorno le permiten separar valores de despliegue del código fuente. Por ejemplo, en lugar codificar una contraseña de base de datos, puede guardarla en una variable de entorno a la que haga referencia en el código fuente.
{: tip}

## Paso 7: Utilice la base de datos
{: #use_database}
Ahora vamos a actualizar el código local para que apunte a esta base de datos. Guardaremos las credenciales correspondientes a los servicios en un archivo de propiedades. Este archivo SOLO se utilizará cuando la aplicación se ejecute localmente. Cuando se ejecute en {{site.data.keyword.Bluemix_notm}}, las credenciales se leerán de la variable de entorno `VCAP_SERVICES`.

1. En Eclipse, abra el archivo src/main/resources/cloudant.properties:

  ```
  cloudant_url=
  ```
  {: codeblock}

2. En su navegador, vaya a {{site.data.keyword.Bluemix_notm}} y seleccione **Apps > _su app_ > Conexiones > Cloudant > Ver credenciales**.

3. Copie y pegue sólo el `url` de las credenciales en el campo `url` del archivo `cloudant.properties` y guarde los cambios.

  ```
  cloudant_url=https://123456789 ... bluemix.cloudant.com
  ```
  {:codeblock}

4. Reinicie el servidor Liberty en Eclipse desde la vista `Servers`.

  Renueve la vista del navegador en http://localhost:9080/GetStartedJava/. Cualquier nombre que especifique en la app se añadirá ahora a la base de datos.

  La app local y la app {{site.data.keyword.Bluemix_notm}} comparten la base de datos. Los nombres que añada desde cualquiera de las apps aparecerán cuando renueve los navegadores.


Recuerde que si no necesita la app en directo en {{site.data.keyword.Bluemix_notm}}, debe detener la app para no incurrir en cargos inesperados.
{: tip}  
