---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: weather package, forecast, functions, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Weather
{: #pkg_weather}

El paquete preinstalado `/whisk.system/weather` ofrece una forma cómoda de invocar la API de Weather Company Data para {{site.data.keyword.cloud}}.
{: shortdesc}

El paquete incluye la acción siguiente.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/weather` | Paquete | `username`, `password` | Servicios de Weather Company Data para la API de {{site.data.keyword.cloud_notm}}.  |
| `/whisk.system/weather/forecast` | Acción | `latitude`, `longitude`, `timePeriod` | Previsión para el periodo de tiempo especificado. |

Se recomienda la creación de un enlace de paquete con los valores de `username` y `password`. Así, no necesita especificar las credenciales cada vez que invoque las acciones del paquete.

## Configuración del paquete de Weather en {{site.data.keyword.cloud_notm}}

Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.cloud_notm}}, se crean automáticamente los enlaces de paquete para sus instancias de servicio de {{site.data.keyword.cloud_notm}} Weather.

1. Cree una instancia de servicio de Weather Company Data en su [panel de control](https://cloud.ibm.com){: external} de {{site.data.keyword.cloud_notm}}.

  Asegúrese de recordar el nombre de la instancia de servicio y la organización y el espacio de
{{site.data.keyword.cloud_notm}} en el que se encuentra.

2. Actualice los paquetes de su espacio de nombres. La renovación crea automáticamente un enlace de paquete para la instancia de servicio de Weather Company Data que ha creado.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  created bindings:
  Weather_Company_Data_Credentials-1
  ```
  {: screen}

  Liste los paquetes para ver que se ha creado el enlace de paquete:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  packages
  /myOrg_mySpace/Weather Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## Configuración de un paquete de Weather fuera de {{site.data.keyword.cloud_notm}}

Si no utiliza {{site.data.keyword.openwhisk_short}} en {{site.data.keyword.cloud_notm}} o si quiere configurar Weather Company Data fuera de {{site.data.keyword.cloud_notm}}, debe crear manualmente un enlace de paquete para el servicio Weather Company Data. Necesita el nombre de usuario del servicio del servicio Weather Company Data y la contraseña.

Cree un enlace de paquete configurado para el servicio Weather.
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Obtención de la previsión meteorológica para una ubicación
{: #pkg_weather_forecast}

La acción `/whisk.system/weather/forecast` devuelve una previsión meteorológica para un lugar,
invocando la API de The Weather Company. Los parámetros son los siguientes.

| Parámetro | Descripción |
| --- | --- |
| `username` | Nombre de usuario de The Weather Company Data para {{site.data.keyword.cloud_notm}} que tiene autorización para invocar a la API de previsión meteorológica. |
| `password` | Contraseña de The Weather Company Data para {{site.data.keyword.cloud_notm}} que tiene autorización para invocar a la API de previsión meteorológica. |
| `latitude` | La coordenada de latitud de la ubicación. |
| `longitude` | La coordenada de longitud de la ubicación. |
| `timePeriod` | Periodo de tiempo de la previsión. |
</br>

La tabla siguiente contiene los valores que puede utilizar con el parámetro `--timePeriod`.
| Valores de `--timePeriod` | Descripción |
| --- | --- |
| `10day` | (predeterminado) Devuelve una previsión diaria de 10 días. |
| `48hour` | Devuelve una previsión horaria de 2 días. |
| `current` | Devuelve las condiciones meteorológicas actuales. |
| `timeseries` | Devuelve tanto observaciones actuales como observaciones pasadas para un máximo de 24 horas a partir de la fecha y hora actuales. |


**Ejemplo**
El siguiente ejemplo muestra cómo crear un enlace de paquete y luego obtener de una previsión a 10 días.

Invocar la acción `forecast` en su enlace de paquete para obtener la previsión meteorológica.
```
ibmcloud fn action invoke myWeather/forecast --result --param latitude 43.7 --param longitude -79.4
```
{: pre}

**Resultado**
```
{
    "forecasts": [
          {
            "dow": "Wednesday",
              "max_temp": -1,
              "min_temp": -16,
              "narrative": "Chance of a few snow showers. Highs -2 to 0C and lows -17 to -15C.",
            ...
        },
        {
            "class": "fod_long_range_daily",
              "dow": "Thursday",
              "max_temp": -4,
              "min_temp": -8,
              "narrative": "Mostly sunny. Highs -5 to -3C and lows -9 to -7C.",
            ...
        },
        ...
    ],
}
```
{: screen}


