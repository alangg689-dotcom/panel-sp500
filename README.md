# SP&500

Aplicación Android con un panel de mercado en español (México) para quien empieza a invertir. Incluye gráfica, mapa del S&P 500, movimientos, screener, calendario, noticias, una guía breve y un simulador de paper trading. Los datos de mercado salen de widgets de TradingView. La cartera simulada sale de un JSON público del repositorio.

**Herramienta educativa.** Los datos pueden ir retrasados. El simulador es una cartera ficticia. Esto no es asesoría financiera ni una recomendación de compra o venta.

## Descargar e instalar el APK

La versión actual es **v1.1.0**. GitHub Actions la compila al actualizar `main` (y también se puede lanzar a mano) y publica el archivo en esa versión:

- Página de la versión: https://github.com/alangg689-dotcom/panel-sp500/releases/tag/v1.1.0
- Descarga directa: https://github.com/alangg689-dotcom/panel-sp500/releases/download/v1.1.0/sp500-panel.apk

En el teléfono Android:

1. Abre el enlace de descarga y guarda `sp500-panel.apk` (desde el navegador del teléfono, o pásalo desde la computadora).
2. Android no instala paquetes que no vienen de Play Store hasta que lo permites. Al abrir el APK, si sale el aviso, toca **Ajustes** y activa **Permitir de esta fuente** para el navegador o la app de archivos que usaste.
3. También puedes activarlo tú antes: **Ajustes → Seguridad y privacidad → Instalar apps desconocidas** (en algunas versiones: **Ajustes → Apps → Acceso especial → Instalar aplicaciones desconocidas**) y activa el permiso para Chrome, Archivos o la aplicación desde la que descargaste.
4. Vuelve a abrir `sp500-panel.apk` y toca **Instalar**.
5. Abre **SP&500**. Hace falta internet para cargar las gráficas, las cotizaciones y el simulador.

La versión **v1.0.0** se firmó con una llave de un solo uso que no se guardó. Si ya la tienes instalada, desinstálala antes de instalar la v1.1.0. Android rechaza una actualización cuando la firma no coincide.

El botón Atrás regresa dentro del panel y, si no hay a dónde volver, cierra la app. Los enlaces que salen del panel (por ejemplo TradingView o el JSON) se abren en el navegador del teléfono.

## Simulador

La pestaña **Simulador** lee este archivo de la rama `main`:

https://raw.githubusercontent.com/alangg689-dotcom/panel-sp500/main/data/simulador.json

Muestra el valor actual de la cartera ficticia frente a comprar y mantener el S&P 500, el rendimiento, la peor caída, una gráfica sencilla y las últimas operaciones. Si el archivo todavía no existe (o viene vacío), la pestaña dice: **La simulación aún no tiene datos**.

El formato está documentado con cifras inventadas en [`data/simulador.example.json`](data/simulador.example.json). Ese archivo es solo un ejemplo: la app no lo usa como cartera. Para publicar datos, copia la estructura, quita las marcas de ejemplo y súbela como `data/simulador.json`.

## Firma estable del APK

Cada compilación puede firmar el APK con la misma llave si el repositorio tiene estos secretos de Actions (Settings → Secrets and variables → Actions):

| Secreto | Contenido |
| --- | --- |
| `ANDROID_KEYSTORE_BASE64` | El archivo `.keystore` codificado en base64, en una sola línea |
| `ANDROID_KEYSTORE_PASSWORD` | Contraseña del keystore |
| `ANDROID_KEY_ALIAS` | Alias de la clave (en el ejemplo, `sp500`) |
| `ANDROID_KEY_PASSWORD` | Contraseña de la clave |

Con los cuatro secretos, las versiones siguientes se instalan encima de la anterior. Hay que subir `versionCode` en `android/app/build.gradle` en cada versión nueva. No subas el keystore ni las contraseñas al repositorio.

Si falta alguno, el flujo genera una llave temporal solo para esa corrida. En ese caso Android pide desinstalar la app antes de instalar el APK nuevo.

Para crear la llave en tu computadora y guardarla como secreto:

```bash
keytool -genkeypair -v \
  -keystore sp500-release.keystore \
  -alias sp500 \
  -keyalg RSA -keysize 2048 -validity 10000 \
  -storepass 'TU_PASSWORD' -keypass 'TU_PASSWORD' \
  -dname "CN=SP500 Panel, OU=Education, O=SP500, L=Mazatlan, ST=Sinaloa, C=MX"

base64 -w 0 sp500-release.keystore | gh secret set ANDROID_KEYSTORE_BASE64 --repo alangg689-dotcom/panel-sp500
gh secret set ANDROID_KEYSTORE_PASSWORD --repo alangg689-dotcom/panel-sp500
printf '%s' sp500 | gh secret set ANDROID_KEY_ALIAS --repo alangg689-dotcom/panel-sp500
gh secret set ANDROID_KEY_PASSWORD --repo alangg689-dotcom/panel-sp500
```

`gh secret set` de una contraseña pide el valor por la terminal (no lo pongas en el comando, para que no quede en el historial). Borra `sp500-release.keystore` de la computadora cuando ya esté en los secretos, o guárdalo fuera del repositorio. En macOS, `base64` no usa `-w 0`; sirve `base64 < sp500-release.keystore | tr -d '\n'`.

Este entorno de automatización no tiene permiso para crear secretos del repositorio. Hasta que existan los cuatro, la v1.1.0 y las compilaciones que sigan se firman con una llave nueva cada vez.

## Aviso

Este panel es solo para aprender. Las cotizaciones y noticias vienen de widgets gratuitos de TradingView y **pueden tener retraso** (en acciones de Estados Unidos suele ser de varios minutos). El simulador es paper trading: números de práctica, no una cuenta real. No es asesoría financiera, no es una recomendación de compra o venta, y no sustituye a una casa de bolsa regulada. Operar en el corto plazo es arriesgado y la mayoría de quienes empiezan pierde dinero.

## El panel en el repositorio

El contenido de la app es el archivo `index.html` de la raíz. La carpeta `android/` lo empaca en un WebView (versión de la app 1.1.0, `versionCode` 2). El flujo `.github/workflows/android-release.yml` arma el APK al empujar a `main` o al lanzarlo a mano, y lo adjunta a la versión `v1.1.0` con el nombre `sp500-panel.apk`.
