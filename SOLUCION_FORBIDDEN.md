# Solución al Error "Forbidden" en API EdiCloud

## Problema Identificado
El error "Forbidden" se debe a que:
1. **Docker Desktop no está corriendo** ❌
2. **Falta configuración VirtualHost** ✅ (Ya solucionado)
3. **Falta configuración del archivo hosts** ⚠️ (Pendiente)

## Pasos para Solucionar

### 1. 🚀 Iniciar Docker Desktop

**IMPORTANTE:** Docker Desktop debe estar corriendo antes de continuar.

1. Abre **Docker Desktop** desde el menú de inicio
2. Espera a que aparezca el ícono de Docker en la bandeja del sistema
3. Verifica que muestre "Docker Desktop is running"

### 2. 🌐 Configurar archivo hosts

Necesitas agregar `api.edicloud.cl` a tu archivo hosts para que apunte a localhost.

**En Windows:**
1. Abre el **Bloc de Notas como Administrador**
2. Abre el archivo: `C:\Windows\System32\drivers\etc\hosts`
3. Agrega al final del archivo:
   ```
   127.0.0.1    api.edicloud.cl
   ```
4. Guarda el archivo

**En macOS/Linux:**
```bash
sudo nano /etc/hosts
# Agregar: 127.0.0.1    api.edicloud.cl
```

### 3. 🔄 Iniciar los servicios Docker

Una vez que Docker Desktop esté corriendo:

```bash
cd C:\Projects\docker-compose-lamp
docker-compose up -d
```

O si ya están corriendo, reinicia el webserver:
```bash
docker-compose restart webserver
```

### 4. ✅ Verificar funcionamiento

1. **Verifica que los contenedores estén corriendo:**
   ```bash
   docker-compose ps
   ```

2. **Prueba estas URLs en tu navegador:**
   - `http://localhost/` - Página principal de LAMP
   - `http://api.edicloud.cl/` - Tu API (debería mostrar JSON de información)
   - `http://api.edicloud.cl/health` - Health check de la API

## Archivos Creados

### ✅ VirtualHost Apache (`config/vhosts/api.edicloud.cl.conf`)
He creado la configuración de Apache para que:
- Resuelva `api.edicloud.cl` al directorio `/var/www/html/api.edicloud.cl/public`
- Permita acceso completo al directorio
- Configure CORS para React Native
- Maneje requests de API REST correctamente

### ✅ Guía de hosts (`hosts_update.txt`)
Archivo con instrucciones para configurar el archivo hosts del sistema.

## URLs de Testing

Una vez solucionado, podrás acceder a:

```
✅ http://api.edicloud.cl/                    - Info de la API
✅ http://api.edicloud.cl/health              - Health check
✅ http://api.edicloud.cl/status              - Status del sistema
✅ http://api.edicloud.cl/api/v1              - Endpoints de la API
✅ http://api.edicloud.cl/api/v1/auth/login   - Login endpoint
```

## Comandos de Verificación

```bash
# Verificar contenedores corriendo
docker-compose ps

# Ver logs del webserver
docker-compose logs webserver

# Reiniciar si es necesario
docker-compose restart

# Parar todo
docker-compose down

# Iniciar todo
docker-compose up -d
```

## Si Persiste el Problema

1. **Verifica permisos:**
   ```bash
   # En el contenedor, verificar permisos
   docker-compose exec webserver ls -la /var/www/html/api.edicloud.cl/
   ```

2. **Verifica configuración Apache:**
   ```bash
   # Ver configuración cargada
   docker-compose exec webserver apache2ctl -S
   ```

3. **Verifica logs de Apache:**
   ```bash
   # Ver errores de Apache
   docker-compose logs webserver
   ```

## Resumen

El problema principal era que **Docker Desktop no estaba corriendo**. Una vez que:

1. ✅ Inicies Docker Desktop
2. ✅ Configures el archivo hosts con `127.0.0.1 api.edicloud.cl`
3. ✅ Ejecutes `docker-compose up -d`

Tu API funcionará perfectamente en `http://api.edicloud.cl/`

---

**¿Necesitas ayuda adicional?** Ejecuta estos comandos y compárteme el resultado:
```bash
docker --version
docker-compose ps
ping api.edicloud.cl
```
