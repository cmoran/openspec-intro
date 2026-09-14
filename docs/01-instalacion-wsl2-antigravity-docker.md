# Guía de Instalación: WSL2, Antigravity (`agy`), OpenSpec y Docker en Windows

Esta guía paso a paso te ayudará a preparar tu entorno en Windows utilizando **WSL2 (Windows Subsystem for Linux)** para poder ejecutar herramientas de desarrollo modernas, CLIs de Inteligencia Artificial (como **Antigravity `agy`**), la herramienta de desarrollo guiado por especificaciones **OpenSpec** y contenedores con **Docker**.

---

## Índice

1. [¿Por qué WSL2 para desarrollo con IA?](#1-por-qué-wsl2-para-desarrollo-con-ia)
2. [Paso 1: Instalación y configuración de WSL2](#paso-1-instalación-y-configuración-de-wsl2)
3. [Paso 2: Instalación de Node.js y npm (vía nvm)](#paso-2-instalación-de-nodejs-y-npm-vía-nvm)
4. [Paso 3: Instalación de Antigravity CLI (`agy`)](#paso-3-instalación-de-antigravity-cli-agy)
5. [Paso 4: Instalación de OpenSpec CLI](#paso-4-instalación-de-openspec-cli)
6. [Paso 5: Instalación y configuración de Docker](#paso-5-instalación-y-configuración-de-docker)
7. [Lista de verificación final](#lista-de-verificación-final)

---

## 1. ¿Por qué WSL2 para desarrollo con IA?

Las herramientas de desarrollo asistido por IA (como Antigravity, OpenSpec, entornos de Python/Node y contenedores Docker) están optimizadas para sistemas operativos tipo UNIX/Linux.

Usar WSL2 te ofrece:
- **Compatibilidad total**: Las herramientas y librerías de terminal funcionan igual que en servidores Linux.
- **Rendimiento de archivos nativo**: Acceso ultra rápido a archivos de código dentro del sistema de archivos Linux (`/home/usuario/...`).
- **Integración fluida con Windows**: Puedes usar tu editor o IDE en Windows mientras todo se ejecuta dentro de WSL2.

---

## Paso 1: Instalación y configuración de WSL2

### Requisitos previos
- Windows 10 (versión 2004 o superior, compilación 19041 o superior) o Windows 11.
- Virtualización habilitada en la BIOS/UEFI de tu placa madre (suele estar activada por defecto; en BIOS se llama Intel VT-x o AMD-V / SVM).

### 1.1 Instalar WSL con Ubuntu
1. Abre **PowerShell** o el **Símbolo del sistema (cmd)** en Windows **como Administrador** (clic derecho -> *Ejecutar como administrador*).
2. Ejecuta el siguiente comando:

   ```powershell
   wsl --install
   ```

   *Nota: Por defecto, este comando instala la distribución Ubuntu más reciente y la versión WSL2.*

3. Si ya tenías WSL1 instalado previamente, asegúrate de que use la versión 2 por defecto:
   ```powershell
   wsl --set-default-version 2
   ```

4. **Reinicia tu equipo** cuando el instalador lo solicite.

### 1.2 Configuración inicial del usuario Linux
1. Al reiniciar, se abrirá automáticamente una ventana de terminal de Ubuntu.
2. Te pedirá crear un **nombre de usuario** (Unix username) y una **contraseña** (password).
   *(Al escribir la contraseña no se verán caracteres en pantalla; es normal por seguridad).*
3. Una vez dentro de la terminal de Linux, actualiza los paquetes base:

   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install -y curl wget git build-essential procps
   ```

> [!TIP]
> Te recomendamos instalar **Windows Terminal** desde la Microsoft Store si aún no lo tienes. Permite abrir pestañas de Ubuntu con soporte completo para colores, fuentes y atajos de teclado.

---

## Paso 2: Instalación de Node.js y npm (vía nvm)

OpenSpec requiere **Node.js (versión 18 o superior)**. La forma recomendada en Linux es usar **`nvm`** (Node Version Manager):

1. En tu terminal de Ubuntu en WSL2, ejecuta:

   ```bash
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
   ```

2. Recarga tu configuración de shell:

   ```bash
   source ~/.bashrc
   ```

3. Instala la versión LTS más reciente de Node.js:

   ```bash
   nvm install --lts
   nvm use --lts
   ```

4. Verifica la instalación:

   ```bash
   node --version   # Debería mostrar v20.x o superior
   npm --version    # Debería mostrar v10.x o superior
   ```

---

## Paso 3: Instalación de Antigravity CLI (`agy`)

**Antigravity** es el entorno de desarrollo y agente de programación avanzada con IA. Permite interactuar mediante su CLI (`agy`) y a través del IDE de Antigravity.

1. **Instalación de `agy`**:
   Sigue las instrucciones de instalación proporcionadas para tu entorno de Antigravity o descarga el binario/paquete correspondiente en tu distribución WSL2.
   
2. **Autenticación e inicio**:
   Ejecuta:
   ```bash
   agy --help
   ```
   Sigue el asistente de inicio de sesión o configuración de credenciales (usualmente vinculación con tu cuenta de Google / Gemini).

3. **Ubicación de tus proyectos**:
   > [!IMPORTANT]
   > Guarda siempre tus proyectos dentro del sistema de archivos de WSL2, por ejemplo en `~/projects/` o `/home/<tu-usuario>/proyectos/`. Evita trabajar dentro de `/mnt/c/...` porque el rendimiento de lectura/escritura y Docker es notablemente menor al cruzar los límites del sistema de archivos de Windows.

---

## Paso 4: Instalación de OpenSpec CLI

OpenSpec es el framework de desarrollo guiado por especificaciones (Spec-Driven Development / SDD) para trabajar con asistentes y agentes de IA.

1. Instala OpenSpec de forma global mediante npm:

   ```bash
   npm install -g @fission-ai/openspec
   ```

2. Verifica que el comando esté disponible:

   ```bash
   openspec --version
   ```

3. Puedes revisar los comandos disponibles ejecutando:

   ```bash
   openspec --help
   ```

---

## Paso 5: Instalación y configuración de Docker

Docker permite empaquetar y ejecutar aplicaciones, bases de datos (PostgreSQL, MySQL, Redis) y microservicios en contenedores aislados.

Existen dos alternativas recomendadas:

### Opción A: Docker Desktop en Windows (con integración WSL2)
*Ideal si prefieres una interfaz gráfica en Windows.*

1. Descarga e instala **Docker Desktop para Windows** desde su sitio oficial.
2. Durante la instalación, marca la casilla **"Use WSL 2 instead of Hyper-V"**.
3. Abre Docker Desktop en Windows y ve a:
   - **Settings (Engranaje)** -> **General** -> Verifica que esté activo *"Use the WSL 2 based engine"*.
   - **Settings** -> **Resources** -> **WSL integration**:
     - Activa *"Enable integration with my default WSL distro"*.
     - Marca la casilla de tu distribución (ej. `Ubuntu`).
4. Haz clic en **Apply & Restart**.
5. Abre tu terminal de Ubuntu en WSL2 y prueba:
   ```bash
   docker --version
   docker compose version
   ```

### Opción B: Docker Engine nativo dentro de Ubuntu WSL2
*Ideal si buscas un entorno liviano sin instalar aplicaciones pesadas en Windows.*

1. Ejecuta el script oficial de instalación en tu terminal de Ubuntu:

   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   rm get-docker.sh
   ```

2. Agrega tu usuario al grupo `docker` para no necesitar `sudo` al ejecutar comandos:

   ```bash
   sudo usermod -aG docker $USER
   newgrp docker
   ```

3. Si en WSL2 Docker no inicia como servicio systemd automáticamente, habilita systemd en WSL agregando lo siguiente a `/etc/wsl.conf`:

   ```bash
   sudo tee /etc/wsl.conf <<EOF
   [boot]
   systemd=true
   EOF
   ```
   Luego en PowerShell de Windows ejecuta `wsl --shutdown` y vuelve a abrir Ubuntu.

> [!NOTE]
> Como regla del proyecto: Recuerda que tú (el usuario) ejecutarás directamente todos los comandos relacionados con Docker (`docker compose up`, `docker build`, etc.) para compilar, reconstruir y levantar tus servicios.

---

## Lista de verificación final

Para asegurarte de que tu entorno está 100% listo, ejecuta esta comprobación en tu terminal de Ubuntu:

```bash
echo "=== Verificando herramientas ==="
echo "Node.js:   $(node -v 2>/dev/null || echo 'No instalado')"
echo "npm:       $(npm -v 2>/dev/null || echo 'No instalado')"
echo "OpenSpec:  $(openspec -V 2>/dev/null || echo 'No instalado')"
echo "Git:       $(git --version 2>/dev/null || echo 'No instalado')"
echo "Docker:    $(docker --version 2>/dev/null || echo 'No instalado')"
```

Si todos los comandos muestran su respectiva versión, ¡tu entorno está completamente configurado para empezar a trabajar con OpenSpec y Antigravity!
