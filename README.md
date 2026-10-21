# 🛡️ Implementación de un SOC Nivel 1 para ERP Pyme Fácil S.A.S. (Caso C21)

![Estado](https://img.shields.io/badge/Estado-Completado_en_un_80%25-green)
![Wazuh](https://img.shields.io/badge/SIEM-Wazuh_4.14-blue)
![Kaspersky](https://img.shields.io/badge/EDR-Kaspersky_Next-green)
![GLPI](https://img.shields.io/badge/ITSM-GLPI-orange)
![UTS](https://img.shields.io/badge/UTS-Ciberseguridad_Defensiva-darkred)

**Autores:** Juan Pérez y Juan Niño  
**Institución:** Unidades Tecnológicas de Santander (UTS)  
**Caso de Estudio:** C21 — ERP Pyme Fácil S.A.S.  

---

## 📖 Descripción del Proyecto
Este repositorio contiene la documentación, configuraciones, evidencias y scripts del diseño e implementación de un **Centro de Operaciones de Seguridad Nivel 1 (SOC N1)** para una pyme de TI ficticia dedicada a la implementación de ERP de código abierto (Odoo). 

El objetivo principal fue desplegar un laboratorio aislado (VirtualBox) para demostrar el ciclo completo de monitoreo, detección, triage y respuesta ante incidentes, integrando herramientas open-source y comerciales bajo un marco ético y legal (Ley 1273 de 2009, Ley 1581 de 2012).

### 🎯 El Problema del Caso
ERP Pyme Fácil S.A.S. publica servidores de demostración en Internet con datos reales de clientes sin enmascarar (Riesgo Crítico R-01). Además, no contaba con un SIEM, EDR centralizado ni mesa de ayuda para gestionar alertas de seguridad.

---

## 🎥 Video Demostrativo
En el siguiente enlace se muestra el funcionamiento real del laboratorio, el despliegue de los agentes, la demostración de los 3 escenarios (FIM, Fuerza Bruta y Vulnerabilidad) y el flujo de gestión de tickets:

▶️ **https://drive.google.com/file/d/1j19dtzeVlBUmIfL_XoglvbK7ylG706uW/view?usp=drive_link**
> *Nota: En los últimos minutos del video se muestra la infraestructura en VirtualBox con el servidor de Wazuh y las dos máquinas (Linux y Windows) vinculadas, junto con la gestión de tickets.*

---

## 🏗️ Arquitectura y Laboratorio
El laboratorio se desplegó en una red **Host-Only (192.168.56.0/24)** aislada de la red institucional y de Internet.

| Máquina Virtual | Rol en el Caso | SO | IP | Estado |
| :--- | :--- | :--- | :--- | :--- |
| `wz-c21pn-01` | Servidor Wazuh (Indexer, Manager, Dashboard) | Ubuntu Server 24.04 | 192.168.56.10 | ✅ Activo |
| `ub-c21pn-01` | Agente Linux (Representa SRV-STG / ERP) | Ubuntu Server 24.04 | 192.168.56.11 | ✅ Activo |
| `wc-c21pn-01` | Agente Windows (Cliente corporativo) | Windows 10 | 192.168.56.12 | ✅ Activo |

---

## 🛠️ Stack Tecnológico Implementado

*   **SIEM:** Wazuh 4.14 (Todo en uno)
    *   Monitoreo de Integridad de Archivos (FIM) en tiempo real.
    *   Evaluación de Configuración (SCA) bajo CIS Ubuntu 24.04 (Puntaje: 45%).
    *   Detección de vulnerabilidades (Módulo Vulnerabilities).
    *   **3 Reglas personalizadas:** 100210, 100211 y 100212.
    *   **Respuesta Activa:** Bloqueo automático de IP vía `firewall-drop` (182 segundos).
*   **EDR/XDR:** Kaspersky Next EDR Foundations (Diseño y configuración de políticas).
*   **ITSM:** GLPI (Diseño en la nube, categorías ITIL, SLAs y KB de 14 artículos).
*   **NGFW:** Cotización y matriz comparativa de Palo Alto, Fortinet y Ruijie.

---

## 🚨 Escenarios Demostrados (Evidencias Reales)

Durante el laboratorio se probaron con éxito los 3 escenarios del caso C21:

1.  **Escenario 1 - Vulnerabilidad Crítica (Falso Positivo):** Wazuh reportó 242 alertas críticas sobre el kernel. Se realizó triage del `CVE-2026-72422` (CVSS 9.8), determinando que era un **falso positivo justificado** porque la versión instalada ya estaba parcheada y el módulo `ksmbd` no estaba en ejecución. *(Ver Anexo D)*.
2.  **Escenario 2 - Fuerza Bruta SSH:** Se realizaron 10 intentos fallidos de SSH contra el servidor. La regla personalizada `100212` (Nivel 12) se disparó y la respuesta activa bloqueó la IP de origen (`192.168.56.1`) durante 182 segundos.
3.  **Escenario 3 - Cambio en Configuración del ERP (FIM):** Al modificar el archivo `/etc/odoo/odoo.conf`, el FIM (en modo realtime) disparó las reglas `550` y `100210` en cuestión de segundos.

---

## 📁 Estructura del Repositorio

*   `📄 Anexo D - Ticket e informe de incidente (Vulnerabilidad crítica...).pdf`: Informe detallado del triage del CVE-2026-72422.
*   `📄 Parcial_PerezJuan_NinoSebastian.pdf`: Informe técnico final (Fases 0 a 8).
*   `📊 tickets.csv`: Registro de los 8 tickets del SOC (3 reales, 5 simulados) listos para importar a GLPI.
*   `🗜️ Evidencias.zip`: Paquete comprimido con capturas de pantalla de los servidores, Wazuh, reglas y configuraciones (organizadas por fases).

---

## 📋 Estado del Proyecto por Fases

| Fase | Descripción | Estado |
| :--- | :--- | :--- |
| **Fase 0** | Alcance, ética, inventario (Anexo A) y riesgos (Anexo B) | ✅ Completado |
| **Fase 1** | Arquitectura, VLANs y flujos de logs | ✅ Completado |
| **Fase 2** | SIEM Wazuh (Agentes, FIM, SCA, Vulns, Reglas, Active Response) | ✅ Completado |
| **Fase 3** | EDR/XDR Kaspersky | 🟡 Diseño listo / Instalación pendiente |
| **Fase 4** | GLPI en la nube | 🟡 Diseño listo / Instalación pendiente |
| **Fase 5** | NGFW (Comparación técnica y TCO) | 🟡 Matriz lista / Cotizaciones pendientes |
| **Fase 6** | Procesos SOC, Playbooks y Tickets | ✅ Completado (Anexo D y tickets.csv) |
| **Fase 7** | Presupuesto, Cronograma (8 sem) y RACI | ✅ Completado |
| **Fase 8** | Documentación y Video | ✅ Completado |

---

## ⚖️ Ética y Legalidad
Todas las pruebas se realizaron en un entorno aislado (`192.168.56.0/24`) propiedad de los estudiantes. No se realizaron escaneos ni ataques a infraestructuras externas, de la universidad o de terceros. Los datos utilizados en la documentación (nombres, correos, IPs) son estrictamente ficticios, cumpliendo con la Ley 1581 de 2012 (Protección de Datos) y la Ley 1273 de 2009 (Delitos Informáticos).

> **Declaración de uso de IA:** El uso de herramientas de inteligencia artificial se limitó a la estructuración del informe, redacción de borradores y explicación de errores de instalación. Todas las pruebas técnicas, configuraciones y resultados fueron ejecutados, verificados y validados por los autores (Ver Anexo I del informe).
