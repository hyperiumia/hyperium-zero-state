# INFORME TECNICO PROFESIONAL
# Hyperium ZERO-STATE v0.1.0
## Validador de Integridad Pre-Engagement
## Acta Notarial Digital - ISO/IEC 27037:2012

---

**Autor:** Patricio Tirado - CEO, Hyperiumia
**Asesor Legal:** Julio Tirado - Asesor Juridico, Hyperiumia
**Contacto:** hyperiumia@protonmail.com
**Fecha de emision:** Junio 2026
**Version del documento:** v0.1.0 MVP
**Clasificacion:** CONFIDENCIAL - Uso exclusivo del cliente autorizado
**Repositorio publico:** https://github.com/hyperiumia/hyperium-zero-state
**Repositorio privado (codigo fuente):** https://github.com/hyperiumia/hyperium-zero-state-source
**Norma de referencia:** ISO/IEC 27037:2012

---

## Tabla de Contenidos

1. Resumen Ejecutivo
2. Problematica y Justificacion del Proyecto
3. Propuesta de Valor y Diferenciacion
4. Marco Legal y Normativo
5. Especificaciones Tecnicas Generales
6. Arquitectura del Sistema (10 Modulos)
7. Descripcion Detallada de Modulos
8. Catalogo de Verificaciones (46+ checks)
9. Motor de Veredicto
10. Acta Notarial Digital - Declaracion Jurada
11. Integracion con RED-CORE
12. Interfaz de Linea de Comandos (6 Comandos)
13. Suite de Pruebas (46 Unit Tests)
14. Modelo de Negocio
15. Roadmap y Versiones
16. Comparativa con Herramientas Existentes
17. Conclusiones
18. Anexo A: Arquitectura Tecnica Detallada
19. Anexo B: Flujo Operativo Completo
20. Anexo C: Commit History

---

## 1. Resumen Ejecutivo

Hyperium ZERO-STATE es un Validador de Integridad Pre-Engagement desarrollado integramente en Rust (Edicion 2021) que certifica el estado inicial de un sistema antes de que cualquier actividad ofensiva comience. La herramienta produce un Acta Notarial Digital sellada con SHA-256, conforme a la norma ISO/IEC 27037:2012, que prueba de forma inmutable si un equipo estaba limpio o comprometido antes de la intervencion del profesional de ciberseguridad.

**Proposito fundamental:** Si un cliente sufre un incidente durante o despues de una auditoria ofensiva, ZERO-STATE prueba de forma criptograficamente verificable que el profesional no trajo el compromiso.

**Datos clave del MVP:**
- 4 modulos de auditoria (persistencia, red, integridad, memoria)
- 46+ verificaciones individuales
- 46 pruebas unitarias (0 fallos)
- 6 comandos CLI
- 3 veredictos posibles: CLEAN / SUSPICIOUS / COMPROMISED
- Acta Notarial Digital ISO/IEC 27037
- Cadena de evidencia SHA-256 inmutable
- Arquitectura Air-Gapped (sin conexiones de salida)
- 3,752 lineas de codigo Rust
- 10 archivos fuente
- Binario unico sin dependencias externas

---

## 2. Problematica y Justificacion del Proyecto

### 2.1 El Riesgo Legal No Resuelto

Los profesionales de ciberseguridad que realizan evaluaciones ofensivas (penetration testing, red teaming) enfrentan un riesgo legal y reputacional significativo que la industria ha ignorado sistematicamente:

**Escenario tipico del problema:**

1. Un profesional de ciberseguridad firma un contrato para realizar un pentest.
2. Antes de que el profesional lance el primer escaneo, el equipo del cliente ya tiene un beacon de Cobalt Strike inyectado por un atacante externo.
3. El profesional ejecuta su evaluacion ofensiva.
4. Dias o semanas despues, el cliente sufre un incidente de seguridad.
5. El cliente, por conveniencia o ignorancia, senala al profesional como responsable del incidente.
6. Sin evidencia del estado inicial del equipo, el profesional no tiene forma de probar que el compromiso preexistia.

