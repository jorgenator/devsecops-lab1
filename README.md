# DevSecOps Lab 1 — OWASP Juice Shop Pipeline

## ¿Qué es OWASP Juice Shop?

OWASP Juice Shop es una aplicación web de comercio electrónico deliberadamente vulnerable, desarrollada y mantenida por la comunidad OWASP. Está construida con Node.js, Express y Angular, e incluye más de 100 desafíos de seguridad que cubren las vulnerabilidades del OWASP Top 10.

Su propósito es educativo: simula una tienda en línea real con autenticación JWT, carrito de compras, búsqueda de productos y panel de administración, pero con vulnerabilidades intencionales para practicar su identificación y corrección en un entorno controlado.

---

## Objetivo de este fork

Este repositorio es un fork de OWASP Juice Shop utilizado como base para el **Laboratorio #1** de la materia de Seguridad en Desarrollo de Software. El objetivo es implementar un pipeline DevSecOps básico que integre herramientas de seguridad automatizadas en el flujo de integración continua.

---

## ¿Qué es DevSecOps?

DevSecOps es la práctica de integrar controles de seguridad directamente en el ciclo de desarrollo de software, en lugar de tratarlos como una etapa separada al final. El principio central es **"shift left"**: detectar y corregir vulnerabilidades lo más temprano posible en el proceso de desarrollo.

En la práctica, esto significa que cada vez que un desarrollador hace un `git push`, un pipeline automatizado ejecuta herramientas de seguridad que analizan el código, las dependencias y el historial de commits, reportando hallazgos antes de que el código llegue a producción.

---

## ¿Qué se hizo en este laboratorio?

Se implementó un pipeline de CI/CD con GitHub Actions compuesto por tres herramientas de seguridad:

- **Semgrep** (SAST): análisis estático del código fuente buscando patrones inseguros como SQL Injection, credenciales hardcodeadas, Open Redirect y eval injection. Encontró 22 findings iniciales.
- **Trivy** (SCA): escaneo del sistema de archivos buscando secretos y vulnerabilidades en dependencias con severidad HIGH y CRITICAL. Detectó una clave privada RSA expuesta.
- **Gitleaks**: análisis del historial de commits buscando credenciales, tokens o claves expuestas accidentalmente.

Adicionalmente se seleccionaron tres vulnerabilidades y se corrigieron con commits independientes, ejecutando el pipeline tras cada fix para evidenciar la mejora.

---

## ¿Qué se logró?

- Pipeline automatizado que corre en cada `git push`
- Detección de 22 vulnerabilidades en el código fuente
- Reducción de findings de 22 a 21 tras aplicar los fixes
- Gitleaks pasó de detectar una clave privada expuesta a reportar "No leaks detected"
- Tres correcciones documentadas con commits descriptivos:
  - `fix: reemplazar private key hardcodeada por variable de entorno (A02 Cryptographic Failures)`
  - `fix: reemplazar HMAC secret hardcodeado por variable de entorno (A02 Cryptographic Failures)`
  - `fix: corregir validacion de open redirect usando startsWith en lugar de includes (A01 Broken Access Control)`

---

## ¿Dónde se ve el pipeline?

Los resultados de cada ejecución están disponibles en la pestaña **Actions** de este repositorio:

👉 [Ver Actions](https://github.com/jorgenator/devsecops-lab1/actions)

---

## Datos del laboratorio

| Campo | Detalle |
|---|---|
| Materia | Seguridad en Desarrollo de Software |
| Laboratorio | #1 — Pipeline DevSecOps Básico |
| Estudiante | Jorge Romero |
| Repositorio base | github.com/juice-shop/juice-shop |

---

## Cómo levantar la aplicación localmente

### Con Docker

```bash
# Paso 1: Descargar la imagen
docker pull bkimminich/juice-shop

# Paso 2: Ejecutar el contenedor
docker run -d -p 3000:3000 bkimminich/juice-shop

# Paso 3: Abrir en el navegador
http://localhost:3000
```

Para detener el contenedor:
```bash
docker ps                        # ver el ID del contenedor
docker stop <ID_DEL_CONTENEDOR>
```

---

### Con npm

```bash
# Paso 1: Clonar el repositorio
git clone https://github.com/jorgenator/devsecops-lab1.git
cd devsecops-lab1

# Paso 2: Instalar dependencias
npm install

# Paso 3: Iniciar la aplicación
npm start

# Paso 4: Abrir en el navegador
http://localhost:3000
```

> Requiere Node.js 18 o superior.
