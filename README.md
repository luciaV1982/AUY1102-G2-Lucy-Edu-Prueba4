![image alt](https://github.com/edoturb/AUY1102-001V-2025--G2/blob/main/Evidencias/logo_duoc.png?raw=true)

<br clear="left"/>

# Evaluación Parcial 3  
## CI/CD con Docker Hub · SonarCloud · Snyk  

**Asignatura:** Ciclo de Vida del Software I – Sección 001V  
**Integrante:** Lucía Villalobos Ospina  Eduardo Urbina
**Docente:** Valentina Paz  
**Fecha de entrega:8 Diciembre 2025  

---

## 1. Objetivo de la evaluación

El objetivo de esta evaluación es implementar un pipeline **CI/CD completo** aplicando herramientas del ciclo de vida del software orientadas a **automatización, calidad, despliegue y seguridad**.

El proyecto integra los siguientes componentes:

- Versionamiento con **Git y GitHub** usando **GitFlow**
- Automatización de pruebas con **GitHub Actions**
- Construcción de imagen Docker y publicación en **Docker Hub**
- Análisis estático de seguridad con **Snyk**
- Análisis de calidad y mantenibilidad con **SonarCloud**
- Análisis de vulnerabilidades en contenedores con **Docker Scout**

Este flujo garantiza un desarrollo continuo con **detección temprana de errores** y **validación automática** antes del despliegue.

---

## 2. Fase 1 – Control de versiones con Git y GitHub (GitFlow)

### 2.1. Fork y clonación del repositorio

Se realizó un **fork** del repositorio base de la evaluación, permitiendo trabajar sobre una copia propia.

- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E01-Fork-creado.png?raw=true)

Luego se clonó la copia a la máquina local:

git clone https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3.git

- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E02-Clonar-Repo.png?raw=true)

```



```
2.2. Creación de la rama develop
Como buena práctica de GitFlow, se creó una rama de desarrollo aislando cambios de la rama principal (main):


git checkout -b develop
git push -u origin develop


Creación local de develop

- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E03-Rama-Develop-Creada.png?raw=true)

Rama develop publicada en GitHub

- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E04-Rama-Develop-Subida-a-GitHub.png?raw=true)


```
2.3. Organización inicial del proyecto

Se crearon las carpetas base del proyecto:

```
mkdir src
mkdir tests

- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E05-Carpetas-src-y-tests-creadas.png?raw=true)

```
En src/ se creó el archivo principal app.py con las funciones base a utilizar en CI/CD:

```
- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E06-Archivo-app-creado.png?raw=true)


### 2.4. Creación de archivos iniciales del proyecto
Se crearon los archivos base necesarios para iniciar la construcción del pipeline de CI/CD con Docker y Pytest.

---

####  2.4.1. Archivo de pruebas `test_app.py`

Este archivo incluye pruebas unitarias para validar las funciones principales del proyecto: sumar, restar, multiplicar, dividir y manejo de errores.

```python
from src.app import sumar, restar, multiplicar, dividir
import pytest

def test_sumar():
    assert sumar(2, 3) == 5

def test_restar():
    assert restar(5, 2) == 3

def test_multiplicar():
    assert multiplicar(4, 3) == 12

def test_dividir():
    assert dividir(10, 2) == 5

def test_dividir_por_cero():
    try:
        dividir(10, 0)
        assert False  # Si llega aquí, la prueba debe fallar
    except ValueError:
        assert True

```
- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E07-Tests-creados.png?raw=true )

####  2.4.2. Archivo requirements.txt


- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E08-requirements-creado.png?raw=true)


```

---

#### 2.4.3. Archivo Dockerfile


Se configuró el Dockerfile para:
✔ Preparar el entorno de ejecución

✔ Ejecutar automáticamente las pruebas al construir la imagen

✔ Ejecutar el código principal al iniciar el contenedor


# Imagen base oficial de Python
FROM python:3.11-slim

# Directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar dependencias del proyecto
COPY requirements.txt .

# Instalar dependencias
RUN pip install --no-cache-dir -r requirements.txt

# Copiar todo el código del proyecto
COPY . .

# Ejecutar pruebas en la construcción de la imagen
RUN pytest -q

# Comando por defecto al iniciar el contenedor
CMD ["python", "src/app.py"]


```
- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E09-Dockerfile-creado.png?raw=true)