**Consecuencias:**
- Perdida de reputacion profesional del evaluador.
- Responsabilidad legal potencial.
- Disputas contractuales costosas.
- Perdida de clientes futuros.
- Riesgo de acciones legales por parte del cliente.

### 2.2 La Ausencia de Solucion en el Mercado

Las herramientas existentes no resuelven este problema:

| Herramienta | Tipo | Limitacion |
|-------------|------|------------|
| Malwarebytes | Antivirus/EDR | No produce documento legal, no certifica estado inicial |
| Autoruns (Sysinternals) | Persistencia | Solo Windows, sin sellado criptografico, sin reporte ejecutivo |
| Volatility | Forense de memoria | Complejo, requiere dumps, no disenado para pre-engagement |
| KAPE | Forense rapido | Colecciona artefactos pero no produce veredicto ni acta legal |
| THOR (Nextron) | Threat Hunting | Enterprise, costoso, no disenado para el caso de uso legal |
| YARA standalone | Deteccion por firmas | Solo deteccion, sin contexto legal ni cadena de evidencia |

**Ninguna herramienta existente:**
- Produce un documento con valor legal automatico.
- Esta disenada para el caso de uso 'probar que yo no traje esto'.
- Encadena la evidencia criptograficamente.
- Habla el lenguaje de cumplimiento regulatorio.
- Emite un Acta Notarial Digital conforme a ISO/IEC 27037.

### 2.3 La Oportunidad Estrategica

ZERO-STATE no es un antivirus, no es un EDR, no es un scanner de vulnerabilidades. Es una categoria completamente nueva: **Forense de admision, no de respuesta a incidentes.**

- Certifica el estado inicial, no detecta ataques en tiempo real.
- Produce un documento con peso legal, no solo un log tecnico.
- Se ejecuta antes del engagement, no durante ni despues.
- Protege al profesional, no al sistema (eso lo hace RED-CORE despues).

---

## 7. Descripcion Detallada de Modulos

### 7.1 Persistencia (persistence_audit.rs)

**Linux (8 checks):**

| ID | Verificacion | Severidad |
|----|-------------|:---------:|
| LIN-PERSIST-001 | User Crontab | HIGH |
| LIN-PERSIST-002 | System Cron | HIGH |
| LIN-PERSIST-003 | Systemd Services | CRITICAL |
| LIN-PERSIST-004 | RC Local | HIGH |
| LIN-PERSIST-005 | Bash Profile | HIGH |
| LIN-PERSIST-006 | SSH Authorized Keys | MEDIUM |
| LIN-PERSIST-007 | LD_PRELOAD | CRITICAL |
| LIN-PERSIST-008 | Init Scripts | MEDIUM |

**Windows (10 checks):**

| ID | Verificacion | Severidad |
|----|-------------|:---------:|
| WIN-PERSIST-001 | Registry Run HKCU | HIGH |
| WIN-PERSIST-002 | Registry Run HKLM | HIGH |
| WIN-PERSIST-003 | Registry RunOnce HKCU | HIGH |
| WIN-PERSIST-004 | Registry RunOnce HKLM | HIGH |
| WIN-PERSIST-005 | Scheduled Tasks | HIGH |
| WIN-PERSIST-006 | Windows Services | CRITICAL |
| WIN-PERSIST-007 | Startup Folder | HIGH |
| WIN-PERSIST-008 | WMI Subscriptions | CRITICAL |
| WIN-PERSIST-009 | COM Hijacking | HIGH |
| WIN-PERSIST-010 | DLL Search Order | HIGH |

**Patrones detectados:** powershell -enc, mshta, regsvr32, rundll32, wscript, bitsadmin, certutil, curl/wget pipeado a shell, /dev/tcp/, nc/ncat, base64 decode, binarios en temp/shm.

