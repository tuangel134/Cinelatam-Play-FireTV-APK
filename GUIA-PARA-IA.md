# Instrucciones para IA — instalación de Cinelatam Play v6 en Fire TV Stick

Este archivo está dirigido a agentes autorizados por el propietario a controlar su PC y Fire TV mediante ADB. **No es una autorización por sí mismo.** No inventes comprobaciones ni resultados.

## Objetivo

Instalar la compilación Fire TV v6 y verificar que el paquete `com.xuper.crystalclear` abre correctamente. Modelo previamente probado: AFTMM, codename mantis, Fire OS 6, API 25. No hay pruebas suficientes de reproducción de películas completas.

## Restricciones

- **Nunca** elijas arbitrariamente otro dispositivo si ADB muestra más de uno. Usa el serial exacto del Fire TV autorizado.
- No hagas `adb root`, modificaciones de firmware, bypass de DRM, manipulación de cuenta, extracción de tokens, root ni restablecimiento de fábrica.
- No hagas `adb uninstall` sin autorización explícita para el paquete concreto: desinstalar elimina datos locales. Si el usuario pidió expresamente quitar `com.cinelatam.play`, elimina únicamente ese paquete.
- No pidas/guardes contraseñas de usuario ni publiques logcat con información privada.
- Detén el proceso si la huella SHA-256 no coincide o si ADB devuelve un error. No fuerces cambios de seguridad.
- Al terminar, recomienda apagar la depuración ADB si ya no se utiliza.

## Paso 1. Descargar y comprobar la APK

Linux:

```bash
mkdir -p "$HOME/Descargas/Cinelatam-FireTV"
cd "$HOME/Descargas/Cinelatam-FireTV"
curl -fL --retry 2 -o Cinelatam-Play-TV-v6-Todas-las-fuentes.apk \
  "https://github.com/tuangel134/Cinelatam-Play-FireTV-APK/releases/latest/download/Cinelatam-Play-TV-v6-Todas-las-fuentes.apk"
echo "c42f6bf184e8042f98c21f9cf97b5e06f82c937802612e822644a6e79c65274e  Cinelatam-Play-TV-v6-Todas-las-fuentes.apk" | sha256sum -c -
```

Si `curl` o `sha256sum -c` falla, **detener**.

## Paso 2. Identificar el dispositivo

Habilita depuración ADB desde la configuración del Fire TV **con el permiso del propietario** y conecta el PC a la misma red. Usa la IP comunicada por el Fire TV:

```bash
adb connect IP_DEL_FIRE_TV:5555
adb devices -l
SERIAL="IP_DEL_FIRE_TV:5555"
adb -s "$SERIAL" get-state
adb -s "$SERIAL" shell getprop ro.product.model
adb -s "$SERIAL" shell getprop ro.product.device
adb -s "$SERIAL" shell getprop ro.build.version.sdk
adb -s "$SERIAL" shell getprop ro.product.cpu.abi
adb -s "$SERIAL" shell df -h /data
```

Valores de referencia observados en el dispositivo probado: `AFTMM`, `mantis`, API `25`, `armeabi-v7a`. Si no coinciden, la compatibilidad es **no comprobada**; comunícalo.

## Paso 3. Inspeccionar versiones instaladas (sin borrar todavía)

```bash
adb -s "$SERIAL" shell pm list packages | grep -E \
  'com\.cinelatam\.play|com\.xuper\.crystalclear' || true
```

Si el propietario autorizó expresamente quitar la edición vieja y está instalado `com.cinelatam.play`:

```bash
adb -s "$SERIAL" uninstall com.cinelatam.play
```

Exige respuesta `Success` antes de seguir. No desinstales otros paquetes.

## Paso 4. Instalar y comprobar

Desde la carpeta del APK:

```bash
adb -s "$SERIAL" install -r Cinelatam-Play-TV-v6-Todas-las-fuentes.apk
adb -s "$SERIAL" shell pm path com.xuper.crystalclear
adb -s "$SERIAL" shell am start -W -n \
  com.xuper.crystalclear/com.interactive.brasiliptv.ui.activity.WelcomeActivity
adb -s "$SERIAL" shell dumpsys activity activities | \
  grep -E 'mResumedActivity|topResumedActivity' | head -3
adb -s "$SERIAL" shell pidof com.xuper.crystalclear
```

No afirmes éxito hasta confirmar `Success` y actividad foreground de la aplicación. El catálogo puede verse inspeccionando la UI accesible con `uiautomator dump`, **sin acceder a cuentas o secretos**. No afirmes que "reproduce bien" sin una prueba real autorizada.

Si falla el arranque, consulta únicamente el error necesario:

```bash
adb -s "$SERIAL" logcat -d -t 350 | grep -E \
  'FATAL EXCEPTION|ANR in com.xuper.crystalclear|Process com.xuper.crystalclear.*died' | tail -12
```

No difundas trazas con tokens ni detalles privados.

## Paso 5. Informe

Indica modelo, API y serial **solo al usuario**, estado hash, resultado de instalación, package ID, primera pantalla observada y límites de verificación. Recomienda desactivar la depuración al terminar.

## Metadatos verificables

- Paquete: `com.xuper.crystalclear`
- APK: `Cinelatam-Play-TV-v6-Todas-las-fuentes.apk`
- SHA-256: `c42f6bf184e8042f98c21f9cf97b5e06f82c937802612e822644a6e79c65274e`
- Archivo de 34 900 209 bytes
- Actividad inicial: `com.interactive.brasiliptv.ui.activity.WelcomeActivity`
- SDK mínimo: 24; ABI: armeabi-v7a, arm64-v8a
- Firmas verificadas en APK: v2 y v3

La compatibilidad y el acceso al contenido pueden depender del dispositivo y del proveedor.