Creamos un Dockerfile que instala dependencias, copia el código y ejecuta las pruebas unitarias en el proceso de construcción para asegurar calidad antes del despliegue.


---

### 2.5 Ejecución de pruebas unitarias con Pytest

Se instalaron las dependencias definidas en requirements.txt:

```bash
pip install -r requirements.txt

```
- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E10.png?raw=true)


Luego se ejecutaron las pruebas unitarias:


pytest -q
```

```
- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E11.png?raw=true)

Tras corregir la estructura del proyecto, las pruebas se ejecutaron exitosamente:


- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E12%20-Pytest%20ejecutado%20correctamente.png?raw=true)



### 3. Ejecución inicial del Workflow CI/CD

Una vez configurado el archivo del workflow en GitHub Actions, se realizó el primer push desde la rama `develop`, generando la primera ejecución automatizada del pipeline.

---


- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E13-primera%20ejecuci%C3%B3n%20del%20pipeline%20fallo.png?raw=true)

En esta primera ejecución, el pipeline falló debido a que aún no se encontraba configurada la autenticación con Docker Hub para la publicación de la imagen Docker.


---

### 3.1 Creación de token de Docker Hub

Para permitir que GitHub Actions pueda autenticarse remotamente y publicar nuestra imagen Docker en Docker Hub, se creó un **Personal Access Token (PAT)** con permisos de lectura y escritura.

- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E14.png?raw=true)

Este token luego se configuró como secreto en GitHub (`DOCKERHUB_TOKEN`), y el nombre de usuario (`DOCKERHUB_USERNAME`) también se registró como variable segura.


---

### 3.2 Workflow CI/CD funcional

Posteriormente, se actualizó el archivo `ci-cd.yml` para incluir correctamente las credenciales seguras, habilitando la construcción y despliegue continuo del proyecto.

- ![A](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E15-workflow-ci-cd-creado.png?raw=true)

```

Con estas configuraciones aplicadas, el workflow quedó registrado y listo para ejecutar el proceso completo de CI/CD de forma automática en cada push o pull request.

```

### 3.3 Evidencia de configuración y autenticación con Docker Hub

Después de crear el token y configurar las credenciales como secretos de GitHub, se realizó un commit y push para ejecutar nuevamente el workflow de CI/CD.

📌 A continuación, se muestran las evidencias del proceso:

---

#### 📌 Commit y push del workflow actualizado

Se observa la ejecución del comando `git add .`, el commit con el mensaje correspondiente y el `git push` hacia el repositorio remoto en GitHub.


- ![Evidencia Commit & Push](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E16-Commit%20+%20Push%20a%20GitHub.png?raw=true)

---

#### 🔐 Registro del nombre de usuario como secreto seguro

Se configuró el secreto `DOCKERHUB_USERNAME` para la autenticación en Docker Hub desde GitHub Actions.


- ![Secreto Docker Hub Username](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E17.png?raw=true)

```
---

#### 🔐 Registro del token del Docker Hub como secreto final

```
Se agregó el `DOCKERHUB_TOKEN`, completando la autenticación segura necesaria para que el pipeline pueda publicar imágenes en Docker Hub.

- ![Secreto Docker Hub Token](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E17b%20Tokens.png?raw=true)

```
---

Estas configuraciones permiten que GitHub Actions pueda autenticarse correctamente contra Docker Hub durante la construcción y despliegue continuo de la imagen Docker del proyecto.


### 3.4 Ejecución del Workflow desde GitHub Actions

Una vez configuradas las credenciales y actualizado el archivo `ci-cd.yml`, se ejecutó el workflow de CI/CD desde la sección **Actions** del repositorio.

A continuación se muestran las evidencias del proceso:

---

#### ▶️ Revisión inicial del estado del workflow

Se visualiza el flujo de trabajo con el estado de ejecución correspondiente al commit que habilitó la integración continua.

```
- ![Ejecución CI/CD lista para re-ejecutar](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E17C.png?raw=true)

---

#### 🔁 Re-ejecución del workflow manualmente

Se seleccionó la opción **Re-run jobs** para volver a ejecutar el pipeline y validar que la configuración fuera correcta.


- ![Re-run del workflow](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E17d.png?raw=true)

