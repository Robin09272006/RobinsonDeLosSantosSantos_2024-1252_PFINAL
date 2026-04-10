# Diseño e Implementación de Red Segura: DMVPN, WAF y Defensa en Profundidad

**Robinson**
**Matrícula:** 2024-1252
**Asignatura:** Seguridad de Redes
**Entorno:** Laboratorio Virtual (GNS3 con Cisco IOS, FortiGate VM y Ubuntu Server)

---

## ## 1. Objetivo del Proyecto

El propósito de esta infraestructura es establecer una arquitectura de red corporativa blindada, integrando múltiples capas de seguridad para proteger la integridad y confidencialidad de los datos entre sedes.

* **`DMVPN Fase 3 con IKEv2` (Conectividad Cifrada):**
    * Establece un túnel multipunto dinámico entre el Hub y el Spoke.
    * **Objetivo:** Asegurar que todo el tráfico WAN viaje encriptado mediante IPsec (AES-256).

* **`FortiGate WAF/IPS` (Seguridad Perimetral):**
    * Actúa como barrera de Capa 7 para el servidor web.
    * **Objetivo:** Mitigar ataques de Inyección SQL y XSS, además de prevenir intrusiones y malware.

* **`Microsegmentación LAN` (Seguridad de Capa 2):**
    * Implementa políticas estrictas en el switch de acceso para dispositivos finales.
    * **Objetivo:** Prevenir movimientos laterales y asegurar la asignación legítima de direcciones IP.

---

## ## 2. Tecnologías y Protocolos Implementados

* **`IPsec / IKEv2` (Seguridad de Transporte):**
    * Implementación de perfiles criptográficos robustos vinculados a la identidad del estudiante.
    * **Configuración:** Uso de llaves pre-compartidas (PSK) y transform-sets con SHA-256.

* **`DHCP Snooping & Port-Security` (Control de Acceso):**
    * Restringe el acceso a la red basándose en direcciones MAC conocidas (Sticky).
    * **Objetivo:** Mitigar ataques de Rogue DHCP y suplantación de identidad en los puertos del switch.

* **`ACLs Extendidas` (Filtrado de Tráfico):**
    * Reglas aplicadas en las subinterfaces del router para controlar el flujo ICMP y UDP.
    * **Objetivo:** Restringir el escaneo de red (Ping) hacia la VLAN 50 y permitir solo DHCP autorizado.

---

## ## 3. Topología de Red

La arquitectura se compone de un núcleo central (Hub) que gestiona el tráfico de las VLANs de usuario, conectado a través de una nube WAN hacia un sitio remoto (Spoke) donde reside el servidor web protegido.

> **<img width="550" height="660" alt="image" src="https://github.com/user-attachments/assets/45b366db-8af5-454a-bf1b-a0983cb77266" />
**

---

## ## 4. Pruebas de Validación (PoC)

* **`Prueba de WAF` (Mitigación SQLi):**
    * Se lanza un payload malicioso `?id=1' OR '1'='1` desde el cliente Windows.
    * **Resultado:** El FortiGate detecta la firma de ataque y genera el log de bloqueo correspondiente.

* **`Verificación de Túnel` (IPSec SA):**
    * Ejecución del comando `show crypto ipsec sa` en los routers.
    * **Resultado:** Los contadores de paquetes encapsulados y cifrados aumentan, validando el túnel activo.

* **`Validación de ACL` (ICMP Drop):**
    * Intento de ping desde la VLAN 60 hacia la VLAN 50.
    * **Resultado:** El tráfico es descartado por el router, cumpliendo con la política de aislamiento.

---

