# SimulacroExamenRedes
https://github.com/csantillgar/SimulacroExamenRedes.git
## Parte I: Capa de Red

### Pregunta 1: Cálculo de Ruta Más Corta
**a)** Describe brevemente cómo funciona el Algoritmo de Dijkstra para hallar la ruta más corta en un grafo con pesos.  
1. **Comienzo**: Se asigna una distancia infinita a todos los nodos, excepto al nodo inicial, que se establece en cero.  
2. **Escoger nodo**: Se selecciona el nodo con menor distancia entre los que aún no han sido visitados.  
3. **Actualización**: Se evalúa cada vecino del nodo elegido; si pasar por ese nodo reduce la distancia, se actualiza la distancia registrada.  
4. **Nodo visitado**: El nodo elegido se marca como visitado y no será procesado de nuevo.  
5. **Iterar**: Se repiten los pasos 2-4 hasta procesar todos los nodos o encontrar el destino.

> **Complejidad**: O((V+E)·log V) al usar una cola de prioridad binaria (V = nodos, E = aristas).

**b)** Explica cómo funciona el Enrutamiento por Inundación (Flooding) y analiza sus pros y contras frente a Dijkstra.  
- **Funcionamiento**: Cada vez que un nodo recibe un paquete, lo reenvía a **todos sus vecinos**, excepto al que lo envió. Se implementan mecanismos como TTL o almacenamiento de paquetes recibidos para evitar ciclos infinitos.  
- **Ventajas**:  
  - No necesita tablas de enrutamiento ni conocimiento completo de la red.  
  - Garantiza la entrega si existe al menos un camino válido.  
- **Desventajas**:  
  - **Sobrecarga excesiva**: genera múltiples copias redundantes.  
  - **Uso ineficiente**: desperdicia ancho de banda.  
  - **No óptimo**: no asegura la mejor ruta ni el menor coste.

---

### Pregunta 2: Cálculo de Direcciones de Broadcast y Subredes
**a)** Para la red `172.29.152.0` con máscara `255.255.248.0`, encuentra la dirección de broadcast y explica el procedimiento.  
1. **Convertir la máscara**:  
   255.255.248.0 → 11111111.11111111.11111000.00000000  
2. **Identificar bits de host**: 32 – 21 = 11 bits.  
3. **Broadcast**: activar todos los bits de host a 1:  
   - 172.29. (152 → 10011000) | host bits = 11111111.111  
   - Resultado: 172.29.159.255

**b)** Para la red `172.18.26.0/23`, calcula su dirección de broadcast y explica el método.  
1. **Máscara**: 255.255.254.0 → 11111111.11111111.11111110.00000000  
2. **Tercer octeto múltiplos de 2**: valor inicial 26 → el múltiplo menor o igual = 26.  
3. **Red base**: 172.18.26.0  
4. **Broadcast**: suma 1 al rango del bloque → 172.18.27.255  
5. **Rango de hosts**:  
   - Primera IP: 172.18.26.1  
   - Última IP: 172.18.27.254

---

### Pregunta 3: Última Dirección Válida y Rango de Hosts
**a)** Con la subred `172.30.67.192` y máscara `255.255.255.192`, indica la última dirección válida.  
- Total direcciones: 2^(32–26) = 64.  
- Rango: 172.30.67.192 – 172.30.67.255  
- **Broadcast**: 172.30.67.255  
- **Última IP válida**: 172.30.67.254

**b)** Para la IP `172.22.53.199` con máscara `255.255.252.0`, encuentra el rango de direcciones válidas.  
1. **Máscara**: 255.255.252.0 → 11111111.11111111.11111100.00000000  
2. **Bloque de 4 en tercer octeto**: 53 → bloque base = 52.  
3. **Red**: 172.22.52.0  
4. **Broadcast**: 172.22.55.255  
5. **Rango de hosts**:  
   - Inicio: 172.22.52.1  
   - Final: 172.22.55.254

---

### Pregunta 4: Capacidad y Segmentación de Subredes
**a)** Calcula cuántos dispositivos pueden conectarse en la red `172.26.0.0` con máscara `255.255.255.192`.  
- Hosts = 2^(32–26) – 2 = 64 – 2 = **62 dispositivos**.

**b)** Para la IP `172.18.171.190/23`, determina la subred a la que pertenece.  
1. /23 implica bloques de 2 en el tercer octeto.  
2. 171 → múltiplo de 2 menor o igual = 170.  
3. **Subred**: 172.18.170.0/23

---

### Pregunta 5: Número de Subredes Necesarias
Explica cómo se calcula el número de subredes posibles usando:  
Nº de subredes = 2^s  
donde _s_ = número de bits que se toman prestados para subred. Si se requieren al menos 4 subredes:  
- 2^s ≥ 4 → s ≥ 2.  
- **Ejemplo**: comenzando desde /24, con s=2 → máscara /26 → se obtienen 4 subredes de 64 direcciones cada una.

---

## Parte II: Capa de Transporte

