# Gophishand MailHog Configuration

## Requisitos previos

- **Sistema operativo:** Linux.
    
- **Conexión a Internet:** para descargar binarios.
    
- **Acceso al terminal / CLI** y **privilegios de administrador** para crear carpetas en `/opt` (opcional).
    
- `unzip` instalado si trabajas con archivos `.zip`.
    

---

## Instalación de Gophish

### Descarga y descompresión

1. Ve al repositorio oficial y descarga la última versión: `https://github.com/gophish/gophish/releases`
    
2. Crea una carpeta para Gophish:
```
sudo mkdir /opt/gophish
```

4. Copia el archivo ZIP descargado a `/opt/gophish` y descomprímelo:
```
unzip gophish-v0.8.0-linux-64bit.zip
```

### Ejecución básica

1. En el directorio donde está el binario (`/opt/gophish`), ejecuta:
```
./gophish
```

3. Al iniciarse, Gophish suele levantar la interfaz administrativa en `https://localhost:3333`. La primera vez te pedirá que cambies la contraseña por defecto.

---

## Instalación de MailHog

### Descarga y ejecución

1. Descarga el binario desde el repositorio oficial:  `https://github.com/mailhog/MailHog/releases`  
  (Descarga el binario para Linux: p.ej. `MailHog_linux_amd64`)
    
2. Crea una carpeta y coloca el binario allí:
```
sudo mkdir /opt/mailhog mv ~/Descargas/MailHog_linux_amd64 /opt/mailhog/MailHog cd /opt/mailhog chmod +x MailHog
```

4. Ejecuta MailHog:
```
./MailHog
```

### Salida esperada / puertos
En la salida verás algo parecido a:
```
Using in-memory storage
[SMTP] Binding to address: 0.0.0.0:1025
[HTTP] Binding to address: 0.0.0.0:8025
Serving under http://0.0.0.0:8025/`
```

- **SMTP (recepción):** puerto `1025` — sirve como servidor SMTP local.
    
- **Interfaz web:** puerto `8025` — panel web donde verás los correos capturados.


---

## Crear un _phishing_ básico usando MailHog + Gophish

> En este flujo se utiliza MailHog como servidor SMTP local para capturar los correos que Gophish envía durante la simulación.

### 1. Crear el _Sending Profile_ (perfil de envío)

- En el panel de Gophish, en el menú lateral elige **Sending Profiles**.
- Crea un perfil nuevo y rellena campos como:
    - **Name:** nombre descriptivo del perfil (p. ej. `MailHog-local`).
    - **From:** dirección visible en el correo (p. ej. `no-reply@laboratorio.local`).
    - **SMTP Host / Port:** `127.0.0.1:1025` o `localhost:1025` (MailHog).
    
- Usa la opción **Send Test Email** para comprobar que Gophish puede conectarse al SMTP local.

- Abre MailHog en `http://localhost:8025` y deberías ver el correo de prueba capturado.

### 2. Configurar la plantilla de correo (Email Templates)
- En Gophish selecciona **Email Templates** → **New Template**.
- Crea el HTML/texto del correo que se enviará. 
### 3. Crear la landing page (Landing Pages)
- En el menú elige **Landing Pages** → **New Landing Page**.
- Añade la página que quieres que se muestre cuando el usuario haga click en el enlace del correo.

### 4. Crear usuarios / grupos (Users & Groups)

- En **Users & Groups** crea un _Group_ con una lista de usuarios a los cuales se les mandará el email.
- Cada usuario puede tener campos personalizados (FirstName, LastName, Email, etc.) que puedes usar en plantillas.

### 5. Iniciar la campaña (Campaigns)
- Ve a **Campaigns** → **New Campaign**:
    - Selecciona el **Sending Profile** que configuraste (MailHog local).
    - Selecciona la **Email Template**.
    - Selecciona la **Landing Page**.
    - Selecciona el **Group** de destinatarios.
- Lanza la campaña.
- Comprueba MailHog (`http://localhost:8025`) para ver los correos enviados.
- Verifica en Gophish las métricas de la campaña (opens, clicks, etc.).


---
## Recursos / enlaces

- Gophish — Releases: [https://github.com/gophish/gophish/releases](https://github.com/gophish/gophish/releases)
    
- MailHog — Releases: [https://github.com/mailhog/MailHog/releases](https://github.com/mailhog/MailHog/releases)
