# Auditivos Web 🔍

Plataforma web para investigación de fuentes abiertas (OSINT) responsable y ética.

## Descripción

**Auditivos** es una aplicación web diseñada para facilitar investigaciones legítimas mediante OSINT. Se integra con **Sherlock** para búsqueda pública de usernames y proporciona un panel centralizado para análisis de información públicamente disponible.

**Alcance:** Solo información pública, con consentimiento informado, y para fines legales y éticos.

---

## ✅ Capacidades (Éticas y Legales)

- 🔍 Búsqueda de usernames públicos en redes sociales
- 🌐 Verificación de presencia web y dominios públicos
- 📊 Análisis y visualización de datos públicos
- 🗂️ Organización de investigaciones
- 🔗 Mapeo de conexiones públicas
- 📱 Interfaz responsiva para móvil y escritorio

## ⛔ Fuera del Alcance

- ❌ Rastreo de GPS o ubicación en tiempo real
- ❌ Acceso a datos privados o no públicos
- ❌ Vigilancia o monitoreo de dispositivos
- ❌ Doxing o exposición de información privada
- ❌ Acceso sin autorización a cuentas de terceros

---

## Requisitos

- **Node.js** v14 o superior
- **npm** o yarn
- **Python 3** (para Sherlock)
- **Git**

---

## Instalación Local (PC/Mac/Linux)

### 1. Clonar el repositorio

```bash
git clone https://github.com/sagcanvir/Auditivos-web.git
cd Auditivos-web
```

### 2. Instalar dependencias de Auditivos

```bash
npm install
```

### 3. Clonar Sherlock (en directorio paralelo)

```bash
cd ..
git clone https://github.com/sherlock-project/sherlock.git
cd sherlock
python3 -m pip install -r requirements.txt
cd ../Auditivos-web
```

### 4. Configurar variables de entorno

```bash
cp .env.example .env
```

### 5. Ejecutar la aplicación

**Modo desarrollo:**
```bash
npm run dev
```

**Modo producción:**
```bash
npm run build
npm start
```

Acceso: `http://localhost:3000`

---

## 📱 Instalación y Ejecución en Termux

### Paso 1: Preparar Termux

```bash
# Actualizar Termux
apt update && apt upgrade -y

# Instalar herramientas necesarias
apt install nodejs npm python3 python3-pip git wget curl -y

# Crear directorio de proyectos
mkdir -p ~/projects
cd ~/projects
```

### Paso 2: Clonar Auditivos-web

```bash
git clone https://github.com/sagcanvir/Auditivos-web.git
cd Auditivos-web
npm install
```

### Paso 3: Clonar Sherlock

```bash
cd ~/projects
git clone https://github.com/sherlock-project/sherlock.git
cd sherlock
python3 -m pip install -r requirements.txt
```

### Paso 4: Crear script de lanzamiento

En Termux, crea un archivo `~/projects/run-osint.sh`:

```bash
nano ~/projects/run-osint.sh
```

Copia y pega el siguiente contenido:

```bash
#!/usr/bin/env bash
set -e

PROJECT_DIR="${HOME}/projects"
AUDITIVOS_DIR="${PROJECT_DIR}/Auditivos-web"
SHERLOCK_DIR="${PROJECT_DIR}/sherlock"
LOG_DIR="${PROJECT_DIR}/logs"

# Crear directorio de logs
mkdir -p "$LOG_DIR"

echo "╔════════════════════════════════════════╗"
echo "║   OSINT AUDITIVOS + SHERLOCK LAUNCHER   ║"
echo "║   Búsqueda Ética y Responsable         ║"
echo "╚════════════════════════════════════════╝"
echo ""

# Verificar directorios
if [ ! -d "$AUDITIVOS_DIR" ]; then
  echo "❌ Error: No se encontró Auditivos-web"
  echo "   Clóna el repositorio primero:"
  echo "   git clone https://github.com/sagcanvir/Auditivos-web.git $AUDITIVOS_DIR"
  exit 1
fi

if [ ! -d "$SHERLOCK_DIR" ]; then
  echo "❌ Error: No se encontró Sherlock"
  echo "   Clóna el repositorio primero:"
  echo "   git clone https://github.com/sherlock-project/sherlock.git $SHERLOCK_DIR"
  exit 1
fi

echo "✅ Repositorios encontrados"
echo ""

# Instalar/actualizar dependencias de Auditivos
echo "📦 Verificando dependencias de Auditivos..."
cd "$AUDITIVOS_DIR"
npm install --loglevel=error > "$LOG_DIR/auditivos-install.log" 2>&1

# Verificar dependencias de Sherlock
echo "📦 Verificando dependencias de Sherlock..."
cd "$SHERLOCK_DIR"
python3 -m pip install -q -r requirements.txt

# Iniciar Auditivos en segundo plano
echo "🚀 Iniciando Auditivos-web..."
cd "$AUDITIVOS_DIR"
npm run dev > "$LOG_DIR/auditivos.log" 2>&1 &
AUDITIVOS_PID=$!

sleep 3

# Obtener IP local
IP=$(hostname -I | awk '{print $1}')

echo ""
echo "╔════════════════════════════════════════╗"
echo "║        SERVICIOS INICIADOS ✅          ║"
echo "╠════════════════════════════════════════╣"
echo "║                                        ║"
echo "║  🌐 AUDITIVOS WEB                      ║"
echo "║     Local:  http://localhost:3000      ║"
echo "║     Red:    http://$IP:3000            ║"
echo "║     PID:    $AUDITIVOS_PID              ║"
echo "║                                        ║"
echo "║  🔍 SHERLOCK (búsqueda de usernames)   ║"
echo "║     Directorio: $SHERLOCK_DIR          ║"
echo "║                                        ║"
echo "║  📊 Logs:                              ║"
echo "║     Auditivos: $LOG_DIR/auditivos.log  ║"
echo "║                                        ║"
echo "╚════════════════════════════════════════╝"
echo ""
echo "PRÓXIMOS PASOS:"
echo "1. Abre navegador en http://localhost:3000"
echo "2. Para búsqueda de username en Sherlock:"
echo "   cd $SHERLOCK_DIR"
echo "   python3 sherlock usuario_ejemplo --print-found"
echo ""
echo "Presiona CTRL+C para detener Auditivos"
echo ""

# Mantener el script activo
wait $AUDITIVOS_PID
```

