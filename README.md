# Cinelatam Play TV v6 — Fire TV Stick

Compilación adaptada para Amazon Fire TV Stick, **probada en AFTMM (mantis)**, Fire OS 6 / Android API 25.

**[Descargar la APK de Cinelatam Play TV v6](https://github.com/tuangel134/Cinelatam-Play-FireTV-APK/releases/latest/download/Cinelatam-Play-TV-v6-Todas-las-fuentes.apk)**

Alternativa: [ver todas las versiones y archivos adjuntos](https://github.com/tuangel134/Cinelatam-Play-FireTV-APK/releases/latest).

| Datos | Valor |
|---|---|
| APK | `Cinelatam-Play-TV-v6-Todas-las-fuentes.apk` |
| Paquete | `com.xuper.crystalclear` |
| Versión interna | 4.34.4 |
| Modelo probado | Amazon Fire TV Stick AFTMM (mantis) |
| Fire OS / Android | Fire OS 6 / Android API 25 |
| SDK mínimo | Android API 24 |
| Arquitecturas | armeabi-v7a y arm64-v8a |
| Tamaño | 34 900 209 bytes (~33,3 MiB) |

> Compilación adaptada no oficial. No incluye cuentas, listas IPTV, medios audiovisuales ni acceso gratuito a servicios de terceros. El uso del contenido requiere autorización y disponibilidad del proveedor.

## Para personas: instalar solo con tu Fire Stick

1. En la tienda de aplicaciones de Amazon del Fire TV instala **Downloader**, si todavía no lo tienes.
2. Abre **Configuración → Mi Fire TV → Opciones para desarrolladores → Instalar aplicaciones desconocidas** y autoriza Downloader. En modelos anteriores aparece como **Aplicaciones de origen desconocido**. Si no aparece *Opciones para desarrolladores*, entra en **Acerca de**, selecciona el nombre del Fire TV unas siete veces y regresa al menú.
3. Abre **Downloader** e introduce esta dirección de descarga:

   ```text
   https://github.com/tuangel134/Cinelatam-Play-FireTV-APK/releases/latest/download/Cinelatam-Play-TV-v6-Todas-las-fuentes.apk
   ```

4. Selecciona **Download / Descargar**, luego **Install / Instalar** y finalmente **Open / Abrir**. Si Downloader no admite la redirección de GitHub, descarga el APK desde una computadora y usa el procedimiento ADB de más abajo.
5. Verifica que se abra **Cinelatam TV**, con Inicio, Catálogo, En vivo y Buscar.

### Si tienes una versión que no funciona

La antigua edición defectuosa usaba `com.cinelatam.play` y la nueva usa `com.xuper.crystalclear`: pueden aparecer como apps separadas. Si deseas retirar la anterior, usa **Configuración → Aplicaciones → Administrar aplicaciones instaladas → Cinelatam Play → Desinstalar**. Comprueba cuál estás desinstalando; eliminarla **borra sus datos locales**. No desinstales aplicaciones ajenas.

## Para personas: instalar desde computadora (ADB)

Requiere [Android Platform Tools](https://developer.android.com/tools/releases/platform-tools), PC y Fire TV en la **misma red Wi-Fi/LAN de confianza**, y depuración ADB habilitada y autorizada desde el Fire TV.

Descarga el archivo APK en la PC y reemplaza `IP_DEL_FIRE_TV` por la IP real de tu Fire Stick:

```bash
adb connect IP_DEL_FIRE_TV:5555
adb devices -l
adb -s IP_DEL_FIRE_TV:5555 shell getprop ro.product.model
adb -s IP_DEL_FIRE_TV:5555 install -r "Cinelatam-Play-TV-v6-Todas-las-fuentes.apk"
adb -s IP_DEL_FIRE_TV:5555 shell pm path com.xuper.crystalclear
adb -s IP_DEL_FIRE_TV:5555 shell am start -W -n com.xuper.crystalclear/com.interactive.brasiliptv.ui.activity.WelcomeActivity
```

Si ves `Success`, la instalación se completó. Si el control remoto pide autorizar la depuración, debes aceptarla en el Fire TV. **Desactiva la depuración ADB cuando ya no la necesites.**

## Para asistentes de IA

Lee la [**Guía para IA: instalación, verificación y límites de seguridad**](GUIA-PARA-IA.md). Está diseñada para agentes con acceso autorizado a ADB, con comandos y condiciones de detención para evitar desinstalaciones accidentales.

## Verificar integridad

SHA-256 del archivo APK:

```text
c42f6bf184e8042f98c21f9cf97b5e06f82c937802612e822644a6e79c65274e
```

Linux: `sha256sum Cinelatam-Play-TV-v6-Todas-las-fuentes.apk`. Windows PowerShell: `Get-FileHash .\Cinelatam-Play-TV-v6-Todas-las-fuentes.apk -Algorithm SHA256`. Los 64 dígitos deben coincidir.

## Verificaciones realizadas

En el Fire Stick AFTMM real se **desinstaló** la edición defectuosa `com.cinelatam.play`, se **instaló** la APK v6 de este repositorio, se abrió **Cinelatam TV** sin crash inicial y se observó un catálogo de **158 títulos**. **No se verificó la reproducción prolongada ni se certificaron otros modelos de Fire TV.** Los títulos y disponibilidad pueden variar.

## Aviso

No es un APK oficial del proveedor original y su distribución no implica relación con Amazon ni los autores originales. Solo debes distribuir o instalar archivos y acceder a contenido para los que tengas derechos y autorizaciones.