### Pregunta 6: Comparación entre TCP y UDP
**a)** Compara TCP y UDP en:  
- Requerimiento de conexión  
- Fiabilidad  
- Control de flujo/congestión  
- Velocidad

| Propiedad                | TCP                                 | UDP                    |
|-------------------------|------------------------------------|-----------------------|
| **Conexión**             | Necesita establecer conexión       | Sin conexión          |
| **Fiabilidad**           | Garantiza entrega con ACKs         | No asegura entrega     |
| **Control de flujo**     | Incluye control de flujo y congestión | No posee control      |
| **Velocidad**            | Más lento por overhead             | Más rápido por simplicidad |

**b)** Nombra dos aplicaciones donde prefieras UDP y explica por qué.  
1. **VoIP**: prioriza baja latencia sobre retransmisión.  
2. **Streaming en directo**: mantiene fluidez pese a posibles pérdidas.

---

### Pregunta 7: Establecimiento y Terminación de Conexión en TCP
Describe cómo TCP establece y finaliza una conexión.  

1. **Establecimiento (Three-Way Handshake)**  
   1. Cliente → Servidor: **SYN** con seq=x  
   2. Servidor → Cliente: **SYN-ACK** con seq=y, ack=x+1  
   3. Cliente → Servidor: **ACK** con ack=y+1  
   > Confirma sincronización de secuencias y disponibilidad de la conexión.

2. **Finalización (Four-Way Handshake)**  
   1. Emisor → Receptor: **FIN** con seq=u  
   2. Receptor → Emisor: **ACK** con ack=u+1  
   3. Receptor → Emisor: **FIN** con seq=v  
   4. Emisor → Receptor: **ACK** con ack=v+1  
   > Garantiza cierre ordenado y recepción final de datos.

---

### Pregunta 8: Multiplexación y Demultiplexación
Explica qué es la multiplexación descendente y ascendente en transporte, con ejemplos.

- **Multiplexación (descendente)**: múltiples procesos de una máquina envían datos usando distintos **puertos fuente** en TCP/UDP.  
  - **Ejemplo**: navegador abre varias conexiones simultáneas usando puertos 49152, 49153…

- **Demultiplexación (ascendente)**: el receptor identifica a qué aplicación entregar los datos según **IP y puerto destino**.  
  - **Ejemplo**: servidor HTTP recibe todas las peticiones en puerto 80 y las asigna al servidor web.

---

## Parte III: Capa de Enlace

### Pregunta 9: Direccionamiento MAC y su Papel
**a)** ¿Qué es una dirección MAC y qué papel cumple en la comunicación de capa de enlace?  
- Es un **identificador físico único** de 48 bits asignado a cada tarjeta de red, expresado en hexadecimal (ej: `00:1A:2B:3C:4D:5E`).  
- Su función es servir de dirección de destino y origen en tramas Ethernet dentro de una red local, garantizando que los datos lleguen al dispositivo correcto.

**b)** ¿Puede cambiar una dirección MAC?  
- Normalmente es fija en hardware, pero algunos sistemas operativos o herramientas permiten **"spoofing"** para cambiarla temporalmente a nivel de software.

---

### Pregunta 10: Protocolos de Acceso al Medio
**a)** Explica la diferencia entre CSMA/CD y CSMA/CA.  
| Característica | CSMA/CD (Collision Detection)   | CSMA/CA (Collision Avoidance) |
|----------------|-------------------------------|-------------------------------|
| Uso principal   | Ethernet cableado             | Redes inalámbricas (WiFi)     |
| Mecanismo       | Detecta colisiones tras transmitir | Intenta evitar colisiones antes de transmitir |
| Actuación       | Retransmite si hay colisión   | Espera turnos antes de enviar |

**b)** ¿Por qué CSMA/CD no se usa en redes inalámbricas?  
Porque en WiFi es difícil detectar colisiones debido a **interferencias y cobertura limitada**; en su lugar, CSMA/CA intenta **evitarlas antes de transmitir** mediante avisos y tiempos de espera.

---

### Pregunta 11: Switch vs Hub
**a)** Diferencia principal entre un switch y un hub en capa de enlace:  
- **Hub**: Reenvía todas las tramas a **todos los puertos**; es un dispositivo **no inteligente**.  
- **Switch**: Aprende direcciones MAC y envía tramas solo al **puerto correspondiente**; reduce colisiones y mejora eficiencia.

**b)** ¿Por qué un switch mejora el rendimiento frente a un hub?  
Porque **segmenta dominios de colisión**, evitando que todas las estaciones compitan por el mismo medio, permitiendo **transmisiones simultáneas** entre pares diferentes.

---

## Parte IV: Capa de Aplicación

### Pregunta 12: Protocolos de Aplicación
**a)** Describe las funciones principales de:  
- **HTTP**: protocolo para la transferencia de **páginas web** y recursos asociados.  
- **DNS**: sistema que traduce nombres de dominio a **direcciones IP**.  
- **SMTP**: protocolo para el **envío de correos electrónicos** entre servidores.