### Paso 5: Dar permisos de ejecución

```bash
chmod +x ~/projects/run-osint.sh
```

### Paso 6: Ejecutar el script

```bash
~/projects/run-osint.sh
```

---

## 🎯 Una vez ejecutado en Termux - ¿Qué hacer?

### Opción 1: Usar Auditivos Web (Panel Gráfico)

1. **Abre el navegador en tu celular:**
   - `http://localhost:3000`

2. **En el panel de Auditivos:**
   - Busca usernames
   - Verifica presencia en redes sociales
   - Consulta dominios públicos
   - Organiza tus hallazgos

### Opción 2: Usar Sherlock (Línea de comandos)

En otra pestaña/sesión de Termux:

```bash
cd ~/projects/sherlock

# Búsqueda simple
python3 sherlock usuario_ejemplo

# Ver solo resultados encontrados
python3 sherlock usuario_ejemplo --print-found

# Limitar tiempo de espera
python3 sherlock usuario_ejemplo --timeout 15

# Buscar múltiples usuarios
python3 sherlock usuario1 usuario2 usuario3
```

### Opción 3: Búsqueda de Dominios Públicos

```bash
# Ver qué redes soporta Sherlock
python3 sherlock --help

# Ejemplo: buscar en 50+ redes sociales
python3 sherlock "nombre.usuario" --print-found
```

---

## 📋 Flujo de Trabajo Completo en Termux

```bash
# Terminal 1: Iniciar servicios
~/projects/run-osint.sh

# Terminal 2: Búsqueda con Sherlock
cd ~/projects/sherlock
python3 sherlock usuario_objetivo --print-found

# Terminal 3 (Navegador): 
# Abre http://localhost:3000 en tu navegador móvil
# - Revisa resultados
# - Organiza información
# - Documenta hallazgos
```

---

## 🔐 Consideraciones Éticas y Legales

✅ **PERMITIDO:**
- Buscar información públicamente disponible
- Verificar identidades públicas con consentimiento
- Usar para investigación académica, periodística o legal
- Analizar presencia web pública
- Documentar hallazgos públicos

❌ **PROHIBIDO:**
- Acceder a información privada
- Rastrear ubicación de personas
- Vigilancia sin consentimiento
- Doxing o exposición de datos privados
- Violación de términos de servicio de plataformas
- Actividades ilícitas

---

## 📁 Estructura del proyecto

```
Auditivos-web/
├── src/              # Código fuente
├── public/           # Archivos estáticos
├── package.json      # Dependencias
├── .env.example      # Variables de entorno
└── README.md         # Este archivo

sherlock/            # Búsqueda de usernames (repo externo)
├── sherlock/        # Código principal
├── requirements.txt  # Dependencias Python
└── sherlock.py      # Script principal
```

---

## 🛠️ Scripts disponibles en Auditivos

```bash
npm run dev          # Modo desarrollo con hot reload
npm run build        # Compilar para producción
npm start            # Ejecutar versión compilada
npm test             # Ejecutar pruebas (si aplica)
npm run lint         # Verificar código
```

---

## 🐛 Solución de problemas en Termux

### Sherlock no encuentra usuarios

```bash
# Actualizar Sherlock
cd ~/projects/sherlock
git pull origin master

# Reinstalar dependencias
python3 -m pip install --upgrade -r requirements.txt
```

### Auditivos no responde

```bash
# Ver logs
tail -f ~/projects/logs/auditivos.log

# Matar proceso Auditivos
pkill -f "npm run dev"

# Reiniciar
~/projects/run-osint.sh
```

### Problemas de permisos

```bash
# En Termux, a veces necesitas:
termux-setup-storage

# Luego reintentar
~/projects/run-osint.sh
```

---

## 🤝 Contribuir

Las contribuciones éticas y responsables son bienvenidas:

1. Fork el repositorio
2. Crea una rama (`git checkout -b feature/mejora`)
3. Commit tus cambios (`git commit -m 'Agrega mejora'`)
4. Push (`git push origin feature/mejora`)
5. Abre un Pull Request

---

## 📞 Contacto y Soporte

- **GitHub:** [@sagcanvir](https://github.com/sagcanvir)
- **Organización:** [OSINT Mindset](https://github.com/OSINT-mindset)
- **Issues:** [Reportar problemas](https://github.com/sagcanvir/Auditivos-web/issues)

---

## ⚖️ Licencia

Este proyecto está bajo licencia MIT. Ver `LICENSE` para más detalles.

---

## 🔍 OSINT Responsable

**Recuerda:**
- La información pública no significa información gratis o sin restricciones
- Respeta la privacidad y dignidad de las personas
- Usa estas herramientas para fines legales y éticos
- Documenta tus fuentes y metodología
- Cumple con leyes locales e internacionales

---

**Únete a OSINT Mindset y aprende investigación responsable.** 🔍✨