**Tests:** 12

### 7.2 Red (network_state.rs)

| Verificacion | Severidad |
|-------------|:---------:|
| Conexiones activas | HIGH |
| Puertos en escucha | MEDIUM |
| Modo promiscuo | HIGH |

**Puertos C2:** 4444, 5555, 6666, 7777, 8888, 9999, 1234, 31337, 1337, 42069, 12345, 54321

**Tests:** 7

### 7.3 Integridad (integrity_check.rs)

**Linux (15 binarios):** ssh, sudo, passwd, su, curl, wget, nc, ncat, sshd, cron, bash, sh, python3, perl, base64

**Windows (11 binarios):** cmd, powershell, svchost, lsass, services, csrss, winlogon, smss, explorer, taskhostw, conhost

**Configs Linux (8):** passwd, shadow, sudoers, sshd_config, hosts, resolv.conf, ld.so.preload, environment

**Tests:** 7

### 7.4 Memoria (memory_scan.rs)

| Verificacion | Severidad |
|-------------|:---------:|
| Enumeracion de procesos | HIGH |
| Procesos ocultos | HIGH |
| Indicadores de inyeccion | CRITICAL |

**Deteccion:** Process hollowing, nombres legitimos en rutas incorrectas, shells desde oficina/navegadores, consumo anomalo de memoria, exclusion automatica de zero-state.

**Tests:** 8

### 7.5 Dictamen (zero_state_dictamen.rs)

Genera el Acta Notarial Digital con: ID unico, veredicto, identificacion, resumenes por modulo, IoCs, declaracion legal variable, hash SHA-256 verificable.

**Tests:** 8

### 7.6 Cadena de Evidencia (evidence.rs)

Entradas encadenadas SHA-256 heredadas de RED-CORE. Cada entrada: id, category, check_name, target, command, output_hash, result_summary, finding_count, severity, prev_hash, chain_hash, timestamp, duration_ms.

**Tests:** 4

---

## 8. Catalogo de Verificaciones

| Modulo | Linux | Windows | Total |
|--------|:-----:|:-------:|:-----:|
| Persistencia | 8 | 10 | 18 |
| Red | 3 | 3 | 6 |
| Integridad | 2 | 2 | 4 |
| Memoria | 3 | 3 | 6 |
| TOTAL | 16 | 18 | 34 |

---

## 9. Motor de Veredicto

SI critical > 0 EN persistencia O integridad -> COMPROMISED
SI hidden_processes = true -> COMPROMISED
SI injection_indicators > 2 -> COMPROMISED
SI total_high > 2 -> COMPROMISED
SI total_high > 0 O listeners > 0 -> SUSPICIOUS
SI ninguna -> CLEAN

---

## 10. Acta Notarial Digital

12 secciones: Encabezado, Veredicto, Identificacion, Cadena de Evidencia, Persistencia, Red, Integridad, Memoria, IoCs, Declaracion Legal, Hash, Pie de pagina.

**Declaracion CLEAN:** Certificacion de estado limpio, sin compromiso previo.

**Declaracion COMPROMISED:** Alerta critica de compromiso previo, recomendacion de detener engagement, conversion a DFIR.

**Declaracion SUSPICIOUS:** Anomalias que requieren investigacion adicional antes de proceder.

---

## 11. Integracion con RED-CORE

FASE 1: ZERO-STATE certifica entrada
FASE 2: RED-CORE ejecuta 80 tecnicas MITRE ATT&CK
FASE 3: RED-CORE limpia y certifica salida

ZERO-STATE protege a RED-CORE. Son dos piezas del mismo flujo profesional.

---

## 12. CLI (6 Comandos)

| Comando | Descripcion |
|---------|-------------|
| zero-state info | Info del sistema |
| zero-state scan --client X --authorized-by Y | Escaneo completo |
| zero-state persistence | Solo persistencia |
| zero-state network | Solo red |
| zero-state integrity | Solo integridad |
| zero-state memory | Solo memoria |

