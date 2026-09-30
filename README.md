# EC_XR_QUISPE-SEBASTIAN - Laboratorio de Realidad Extendida

## Información del Estudiante
- **Nombre:** Quispe Sebastian
- **Curso:** Laboratorio de Realidad Extendida (XR) para VideoJuegos
- **Universidad:** Universidad Autónoma del Perú
- **Fecha:** 2026

---

## Requisitos del Sistema

| Software | Versión |
|----------|---------|
| Unity | 2022.3 LTS (2022.3.62f1 o superior) |
| XR Interaction Toolkit | 2.5.4 |
| AR Foundation | 5.1.5 |
| OpenXR Plugin | 1.10.0 |
| Meta Quest | 2 o 3 |

---

## Estructura del Proyecto

```
EC_XR_QUISPE-SEBASTIAN/
├── Assets/
│   ├── Scripts/
│   │   ├── GripColorChange.cs      # RETO 3: Interacción Grip vs Trigger
│   │   ├── ImageTargetSpawner.cs   # RETO 2: Image Target AR
│   │   ├── PerformanceSettings.cs  # RETO 4: Optimización FPS
│   │   └── SceneDiagnostics.cs     # RETO 1: Debugging de Escena
│   ├── Editor/
│   │   └── ECSetup.cs              # Menú de configuración automática
│   ├── Scenes/
│   │   ├── EC_VR.unity             # Escena VR (crear con menú)
│   │   └── EC_AR.unity             # Escena AR (crear con menú)
│   ├── Prefabs/
│   │   └── InteractableCube.prefab # Objeto interactuable
│   ├── ImageTargets/
│   │   └── target_ec.png           # Imagen para Image Target
│   └── Settings/
│       └── (Archivos URP generados automáticamente)
├── Packages/
│   └── manifest.json
└── ProjectSettings/
```

---

## Configuración Rápida (Menú EC XR)

El proyecto incluye un menú de editor **"EC XR"** en la barra superior de Unity:

### Paso 1: Configurar Rendimiento
```
EC XR > 1 - Configurar rendimiento (URP, AA, Render Scale)
```
- Crea URP Asset con MSAA 4x
- Configura Render Scale 0.9
- Aplica a todos los Quality Levels

### Paso 2: Configurar Android
```
EC XR > 2 - Configurar Android (Quest)
```
- IL2CPP scripting backend
- ARM64 architecture
- API Level 26 mínimo
- OpenGLES3

### Paso 3: Crear Escena VR
```
EC XR > 3 - Crear escena VR (Quest)
```
- Crea XR Origin con Controllers
- Cubo interactuable con GripColorChange
- PerformanceSettings y SceneDiagnostics

### Paso 4: Crear Escena AR
```
EC XR > 4 - Crear escena AR (Image Target)
```
- Crea AR Session y AR Session Origin
- Configura ARTrackedImageManager
- Asigna el prefab InteractableCube

### Paso 5: Ejecutar Diagnósticos
```
EC XR > 5 - Ejecutar diagnósticos de escena
```
- Verifica XR Origin
- Verifica Controllers
- Verifica AR Components
- Verifica Input Actions

---

## Los 4 Retos de la Práctica

### RETO 1: Debugging de Escena
**Archivo:** `SceneDiagnostics.cs`

**Objetivo:** Verificar que la escena esté correctamente configurada para XR.

**Verificaciones:**
- XR Origin existe en la escena
- XR Interaction Manager existe
- XR Controllers están configurados
- AR Components (si aplica)
- Input Actions están asignadas

**Solución de errores comunes:**
| Error | Solución |
|-------|----------|
| No XR Origin | GameObject > XR > XR Origin (Action-based) |
| No XR Interaction Manager | GameObject > XR > XR Interaction Manager |
| No Controllers | Agregar XR Controller (Left/Right) al XR Origin |
| Input Actions no funcionan | Importar Starter Assets del XR Interaction Toolkit |

---

### RETO 2: Implementación AR - Image Target
**Archivo:** `ImageTargetSpawner.cs`

**Objetivo:** Al detectar un Image Target específico, instanciar un objeto interactuable.

