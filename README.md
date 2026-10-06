# SP&500

Aplicación Android con un panel de mercado en español (México) para quien empieza a invertir. Incluye gráfica, mapa del S&P 500, movimientos, screener, calendario, noticias y una guía breve. Los datos salen de widgets de TradingView.

**Herramienta educativa.** Los datos pueden ir retrasados. Esto no es asesoría financiera ni una recomendación de compra o venta.

## Descargar e instalar el APK

Cada vez que se actualiza `main`, GitHub Actions compila el APK y lo publica en la versión **v1.0.0**:

- Página de la versión: https://github.com/alangg689-dotcom/panel-sp500/releases/tag/v1.0.0
- Descarga directa: https://github.com/alangg689-dotcom/panel-sp500/releases/download/v1.0.0/sp500-panel.apk

En el teléfono Android:

1. Abre el enlace de descarga y guarda `sp500-panel.apk` (desde el navegador del teléfono, o pásalo desde la computadora).
2. Android no instala paquetes que no vienen de Play Store hasta que lo permites. Al abrir el APK, si sale el aviso, toca **Ajustes** y activa **Permitir de esta fuente** para el navegador o la app de archivos que usaste.
3. También puedes activarlo tú antes: **Ajustes → Seguridad y privacidad → Instalar apps desconocidas** (en algunas versiones: **Ajustes → Apps → Acceso especial → Instalar aplicaciones desconocidas**) y activa el permiso para Chrome, Archivos o la aplicación desde la que descargaste.
4. Vuelve a abrir `sp500-panel.apk` y toca **Instalar**.
5. Abre **SP&500**. Hace falta internet para cargar las gráficas y cotizaciones.

Si al instalar una versión nueva Android dice que entra en conflicto con la app ya instalada, desinstala la anterior y vuelve a instalar el APK. Cada publicación se firma con una clave nueva, así que no siempre se puede actualizar encima.

El botón Atrás regresa dentro del panel y, si no hay a dónde volver, cierra la app. Los enlaces que salen del panel (por ejemplo TradingView) se abren en el navegador del teléfono.

## Aviso

Este panel es solo para aprender. Las cotizaciones y noticias vienen de widgets gratuitos de TradingView y **pueden tener retraso** (en acciones de Estados Unidos suele ser de varios minutos). No es asesoría financiera, no es una recomendación de compra o venta, y no sustituye a una casa de bolsa regulada. Operar en el corto plazo es arriesgado y la mayoría de quienes empiezan pierde dinero.

## El panel en el repositorio

El contenido de la app es el archivo `index.html` de la raíz. La carpeta `android/` lo empaca en un WebView. El flujo `.github/workflows/android-release.yml` arma el APK al empujar a `main` o al lanzarlo a mano, y lo adjunta a la versión `v1.0.0` con el nombre `sp500-panel.apk`.