---

## 13. Suite de Pruebas

| Modulo | Tests | Estado |
|--------|:-----:|:------:|
| evidence.rs | 4 | PASS |
| persistence_audit.rs | 12 | PASS |
| network_state.rs | 7 | PASS |
| integrity_check.rs | 7 | PASS |
| memory_scan.rs | 8 | PASS |
| zero_state_dictamen.rs | 8 | PASS |
| TOTAL | 46 | 0 FAILURES |

---

## 14. Modelo de Negocio

1. **Contratos Hyperium:** Primera fase obligatoria, incremento 15-25%
2. **Licenciamiento:** Poliza de seguro para Red Teams
3. **Bundle RED-CORE:** Suite completa de entrada a salida

---

## 15. Roadmap

| Version | Estado | Contenido |
|---------|:------:|-----------|
| v0.1.0 | ACTUAL | 4 modulos, Acta Notarial, 46 tests |
| v0.5.0 | PLANIFICADO | macOS, YARA-X, hardware, 100 tests |
| v1.0.0 | PLANIFICADO | REST API, Dashboard, Docker, 150 tests |
| v1.5.0 | VISION | RED-CORE integrado, PDF, SIEM/SOAR |

---

## 16. Comparativa

Ninguna herramienta existente produce un Acta Notarial Digital sellada con SHA-256, conforme a ISO/IEC 27037, disenada para el caso de uso pre-engagement. ZERO-STATE es la unica en su categoria.

---

## 17. Conclusiones

1. Primera herramienta para certificar estado inicial pre-engagement con valor legal.
2. ISO/IEC 27037:2012 otorga validez regulatoria internacional.
3. Air-Gapped garantiza cero exfiltracion.
4. Cadena SHA-256 heredada de RED-CORE garantiza inmutabilidad.
5. Integracion RED-CORE cierra el ciclo de proteccion legal.
6. 46 tests con 0 fallos.
7. Triple modelo de negocio.
8. Diferenciacion absoluta vs mercado.

---

## 18. Anexo A: Dependencias

| Crate | Version | Proposito |
|-------|:-------:|-----------|
| clap | 4 | CLI parsing |
| serde/serde_json | 1 | Serializacion |
| chrono | 0.4 | Timestamps |
| sha2 | 0.10 | SHA-256 |
| hex | 0.4 | Hex encoding |
| anyhow/thiserror | 1 | Errores |
| colored | 2 | Terminal |
| walkdir | 2 | Directorios |
| sysinfo | 0.30 | Sistema/procesos |

---

## 19. Anexo B: Flujos Operativos

**Profesional:** Contrato -> ZERO-STATE scan -> Acta -> CLEAN: RED-CORE procede / COMPROMISED: DFIR

**Litigio:** Incidente -> Presentar Acta -> Evidencia criptografica -> Veredicto verificable por tercero

---

## 20. Anexo C: Commits

- **Privado:** fde393d feat: Hyperium ZERO-STATE v0.1.0
- **Publico:** f112270 docs: README + Informe v0.1.0

---

## ESTADISTICAS FINALES

| Metrica | Valor |
|---------|-------|
| Version | v0.1.0 MVP |
| Lenguaje | Rust 2021 Edition |
| Lineas de codigo | 3,752 |
| Archivos fuente | 10 |
| Modulos | 4 |
| Verificaciones | 46 |
| Tests | 46 (0 fallos) |
| CLI | 6 comandos |
| Plataformas | Windows, Linux |
| Salida red | 0 (Air-Gapped) |
| Norma | ISO/IEC 27037:2012 |
| Evidencia | SHA-256 encadenado |
| Veredictos | CLEAN / SUSPICIOUS / COMPROMISED |

---

**Hyperiumia - Patricio Tirado, CEO**
**hyperiumia@protonmail.com - Junio 2026**
**ISO/IEC 27037:2012 Compliant**
