---

copyright:
  years: 2015, 2018
lastupdated: "2017-10-26"
subcollection: "liberty"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Valores predeterminados de paquete de compilación
{: #buildpack_defauts}

El paquete de compilación Liberty se actualiza con frecuencia en {{site.data.keyword.Bluemix}}. Cada release podría incluir arreglos de seguridad o mejoras de características.

El paquete de compilación tiene valores predeterminados para el establecimiento de valores,
como la versión de Java o el conjunto de características de Liberty para aplicaciones WAR o EAR. Algunos de los valores predeterminados podrían cambiar entre releases del paquete de compilación,
que pueden afectar negativamente a la aplicación. Para evitar que la aplicación se vea afectada
por el cambio de los valores predeterminados del paquete de compilación, realice acciones para
configurar su aplicación de forma que evite basarse en los valores predeterminados del paquete
de compilación.

## Versiones de Liberty
{: #liberty_versions}

El paquete de compilación proporciona dos versiones del tiempo de ejecución de Liberty:
1. Un release estable de largo plazo
  * Es el tiempo de ejecución de Liberty predeterminado.
  * No proporciona ninguna [función beta](/docs/runtimes/liberty/usingBetaFeatures.html).
  * Normalmente se actualiza cada trimestre.

2. El release mensual
  * Debe habilitarse de forma explícita estableciendo la variable de entorno **JBP_CONFIG_LIBERTY** con el valor **"version: +"** y
 la variable de entorno **IBM_LIBERTY_MONTHLY** con el valor **true**.
  * Proporciona [características mensuales](/docs/runtimes/liberty/usingMonthlyRuntime.html).
  * Normalmente se actualiza cada 4 semanas.

## Características de Liberty
{: #liberty_features}

Cuando despliega archivos WAR o EAR, el paquete de compilación
proporciona una configuración para la aplicación con un conjunto predeterminado de características de Liberty. Aunque no es frecuente,
dicho conjunto predeterminado de características de Liberty podría cambiar entre releases del paquete de
compilación. El cambio en el conjunto de características predeterminado podría afectar negativamente a la
aplicación. Hay opciones para asegurarse de que la aplicación no se vea afectada por los cambios en los
valores predeterminados de la característica.

* Establezca la variable de entorno JBP_CONFIG_LIBERTY de forma que especifique de forma explícita una
lista de funciones habilitadas para la aplicación. Para obtener más información, consulte el tema sobre [Aplicaciones autónomas](/docs/runtimes/liberty/optionsForPushing.html#stand_alone_apps).
* Despliegue su aplicación como [directorio de servidor](/docs/runtimes/liberty/optionsForPushing.html#server_directory) o como [servidor empaquetado](/docs/runtimes/liberty/optionsForPushing.html#packaged_server). Proporcione un archivo server.xml personalizado que especifique
el conjunto exacto de características que su aplicación necesita.

Las aplicaciones que se despliegan
como directorio de servidor o como un servidor empaquetado no se ven afectadas por el cambio en los
valores predeterminados de las características de Liberty.

## Versión de Java
{: #java_version}

El paquete de compilación proporciona un JRE predeterminado
para la aplicación. La versión principal o secundaria del JRE podría cambiar entre releases del paquete de compilación. La versión
secundaria del JRE podría actualizarse con frecuencia, cosa que no sucede con la versión principal. El cambio
en una versión principal del JRE podría afectar negativamente a la aplicación.

Para asegurarse de que la aplicación no se vea afectada por el cambio de versión principal, establezca la variable de entorno con la versión JRE adecuada según se describe en [Personalización del JRE](/docs/runtimes/liberty/customizingJRE.html). Se obtendrán los mejores resultados si usa Java 8 en sus aplicaciones.
