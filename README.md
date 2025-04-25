## Open-meteo forecast retrieval CLI

This program was primarily written to display
[graphcast](https://deepmind.google/discover/blog/graphcast-ai-model-for-faster-and-more-accurate-global-weather-forecasting/)
forecasts, which have been [integrated into
open-meteo](https://openmeteo.substack.com/p/exploring-graphcast)
since April 2024.

## Usage

The program is invoked as `forecast place [date]`.

Place can be a place name like `london`, `new york`, or `london,
canada`, which will be resolved using OpenStreetmap.  It can also be
latitude/longitude
pair, such as `45.8150,15.9819`. 

Date is specified as `YYYY-MM-DD` or shorthands `today` or `tomorrow`.
If omitted, date defaults to today.

For example:
```
$ forecast london        
gfs_graphcast025 forecast for London, Greater London, England, United Kingdom
Date       Hour  Temp[°C] Rain[mm]
2025-04-25 22:00     10.9        0
           23:00      9.9        0
```

Forecast for tomorrow:
```
$ forecast zagreb tomorrow
gfs_graphcast025 forecast for Grad Zagreb, Hrvatska
Date       Hour  Temp[°C] Rain[mm]
2025-04-26 00:00     11.9      0.4
           01:00     11.7      0.4
           02:00     11.6      0.4
           03:00     11.5      0.3
           04:00     11.4      0.3
           05:00     11.4      0.3
...
```

Forecast for arbitrary date in the future (within 16 days):
```
$ forecast hutovo 2025-05-04
gfs_graphcast025 forecast for Hutovo, Općina Neum, Hercegovačko-neretvanska županija (kanton), Federacija Bosne i Hercegovine, Bosna i Hercegovina / Босна и Херцеговина
Date       Hour  Temp[°C] Rain[mm]
2025-05-04 00:00     11.9        0
           01:00     11.4        0
           02:00     11.1        0
           03:00     11.2        0
           04:00     11.4        0
           05:00     11.8        0
...
```