---

#### 🏗️ Workflow en ejecución: construcción, prueba y despliegue

Se observa el proceso automático ejecutándose correctamente en GitHub Actions, siguiendo el archivo `ci-cd.yml`.

- ![Workflow en curso](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E17e.png?raw=true)

---

Estos pasos confirman la integración correcta del repositorio con GitHub Actions y el despliegue continuo automatizado basado en cada cambio realizado en la rama `develop`.


### 3.5 Validación final del Workflow CI/CD y despliegue de imagen Docker

Una vez corregida la configuración de credenciales en GitHub Actions, se volvió a ejecutar el workflow logrando un resultado exitoso. Esto confirma que el CI/CD está funcionando correctamente y publicando la imagen Docker en Docker Hub.

📌 Evidencias del resultado:

---

#### 🟢 Ejecución exitosa del workflow

Se observa que el pipeline completó correctamente todas las etapas: construcción, pruebas y despliegue, sin errores.

- ![Workflow exitoso en GitHub Actions](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E17f.png?raw=true)

---

#### 📌 Historial de ejecuciones indicando estado exitoso

El registro de acciones muestra la ejecución con estado **Éxito** en la rama `desarrollar`.

- ![Registro exitoso del workflow](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E17g.png?raw=true)

---

#### 🐳 Imagen publicada correctamente en Docker Hub

Se confirma que la imagen fue enviada al repositorio `lucia1982/auy1102-prueba3` en Docker Hub.

- ![Repositorio de Docker Hub con la imagen subida](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E18.png?raw=true)

---

Gracias a esta integración, cada cambio en el repositorio puede generar automáticamente una nueva versión de la imagen Docker, garantizando un flujo de entrega continuo y controlado.


#### 🔎 Visualización detallada del repositorio de Docker Hub

Se evidencia que la imagen fue correctamente empujada al repositorio `lucia1982/auy1102-prueba3`, mostrando la etiqueta generada por el pipeline, el sistema operativo base y la confirmación del push realizado hace pocos minutos.

- ![Vista del repositorio en Docker Hub](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E18a.png?raw=true)

Esta vista confirma la disponibilidad de la imagen para su despliegue o ejecución en cualquier entorno Docker.



#### 🟢 Ejecución exitosa del workflow CI/CD

Se confirma que el pipeline completó correctamente todas las etapas: construcción de la imagen Docker, pruebas y despliegue.

- ![Evidencia – Docker Build Exitoso](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E18b.png?raw=true)

Esta evidencia demuestra que la imagen Docker fue construida localmente sin errores.
---

#### 🚀 Pipeline exitoso en GitHub Actions

El historial de ejecuciones indica que el workflow finalizó con estado **Éxito** en la rama `desarrollar`.

- ![Evidencia – Pipeline Exitoso GitHub Actions](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E19.png?raw=true)

Esta ejecución valida la correcta automatización del proceso CI/CD.


#### 🐳 Validación local de la imagen Docker ejecutada desde Docker Hub

Se prueba la ejecución del contenedor desplegando la aplicación localmente en el puerto `5000`, usando la imagen publicada en Docker Hub.

- ![Ejecución del contenedor en modo detached](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E20-2_run_detached.png?raw=true)

Esta salida confirma que el contenedor se encuentra corriendo exitosamente en segundo plano.
---

#### 📌 Verificación del funcionamiento de Docker en el sistema

Se valida el correcto funcionamiento del motor Docker ejecutando la imagen de prueba `hello-world`.

- ![Hello World Docker funcionando](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E20-hello-world-ok.png?raw=true)

Se confirma que la instalación de Docker está operativa y lista para manejo de contenedores.
---

#### 🔄 Descarga de la imagen desde Docker Hub

La imagen `luciaV1982/auy1102-prueba3` es descargada exitosamente desde el repositorio remoto.

- ![Docker Pull desde Docker Hub](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E20-Parte_1.png?raw=true)

Esto valida que la imagen se encuentra accesible públicamente para despliegues en cualquier entorno.


#### 📦 Verificación de contenedores creados localmente

Se listan los contenedores existentes en el entorno Docker para validar que la imagen ejecutada se encuentra registrada correctamente en el sistema.

- ![Listado de contenedores creados](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E21_container_created.png?raw=true)

