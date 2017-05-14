# Introducción
En referencia al ataque de Ransomware detectado inicialmente en España (Empresa Telefonica), con afectación masiva en equipos Windows de múltiples versiones, la cual utiliza una versión del malware WannaCry, sobre lo cual se añade la siguiente información:

![grafico](https://i.imgur.com/V8pJ1K4.png)

Se han detectado ataques en 74 paises a rededor del mundo, siendo Rusia el principal afectado.
> Fuente Laboratorio de Karpesky https://securelist.com/blog/incidents/78351/wannacry-ransomware-used-in-widespread-attacks-all-over-the-world/

# Mitigaciones de alto nivel Recomendadas
## Medidas Reactivas
* Desconectar equipo de la red.

* Aplicar herramientas actuales de Anti-Ransomware (en caso que estén disponibles) liberadas para cepas ya conocidas, como por ejemplo: HidraCrypt, Petya, etc.

* Reportar a la brigada de cibercrimen este tipo de delitos para enviar la señal que este tipo de incidentes si son delitos y debe perseguirse las responsabilidades penales de los involucrados, al verse afectada la fe pública, los sistemas institucionales, y la privacidad de la información de ciudadanos.

* Si la identificación del Ransomware ocurre mientras esta cifrando el disco, sacar disco, y buscar posibles la llave de cifrado para revertir el proceso.”

## Medidas Preventivas:
* Revisar si los equipos de la compañia tienen instalado el parche de actualización ms17_010 de Microsoft.

* Detener el servicio SMB mediante políticas GPO

* Detección de nuevos equipos en la red interna

* Revisar las reglas de Firewall sobre comunicaciones hacia internet o redes no seguras sobre el puerto 445 (SMB). Bloquear en caso de sospecha.

* Habilitar las reglas de SNORT, IDS e IPS sobre los indicadores del documento

# Comportamiento del CyberAtaque
> **Nota:** Al ser un ataque en progreso, no existe una completa certeza de como se desarrolla, sin embargo lo descrito en esta sección es producto del análisis y la información compartida entre centros de Ciberseguridad. Una vez mitigado el Ciberataque se realizarán todos los análisis forenses para detectar el origen y falla explotadas.

Se cree que el malware pudo haber infectado a las compañías por una vulnerabilidad en los equipos Windows en los servicios SMB (puerto 445) la cual una vez explotada permite tomar completo control del equipo de manera remota, y en este caso, descargar y ejecutar el Ransomware. Esta información es en base a lo comunicado por el CCN-CERT de España.

Esta vulnerabilidad fue parchada por Microsoft el 14 de Marzo del 2017 bajo el código ms17_010, de la mano de la filtración de las herramientas de la CIA por parte del equipo de Hackers Shadowbrokers. Esta filtración contenía los exploits necesarios para aprovecharse de esta vulnerabilidad, incluso con una interfaz gráfica para su facilidad de uso. Existen múltiples guías en internet que explicaban paso a paso (con fotos y videos) sobre como explotar esto.

La explotación de la vulnerabilidad es bastante sencilla y se realiza a través del protocolo SMB (Puerto 445) de las máquinas Windows utilizando la técnica de Eternalblue con Doublepulsar. Una vez explotada la vulnerabilidad e instalado el Backdoor se procede a descargar el ransomware y a realizar su infección.

Según el CCN-CERT de España el ransomware utilizado es el WannaCry, el cual una vez infectado el equipo encripta todos los archivos del disco duro y solicita una recompensa por ello la cual debe pagarse a través de Bitcoins y la red Tor.

# Sistemas Afectados

Las siguiente versiones Windows que tengan el servicio SMB habilitado pueden verse afectados:
* Microsoft Windows Vista SP2
* Windows Server 2008 SP2 and R2 SP1
* Windows 7
* Windows 8.1
* Windows RT 8.1
* Windows Server 2012 and R2
* Windows 10
* Windows Server 2016

---
# Contexto Técnico

A continuacion se describiran los diferentes aspectos técnicos del ataque, como vectores, vulnerabilidades explotadas, hashes, reglas de snort, etcétera.

## Hashes del Malware
La siguiente tabla incluye las firmas de los diferentes versiones del Malware utilizado.


Tipo | Hash
---- | ---
FileHash-SHA256|ed01ebfbc9eb5bbea545af4d01bf5f1071661840480439c6e5babe8e080e41aa
FileHash-SHA256|b9c5d4339809e0ad9a00d4d3dd26fdf44a32819a54abf846bb9b560d81391c25
FileHash-SHA256|2584e1521065e45ec3c17767c065429038fc6291c091097ea8b22c8a502c41dd
FileHash-SHA256|ed01ebfbc9eb5bbea545af4d01bf5f1071661840480439c6e5babe8e080e41aa
FileHash-SHA256|09a46b3e1be080745a6d8d88d6b5bd351b1c7586ae0dc94d0c238ee36421cafa
FileHash-SHA256|24d004a104d4d54034dbcffc2a4b19a11f39008a575aa614ea04703480b1022c
FileHash-SHA256|f8812f1deb8001f3b7672b6fc85640ecb123bc2304b563728e6235ccbe782d85
FileHash-MD5|509c41ec97bb81b0567b059aa2f50fe8
FileHash-MD5|7bf2b57f2a205768755c07f238fb32cc
FileHash-MD5|7f7ccaa16fb15eb1c7399d422f8363e8
FileHash-MD5|84c82835a5d21bbcf75a61706d8ab549
FileHash-MD5|db349b97c37d22f5ea1d1841e3c89eb4
FileHash-MD5|f107a717f76f4f910ae9cb4dc5290594
FileHash-SHA1|51e4307093f8ca8854359c0ac882ddca427a813c
FileHash-SHA1|87420a2791d18dad3f18be436045280a4cc16fc4
FileHash-SHA1|e889544aff85ffaf8b0d0da705105dee7c97fe26
FileHash-SHA1|45356a9dd616ed7161a3b9192e2f318d0ab5ad10
FileHash-SHA1|bd44d0ab543bf814d93b719c24e90d8dd7111234
FileHash-SHA256|2ca2d550e603d74dedda03156023135b38da3630cb014e3d00b1263358c5f00d
FileHash-SHA256|4a468603fdcb7a2eb5770705898cf9ef37aade532a7964642ecd705a74794b79

## Parches de Seguridad
La siguiente tabla incluye los diferentes parches para mitigar las vulnerabilidades explotadas por este malware:

Nombre | Vulnerabilidad | Parche
---- | --- | ---
EternalBlue EternalSynergy EternalRomance EternalChampion | MS17-010 | msft-cve-2017-0143 msft-cve-2017-0144 msft-cve-2017-0145 msft-cve-2017-0146 msft-cve-2017-0147 msft-cve-2017-0148
EmeraldThread | MS10-061 | WINDOWS-HOTFIX-MS10-061
EducatedScholar | MS09-050 | WINDOWS-HOTFIX-MS09-050
EclipsedWing | MS08-067 | WINDOWS-HOTFIX-MS08-067


## Exploits Disponibles
La siguiente table incluye los exploits utilizados por el malware para la explotacion de las vulnerabilidades.

Nombre | Vulnerabilidad | Modulo Metasploit
---- | --- | ---
EternalBlue | MS17-010 | auxiliary/scanner/smb/smb_ms17_010
EmeraldThread | MS10-061| exploit/windows/smb/psexec
EternalChampion |MS17-010 |auxiliary/scanner/smb/smb_ms17_010
EternalRomance|MS17-010|auxiliary/scanner/smb/smb_ms17_010
EducatedScholar|MS09-050|auxiliary/dos/windows/smb/ms09_050_smb2_negotiate_pidhigh, auxiliary/dos/windows/smb/ms09_050_smb2_session_logoff, exploits/windows/smb/ms09_050_smb2_negotiate_func_index
EternalSynergy|MS17-010|auxiliary/scanner/smb/smb_ms17_010
EclipsedWing|MS08-067|auxiliary/scanner/smb/ms08_067_check exploits/windows/smb/ms08_067_netapi

# Direcciones IPs del Malware

* 149.202.160.69
* 197.231.221.211
* 128.31.0.39
* 46.101.166.19
* 91.121.65.179
* 129.128.31.0.39
* 188.166.23.127
* 193.23.244.244
* 2.3.69.209
* 146.0.32.144
* 50.7.161.218
* 192.42.113.102
* 83.169.6.12
* 158.69.92.127
* 86.59.21.38
* 62.138.7.171
* 51.255.203.235
* 51.15.36.164
* 217.79.179.177:9001
* 128.31.0.39:9101
* 213.61.66.116:9003
* 212.47.232.237:9001
* 81.30.158.223:9001
* 79.172.193.32:443
* 163.172.149.155
* 167.114.35.28
* 176.9.39.218
* 193.11.114.43
* 199.254.238.52
* 89.40.71.149

# Imágenes del Ransomware
> Fuente: https://www.hybrid-analysis.com/sample/b9c5d4339809e0ad9a00d4d3dd26fdf44a32819a54abf846bb9b560d81391c25?environmentId=100

![img1](https://www.hybrid-analysis.com/sample/b9c5d4339809e0ad9a00d4d3dd26fdf44a32819a54abf846bb9b560d81391c25%23100/screenshots/screen_0.png?1494586973)
![img2](https://www.hybrid-analysis.com/sample/b9c5d4339809e0ad9a00d4d3dd26fdf44a32819a54abf846bb9b560d81391c25%23100/screenshots/screen_1.png?1494586973)
![img3](https://www.hybrid-analysis.com/sample/b9c5d4339809e0ad9a00d4d3dd26fdf44a32819a54abf846bb9b560d81391c25%23100/screenshots/screen_2.png?1494586973)


# URL descubiertas utilizadas por el Malware

* hxxtp://www[.]btcfrog[.]com/qr/bitcoinpng[.]php?address
* hxxp://www[.]rentasyventas([.])com/incluir/rk/imagenes[.]html
* hxxp://www[.]rentasyventas[.]com/incluir/rk/imagenes[.]html?retencion=081525418
* hxxp://www[.]iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea[.]com



### Nodos TOR utilizados ed01ebfbc9eb5bbea545af4d01bf5f1071661840480439c6e5babe8e080e41aa
> (mssecsvckkk.exe)
 
* 188.166.23.127:443
* 193.23.244.244:443
* 2.3.69.209:9001
* 146.0.32.144:9001
* 50.7.161.218:9001

# Reglas para detectar IoC:
Las siguientes Reglas ayudan a la rapida deteccion de una infeccion.

## Ficheros modificados:
    C:\WINDOWS\system32\msctfime.ime
    C:\WINDOWS\win.ini
    C:\DOCUME~1\User\LOCALS~1\Temp\c.wnry
    C:\DOCUME~1\User\LOCALS~1\Temp\msg\m_English.wnry

## Snort
Las siguientes Reglas de SNORT ayudan a una rapida deteccion de la infeccion.

```snort
alert tcp $EXTERNAL_NET any -> $HOME_NET 445 (msg:"OS-WINDOWS Microsoft Windows SMB remote code execution attempt"; flow:to_server,established; content:"|FF|SMB3|00 00 00 00|"; depth:9; offset:4; byte_extract:2,26,TotalDataCount,relative,little; byte_test:2,>,TotalDataCount,20,relative,little; metadata:policy balanced-ips drop, policy connectivity-ips drop, policy security-ips drop, ruleset community, service netbios-ssn; reference:cve,2017-0144; reference:cve,2017-0146; reference:url,isc.sans.edu/forums/diary/ETERNALBLUE+Possible+Window+SMB+Buffer+Overflow+0Day/22304/; reference:url,technet.microsoft.com/en-us/security/bulletin/MS17-010; classtype:attempted-admin; sid:41978; rev:3;)
alert tcp any any -> $HOME_NET 445 (msg:"OS-WINDOWS Microsoft Windows SMB large NT RENAME transaction request information leak attempt"; flow:to_server,established; content:"|FF|SMB|A0 00 00 00 00|"; depth:9; offset:4; content:"|05 00|"; within:2; distance:60; byte_test:2,>,1024,0,relative,little; metadata:policy balanced-ips drop, policy security-ips drop, ruleset community, service netbios-ssn; reference:url,msdn.microsoft.com/en-us/library/ee441910.aspx; reference:url,technet.microsoft.com/en-us/security/bulletin/MS17-010; classtype:attempted-recon; sid:42338; rev:1;)
alert tcp $HOME_NET 445 -> any any (msg:"OS-WINDOWS Microsoft Windows SMB possible leak of kernel heap memory"; flow:to_client,established; content:"Frag"; fast_pattern; content:"Free"; content:"|FA FF FF|"; content:"|F8 FF FF|"; within:3; distance:5; content:"|F8 FF FF|"; within:3; distance:5; metadata:policy balanced-ips alert, policy security-ips drop, ruleset community, service netbios-ssn; reference:cve,2017-0147; reference:url,technet.microsoft.com/en-us/security/bulletin/MS17-010; classtype:attempted-recon; sid:42339; rev:2;)
alert tcp any any -> $HOME_NET 445 (msg:"DOUBLEPULSAR SMB implant - Unimplemented Trans2 Session Setup Subcommand Request"; flow:to_server, established; content:"|FF|SMB|32|"; depth:5; offset:4; content:"|0E 00|"; distance:56; within:2; reference:url,https://twitter.com/countercept/status/853282053323935749; sid:1618009; classtype:attempted-user; rev:1;)
alert tcp $HOME_NET 445 -> any any (msg:"DOUBLEPULSAR SMB implant - Unimplemented Trans2 Session Setup Subcommand - 81 Response"; flow:to_client, established; content:"|FF|SMB|32|"; depth:5; offset:4; content:"|51 00|"; distance:25; within:2; reference:url,https://twitter.com/countercept/status/853282053323935749; sid:1618008; classtype:attempted-user; rev:1;)
alert tcp $HOME_NET 445 -> any any (msg:"DOUBLEPULSAR SMB implant - Unimplemented Trans2 Session Setup Subcommand - 82 Response"; flow:to_client, established; content:"|FF|SMB|32|"; depth:5; offset:4; content:"|52 00|"; distance:25; within:2; reference:url,https://twitter.com/countercept/status/853282053323935749; sid:1618010; classtype:attempted-user; rev:1;)
```

# Claves de registro afectadas
* HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\IMM
* HKEY_USERS\S-1-5-21-1547161642-507921405-839522115-1004\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers
* HKEY_CURRENT_USER\SOFTWARE\Microsoft\CTF
* HKEY_LOCAL_MACHINE\Software\Microsoft\CTF\SystemShared
* HKEY_USERS\S-1-5-21-1547161642-507921405-839522115-1004
* HKEY_LOCAL_MACHINE\Software\WanaCrypt0r
* HKEY_CURRENT_USER\Software\WanaCrypt0r
* HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\ProfileList\S-1-5-21-1547161642-507921405-839522115-1004
* HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager
* HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\currentVersion\Time Zones\W. Europe Standard Time
* HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Time Zones\W. Europe Standard Time\Dynamic DST
* HKEY_CURRENT_USER\SOFTWARE\Microsoft\CTF\LangBarAddIn\
* HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\CTF\LangBarAddIn\

# Detalles de Hash Ransomware

Campo | Valor
---- | ----
FILE NAME |	_WanaDecryptor_.exe
FILE SIZE	|245760 bytes
FILE TYPE	|PE32 executable (GUI) Intel 80386, for MS Windows
MD5	|7bf2b57f2a205768755c07f238fb32cc
SHA1	|45356a9dd616ed7161a3b9192e2f318d0ab5ad10
SHA256	|b9c5d4339809e0ad9a00d4d3dd26fdf44a32819a54abf846bb9b560d81391c25
SHA512|	91a39e919296cb5c6eccba710b780519d90035175aa460ec6dbe631324e5e5753bd8d87f395b5481bcd7e1ad623b31a34382d81faae06bef60ec28b49c3122a9
CRC32|	4E6C168D
SSDEEP|	3072:Rmrhd5U1eigWcR+uiUg6p4FLlG4tlL8z+mmCeHFZjoHEo3m:REd5+IZiZhLlG4AimmCo
YARA|	None matched
% | 4a468603fdcb7a2eb5770705898cf9ef37aade532a7964642ecd705a74794b79
% | 24d004a104d4d54034dbcffc2a4b19a11f39008a575aa614ea04703480b1022c
% | b9c5d4339809e0ad9a00d4d3dd26fdf44a32819a54abf846bb9b560d81391c25
% | ed01ebfbc9eb5bbea545af4d01bf5f1071661840480439c6e5babe8e080e41aa



# Mitigaciones de alto nivel Recomendadas
## Medidas Reactivas
* Desconectar equipo de la red.

* Aplicar herramientas actuales de Anti-Ransomware (en caso que estén disponibles) liberadas para cepas ya conocidas, como por ejemplo: HidraCrypt, Petya, etc.

* Reportar a la brigada de cibercrimen este tipo de delitos para enviar la señal que este tipo de incidentes si son delitos y debe perseguirse las responsabilidades penales de los involucrados, al verse afectada la fe pública, los sistemas institucionales, y la privacidad de la información de ciudadanos.

* Si la identificación del Ransomware ocurre mientras esta cifrando el disco, sacar disco, y buscar posibles la llave de cifrado para revertir el proceso.”

## Buenas Prácticas Generales:

* Tener una declaración de activos críticos actualizada y políticas de protección ad hoc para la protección de dichos activos priorizados en base a los riesgos (probabilidad de materialización de una amenaza versus impacto de dicha materialización, por ejemplo).

* Verificar que los activos críticos se encuentren respaldados con pruebas de recuperación realizas con una frecuencia acorde a la criticidad de los activos y las ventanas de tiempo óptimas ante potenciales pérdidas de datos y los niveles de confianza de las herramientas de respaldo implementados.

* Evitar el uso cotidiano de cuentas de administrador tanto de dominio como local, para usos que no requieran esos privilegios elevados. Las actividades deben realizarse en general con el perfil de usuario normal.

* Los equipos que no cuenten con últimas versiones de actualizaciones en Sistemas Operativos y programas como flash, java, adobe, Internet Explorer se recomienda que no sean conectados a Internet.

* En el contexto del manejo de entorno de los computadores de usuarios, es necesario para mitigar técnicas de ataque en las que se pretende esconder la extensión real de archivos enviados a los usuarios, obligar al sistema operativo a mostrarla. En conjunto a esta medida se debe educar al usuario para que sepa reconocer las extensiones y cuáles de ellas son potencialmente peligrosas. Para aplicar el control que muestre las extensiones, debe aplicarse en lo posible mediante política (GPO) a todos los equipos o en su defecto para algún caso relevante, verificando que en propiedades de Windows esté habilitado “No esconder extensiones de archivos”.

* Si la institución ha de mantener aplicaciones “legacy” sobre sistemas operativos que ya no cuentan con soporte de seguridad por parte del fabricante, debiera considerar no exponer a internet estos equipos, en atención a su alta vulnerabilidad y probabilidad de ser impactados por malware.

# Contacto
Centro de Ciber Inteligencia de Entel

# Fuentes
* https://otx.alienvault.com/pulse/5915db384da2585b4feaf2f6/
* https://otx.alienvault.com/pulse/5915d8374da2585a08eaf2f6/
* https://otx.alienvault.com/pulse/5915abfa0d3cde45e3669850/
* https://www.ccn-cert.cni.es/seguridad-al-dia/comunicados-ccn-cert/4464-ataque-masivo-de-ransomware-que-afecta-a-un-elevado-numero-de-organizaciones-espanolas.html
* https://malwr.com/analysis/YTllMjk1N2I0MTlmNGRlMmFhY2UyOTExMjg5ZTFiYjA/
* https://isc.sans.edu/forums/diary/ETERNALBLUE+Windows+SMBv1+Exploit+Patched/22304/
* https://www.euroweeklynews.com/3.0.15/news/on-euro-weekly-news/spain-news-in-english/144385-telefonica-allegedly-hacked-and-held-to-ransom
* https://www.hybrid-analysis.com/sample/b9c5d4339809e0ad9a00d4d3dd26fdf44a32819a54abf846bb9b560d81391c25?environmentId=100
* http://www.bbc.com/news/health-39899646
* Alerta CCN-CERT: https://www.ccn-cert.cni.es/seguridad-al-dia/comunicados-ccn-cert/4464-ataque-masivo-de-ransomware-que-afecta-a-un-elevado-numero-de-organizaciones-espanolas.html
* Microsoft Security Bulletin: https://technet.microsoft.com/en-us/library/security/ms17-010.aspx#ID0ERPAG
* Información sobre: https://support.microsoft.com/en-us/help/2696547/how-to-enable-and-disable-smbv1,-smbv2,-and-smbv3-in-windows-vista,-windows-server-2008,-windows-7,-windows-server-2008-r2,-windows-8,-and-windows-server-2012
* https://support.microsoft.com/en-us/help/204279/direct-hosting-of-smb-over-tcp-ip

* Laboratorio interno CCI-ENTEL

> Referencia:https://hackmd.io/s/H1HYNvmxW
