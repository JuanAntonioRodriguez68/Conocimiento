
# **Guía Extensa y Detallada para Bug Bounty**

---

## **Índice**
1. [Introducción](#1-introducción)  
2. [Reconocimiento Exhaustivo (Recon)](#2-reconocimiento-exhaustivo-recon)  
    - [Identificación del Alcance](#21-identificación-del-alcance)  
    - [Recolección de Subdominios](#22-recolección-de-subdominios)  
    - [Verificación de Subdominios Activos](#23-verificación-de-subdominios-activos)  
    - [Escaneo de Servicios y Puertos](#24-escaneo-de-servicios-y-puertos)  
    - [Crawling de Sitios Web](#25-crawling-de-sitios-web)  
3. [Análisis y Pruebas Activas](#3-análisis-y-pruebas-activas)  
    - [Identificación de Vulnerabilidades](#31-identificación-de-vulnerabilidades)  
    - [Fuzzing Avanzado](#32-fuzzing-avanzado)  
4. [Post-Explotación](#4-post-explotación)  
    - [Validación del Impacto](#41-validación-del-impacto)  
5. [Reporte Detallado](#5-reporte-detallado)  
6. [Recursos y Wordlists](#6-recursos-y-wordlists)

---

## **1. Introducción**
El bug bounty consiste en encontrar vulnerabilidades en sistemas y reportarlas de forma ética, siguiendo reglas específicas del programa. Este proceso se compone de varias fases diseñadas para maximizar la eficiencia, comenzando desde la recopilación de datos hasta la validación de vulnerabilidades.

---

## **2. Reconocimiento Exhaustivo (Recon)**  
La clave de un buen bug bounty es la calidad del reconocimiento. Aquí se busca recopilar información sobre el objetivo antes de realizar pruebas activas.

---

### **2.1. Identificación del Alcance**
1. **Leer y Analizar las Reglas del Programa**:
    - Identifica los dominios, APIs, aplicaciones o activos en alcance.
    - Revisa las restricciones (prohibición de ataques DoS, escaneo masivo, etc.).

2. **Priorizar Objetivos**:
    - **Activos críticos:** Login, APIs, portales admin.  
    - **Activos secundarios:** Blogs, sistemas no esenciales.

3. **Documentar el Alcance**:
    - Usa un archivo `scope.txt`:
      ```txt
      *.example.com
      api.example.com
      admin.example.com
      ```

---

### **2.2. Recolección de Subdominios**
El descubrimiento de subdominios es esencial para ampliar el rango de ataque.

1. **Enumeración Pasiva con Subfinder:**
   ```bash
   subfinder -d example.com -silent -o subdomains.txt
   ```

2. **Reconocimiento Activo con Amass:**
   - Ejecuta en modo activo para más resultados:
     ```bash
     amass enum -active -d example.com -o amass_output.txt
     ```

3. **Buscar Subdominios en Certificados Públicos:**
   - Usa herramientas web como [crt.sh](https://crt.sh) o **CertSpotter**.

4. **Combinar y Filtrar Resultados:**
   - Ordena y elimina duplicados:
     ```bash
     cat subdomains.txt amass_output.txt | sort -u > all_subdomains.txt
     ```

---

### **2.3. Verificación de Subdominios Activos**
Filtrar subdominios inactivos evita perder tiempo.

1. **Httpx para Verificar Subdominios Activos:**
   ```bash
   httpx -l all_subdomains.txt -o live_subdomains.txt
   ```

2. **Extracción de Certificados SSL:**
   - Encuentra subdominios adicionales:
     ```bash
     certspotter example.com
     ```

---

### **2.4. Escaneo de Servicios y Puertos**
Un escaneo exhaustivo revela servicios vulnerables.

1. **Masscan para Escaneo Rápido:**
   ```bash
   masscan -p1-65535 -iL live_subdomains.txt --rate=1000 -oJ masscan_output.json
   ```

2. **Escaneo Detallado con Nmap:**
   - Verifica servicios y versiones:
     ```bash
     nmap -sV -sC -iL live_subdomains.txt -oN nmap_detailed_scan.txt
     ```

---

### **2.5. Crawling de Sitios Web**
El rastreo de sitios web ayuda a encontrar endpoints ocultos.

1. **GoSpider para Crawling Profundo:**
   ```bash
   gospider -S live_subdomains.txt -o crawl_output/ -d 2
   ```

2. **Extracción de Endpoints en Javascript:**
   - Usa **LinkFinder**:
     ```bash
     python3 linkfinder.py -i https://example.com/script.js -o output.html
     ```

---

## **3. Análisis y Pruebas Activas**

---

### **3.1. Identificación de Vulnerabilidades**
1. **Cross-Site Scripting (XSS):**
   - Identifica puntos de inyección reflejada y DOM.
   - Prueba payloads básicos:
     ```html
     <script>alert('XSS')</script>
     ```

2. **Inyección SQL:**
   - Pruebas manuales y automatizadas con Sqlmap:
     ```bash
     sqlmap -u "https://example.com?id=1" --dbs
     ```

3. **SSRF:**
   - Manipula parámetros para redirecciones internas:
     ```bash
     https://example.com?url=http://127.0.0.1:80/admin
     ```

4. **Pruebas en APIs:**
   - Usa **Postman** o **Burp Suite** para analizar parámetros y métodos HTTP.

---

### **3.2. Fuzzing Avanzado**
1. **Fuzzing de Directorios:**
   - Usa FFuf para descubrir directorios ocultos:
     ```bash
     ffuf -u https://example.com/FUZZ -w /path/to/wordlist -o ffuf_results.txt
     ```

2. **Fuzzing de Parámetros:**
   - Encuentra parámetros ocultos:
     ```bash
     wfuzz -u https://example.com/page.php?id=FUZZ -w /path/to/wordlist
     ```

---

### **4. Post-Explotación**
1. **Validación del Impacto:**
   - Determina el alcance real de la vulnerabilidad:  
     ¿se pueden robar datos? ¿puede un atacante obtener acceso persistente?

2. **Escalada de Privilegios:**
   - Examina cookies y tokens:
     ```bash
     jwt_tool -t <token> --crack
     ```

---

### **5. Reporte Detallado**
1. **Estructura:**
   - Título, resumen, pasos reproducibles, impacto, y pruebas adjuntas.

---

### **6. Recursos y Wordlists**
1. **SecLists:** Wordlists para directorios, fuzzing y vulnerabilidades específicas.  
2. **PayloadsAllTheThings:** Gran colección de payloads y técnicas.  

---
