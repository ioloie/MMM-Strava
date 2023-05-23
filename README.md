# MagicMirror Module: MMM-Strava

A MagicMirror Module for displaying your Strava data.

[![Platform](https://img.shields.io/badge/platform-MagicMirror-informational)](https://MagicMirror.builders)
[![License](https://img.shields.io/badge/license-MIT-informational)](https://raw.githubusercontent.com/ianperrin/MMM-Strava/master/LICENSE)
![Test Status](https://github.com/ianperrin/MMM-Strava/actions/workflows/node.js.yml/badge.svg)
[![Code Climate](https://codeclimate.com/github/ianperrin/MMM-Strava/badges/gpa.svg)](https://codeclimate.com/github/ianperrin/MMM-Strava)
[![Known Vulnerabilities](https://snyk.io/test/github/ianperrin/MMM-Strava/badge.svg)](https://snyk.io/test/github/ianperrin/MMM-Strava)

## Example

![Table mode screenshot](.github/example-table.gif) ![Chart mode screenshot](.github/example-chart.gif)

### The module displays activity information in one of two modes

- `table` mode, which displays a table showing the number of activities, the total distance and (for recent `period` only) the number of achievements.
- `chart` mode, which displays the total distance, moving time and elevation gain alongside a chart showing the total distance grouped by
  - day (for `recent` period),
  - month (for `ytd` period),
  - year (for `all` period).

### In addition you can configure the following options

- Which `activities` (and the order activities) should be displayed.
- Which `stats` should be displayed.
  - The number of activities (`count`).
  - The total distance (`distance`).
  - The total elevation gain (`elevation`).
  - The total moving time for the period. (`moving_time`).
  - The total elapsed time for the period. (`elapsed_time`).
  - The total number of achievements (`achievements`) - in `table` mode, only available for `recent` period).
- Which `period` to display stats for your activities:
  - "recent" - last 4 weeks in `table` mode or current week in `chart` mode.
  - "ytd" - current year to date.
  - "all" - all time.
- Which `chartType` should be used in `chart` mode:
  - "bar" - a simple bar chart.
  - "radial" - a radial histogram ![Beta](https://img.shields.io/badge/-BETA-red).
- The `firstYear` to group activities by in `chart` mode when the `period` is "all".
- Whether the module should `auto_rotate` through the different periods, and the `updateInterval` between rotations. (only applicable in `table` mode).
- The `units` (miles/feet or kilometres/metres) used to display the total distance and elevation gain statistics.
- The `locale` used for determining the date (day or month) labels in `chart` mode and number format in both modes.
- The number of decimal `digits` displayed for the total distance and elevation gain statistics.

## Installation

1. Stop your MagicMirror and clone the repository into the modules folder

   ```bash
   cd ~/MagicMirror/modules
   git clone https://github.com/ianperrin/MMM-Strava.git
   cd ~/MagicMirror/modules/MMM-Strava
   npm install --production
   ```

2. Create a Strava API Application and note the `client_id` and `client_secret`

   - Browse to your [My API Application](https://www.strava.com/settings/api) page and log in to Strava if prompted.
   - Make sure the callback domain matches the IP address (or URL) used to access the MagicMirror.
   - Make a note of the `client_id` and `client_secret`

3. Add the module to the config file (`~/MagicMirror/config/config.js`) for your mirror.

   ```javascript
   modules: [
     {
       module: "MMM-Strava",
       position: "top_right",
       config: {
         client_id: "your_strava_client_id",
         client_secret: "your_strava_api_client_secret"
       }
     }
   ];
   ```

   The full list of config options can be found in the [configuration options](#configuration-options) table.

4. Restart the MagicMirror

   ```bash
   pm2 restart mm
   ```

5. Authenticate the module to allow access to the Strava API.

   - Browse to the Strava authentication page: [http://localhost:8080/MMM-Strava/auth/](http://localhost:8080/MMM-Strava/auth/) - _the exact URL may vary depending on your configuration._
   - Select the module you wish to authenticate (e.g. `module_4_MMM-Strava`) and click/tap _Authorise_ -_The number of the modules will vary depending on your configuration._
   - On the Strava Authorisation page, select the level of access you wish to give to the Magic Mirror, and click/tap _Authorize_ - _the module requires at least `View data about your public profile` and `View data about your activities` but it's up to you whether you want to allow access to `private activities`._
   - Once the successful authorisation message appears, restart your Magic Mirror.

## Updating the module

To update the module to the latest version,

1. Pull the changes from this repository into the MMM-Strava folder:

   ```bash
   cd ~/MagicMirror/modules/MMM-Strava
   git pull
   npm install --production
   ```

2. Update your config file to remove the `strava_id` and `access_token` options and add the new `client_id` and `client_secret` options - _See steps 2 and 4 in the [installation notes](#installation)_.

**Please Note** Following the changes to Strava’s [authentication model](https://developers.strava.com/docs/authentication/), the client_id and client_secret must be included in the config in place of the deprecated strava_id and access_token options.

If you haven't changed the module, this should work without any problems. If you have a problem, you can reset the module using `git reset --hard`, after which `git pull` should be possible. You may wish to use `git status` to see any changes before doing so.

## Configuration options

The following properties can be added to the configuration:

| **Option**                                                  | **Default**                                                                                                                 | **Description**                                                                                                                                                                                                                                                                    | **Possible Values**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `client_id`                                                 |                                                                                                                             | _Required_ - The Client ID for your Strava API Application, obtained from [your My API Application page](https://www.strava.com/settings/api).                                                                                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `client_secret`                                             |                                                                                                                             | _Required_ - The Client Secret for your Strava API Application, obtained from [your My API Application page](https://www.strava.com/settings/api).                                                                                                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `mode`                                                      | `table`                                                                                                                     | _Optional_ - Determines which mode should be used to display activity information.                                                                                                                                                                                                 | `"table"`, `"chart"`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `chartType` ![Beta](https://img.shields.io/badge/-BETA-red) | `bar`                                                                                                                       | _Optional_ - Determines the type of chert which should be displayed in `chart`.                                                                                                                                                                                                    | `"bar"`, `"radial"`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `activities`                                                | `["ride", "run", "swim"]`                                                                                                   | _Optional_ - Determines which activities to display and in which order they are displayed. _Note:_ - The activities can be listed in any order, and only one is required. However, they must be entered as an array of strings i.e. comma separated values within square brackets. | In `table` mode: `"ride"`, `"run"`, `"swim"` <br /> In `chart` mode: `"alpineski"`, `"backcountryski"`, `"canoeing"`, `"crossfit"`, `"ebikeride"`, `"elliptical"`, `"golf"`, `"handcycle"`, `"hike"`, `"iceskate"`, `"inlineskate"`, `"kayaking"`, `"kitesurf"`, `"nordicski"`, `"ride"`, `"rockclimbing"`, `"rollerski"`, `"rowing"`, `"run"`, `"sail"`, `"skateboard"`, `"snowboard"`, `"snowshoe"`, `"soccer"`, `"stairstepper"`, `"standuppaddling"`, `"surfing"`, `"swim"`, `"velomobile"`, `"virtualride"`, `"virtualrun"`, `"walk"`, `"weighttraining"`, `"wheelchair"`, `"windsurf"`, `"workout"`, `"yoga"` |
| `period`                                                    | `recent`                                                                                                                    | _Optional_ - What period should be used to summarise the activities in `table` and `chart` mode.                                                                                                                                                                                   | `recent` = last 4 weeks in `table` mode or current week in `chart` mode, `ytd` = year to date, `all` = all time                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `stats`                                                     | In `table` mode: `["count", "distance", "achievements"]` <br /> In `chart` mode: `["distance", "moving_time", "elevation"]` | _Optional_ - Determines which statistics to display. <br /> _Note:_ - The stats can be listed in any order, and only one is required. However, they must be entered as an array of strings i.e. comma separated values within square brackets.                                     | `"count"`, `"distance"`, `"elevation"`, `"moving_time"`, `"elapsed_time"`, `"achievements"`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `auto_rotate`                                               | `false`                                                                                                                     | _Optional_ - Whether the summary of activities should rotate through the different periods in `table` mode.                                                                                                                                                                        | `true` = rotates the summary through the different periods, `false` = displays the specified period only.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `units`                                                     | `config.units`                                                                                                              | _Optional_ - What units to use. Specified by config.js                                                                                                                                                                                                                             | `config.units` = Specified by config.js, `metric` = Kilometres/Metres, `imperial` = Miles/Feet                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `updateInterval`                                            | `10000` (10 seconds)                                                                                                        | _Optional_ - How often does the period have to change? (Milliseconds).                                                                                                                                                                                                             | `1000` - `86400000`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `reloadInterval`                                            | `300000` (5 minutes)                                                                                                        | _Optional_ - How often does the data needs to be reloaded from the API? (Milliseconds). See [Strava documentation](http://strava.github.io/api/#rate-limiting) for API rate limits                                                                                                 | `7500` - `86400000`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `animationSpeed`                                            | `2500`                                                                                                                      | _Optional_ - The speed of the update animation. (Milliseconds)                                                                                                                                                                                                                     | `0` - `5000`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `locale`                                                    | `config.language`                                                                                                           | _Optional_ - The locale to be used for displaying dates - e.g. the days of the week or months or the year in chart mode. If omitted, the config.language will be used.                                                                                                             | e.g. `en`, `en-gb`, `fr` etc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `showPrivateStats`                                          | `false`                                                                                                                     | *Optional* - Strava excludes private activities from stats.                                                                                                                                                                                                                        | `true` = include private activities to stats, `false` = only public activities in stats                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `debug`                                                     | `false`                                                                                                                     | _Optional_ - Outputs extended logging to the console/log                                                                                                                                                                                                                           | `true` = enables extended logging, `false` = disables extended logging                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `digits`                                                    | `1`                                                                                                                         | _Optional_ - Digits for total distance and elevation gain statistics                                                                                                                                                                                                               | `0` - ...                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `firstYear`                                                 | _Five years before the current date._                                                                                       | _Optional_ - The first year activities should be grouped by in `chart` mode when the `period` is "all".                                                                                                                                                                            | `0` - ...                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