**Pasos para configurar:**
1. Crear un **Reference Image Library**:
   - `Assets > Create > XR > Reference Image Library`
   - Agregar la imagen `target_ec.png` desde `Assets/ImageTargets/`

2. Configurar el ARTrackedImageManager:
   - Seleccionar el AR Session Origin
   - En el Inspector, asignar el Reference Image Library

3. Asignar el prefab:
   - Seleccionar el AR Session Origin
   - En ImageTargetSpawner, asignar `InteractableCube` en el campo `Prefab`

**Comportamiento:**
- Al detectar la imagen, instancia el cubo interactuable
- El cubo sigue la posición de la imagen mientras se rastrea
- Si se pierde el tracking, el cubo se oculta (configurable)

---

### RETO 3: Configuración de Interacción - Grip vs Trigger
**Archivo:** `GripColorChange.cs`

**Objetivo:** El objeto cambia de color SOLO cuando es agarrado con "Grip" (Select) y NO con "Trigger" (Activate).

**Configuración correcta:**
```csharp
// En OnEnable():
interactable.selectEntered.AddListener(OnSelectEntered);   // Grip
interactable.selectExited.AddListener(OnSelectExited);     // Grip release
interactable.activated.AddListener(OnActivated);           // Trigger (solo debug)
```

**Verificación en el XR Controller:**
- **Select Action** debe estar mapeado a **Grip** (botón de agarre)
- **Activate Action** debe estar mapeado a **Trigger** (gatillo)

**Para verificar:**
1. Abrir el Input Action Asset
2. Verificar que `XRController > selectAction` usa el control `grip`
3. Verificar que `XRController > activateAction` usa el control `trigger`

---

### RETO 4: Optimización Inicial
**Archivo:** `PerformanceSettings.cs`

**Objetivo:** Ajustar Anti-aliasing y Render Scale para mantener el target FPS.

**Valores configurados:**
| Parámetro | Valor | Descripción |
|-----------|-------|-------------|
| Target FPS | 72 | Quest 2 (90 para Quest 3) |
| Render Scale | 0.9 | Balance calidad/rendimiento |
| MSAA | 4x | Anti-aliasing |
| Shadow Distance | 20m | Reducido para mejor rendimiento |
| vSync | Disabled | VR maneja su propio sync |

**Para ajustar según el visor:**
- **Quest 2:** FPS 72, Render Scale 0.9
- **Quest 3:** FPS 90, Render Scale 1.0

---

## Importar Starter Assets (IMPORTANTE)

Para que los Controllers funcionen correctamente:

1. Abrir **Window > Package Manager**
2. Seleccionar **XR Interaction Toolkit**
3. Ir a la pestaña **Samples**
4. Importar **Starter Assets**

Esto creará los Input Actions necesarios para los Controllers.

---

## Probar en Meta Quest

### Opción 1: Unity Editor (Simulación)
1. Instalar **XR Device Simulator** (Package Manager)
2. Activar **Window > XR > XR Device Simulator**
3. Presionar Play

### Opción 2: Build & Run
1. Conectar Quest por USB o Air Link
2. `File > Build Settings > Android`
3. Seleccionar Quest como dispositivo
4. Click en **Build And Run**

---

## Entregables de la Práctica

1. **Repositorio GitHub** con el proyecto corregido
2. **Video 30 segundos** demostrando:
   - El cubo cambia de color al agarrarlo con Grip
   - NO cambia con Trigger
   - El Image Target instancia el objeto
3. **PDF** con capturas de pantalla y enlace al repositorio

---

## Referencias

- [Unity Manual: XR](https://docs.unity3d.com/Manual/XR.html)
- [XR Interaction Toolkit](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@2.5/manual/index.html)
- [AR Foundation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@5.1/manual/index.html)
- [OpenXR Validation Layers](https://www.khronos.org/openxr/)

---

## Soporte

Si tienes problemas con la configuración:
1. Revisa la consola de Unity (Window > General > Console)
2. Ejecuta los diagnósticos: `EC XR > 5 - Ejecutar diagnósticos de escena`
3. Verifica que todos los paquetes estén instalados correctamente
