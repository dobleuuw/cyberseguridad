---
tags: [ctf, moc, pentesting, ot, active-directory, reversing]
---

# 🚩 CTF — Mapa de Contenido (MOC)

> Índice de los desafíos resueltos, sus técnicas y remediaciones.
> Cada entrada enlaza a su informe detallado (procedimiento + evidencia).

---

## Resumen ejecutivo

Cadena de compromiso sobre una **maqueta Smartcity (generación y distribución eléctrica)**: desde cero credenciales en la red IT, pivote hacia la red OT y manipulación del proceso industrial. En paralelo, desafíos de **reversing y mobile**.

| # | Desafío | Categoría | Valor | Flag | Técnica principal |
|---|---------|-----------|-------|------|-------------------|
| 1 | [[Caida-del-Dominio-1-5]] | Active Directory | 300 | `ciberlab{ad_recon_pwn3d}` | RBCD (delegación) |
| 2 | [[El-Puente-IT-OT-2-5]] | Linux / Pivote | 250 | `ciberlab{acl_abus3_gr4nted}` | ACL abuse (clave root legible) |
| 3 | [[La-Firma-del-Controlador-3-5]] | OT / Modbus | 500 | `ciberlab{m0dbus_d1sc0v3r_1d3nt1f13d}` | Read Device Identification |
| 4 | [[Proteccion-en-Estado-Critico-4-5]] | OT / PLC | 600 | `ciberlab{plc_r3g_pwn}` | Manipulación de holding registers |
| 5 | [[Prime-Reversing]] | Reversing | 90 | `52667` | Congruencia lineal (inverso modular) |
| 6 | [[Pass-Reversing]] | Reversing | 90 | `56026` | Checksum MD5 (fuerza bruta) |
| 7 | [[Pehuen-Movil]] | Mobile / Android | 80 | `flag{secretos-en-el-apk-8ecc7f}` | Secretos hardcodeados |
| 8 | [[Smart-Body]] | Mobile / Android | 80 | `flag{plan-full-sin-pagar-fed746}` | Broken access control (cabecera) |

**Total: 1990 puntos.**

---

## Cadena de ataque (Kill Chain IT → OT)

```
[Red IT]                                   [Puente]              [Red OT]
n.errazti (cred filtrada)                                   PLC 172.16.0.25:502
   │ ForceChangePassword                                          ▲
   ▼                                                              │ Modbus (sin auth)
f.soto ── AddAllowedToAct ──► DC01 (RBCD)                         │
   │ S4U / impersonar Administrator                               │
   ▼                                                              │
Domain Admin ──► (clave SSH monitoreo) ──► ubuntubridge ──► root ─┘
                                           (clave root legible en /tmp)
```

1. **Caída del dominio** (AD) → Domain Admin.
2. **Puente IT/OT** → root en el servidor bisagra.
3. **Firma del controlador** → ubicar e identificar el PLC.
4. **Estado crítico** → manipular el proceso físico simulado.

---

## Técnicas por categoría (TTPs)

### Active Directory
- Password/user enumeration con **kerbrute**.
- Enumeración de rutas de ataque con **BloodHound**.
- **RBCD** (Resource-Based Constrained Delegation): `impacket-addcomputer`, `impacket-rbcd`, `impacket-getST` (S4U2Self/S4U2Proxy).
- Manejo de **clock skew** de Kerberos sin privilegios.

### OT / Industrial (Modbus)
- Identificación de activos vía **análisis de tráfico ICS** (tcpdump sobre pcap).
- **Modbus** FC 0x2B/0x0E (Read Device Identification), FC 0x03 (Read HR), FC 0x06/0x10 (Write).
- Mapeo de **holding registers** a elementos físicos (setpoints, modos, interlocks).

### Reversing / Mobile
- Análisis de binarios ELF con **objdump / radare2**.
- Reconstrucción de algoritmos (congruencia modular, checksum MD5).
- Análisis estático de **APK** con androguard (secretos hardcodeados, bypass de autorización cliente).

---

## Recomendaciones de remediación

| Hallazgo | Riesgo | Remediación |
|----------|--------|-------------|
| Credencial filtrada en mensajes de error | Foothold inicial | Sanitizar errores, no exponer datos sensibles |
| RBCD abusable sobre el DC | Compromiso total del dominio | Restringir `msDS-AllowedToActOnBehalfOfOtherIdentity`, revisar ACLs, deshabilitar creación de cuentas máquina por usuarios (`ms-DS-MachineAccountQuota=0`) |
| Clave privada de root legible en `/tmp` | Escalada a root | Permisos `600`, nunca dejar claves en rutas world-readable |
| **Modbus sin autenticación** | Control total del PLC | Segmentar OT, firewall/DPI industrial, no exponer 502, monitoreo de writes anómalos |
| Secretos hardcodeados en APK | Exposición de credenciales | Nunca guardar secretos en el cliente; secret management del lado servidor |
| Control de acceso en el cliente (X-Plan) | Bypass de autorización | **Validar siempre en el servidor**, nunca confiar en cabeceras del cliente |

---

## Lecciones aprendidas

- En entornos **IT/OT**, la barrera real no es el dominio sino la **segmentación**: un solo puente mal asegurado conecta ambos mundos.
- **Modbus no tiene autenticación por diseño** → quien alcanza la red OT controla el proceso. La defensa es de red, no de protocolo.
- La mayoría de los foothold no fueron exploits, sino **malas configuraciones** (credenciales filtradas, permisos, confianza en el cliente).

---

> Metodología de referencia: [[01-Metodologia]] · Plantillas: [[00-Plantillas]]
