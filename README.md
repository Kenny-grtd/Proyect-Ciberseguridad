# Proyect-Ciberseguridad

# Red Corporativa Descentralizada con Enfoque CID

Proyecto del curso de Ciberseguridad, Universidad del Valle Sede Pacífico.

## 1. Descripción

Diseño e implementación de una red corporativa descentralizada y segura para
una empresa agroindustrial con sede principal en Buenaventura y sedes en Cali
y Guapi (120 usuarios). Procesos críticos: facturación, correo corporativo y
portal de clientes.

Cada control está asociado a un pilar de la tríada CID
(Confidencialidad, Integridad, Disponibilidad) y se acompaña de su prueba.

## 2. Equipo

| Integrante | Pilar responsable | VMs a cargo |
|---|---|---|
| Kenny  | Confidencialidad | pfSense #1, DNS/DHCP #1, Kali, cliente |
| Ronald | Integridad | PostgreSQL x2, TrueNAS x2, Wazuh |
| Samuel | Disponibilidad | Web x2, Mailcow, Zabbix, DNS/DHCP #2 |

## 3. Arquitectura

![Topología](docs/diseno/topologia.png)

### Plan de direccionamiento (resumen)

| VLAN | Zona | Red |
|---|---|---|
| 10 | Usuarios | 10.10.10.0/24 |
| 20 | Servidores internos | 10.10.20.0/24 |
| 30 | DMZ | 10.10.30.0/24 |
| 99 | Gestión | 10.10.99.0/24 |

Detalle completo en `docs/diseno/direccionamiento.md`.
Matriz de reglas de firewall en `docs/diseno/matriz-flujos.md`.

## 4. Inventario de servicios

| Servicio | Herramienta | VM / IP | Responsable | Config |
|---|---|---|---|---|
| Firewall HA | pfSense + CARP | ... | ... | `configs/pfsense/` |
| Web HA | nginx + keepalived | ... | ... | `configs/nginx/` |
| DNS | BIND9 | ... | ... | `configs/bind/` |
| DHCP | Kea | ... | ... | `configs/kea/` |
| Correo | Mailcow | ... | ... | `configs/mailcow/` |
| Base de datos | PostgreSQL 16 | ... | ... | `configs/postgres/` |
| Monitoreo | Zabbix | ... | ... | `configs/zabbix/` |
| SIEM / FIM | Wazuh | ... | ... | `configs/wazuh/` |
| Almacenamiento | TrueNAS | ... | ... | `configs/truenas/` |

## 5. Cobertura de la tríada CID

| Pilar | Control | Estado | Prueba |
|---|---|---|---|
| C | Segmentación con default-deny | Hecho | `docs/pruebas/C01-default-deny.md` |
| C | Cifrado en tránsito y en reposo | Pendiente | `docs/pruebas/C02-cifrado.md` |
| C | VPN con MFA / admin solo por gestión | Pendiente | `docs/pruebas/C03-vpn-mfa.md` |
| I | Replicación con checksums y lag | Pendiente | `docs/pruebas/I01-replicacion.md` |
| I | FIM con alerta | Pendiente | `docs/pruebas/I02-fim.md` |
| I | Logs centralizados + NTP | Pendiente | `docs/pruebas/I03-logs-ntp.md` |
| I | Restauración con RTO/RPO | Pendiente | `docs/pruebas/I04-restauracion.md` |
| D | Web en HA | Pendiente | `docs/pruebas/D01-web-ha.md` |
| D | Failover de la BD | Pendiente | `docs/pruebas/D02-failover-bd.md` |
| D | HA del perímetro (CARP) | Pendiente | `docs/pruebas/D03-carp.md` |
| D | Monitoreo con umbrales y alertas | Pendiente | `docs/pruebas/D04-alertas.md` |

Cada archivo de prueba incluye: objetivo, comando o procedimiento,
resultado esperado, captura y resultado obtenido.

## 6. Estructura del repositorio

```
docs/       actas, diseño, análisis de riesgos, anexo de pruebas
configs/    archivos de configuración por servicio (sin credenciales)
scripts/    automatización: respaldos, failover, pruebas de demostración
```

## 7. Cómo reproducir una prueba

1. Abrir el archivo de la prueba en `docs/pruebas/`.
2. Ejecutar el procedimiento indicado (o el script en `scripts/pruebas-demo/`).
3. Comparar con el resultado esperado.

## 8. Gestión del proyecto

- Tablero Trello: <enlace>
- Cronograma y actas: `docs/actas/`
- Análisis de riesgos: `docs/riesgos/`

## 9. Seguridad del repositorio

No se almacenan contraseñas, llaves privadas ni certificados con clave.
Los secretos se gestionan fuera del repositorio. Los archivos sensibles
están en `.gitignore`.

## 10. Referencias

ISO/IEC 27001:2022, NIST CSF 2.0, CIS Controls v8.
