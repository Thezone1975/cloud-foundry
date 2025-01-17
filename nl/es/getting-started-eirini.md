---

copyright:
  years: 2017, 2018
lastupdated: "2019-04-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Iniciación a la presentación técnica de Eirini
{: #getting-started-eirini}

{{site.data.keyword.cfee_full}} (CFEE) es una plataforma en la nube para alojar apps y servicios en la nube. {{site.data.keyword.cfee_full_notm}} facilita el escalado de apps a medida que aumenta el consumo, simplificando el tiempo de ejecución, el middleware y la infraestructura para que pueda centrarse en desarrollar apps.
{: shortdesc}

Esta guía de aprendizaje de iniciación muestra cómo crear un {{site.data.keyword.cfee_full_notm}} a partir del plan de presentación técnica de Eirini, añadir usuarios, crear una organización y espacios, desplegar apps y enlazar dichas apps a servicios.
Consulte el [proyecto Eirini](https://www.cloudfoundry.org/project-eirini/) para obtener información sobre el proyecto de la incubadora Eirini de Cloud Foundry Foundation.

## Introducción a los permisos
{: #permissions}

Necesita las políticas de acceso adecuadas para poder crear instancias de {{site.data.keyword.cfee_full_notm}}. Para
obtener más información, consulte [Permisos](https://cloud.ibm.com/docs/cloud-foundry/permissions.html).

## Paso 1: Asegurarse de que la cuenta de {{site.data.keyword.Bluemix_notm}} puede crear recursos de infraestructura
{: #accountprep-environment}

Las instancias de CFEE se despliegan en nodos trabajadores de Kubernetes desde el servicio de Kubernetes.  [Prepare su cuenta de IBM Cloud](https://cloud.ibm.com/docs/cloud-foundry/prepare-account.html) para garantizar que puede crear los recursos de infraestructura necesarios para una instancia de CFEE.

## Paso 2: Crear su instancia de CFEE
{: #create-environment}

Antes de crear su CFEE, asegúrese de que está utilizando la cuenta de {{site.data.keyword.Bluemix_notm}} en la que desea crear el entorno y de que dispone de las políticas de acceso necesarias (paso 1 anterior).

1.  Abra el catálogo de {{site.data.keyword.Bluemix_notm}} [](https://cloud.ibm.com/catalog).

2.  Localice el servicio {{site.data.keyword.cfee_full_notm}} en el catálogo y púlselo para abrir la página de visión general del servicio.  La primera página contiene un resumen de las características principales del servicio. Pulse **Continuar**.

3.  Configure la instancia de CFEE que se debe crear:
    * Seleccione el plan _Presentación técnica de Eirini_.
    * Escriba un **Nombre** para la instancia de CFEE.
    * Seleccione un **Grupo de recursos** en el que se agrupa el entorno. Únicamente se listarán en el desplegable _Grupos de recursos_ los grupos de recursos a los que tiene acceso en la cuenta de IBM Cloud actual, lo que significa que necesita permisos para acceder como mínimo a un grupo de recursos en la cuenta para poder crear un CFEE.
    * Seleccione la **Geografía** y la **Ubicación** en las que debe suministrarse la instancia de servicio. Consulte la lista de [ubicaciones de suministro y centros de datos disponibles](https://cloud.ibm.com/docs/cloud-foundry/index.html#provisioning-targets){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") por geografía para los servicios de soporte y CFEE. 

4. Las instancias de CFEE se suministran en un clúster de Kubernetes. Las células de Cloud Foundry se suministran dentro de zonas de nodos trabajadores del clúster de Kubernetes. Puede configurar la ubicación, el hardware y el cifrado del clúster:
    * Seleccione el aislamiento de hardware para el clúster de Kubernetes:   
      * _Virtual compartido_: los recursos de la infraestructura del clúster, como el hipervisor y el hardware físico, se distribuyen entre el cliente y otros clientes de IBM, pero cada uno de los nodos trabajadores es de un único arrendatario.
      * _Virtual dedicado_: los nodos trabajadores del clúster se alojan en una infraestructura está dedicada a su cuenta.
    * Los datos del clúster están cifrados de forma predeterminada. Tiene la opción de desactivar la opción _Cifrar disco local_.
    * Seleccione la **Geografía** y la **Región** donde se suministrará el clúster de Kubernetes.
    * Seleccione la **Zona** en la que se desplegarán los nodos trabajadores de Kubernetes. Las VLAN disponibles se recuperan automáticamente para las zonas seleccionadas. Si no hay VLAN disponibles, se crearán automáticamente.
    A partir de CFEE v2.2.0, las instancias de CFEE pueden funcionar detrás de una **red aislada**. Puede suministrar una instancia de CFEE en una red aislada seleccionando las VLAN privadas en la red aislada. Una vez que se ha creado CFEE, las direcciones IP para las instancias de CFEE Compose para PostgreSQL y Cloudant Object Storage, así como para los equilibradores de carga de aplicaciones ([ALB](https://cloud.ibm.com/docs/containers/cs_ingress.html#private_ingress)) del clúster de Kubernetes, se deben direccionar en la red aislada por el administrador de la red para que CFEE sea operativo.
    
    Una vez que se ha creado CFEE, aparecerá el clúster de Kubernetes en el que se suministrará el entorno (como cualquier otro recurso de su cuenta de IBM Cloud) en el {{site.data.keyword.Bluemix_notm}}[panel de control](https://cloud.ibm.com/dashboard/apps/). Para obtener más información, consulte la documentación de [Servicio de Kubernetes](https://cloud.ibm.com/docs/containers/cs_why.html#cs_ov).

5.  Configure la capacidad de CFEE:
    * Seleccione el **Número de células** para CFEE. Se trata de las células de aplicación que alojarán las aplicaciones desplegadas en CFEE.  
    * Seleccione el **Tamaño de nodo**, que determina la capacidad de las células de Cloud Foundry (CPU y memoria).
    
    Además de las células de aplicación que especifique arriba, se crean y se reservan _células de plano de control_ adicionales para el funcionamiento y el control de la plataforma Cloud Foundry en CFEE. 

    **Nota:** Recomendamos que se habilite la expansión de VLAN si los nodos de trabajo del clúster de Kubernetes se suministran en subredes diferentes.  Los nodos de trabajo en distintas subredes pueden impedir la conectividad entre ellos, si la expansión de VLAN está inhabilitada.  Consulte la documentación de [expansión de VLAN](https://cloud.ibm.com/docs/containers/cs_subnets.html#vlan-spanning) para obtener más información.
    
6.  En los campos **Compose for PostgreSQL**, seleccione una de las organizaciones públicas y, a continuación, seleccione uno de los espacios disponibles en dicha organización. La instancia de Compose for PostgreSQL se suministrará en el espacio seleccionado. El servicio de Compose for PostgreSQL es una dependencia necesaria del servicio CFEE.

    **Nota:** Bajo _Compose for PostgreSQL_, solo las organizaciones de la ubicación en la que tiene la intención de suministrar la instancia CFEE (paso 4 anterior) y a las que tiene acceso están disponibles para su selección.  Dentro de una organización específica, solo los espacios a los que tiene acceso de _desarrollador_ están disponibles para la selección. 

7.  El **Resumen del pedido** en la parte derecha de la página refleja el coste de la instancia CFEE y los servicios auxiliares junto con el total mensual estimado.

8.  Pulse **Crear**, lo que abre el panel de control del entorno y muestra el estado y el progreso de creación.

**Nota:** Una vez comenzado el proceso de creación, se muestra una fila en la tabla correspondiente a CFEE en el panel de control de {{site.data.keyword.Bluemix_notm}}, en la _Lista de recursos_ y en el [panel de control de Cloud Foundry Environments](https://cloud.ibm.com/dashboard/cloudfoundry?filter=cf_environments).  El estado de la fila de la tabla indica cuándo finaliza la creación.

El proceso automatizado que crea el entorno despliega la infraestructura en un clúster de Kubernetes y la configura para que se pueda utilizar. El proceso tarda de 90 a 120 minutos.

Una vez que se haya creado CFEE, recibirá varios correos electrónicos confirmando el suministro de los servicios de CFEE y soporte, así como correos electrónicos en los que se notificará el estado de los pedidos correspondientes.

Cuando crea una instancia de CFEE, hay tres instancias de servicio de soporte adicionales que se crean en la misma cuenta de IBM Cloud. Estas instancias de servicio de soporte se llaman como la instancia de CFEE. Por lo tanto, la creación de un CFEE da lugar a la creación de un total de cuatro instancias de servicio en la cuenta de IBM Cloud:
* Instancia de CFEE ("_cfeename_").
* Clúster Kubernetes ("_cfeename_-cluster"). El clúster proporciona la infraestructura en la que se suministra la instancia de CFEE.
* Instancia de Cloud Object Storage ("_cfeename_-cos"). La instancia se utiliza para almacenar los datos generados durante la creación de los contenedores de aplicaciones CFEE (por ejemplo, paquetes de aplicaciones cargados, paquetes de compilación y ejecutables compilados).
* Instancia de Compose for PosgreSQL ("_cfeename_-postgres"). La instancia se utiliza para almacenar los datos de Cloud Foundry en la instancia de CFEE (p. ej. la auditoría del despliegue de aplicación, inicio y detención de sucesos; y el mantenimiento de registros de la pertenencia de usuarios de CFEE, organizaciones, espacios, aplicaciones y conexiones de servicio). 

## Paso 3: Crear organizaciones y espacios
{: #create-orgsandspaces}

Después de crear el {{site.data.keyword.cfee_full_notm}}, consulte [Creación de organizaciones y espacios](https://cloud.ibm.com/docs/cloud-foundry/orgs-spaces.html) para obtener información sobre cómo estructurar el entorno creando organizaciones y espacios. Las apps de {{site.data.keyword.cfee_full_notm}} abarcan espacios específicos. A su vez, el espacio existe en una organización específica. Los miembros de una organización comparten un plan de cuotas, apps, instancias de servicio y dominios personalizados.

Cuando se crea una organización, se le asigna una cuota.  La cuota fija los límites de recursos (memoria, cpu, etc.) disponible para dicha organización. Puede asignar una cuota distinta posteriormente. Hay un conjunto de cuotas preconfiguradas disponibles en cada CFEE, pero también puede crear sus propias cuotas personalizadas en la página **Cuotas** de la interfaz de usuario de CFEE.  Puede convertir una cuota personalizada en la cuota _predeterminada_ invocando la acción _Copiar en valor predeterminado_ del menú de la cuota, que copia los valores de la cuota personalizada en la cuota predeterminada.

**Nota:** La página _Organizaciones de Cloud Foundry_ disponible desde el menú **_Gestionar > Cuenta > Organizaciones de Cloud Foundry_** ubicado en la cabecera de IBM Cloud superior está destinado únicamente a organizaciones de IBM Cloud públicas, **no a organizaciones de CFEE**. Las organizaciones de CFEE se gestionan en la página _Organizaciones_ de una instancia de CFEE.  Abra la interfaz de usuario de CFEE y pulse en la página **_Organizaciones_** para crear y gestionar organizaciones para el CFEE.

## Paso 4: Añadir usuarios a organizaciones y espacios
{: #add-users}

[Añada usuarios](https://cloud.ibm.com/docs/cloud-foundry/add-users.html) a las organizaciones y espacios con asignaciones de roles específicos controlando su nivel de acceso.  Antes de poder añadir a los usuarios como miembros de organizaciones y espacios en un CFEE, los usuarios deben tener acceso previo a la instancia de CFEE. Consulte [Permisos](https://cloud.ibm.com/docs/cloud-foundry/permissions.html).

## Paso 5: Desplegar y ver las aplicaciones
{: #deploy-apps}

Cuando esté listo, puede [desplegar la app](https://console.bluemix.net/docs/cloud-foundry/deploy-apps.html) con la interfaz de línea de mandatos de {{site.data.keyword.Bluemix_notm}}.  Visualice la lista de aplicaciones desplegadas en la interfaz de usuario, en el contexto de un espacio de CFEE específico o de forma global en todas las instancias de CFEE.  También puede iniciar, detener o suprimir las aplicaciones de dichas vistas.

## Paso 6: Crear o añadir instancias de servicio de IBM Cloud a espacios de CFEE
{: #service-instances}

Las funciones [Crear servicios](https://console.bluemix.net/docs/cloud-foundry/add-serv-inst.html#workingwith-services#creating-services-inspace) o [Añadir servicios existentes](https://console.bluemix.net/docs/cloud-foundry/add-serv-inst.html#workingwith-services#adding-services-inspace) se encuentran disponibles en la cuenta de IBM Cloud.  Cuando se encuentre disponible una instancia de servicio en un espacio CFEE, puede enlazarla a las aplicaciones CFEE desplegadas en dicho espacio.

## Paso 7: Enlazar aplicaciones a instancias de servicio
{: #bind-apps}

[Enlace su app](https://console.bluemix.net/docs/cloud-foundry/binding.html) al alias de una instancia de servicio para poder utilizar las funciones del servicio.

En el [panel de control de Cloud Foundry](https://console.bluemix.net/dashboard/cloudfoundry/overview){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") tendrá una vista consolidada de todas las aplicaciones y servicios, tanto *públicos* como de *empresa*.  Una vez en el panel de control de Cloud Foundry, pulse *Público* en el panel de navegación de la izquierda para ver las aplicaciones y servicios públicos disponibles en la cuenta de IBM Cloud.  Pulse en *Enterprise* para ver todos los entornos de CFEE, aplicaciones desplegadas en espacios de CFEE y servicios disponibles en espacios de CFEE.
{:tip}

## Paso 8: Instalar Stratos Console para gestionar aplicaciones (opcional)
{: #install-stratos}

Stratos Console es una herramienta basada en web de código abierto para trabajar con Cloud Foundry. La aplicación Stratos Console se puede instalar de forma opcional y utilizarse en un entorno de CFEE específico para gestionar sus organizaciones, espacios y aplicaciones.

Los usuarios con roles de administrador o editor de IBM Cloud en la instancia de CFEE pueden instalar la aplicación Stratos Console en dicha instancia de CFEE.

Para instalar la aplicación Stratos Console:

1. Abra la instancia de CFEE en la que desea instalar Stratos Console.
2. Pulse **Instalar Stratos Console** en la página de visión general. El botón está visible únicamente para los usuarios con permisos de administrador o editor en la instancia de CFEE.
3. En el diálogo Instalar Stratos Console, seleccione una opción de instalación. Puede instalar la aplicación Stratos en una célula de Cloud Foundry o en el clúster Kubernetes. Seleccione una versión de Stratos Console y el número de instancias de la aplicación a instalar. Si instala la app Stratos Console en una célula de Cloud Foundry, se le solicitará que proporcione una organización y espacio en los que desplegar la aplicación.
4. Pulse **Instalar**.

La instalación de la aplicación tarda 5 minutos aproximadamente. Una vez completada la instalación, aparecerá el botón de **Stratos Console** en lugar del botón _Instalar Stratos Console_ en la página de visión general. El botón _Stratos Console_ inicia la consola y está disponible para todos los usuarios con acceso a la instancia de CFEE. Es posible que las asignaciones de pertenencia a un espacio y organización limiten lo que un usuario puede ver y gestionar en Stratos Console.

Para iniciar Stratos Console:

1. Abra la instancia de CFEE en la que se ha instalado Stratos Console.
2. Pulse **Stratos Console** en la página de visión general.
3. Stratos Console se abre en un separador del navegador distinto. Cuando abre la consola por primera vez, se le solicita que acepte dos avisos consecutivos debido a certificados autofirmados.
4. Pulse **Iniciar sesión** para abrir la consola. No se necesitan credenciales puesto que la aplicación utiliza sus credenciales de {{site.data.keyword.Bluemix_notm}}.


## Recursos adicionales
{: #additional-resources}

Puede realizar algunas tareas de administración en CFEE con los mandatos de CLI `ibmcloud CFEE`. Los mandatos le permiten obtener información sobre una instancia de CFEE, así como gestionar sus organizaciones y espacios. Revise la [Consulta de mandatos de CFEE de CLI de IBM Cloud](https://console.cloud.ibm.com/docs/cli/reference/ibmcloud/cli_cfee.html#ibmcloud_commands_cfee){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").

Encontrará vídeos y explicaciones y demostraciones exhaustivas sobre diversos temas de CFEE en la [lista de reproducciones de vídeos de CFEE](https://ibm.biz/CFEE-Playlist){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").

Encontrará información sobre las API de CFEE en la [documentación de API](https://cloud.ibm.com/apidocs/cfaas){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").
