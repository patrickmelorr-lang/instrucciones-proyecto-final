# 📋 INSTRUCCIONES PARA INTEGRANTES - PROYECTO DE CÁLCULO

## Objetivo General
Crear notebooks individuales que demuestren **relaciones lineales (y = ax + b)** usando **APIs en tiempo real**, en diferentes ámbitos: software, finanzas, música, criptomonedas y GitHub.

---

## 🎯 Asignación de APIs por Integrante

| Integrante | API | Objetivo | Variable X | Variable Y |
|-----------|-----|---------|-----------|-----------|
| **Integrante 1** | GitHub | Efecto umbral en RAM | Nº Repositorios | RAM Consumida (MB) |
| **Integrante 2** | OpenAI / Anthropic (LLM) | Tokens vs Tiempo respuesta | Tokens input | Tiempo respuesta (ms) |
| **Integrante 3** | CoinGecko (Crypto) | Bitcoin en tendencia alcista | Días transcurridos | Precio BTC (USD) |
| **Integrante 4** | Spotify / LastFM (Música) | Canciones vs Duración | Nº Canciones | Duración total (min) |
| **Integrante 5** | Software Estimation (COCOMO) | Tamaño código vs Esfuerzo | KLOC (miles líneas) | Esfuerzo (meses-hombre) |

> **Nota:** Esta es la asignación base. Consulta con tu profesor si tu grupo tiene una estructura diferente.

---

## 📝 Estructura del Notebook (.ipynb) - 5 CELDAS OBLIGATORIAS

Tu notebook debe tener **EXACTAMENTE 5 celdas** en este orden:

### **CELDA 1: PRESENTACIÓN (Markdown)**
```markdown
# [NOMBRE DE TU API] - Integración y Análisis

**Autor:** [Tu Nombre Completo]  
**Fecha:** [Fecha de Creación]  
**Versión:** 1.0  
**API Asignada:** [Nombre de la API]  
**Documentación Oficial:** [URL]  

---

## 📌 **1. Descripción del Proyecto**

[Escribe 2-3 párrafos sobre:
- ¿Qué hace esta API?
- ¿Para qué sirve en el mundo real?
- ¿Por qué demuestra una relación lineal?
- Cómo se relaciona con el objetivo grupal]

---

## 🎯 **Objetivo Específico**

[Ej: Determinar la relación lineal entre días transcurridos y precio del Bitcoin...]

---

## 📊 **Variables a Analizar**

| Métrica | Variable X | Variable Y | Ecuación Esperada |
|---------|-----------|-----------|------------------|
| Principal | [Ej: Días] | [Ej: Precio USD] | y = ax + b |
```

---

### **CELDA 2: IMPORTACIONES Y CONFIGURACIÓN (Code)**

```python
# ============================================================================
# CELDA 2: IMPORTACIONES Y CONFIGURACIÓN
# ============================================================================
# Esta celda:
# 1. Importa todas las librerías necesarias
# 2. Solicita credenciales de API (si existen) de forma segura
# 3. Crea estructura de carpetas para guardar resultados
# 4. Configura parámetros de búsqueda/consulta

# Librerías estándar
import requests
import json
import time
from datetime import datetime
from pathlib import Path

# Librerías de análisis
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

# Configuración visual
plt.rcParams['figure.figsize'] = (14, 5)
plt.rcParams['savefig.dpi'] = 300

# Crear carpetas
Path("assets/images").mkdir(parents=True, exist_ok=True)
Path("assets/outputs").mkdir(parents=True, exist_ok=True)

print("✅ Librerías importadas y carpetas creadas")

# ============================================================================
# SOLICITAR CREDENCIALES (SI ES NECESARIO)
# ============================================================================
# Para APIs que requieren token/API key, solicitarlas interactivamente
from getpass import getpass

API_KEY = getpass("🔑 Ingresa tu API key (no se verá mientras escribes): ")

# ============================================================================
# PARÁMETROS DE CONSULTA
# ============================================================================
BASE_URL = "https://api.ejemplo.com"  # Reemplazar con URL real
QUERY_PARAMS = {
    "parametro1": "valor1",
    "parametro2": "valor2",
}

print("⚙️ Configuración completada")
```

**✅ QUÉ DEBE TENER ESTA CELDA:**
- [ ] Imports de `requests`, `numpy`, `matplotlib`, `scipy`
- [ ] Solicitud interactiva de API key/token (NO escribirlo en código)
- [ ] Creación de carpetas `assets/images` y `assets/outputs`
- [ ] Configuración de `plt` para gráficas
- [ ] Parámetros de búsqueda comentados

---

### **CELDA 3: FUNCIONES PRINCIPALES (Code)**

