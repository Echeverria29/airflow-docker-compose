# Levantar Apache Airflow con Docker Compose (Entorno Local)

Este proyecto tiene como objetivo enseñar cómo configurar y ejecutar **Apache Airflow** de forma local utilizando **Docker Compose**. El enfoque es didáctico, para que cualquier persona pueda levantar el entorno con Airflow y entender su estructura básica. Ideal para prácticas de desarrollo, pruebas o como punto de partida para proyectos ETL más complejos.

---

## Requisitos previos

### Herramientas necesarias:

- **WSL2** (si estás usando Windows)  
  [Ver video](https://www.youtube.com/watch?v=nkwvDatrKGM&ab_channel=JashTechTV)

- **Docker Desktop**  
  [Ver video](https://www.youtube.com/watch?v=jiJFDwmWrWk&ab_channel=UskoKruM2010)

- **Visual Studio Code + Python** o cualquier editor de código  
  [Ver video](https://www.youtube.com/watch?v=1E44n9NL2gw&ab_channel=OssabaTech)

- **Git** (para clonar el repositorio)  
  [Ver video](https://www.youtube.com/watch?v=wVKyeLs0hfg&ab_channel=FerDaniele)

---

## Pasos para levantar el entorno

### Paso 1: Clonar el repositorio

```bash
git clone https://github.com/Echeverria29/airflow-docker-compose
cd airflow-docker-compose
```
### Puedes desactivar los Dags de ejemplo
AIRFLOW__CORE__LOAD_EXAMPLES: 'false'

### Paso 2: Crear archivo `.env`

Este archivo define el UID de tu usuario para que los contenedores puedan trabajar con permisos correctos.

**En Linux/WSL:**

```bash
echo -e "AIRFLOW_UID=$(id -u)" > .env
```

**En otros sistemas (como Windows):**

```env
AIRFLOW_UID=50000
```

### Paso 3: Inicializar configuración de Airflow (opcional)

Esto generará un archivo `airflow.cfg` con los valores por defecto:

```bash
docker compose run airflow-cli airflow config list
```

### Paso 4: Inicializar la base de datos

Antes de ejecutar los servicios, es necesario aplicar las migraciones de la base de datos de Airflow y crear el usuario administrador:

```bash
docker compose up airflow-init
```

### Paso 5: Ejecutar Airflow

Ahora puedes iniciar todos los servicios necesarios:

```bash
docker compose up
```

Airflow estará disponible en: [http://localhost:8080](http://localhost:8080)

---

## Solución a errores de permisos en WSL/Linux

En algunos casos, los archivos o carpetas pueden haber sido creados por el usuario `root`, lo cual impide que puedas editarlos. Aquí te mostramos cómo corregirlo.

### ✅ Verificar permisos del archivo

```bash
ls -l /home/tu-usuario/airflow-docker-compose/dags/basic_dag.py
```

Si ves algo como:

```bash
-r--r--r-- 1 root root 1234 may 12 10:00 basic_dag.py
```

Significa que no tienes permisos de edición.

### ✅ Cambiar el propietario del archivo o carpeta

Para cambiar un solo archivo:

```bash
sudo chown tu-usuario:tu-usuario /home/tu-usuario/airflow-docker-compose/dags/basic_dag.py
```

Para cambiar todo el directorio del proyecto:

```bash
sudo chown -R tu-usuario:tu-usuario /home/tu-usuario/airflow-docker-compose
```

### ✅ Asegurar permisos de escritura

Para un archivo:

```bash
chmod u+w /home/tu-usuario/airflow-docker-compose/dags/basic_dag.py
```

Para toda la carpeta:

```bash
chmod -R u+rw /home/tu-usuario/airflow-docker-compose
```

---

## Limpieza del entorno

Si necesitas eliminar todo y comenzar desde cero:

```bash
docker compose down --volumes --remove-orphans
rm -rf <directorio-del-proyecto>
```

---

## Acceso por defecto

- **Usuario:** `airflow`  
- **Contraseña:** `airflow`
