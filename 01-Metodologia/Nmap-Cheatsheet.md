# Nmap — Cheatsheet

> Metodología rápida de escaneo. ⚠️ Solo contra objetivos autorizados.

## Flujo recomendado
```bash
# 1) Descubrir hosts vivos
sudo nmap -sn 192.168.1.0/24

# 2) Todos los puertos TCP, rápido
sudo nmap -p- --min-rate 5000 -T4 -n -Pn <IP> -oN puertos.txt

# 3) Detalle de los puertos abiertos encontrados
sudo nmap -sC -sV -p 22,80,443 <IP> -oN detalle.txt

# 4) UDP (lento), top 20
sudo nmap -sU --top-ports 20 <IP>
```

## Tipos de escaneo
| Flag | Qué hace |
|---|---|
| `-sS` | SYN scan (sigiloso, default con sudo) |
| `-sT` | TCP connect (default sin sudo) |
| `-sU` | UDP |
| `-sn` | Ping sweep (solo descubre hosts) |
| `-sA` | ACK (detecta reglas de firewall) |

## Detección
| Flag | Qué hace |
|---|---|
| `-sV` | Versión de servicios |
| `-O` | Sistema operativo |
| `-sC` | Scripts NSE por defecto |
| `-A` | Agresivo (`-sV -O -sC --traceroute`) |

## Puertos
| Flag | Qué hace |
|---|---|
| `-p 80` / `-p 1-1000` / `-p 22,80,443` | Puertos específicos |
| `-p-` | Los 65535 |
| `-F` | Top 100 (rápido) |
| `--top-ports 1000` | Top N |

## Velocidad y evasión
| Flag | Qué hace |
|---|---|
| `-T0..-T5` | Timing (3 normal, 4 agresivo) |
| `--min-rate 5000` | Mínimo paquetes/seg |
| `-Pn` | No hacer ping (host asumido vivo) |
| `-n` | No resolver DNS |
| `-f` | Fragmentar paquetes |
| `-D RND:5` | Señuelos (decoys) |

## Salida
| Flag | Formato |
|---|---|
| `-oN` | Normal |
| `-oX` | XML |
| `-oG` | Grepeable |
| `-oA` | Los tres |

## Estados de puertos
- **open**: servicio escuchando.
- **closed**: accesible pero nada escuchando.
- **filtered**: firewall bloquea, sin respuesta.
- **open|filtered**: no se puede distinguir (común en UDP).

## NSE
```bash
nmap --script vuln <IP>                 # vulnerabilidades conocidas
nmap --script http-enum -p 80 <IP>      # enumerar web
nmap --script smb-os-discovery -p 445 <IP>
nmap --script ftp-anon -p 21 <IP>       # FTP anónimo
```

## Puertos comunes
21 FTP · 22 SSH · 23 Telnet · 25 SMTP · 53 DNS · 80 HTTP · 110 POP3 · 139/445 SMB · 143 IMAP · 443 HTTPS · 3306 MySQL · 3389 RDP · 5432 PostgreSQL · 8080 HTTP-alt