**b)** Menciona un protocolo que use UDP y otro que use TCP, y justifica.  
- **UDP**: DNS → porque prioriza **rapidez** sobre fiabilidad en consultas.  
- **TCP**: HTTP → necesita transmisión **fiable y ordenada** de datos.

---

### Pregunta 13: FTP y Puertos
**a)** ¿Qué es FTP y qué puertos usa?  
- Protocolo para la **transferencia de archivos** entre cliente y servidor.  
- Puertos:  
  - **21**: control de conexión  
  - **20**: transferencia de datos (modo activo)

**b)** ¿Diferencia entre modo activo y pasivo en FTP?  
- **Activo**: servidor inicia la conexión de datos hacia el cliente.  
- **Pasivo**: cliente inicia ambas conexiones (control y datos) → preferido tras firewalls/NAT.

---

### Pregunta 14: Cliente-Servidor vs P2P
**a)** Diferencia clave entre arquitecturas cliente-servidor y peer-to-peer:  
- **Cliente-servidor**: un servidor centralizado proporciona servicios a múltiples clientes.  
- **P2P**: todos los nodos actúan como **cliente y servidor simultáneamente**, compartiendo recursos entre iguales.

**b)** Ejemplo de aplicación P2P y su ventaja:  
- **BitTorrent**: permite **descargas distribuidas** al obtener partes del archivo desde múltiples fuentes, reduciendo la carga en un solo servidor.

---

### Pregunta 15: Capas y Protocolos
Relaciona cada protocolo con su capa correspondiente:

| Protocolo | Capa OSI       |
|------------|----------------|
| HTTP       | Aplicación      |
| TCP        | Transporte      |
| IP         | Red             |
| Ethernet   | Enlace de datos |
| ARP        | Enlace de datos |

### Pregunta 16: NAT y su Funcionamiento
**a)** ¿Qué es NAT y qué problema resuelve?  
- **NAT (Network Address Translation)** es un mecanismo que traduce direcciones IP privadas a una IP pública (y viceversa), permitiendo que múltiples dispositivos internos compartan una única dirección IP pública.  
- Resuelve el problema de la **escasez de direcciones IPv4**.

**b)** ¿Qué limitaciones tiene NAT?  
- Algunas aplicaciones (VoIP, juegos online) pueden fallar al esperar conexiones entrantes.  
- Complica el uso de **servidores locales accesibles desde internet**.  
- Puede **romper el principio extremo-a-extremo** de la comunicación.

---

### Pregunta 17: DHCP
**a)** ¿Qué es DHCP y cómo funciona?  
- **DHCP (Dynamic Host Configuration Protocol)** asigna direcciones IP de forma automática a dispositivos en una red.  
- Funcionamiento:  
  1. Cliente envía **DHCPDISCOVER** (broadcast).  
  2. Servidor responde con **DHCPOFFER** (IP propuesta).  
  3. Cliente responde con **DHCPREQUEST** (aceptando oferta).  
  4. Servidor confirma con **DHCPACK**.

**b)** ¿Qué ventajas aporta frente a configuración manual?  
- Configuración rápida y automática.  
- Evita conflictos de IP duplicadas.  
- Facilita administración en redes grandes.

---

### Pregunta 18: VPN
**a)** ¿Qué es una VPN y para qué se usa?  
- **VPN (Virtual Private Network)** crea un túnel cifrado entre el dispositivo y la red remota, protegiendo los datos que viajan por internet.  
- Usos:  
  - Acceso seguro a la red de una empresa.  
  - Proteger privacidad al navegar.  
  - Evadir restricciones geográficas.

**b)** Nombra un protocolo de VPN y su característica.  
- **OpenVPN**: protocolo de código abierto, usa SSL/TLS para cifrado, muy flexible y compatible con firewalls.

---

### Pregunta 19: DNS Cache Poisoning
**a)** Explica qué es el DNS Cache Poisoning.  
- Es un ataque que **inyecta registros DNS falsos** en la caché de un servidor DNS o cliente, redirigiendo solicitudes legítimas a sitios maliciosos.

**b)** ¿Cómo puede mitigarse?  
- Usando **DNSSEC** (DNS Security Extensions) para firmar respuestas DNS.  
- Configurar tiempos de expiración cortos en la caché.  
- Actualizar software DNS con parches de seguridad.

---

### Pregunta 20: IPv4 vs IPv6
**a)** Diferencias clave entre IPv4 e IPv6:

| Característica       | IPv4                         | IPv6                         |
|---------------------|-----------------------------|-----------------------------|
| Longitud dirección   | 32 bits                     | 128 bits                    |
| Formato              | Decimal (ej: 192.168.1.1)   | Hexadecimal (ej: 2001:db8::1) |
| Nº direcciones       | ~4.3 mil millones           | 3.4×10³⁸                   |
| NAT necesario        | Sí                          | No (en teoría)              |
| Configuración        | Manual/DHCP                 | Autoconfiguración (SLAAC)   |

**b)** Ventajas de IPv6:  
- Espacio de direcciones **prácticamente ilimitado**.  
- Mejora en **seguridad integrada** (IPSec).  
- Eliminación de NAT, simplificando la comunicación extremo-a-extremo.