Esta visualización confirma que el contenedor correspondiente a la imagen del proyecto se ejecutó de forma satisfactoria.
---

#### 🧪 Integración del proyecto con SonarCloud para análisis de calidad

Se configura SonarCloud con el repositorio del proyecto alojado en GitHub con el objetivo de analizar la calidad del código fuente.

- ![Acceso al panel principal del proyecto en SonarCloud](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E21b.png?raw=true)

Este panel permitirá visualizar métricas de seguridad, fiabilidad y mantenibilidad del código.
---

#### 🔗 Importación del repositorio de GitHub hacia SonarCloud

Se selecciona e importa el repositorio del proyecto para habilitar los análisis automáticos tras cada ejecución del pipeline CI/CD.

- ![Importación del repositorio desde GitHub hacia SonarCloud](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E21-Proyecto%20importado%20desde%20GitHub%20a%20SonarCloud.png?raw=true)

Esto habilitará la generación de reportes de calidad del código en cada push hacia GitHub.




#### 📋 Revisión de logs del contenedor

Se verifican los registros del contenedor Docker para comprobar que no existan errores de ejecución.

- ![Logs del contenedor sin errores](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E22_logs_vacios.png?raw=true)

Esto confirma que el contenedor finalizó correctamente sin fallas.
---

#### 🛠 Configuración inicial del proyecto en SonarCloud

Desde la sección **Información**, se pueden consultar los perfiles de calidad aplicados y configuraciones del análisis.

- ![Panel de información del proyecto en SonarCloud](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E23b.png?raw=true)

Aquí se validan los estándares de análisis aplicados al código.
---

#### 🏷 Configuración de insignias y parámetros del análisis

Se accede a la vista donde se definen los parámetros del proyecto, clave, organización e insignias para su publicación.

- ![Configuración de Insignias y parámetros de proyecto](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E23c.png?raw=true)

Esto permite vincular el estado de calidad del código con el README.
---

#### 🔐 Generación de token de seguridad en SonarCloud

Se genera un token personalizado para permitir que GitHub Actions ejecute el análisis automatizado del código.

- ![Generación de token SONAR_TOKEN](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E24_token_sonar1.png?raw=true)

Este token se usará como credencial segura en el pipeline.
---

#### 🗝️ Agregado de secretos de SonarCloud en GitHub

Los valores `SONAR_ORG`, `SONAR_PROJECT_KEY` y `SONAR_TOKEN` se almacenan como secretos de seguridad en GitHub.

- ![Secretos de SonarCloud configurados en GitHub](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E24_tokens_sonar.png?raw=true)

Estos secretos permiten la integración segura entre SonarCloud y CI/CD sin exponer credenciales.


#### 🔐 Configuración del token de seguridad para Snyk

Se agrega el token `SNYK_TOKEN` como secreto dentro del repositorio para habilitar
el análisis automatizado de vulnerabilidades en el pipeline CI/CD.

- ![Secreto SNYK_TOKEN agregado en GitHub](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E25_tokensnyk.png?raw=true)

Esto garantiza la comunicación segura entre GitHub Actions y Snyk Security Scan.
---

#### 🛡️ Conexión del proyecto con Snyk Security

El repositorio del proyecto es importado correctamente en Snyk desde GitHub,
permitiendo ejecutar análisis de vulnerabilidades en dependencias y código base.

- ![Proyecto importado y conectado con Snyk](https://github.com/luciaV1982/AUY1102-G2-Lucy-Edu-Prueba3/blob/main/Evidencias-Prueba3-Lucy-Edu/E25snyk.png?raw=true)

Así se habilita la protección del ciclo de vida del software mediante seguridad continua.
---

#### 🔁 Merge a rama main con CI/CD completamente funcional

Se realiza la integración de la rama `develop` hacia `main` una vez finalizada
la implementación completa del pipeline CI/CD.

- ![Pull Request mergeado con éxito]()

Este merge consolida la entrega final, integrando:
✔ Build & Push de imagen Docker  
✔ Análisis con Snyk Security Scan  
✔ Análisis de calidad con SonarCloud  
✔ Gestión de secretos segura  
✔ Flujo optimizado de integración continua






📘 Repositorio oficial del grupo:
https://github.com/edoturb/AUY1102-001V-2025--G2


