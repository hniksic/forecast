## Simple Open-Meteo retrieval CLI

This program retrieves temperature and precipitation forecast and current data from
Open-Meteo. It is designed to make it simple to retrieve and compare forecasts by
different models exposed by open-meteo, in particular the [DeepMind
GraphCast](https://deepmind.google/discover/blog/graphcast-ai-model-for-faster-and-more-accurate-global-weather-forecasting/)
AI-based model, which Google claims to be more accurate than ECMWF's state-of-the-art HRES
model. GraphCast forecasts have been [integrated into
Open-Meteo](https://openmeteo.substack.com/p/exploring-graphcast) since April 2024.

## Forecast

The `forecast` subcommand shows the forecast for a location and a range of dates:

```
openmeteo forecast zagreb tomorrow
```

Location is typically a place name such as `London`, `New York`, or more detailed `London,
Ontario`. Names are resolved using OpenStreetmap's [Nominatim search
API](https://nominatim.org/release-docs/develop/api/Search/). You can also directly
specify geographic coordinate such as pair of latitude and longitude, such as
`45.8150,15.9819`. In that case the Nominatim lookup will be omitted and the coordinate
used directly.

The date range is either a single date or a pair of beginning and end dates separated by
`..`. Date can be formatted as `YYYY-MM-DD`, or you can use shorthands `today`,
`tomorrow`, or a day of the week. Range is specified as `START..END`, such as
`2025-04-27..2025-05-01` or `mon..thu`. Note that ranges are inclusive, so the range
`mon..thu` includes Thursday. If the date range argument is omitted altogether, today's
forecast is shown.

The program outputs forecast as a text table. The first column is the date (with
consecutive equal dates omitted for readability), followed by hour, and then by
temperature and precipitation for each forecast model:

```
$ forecast zagreb today
Forecast for Grad Zagreb, Hrvatska (g=gfs_graphcast025, e=ecmwf_ifs025)
Date       Hour T/°C (g) Rain/mm (g) T/°C (e) Rain/mm (e)
2025-04-27   10     13.5           0     12.9           0
             11     15.2           0     14.0           0
             12     16.9           0     15.1           0
             13     18.2           0     16.3           0
             14     19.0           0     17.1           0
             15     19.0           0     17.6           0
             16     18.8           0     17.9           0
             17     18.1           0     17.6           0
             18     17.2           0     16.6           0
             19     16.4           0     15.2           0
             20     15.6           0     13.9           0
             21     14.6           0     13.1           0
             22     13.6           0     12.4           0
             23     12.4           0     11.7           0
```

## Current weather

The `current` subcommand shows the current weather for a location:

```
openmeteo current zagreb
Current weather for Grad Zagreb, Hrvatska
Time             T/°C Rain/mm
2025-04-27 22:15 15.0       0
```

The location is interpreted the same as for `forecast`.

## Forecast models

You can use the `--models` argument to `forecast` to specify a comma-separated list of
forecast models to retrieve.  Each model will add two columns, one for temperature and one
for precipitation.  The default is to retrieve and show GraphCast (`gfs_graphcast025`) and
classic ECMWF (`ecmwf_ifs025`) forecasts.

Here is the current list of known models, taken from [the
documentation](https://open-meteo.com/en/docs). Note that not all of those work on all
locations, and many of them don't support full 16 days.

* `best_match`
* `bom_access_global`
* `cma_grapes_global`
* `dmi_harmonie_arome_europe`
* `dmi_seamless`
* `ecmwf_aifs025_single`
* `ecmwf_ifs025`
* `gem_global`
* `gem_hrdps_continental`
* `gem_regional`
* `gem_seamless`
* `gfs_global`
* `gfs_graphcast025`
* `gfs_hrrr`
* `gfs_seamless`
* `icon_d2`
* `icon_eu`
* `icon_global`
* `icon_seamless`
* `italia_meteo_arpae_icon_2i`
* `jma_gsm`
* `jma_msm`
* `jma_seamless`
* `kma_gdps`
* `kma_ldps`
* `kma_seamless`
* `knmi_harmonie_arome_europe`
* `knmi_harmonie_arome_netherlands`
* `knmi_seamless`
* `meteofrance_arome_france_hd`
* `meteofrance_arome_france`
* `meteofrance_arpege_europe`
* `meteofrance_arpege_world`
* `meteofrance_seamless`
* `metno_nordic`
* `metno_seamless`
* `ncep_nbm_conus`
* `ukmo_global_deterministic_10km`
* `ukmo_seamless`
* `ukmo_uk_deterministic_2km`
