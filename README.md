# TOPTEL-RETO-04
## Sara Rodriguez
### Topicos de Telematica - Docente Alvaro Ospina

# DEFINICIONES Y TEORIA

### Que es Kubernetes
Kubernetes es una plataforma de código abierto diseñada para automatizar, implementar, escalar y administrar aplicaciones en contenedores. Proporciona un entorno de administración de clústeres altamente adaptable que permite la gestión eficiente de aplicaciones en contenedores a lo largo de su ciclo de vida, desde el desarrollo hasta la producción.

### Características de Kubernetes:
- Orquestación de contenedores: Kubernetes permite gestionar múltiples contenedores en un entorno de clúster, facilitando la orquestación de aplicaciones complejas con varios componentes.
- Escalado automático: Puede escalar automáticamente los recursos de la aplicación en función de la demanda, lo que garantiza un rendimiento óptimo y una alta disponibilidad.
- Despliegue declarativo: Las aplicaciones se definen en archivos de configuración YAML o JSON, lo que permite un despliegue consistente y predecible.
- Auto-reparación: Kubernetes puede detectar y recuperarse automáticamente de fallos en los contenedores o nodos del clúster.
- Actualizaciones en caliente: Permite realizar actualizaciones de aplicaciones sin tiempo de inactividad mediante la implementación gradual de nuevas versiones.
- Almacenamiento y redes: Proporciona capacidades integradas para administrar almacenamiento persistente y redes de contenedores.

### Herramientas y Plataformas

- Google Kubernetes Engine (GKE): Es un servicio de Google Cloud Platform que ofrece Kubernetes como un servicio administrado en la nube.
- Amazon Elastic Kubernetes Service (EKS): Es un servicio de AWS que proporciona Kubernetes como un servicio administrado en la nube de Amazon Web Services.
- Microsoft Azure Kubernetes Service (AKS): Es un servicio de Azure que facilita la implementación, administración y operación de clústeres de Kubernetes en Azure.
- Minikube: Es una herramienta que permite ejecutar un clúster de Kubernetes en una máquina local para propósitos de desarrollo y pruebas.
- Docker Desktop: Proporciona una forma fácil de instalar y configurar Kubernetes en máquinas locales para el desarrollo de aplicaciones en contenedores.
- Kops: Es una herramienta de línea de comandos que facilita la creación, actualización y administración de clústeres de Kubernetes en la infraestructura de AWS.
- Rancher: Es una plataforma de administración de contenedores que incluye Kubernetes como una de las opciones de orquestación, proporcionando una interfaz gráfica para administrar clústeres y aplicaciones.

# INSTALACIÓN

Para correr el proyecto bajamos:

- Cliente de kubernetes
- Cliente de Azure cloud
- Cliente de Google Cloud
- Cliente de certbot


La primera versión del despliegue se realizó con Google Cloud, sin embargo por el tamaño del proyecto pedian una cuota extra de almacenamiento 3 veces superior a la gratuita, y aunque pasé mucho tiempo tratando de optimizar el proyecto no se pudo correr en esa plataforma y no pude acceder a la expansión de almacenamiento.

Luego consideré usar minikube y correrlo local, sin embargo queria poder usar el dominio que tengo comprado, asi que probé nuevamente en AWS, pero la configuración allí es muy confusa y puede ser abrumador.

Por último opté por desplegarlo en Azure Cloud, ya que aunque no tengo créditos estudiantiles en esta plataforma tiene la interfaz de kubernetes mas amigable entre las opciones vistas, y aqui si pude solicitar exitosamente la ampliacion de almacenamiento y procesamiento para correr el proyecto correctamente. 


# DESPLIEGUE

Se deben usra varios archivos de configuración con diversas funcionalidades.

- Deployment (implementación):
Descripción: Define cómo se debe implementar una aplicación o conjunto de aplicaciones en el clúster de Kubernetes.
- Service (servicio):
Descripción: Define un conjunto de pods y una política de acceso de red para acceder a ellos.
- Pod (cápsula):
Descripción: Define un conjunto de uno o más contenedores que comparten almacenamiento y una única dirección IP.
- ConfigMap (mapa de configuración):
Descripción: Define la configuración que se puede utilizar por otros recursos en el clúster.
- Secret (secreto):
Descripción: Define datos sensibles, como contraseñas, claves de API, etc., que se almacenan de forma segura en el clúster.
- Ingress:
Descripción: El Ingress es un recurso de Kubernetes que gestiona las reglas de enrutamiento del tráfico HTTP y HTTPS entrante desde fuera del clúster hacia los servicios dentro del clúster.

En kubernetes para correr nuestro proyecto se usaron los comandos: 

```
#Para correr cada archivo de configuración:
kubectl apply -f Nombre.yaml

#o recursivo:
kubectl apply -R -f .

#Describir los servicios:
kubectl get pv
kubectl get secrets
kubectl get replicasets
kubectl get pods
kubectl describe pod $YOURPOD
kubectl logs $YOURPOD


# describir un proyecto
kubectl get deployments --all-namespaces=true
kubectl get deployments --namespace <namespace-name>
kubectl describe deployment <deployment-name> --namespace <namespace-name>
kubectl logs -l <label-key>=<label-value>


#Generar un key para el SSL

openssl genrsa -out reto4.srodriguev.space.key 2048
openssl req -new -key reto4.srodriguev.space.key -out reto4.srodriguev.space.csr

openssl x509 -req -days 365 -in reto4.srodriguev.space.csr -signkey reto4.srodriguev.space.key -out reto4.srodriguev.space.crt
openssl x509 -req -days 365 -in reto4.srodriguev.space.csr -signkey reto4.srodriguev.space.key -out reto4.srodriguev.space.crt

openssl x509 -in reto4.srodriguev.space.crt -text -noout

kubectl create secret tls reto4.srodriguev.space-tls-secret --cert=/Users/makata/Documents/05_TOPICOS_TELEMATICA/RETO_04_V5/reto4.srodriguev.space.crt --key=/Users/makata/Documents/05_TOPICOS_TELEMATICA/RETO_04_V5/reto4.srodriguev.space.key



```


# CONCLUSIONES

- Deberian aumentar la capcidad de los proyectos gratuitos de Kubernetes para poder probar sus capacidades a totalidad, con el rango gratuito se pueden probar aplicaciones muy pequeñas como el proyecto guestbook que es un tutorial de kubernetes creado por ibm con una app muy sencilla y un redis, pero al tratar de meterle un service de mysql ya se superaba la cuota con creces.

- Se pudo evidenciar lo facil que es escalar el numero de replicas en kubernetes sin tener tiempos de downtime significativos

- Se pudo evidenciar el gran espectro de personalización de kubernetes para un amplio rango de proyectos, sin embargo tambien se denota que tal como se abren las posibilidades de despliegue tambien, si no se es experto en el tema, hay muchas formas de equivocarse. 


# REFERENCIAS

- WordPress on Kubernetes Cluster — Step-by-Step Guide. Desplegar Wordpress en Kubernetes. Syedusmanahmad.  Disponible en: https://engr-syedusmanahmad.medium.com/wordpress-on-kubernetes-cluster-step-by-step-guide-749cb53e27c7 

- Manually create and use a Linux NFS (Network File System) Server with Azure Kubernetes Service (AKS). Disponible en: https://learn.microsoft.com/en-us/azure/aks/azure-nfs-volume 

- Set up a custom domain name and SSL certificate with the application routing add-on. Azure DNS Integration, Disponible en: https://learn.microsoft.com/en-us/azure/aks/app-routing-dns-ssl 