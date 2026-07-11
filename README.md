# Tienda de Perritos - Despliegue Automatizado en Amazon EKS

Este repositorio contiene la arquitectura de microservicios, los manifiestos de Kubernetes (`k8s/`) y el pipeline de CI/CD para desplegar la aplicación **"Tienda de Perritos"** en un clúster de Amazon EKS (Elastic Kubernetes Service).

La aplicación está compuesta por tres capas independientes:
*   **Frontend:** Interfaz de usuario web.
*   **Backend:** Lógica de negocio y API REST.
*   **Database:** Motor de base de datos relacional MySQL.

---

## 🚀 Arquitectura del Pipeline CI/CD

El flujo de trabajo automatizado (`.github/workflows/deploy-eks.yaml`) realiza las siguientes tareas de forma automática ante cada cambio en la rama principal:

1. **Autenticación:** Conexión segura a AWS mediante credenciales cifradas.
2. **Build & Tag:** Compilación de imágenes Docker locales para las tres capas y etiquetado con el hash del commit de Git (ej. `efed00e`).
3. **Push:** Publicación de las imágenes en los repositorios privados de **Amazon ECR**.
4. **Deploy:** Actualización y aplicación de los manifiestos en el clúster **Amazon EKS** bajo el namespace dedicado `tienda`.

---

## 🛠️ Prerrequisitos de Configuración

Para poner en marcha este repositorio o replicar el entorno, asegúrate de contar con:

### 1. Secretos en GitHub
Para que el workflow de GitHub Actions pueda interactuar con tu infraestructura de AWS, debes registrar los siguientes **Repository Secrets** (`Settings > Secrets and variables > Actions`):

| Secreto | Descripción |
|---|---|
| `AWS_ACCESS_KEY_ID` | ID de la llave de acceso de tu usuario IAM con permisos para EKS/ECR. |
| `AWS_SECRET_ACCESS_KEY` | Llave de acceso secreta de AWS. |
| `AWS_REGION` | Región de AWS donde reside el clúster (ej. `us-east-1`). |

### 2. Estructura de Manifiestos (`k8s/`)
Asegúrate de que las rutas de las imágenes en tus archivos YAML de despliegue (`backend-deployment.yaml`, `frontend-deployment.yaml`, `mysql-deployment.yaml`) apunten a tu ID de cuenta correcto de AWS:

```yaml
image: <TU_ID_DE_CUENTA_AWS>.dkr.ecr.<TU_REGION>[.amazonaws.com/tienda-frontend:latest](https://.amazonaws.com/tienda-frontend:latest)