```python
# ============================================================================
# CELDA 3: FUNCIONES PRINCIPALES
# ============================================================================
# Esta celda define las funciones que usaremos en la ejecución principal.
# IMPORTANTE: Cada función debe tener comentarios explicativos
# y docstring describiendo qué hace, parámetros y retorno.

# ============================================================================
# FUNCIÓN 1: CONECTAR A LA API Y OBTENER DATOS
# ============================================================================
# Realiza la solicitud HTTP a la API y maneja errores automáticamente.
# Si falla, reintenta hasta 2 veces con espera progresiva.

def obtener_datos_api(parametros):
    """
    Realiza solicitud GET a la API con reintentos automáticos.
    Si falla (timeout, error 5XX), espera y reintenta.
    
    Parámetros:
        parametros (dict): Parámetros de la consulta
    
    Retorna:
        dict: Respuesta JSON o dict con error
    """
    reintentos = 2
    for intento in range(reintentos):
        try:
            response = requests.get(
                BASE_URL,
                headers={"Authorization": f"Bearer {API_KEY}"},
                params=parametros,
                timeout=10
            )
            
            if response.status_code == 200:
                return response.json()
            else:
                raise Exception(f"Error {response.status_code}")
        
        except Exception as e:
            espera = 2 ** intento
            if intento < reintentos - 1:
                print(f"❌ Error: {e}. Reintentando en {espera}s...")
                time.sleep(espera)
            else:
                return {"error": str(e)}
    
    return {"error": "Falló después de reintentos"}


# ============================================================================
# FUNCIÓN 2: PROCESAR DATOS (EXTRAER LO QUE NECESITAS)
# ============================================================================
# Filtra y reorganiza los datos de la API para dejar solo lo que usaremos.

def procesar_datos(datos_api):
    """
    Procesa respuesta de API y extrae solo lo necesario.
    
    Parámetros:
        datos_api (dict): Respuesta completa de la API
    
    Retorna:
        list: Lista de datos procesados
    """
    procesados = []
    for item in datos_api.get("items", []):  # Adaptar según estructura
        procesados.append({
            "variable_x": item.get("valor_x"),  # Reemplazar con campo real
            "variable_y": item.get("valor_y"),  # Reemplazar con campo real
            "timestamp": datetime.now().isoformat(),
        })
    return procesados


# ============================================================================
# FUNCIÓN 3: CALCULAR REGRESIÓN LINEAL
# ============================================================================
# Encuentra la ecuación y = ax + b que mejor ajusta los datos.

def calcular_regresion(x_data, y_data):
    """
    Calcula regresión lineal: y = ax + b
    
    Parámetros:
        x_data (list): Variable independiente
        y_data (list): Variable dependiente
    
    Retorna:
        dict: pendiente, intercepto, R², p_valor
    """
    pendiente, intercepto, r_valor, p_valor, std_err = stats.linregress(x_data, y_data)
    return {
        "pendiente": pendiente,
        "intercepto": intercepto,
        "r_cuadrado": r_valor ** 2,
        "p_valor": p_valor
    }


# ============================================================================
# FUNCIÓN 4: GUARDAR DATOS A JSON
# ============================================================================
# Exporta resultados en formato JSON para HTML dashboard posterior.

def guardar_json(datos, archivo="assets/outputs/datos.json"):
    """
    Guarda datos en JSON.
    
    Parámetros:
        datos (dict): Datos a guardar
        archivo (str): Ruta del archivo
    """
    with open(archivo, 'w', encoding='utf-8') as f:
        json.dump(datos, f, indent=2, ensure_ascii=False)
    print(f"✅ JSON guardado: {archivo}")


print("✅ Todas las funciones definidas")
```

**✅ QUÉ DEBE TENER ESTA CELDA:**
- [ ] Mínimo 3 funciones principales comentadas
- [ ] Cada función con docstring (descripción, parámetros, retorno)
- [ ] Función para conectar API (con reintentos automáticos)
- [ ] Función para procesar datos
- [ ] Función para calcular regresión lineal
- [ ] Función para guardar JSON

---

### **CELDA 4: EJECUCIÓN PRINCIPAL Y ANÁLISIS (Code)**

