## Open-meteo forecast retrieval CLI

This program retrieves temperature and precipitation forecast from Open-Meteo. It is
designed to make it simple to retrieve from different models exposed by open-meteo, in
particular the [DeepMind
GraphCast](https://deepmind.google/discover/blog/graphcast-ai-model-for-faster-and-more-accurate-global-weather-forecasting/)
AI-based model, which Google claims to be more accurate than ECMWF's state-of-the-art HRES
model. GraphCast forecasts have been [integrated into
Open-Meteo](https://openmeteo.substack.com/p/exploring-graphcast) since April 2024.

The program makes it particularly easy to compare forecast data from different models.

## Invocation

`forecast` takes a location and date range:

```
forecast zagreb tomorrow
```

Location is typically a place name such as `London`, `New York`, or more detailed `London,
Ontario`. Names are resolved using OpenStreetmap's [Nominatim search
API](https://nominatim.org/release-docs/develop/api/Search/). You can also directly
specify geographic coordinate such as pair of latitude and longitude, such as
`45.8150,15.9819`. In that case the Nominatim lookup will be omitted and the coordinate
used directly.

The date range is specified as a single date or a beginning and end date. `today` will
show today's forecast, `tomorrow` that of tomorrow, and `YYYY-MM-DD` the forecast of a
particular day in the future (within the 16-day window supported by Open-Meteo). Range is
specified as `START..END`, such as `2025-04-27..2025-05-01`. Ranges are inclusive, so the
range shown would include May 1st.  Combinations like `today..tomorrow` do what you'd
expect. If the date range argument is omitted altogether, today's forecast is shown.

## Output

The program outputs forecast data as a text table. The first column is the date (with
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

## Models

You can use the `--models` argument to specify a comma-separated list of forecast models
to retrieve.  Each model will add two columns, one for temperature and one for
precipitation.  The default is to retrieve and show GraphCast (`gfs_graphcast025`) and
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

## Examples

```
$ forecast london
Forecast for London, Greater London, England, United Kingdom (g=gfs_graphcast025, e=ecmwf_ifs025)
Date       Hour T/°C (g) Rain/mm (g) T/°C (e) Rain/mm (e)
2025-04-27   02      7.5           0      9.4           0
             03      7.1           0      8.9           0
             04      6.9           0      8.6           0
             05      6.9           0      8.4           0
             06      7.2           0      8.3           0
             07      7.9           0      8.9           0
             08      9.2           0     10.5           0
             09     11.1           0     12.8           0
             10     13.3           0     14.8           0
             11     15.4           0     16.5           0
             12     17.4           0     18.0           0
             13     18.6           0     19.1           0
             14     19.2           0     19.6           0
             15     19.5           0     19.7           0
             16     19.4           0     19.6           0
             17     19.0           0     19.5           0
             18     18.5           0     19.4           0
             19     17.9           0     18.9           0
             20     16.9           0     18.0           0
             21     15.5           0     16.8           0
             22     13.9           0     15.6           0
             23     12.4           0     14.6           0
```

```
$ forecast zagreb tomorrow..2025-04-30
Forecast for Grad Zagreb, Hrvatska (g=gfs_graphcast025, e=ecmwf_ifs025)
Date       Hour T/°C (g) Rain/mm (g) T/°C (e) Rain/mm (e)
2025-04-28   00     11.4           0     10.5           0
             01     10.6           0      9.8           0
             02     10.0           0      9.4           0
             03      9.8           0      9.6           0
             04      9.9           0     10.1           0
             05     10.1           0     10.7           0
             06     10.6           0     11.1           0
             07     11.2           0     11.4           0
             08     12.1           0     12.2           0
             09     13.4           0     13.6           0
             10     15.1           0     15.2           0
             11     17.1           0     16.8           0
             12     19.0           0     17.8           0
             13     20.5           0     18.6           0
             14     21.5           0     19.2           0
             15     21.6           0     19.6           0
             16     21.2           0     19.9           0
             17     20.5           0     19.6           0
             18     19.5           0     18.6           0
             19     18.5           0     17.2           0
             20     17.6           0     16.0           0
             21     16.6           0     15.3           0
             22     15.4           0     14.7           0
             23     14.2           0     14.1           0
2025-04-29   00     13.0           0     13.2           0
             01     12.1           0     12.3           0
             02     11.4           0     11.5           0
             03     11.1           0     10.8           0
             04     11.1           0     10.2           0
             05     11.2           0     10.1           0
             06     11.6           0     10.6           0
             07     12.2           0     11.6           0
             08     13.0           0     12.9           0
             09     14.3           0     14.6           0
             10     16.1           0     16.6           0
             11     18.1           0     18.3           0
             12     20.1           0     19.4           0
             13     21.6           0     20.2           0
             14     22.5           0     20.7           0
             15     22.5           0     21.0           0
             16     22.0           0     20.9           0
             17     21.0           0     20.4           0
             18     19.8           0     19.0           0
             19     18.5           0     17.3           0
             20     17.5           0     15.7           0
             21     16.4           0     14.3           0
             22     15.2           0     13.0           0
             23     13.9           0     12.1           0
2025-04-30   00     12.8           0     11.5           0
             01     11.9           0     11.2           0
             02     11.3           0     10.9           0
             03     11.1           0     10.4           0
             04     11.0           0      9.8           0
             05     11.1           0      9.6           0
             06     11.6           0     10.1           0
             07     12.2           0     11.1           0
             08     13.2           0     12.5           0
             09     14.9           0     14.4           0
             10     17.0           0     16.8           0
             11     19.5           0     18.6           0
             12     22.0           0     19.8           0
             13     23.9           0     20.4           0
             14     25.0           0     20.9           0
             15     25.0           0     21.1           0
             16     24.5           0     21.2           0
             17     23.4           0     20.9           0
             18     22.0           0     19.8           0
             19     20.7           0     18.1           0
             20     19.5           0     16.7           0
             21     18.4           0     15.4           0
             22     17.1           0     14.2           0
             23     15.7           0     13.2           0
```