```python
# ============================================================================
# CELDA 4: EJECUCIÓN PRINCIPAL
# ============================================================================
# Esta celda:
# 1. Llama a las funciones definidas anteriormente
# 2. Recolecta datos de la API
# 3. Calcula regresiones lineales
# 4. Muestra estadísticas y análisis

print("🚀 INICIANDO DESCARGA DE DATOS\\n")

# ============================================================================
# PASO 1: OBTENER DATOS DE LA API
# ============================================================================

datos_api = obtener_datos_api(QUERY_PARAMS)

if "error" in datos_api:
    print(f"❌ Error al conectar: {datos_api['error']}")
else:
    print(f"✅ Datos obtenidos exitosamente")

# ============================================================================
# PASO 2: PROCESAR DATOS
# ============================================================================

datos_procesados = procesar_datos(datos_api)
print(f"✅ {len(datos_procesados)} registros procesados")

# ============================================================================
# PASO 3: EXTRAER VARIABLES PARA REGRESIÓN
# ============================================================================

x_data = [d["variable_x"] for d in datos_procesados]
y_data = [d["variable_y"] for d in datos_procesados]

# ============================================================================
# PASO 4: CALCULAR REGRESIÓN LINEAL
# ============================================================================

regresion = calcular_regresion(x_data, y_data)

print(f"\n📈 ECUACIÓN DESCUBIERTA:")
print(f"   y = {regresion['pendiente']:.4f}x + {regresion['intercepto']:.4f}")
print(f"   R² = {regresion['r_cuadrado']:.4f}")
print(f"   Ajuste: {'✅ EXCELENTE' if regresion['r_cuadrado'] > 0.90 else '⚠️ ACEPTABLE'}")

# ============================================================================
# PASO 5: ESTADÍSTICAS FINALES
# ============================================================================

print(f"\n📊 ESTADÍSTICAS:")
print(f"   Total de registros: {len(datos_procesados)}")
print(f"   Variable X: {min(x_data):.2f} a {max(x_data):.2f}")
print(f"   Variable Y: {min(y_data):.2f} a {max(y_data):.2f}")
```

**✅ QUÉ DEBE TENER ESTA CELDA:**
- [ ] Llamadas a funciones de CELDA 3
- [ ] Obtención de datos de API
- [ ] Procesamiento de datos
- [ ] Extracción de X e Y
- [ ] Cálculo de regresión
- [ ] Muestra de ecuación y = ax + b
- [ ] Estadísticas finales

---

### **CELDA 5: VISUALIZACIÓN Y EXPORTACIÓN (Code)**

```python
# ============================================================================
# CELDA 5: VISUALIZACIÓN, EXPORTACIÓN Y DASHBOARD
# ============================================================================
# Esta celda:
# 1. Crea gráficas profesionales (PNG/SVG)
# 2. Exporta datos a JSON
# 3. Muestra dashboard resumen

print("📊 GENERANDO VISUALIZACIONES\\n")

# ============================================================================
# PARTE 1: CREAR GRÁFICAS
# ============================================================================

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 5))

# Gráfica 1: Scatter + Regresión
ax1.scatter(x_data, y_data, alpha=0.6, s=50, label='Datos reales')
linea = [regresion['pendiente'] * x + regresion['intercepto'] for x in x_data]
ax1.plot(x_data, linea, 'r-', linewidth=2, label='Regresión lineal')
ax1.set_xlabel('Variable X', fontweight='bold')
ax1.set_ylabel('Variable Y', fontweight='bold')
ax1.set_title(f'Análisis: y = {regresion[\"pendiente\"]:.4f}x + {regresion[\"intercepto\"]:.4f}\\nR² = {regresion[\"r_cuadrado\"]:.4f}')
ax1.legend()
ax1.grid(True, alpha=0.3)

# Gráfica 2: Residuales (opcional, para análisis más profundo)
residuales = [y_data[i] - linea[i] for i in range(len(y_data))]
ax2.scatter(x_data, residuales, alpha=0.6, s=50)
ax2.axhline(y=0, color='r', linestyle='--', linewidth=2)
ax2.set_xlabel('Variable X', fontweight='bold')
ax2.set_ylabel('Residual', fontweight='bold')
ax2.set_title('Análisis de Residuales')
ax2.grid(True, alpha=0.3)

plt.tight_layout()

# Guardar PNG
plt.savefig("assets/images/regresion.png", dpi=300, bbox_inches='tight')
print("✅ Gráfica guardada (PNG): assets/images/regresion.png")

# Guardar SVG
plt.savefig("assets/images/regresion.svg", format='svg', bbox_inches='tight')
print("✅ Gráfica guardada (SVG): assets/images/regresion.svg")

plt.show()

# ============================================================================
# PARTE 2: EXPORTAR A JSON
# ============================================================================

datos_finales = {
    "api": "[Tu API]",
    "ecuacion": f"y = {regresion['pendiente']:.4f}x + {regresion['intercepto']:.4f}",
    "r_cuadrado": regresion['r_cuadrado'],
    "p_valor": regresion['p_valor'],
    "total_registros": len(datos_procesados),
    "timestamp": datetime.now().isoformat()
}

guardar_json(datos_finales)

# ============================================================================
# PARTE 3: DASHBOARD
# ============================================================================

from IPython.display import HTML, display

dashboard = f"""
<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white;">
    <h2>📊 RESUMEN DE ANÁLISIS</h2>
    <p><strong>API:</strong> [Tu API]</p>
    <p><strong>Ecuación:</strong> y = {regresion['pendiente']:.4f}x + {regresion['intercepto']:.4f}</p>
    <p><strong>R² (Ajuste):</strong> {regresion['r_cuadrado']:.4f} ✅</p>
    <p><strong>Registros:</strong> {len(datos_procesados)}</p>
</div>
"""

display(HTML(dashboard))

print("\\n✅ ANÁLISIS COMPLETADO")
```

**✅ QUÉ DEBE TENER ESTA CELDA:**
- [ ] Gráfica scatter + regresión lineal
- [ ] Guardado PNG en `assets/images/`
- [ ] Guardado SVG en `assets/images/`
- [ ] Exportación JSON a `assets/outputs/`
- [ ] Dashboard HTML con resumen
- [ ] Muestra de ecuación y = ax + b visiblemente

---

## 📁 Estructura de Carpetas Local

Antes de empezar, crea esta estructura en tu computadora:

```
[TU NOMBRE USUARIO]/
│
├── proyecto_calculo_[TU_API]/
│   │
│   ├── Mi_Notebook_[TU_API].ipynb  ← TU ARCHIVO
│   │
│   ├── assets/
│   │   ├── images/                  ← Aquí van PNG y SVG (se crean automáticamente)
│   │   └── outputs/                 ← Aquí va JSON (se crea automáticamente)
│   │
│   └── README.md                    ← Descripción breve de tu trabajo
```

> **NOTA:** Las carpetas `assets/images` y `assets/outputs` se crean **automáticamente** cuando ejecutas la CELDA 2.

---

## ✅ CHECKLIST ANTES DE ENVIAR

Antes de enviar tu notebook al repositorio grupal, verifica:

- [ ] **Celda 1 (Markdown):** Título, autor, fecha, documentación API
- [ ] **Celda 2 (Code):** Imports, solicitud de API key interactiva, carpetas creadas
- [ ] **Celda 3 (Code):** Mínimo 3-4 funciones con comentarios y docstrings
- [ ] **Celda 4 (Code):** Ejecución, obtención de datos, cálculo de regresión
- [ ] **Celda 5 (Code):** Gráficas (PNG + SVG), JSON, dashboard HTML

- [ ] **Archivos generados:** 
  - PNG en `assets/images/`
  - SVG en `assets/images/`
  - JSON en `assets/outputs/`

- [ ] **Código comentado:** Cada función y paso principal tiene explicación
- [ ] **Sin errores:** El notebook ejecuta sin errores (revisa dos veces)
- [ ] **API Key segura:** El token NO está escrito en el código (se solicita interactivamente)

---

## 🚀 Cómo Ejecutar tu Notebook

1. **Abre Jupyter Notebook:**
   ```bash
   jupyter notebook Mi_Notebook_[TU_API].ipynb
   ```

2. **Ejecuta de arriba a abajo:**
   - Celda 1 (Markdown) - Solo lectura
   - Celda 2 - Escribe tu API key cuando pida
   - Celda 3 - Sin interacción
   - Celda 4 - Descarga y analiza datos
   - Celda 5 - Crea gráficas

3. **Revisa los archivos generados:**
   - Gráficas en `assets/images/`
   - JSON en `assets/outputs/`

---

## 📚 Documentación de APIs por Tipo

### Si usas una API REST (GitHub, CoinGecko, OpenWeather, etc.)
- Documentación oficial es siempre: `https://docs.{nombre}.com` o similar
- Busca ejemplos en GitHub: `https://github.com/search?q={api}+python+example`

### Si usas una API de LLM (OpenAI, Anthropic, etc.)
- Instala con: `pip install openai` o `pip install anthropic`
- Documentación: `https://platform.openai.com/docs` o similar

### Si usas una API de datos financieros (CoinGecko, AlphaVantage, etc.)
- Muchas son gratuitas pero con límites de requests/minuto
- Respeta los rate limits (la CELDA 2 te ayuda con reintentos automáticos)

---

## 💡 Consejos Finales

✅ **RECOMENDADO:**
- Prueba tu notebook localmente 2-3 veces antes de enviarlo
- Usa 50-100 datos para la regresión (no hace falta millones)
- Tus comentarios deben ser claros: imagina que otro compañero lo lee
- Si tu API tiene limitaciones, úsalas de forma responsable

❌ **NO HAGAS:**
- No copies código de internet sin entenderlo
- No escribas el API key en el código
- No hagas cientos de requests sin parar (respeta rate limits)
- No olvides comentarios en las funciones

---

## 📞 Si Tienes Dudas

1. Revisa la documentación oficial de tu API
2. Pregunta a tu profesor o compañeros de grupo
3. Consulta ejemplos en StackOverflow o GitHub
4. Si algo no funciona, describe el error completo (no solo "no funciona")

---

**¡Éxito! 🚀 Tu trabajo será parte de una presentación grupal impactante.**
