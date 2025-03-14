# Google Fit Home Assistant Custom Integration

[![GitHub Release][releases-shield]][releases]
[![HA Installs][installs]](https://analytics.home-assistant.io/custom_integrations.json)
[![GitHub Activity][commits-shield]][commits]
[![GitHub Downloads][downloads-shield]][downloads]

[![License][license-shield]](LICENSE)
[![hacs][hacsbadge]][hacs]

[![Project Maintenance][maintenance-shield]](https://github.com/YorkshireIoT)
[![GitHub Sponsors][github-sponsors-badge]][github-sponsors]
[![BuyMeCoffee][buymecoffeebadge]][buymecoffee]

> [!CAUTION]
> **This integration is no longer being actively maintained due to Google pulling the Fit API**
>
> #### What's the alternative?
>
> For users who want health data in Home Assistant, it is recommended to use the Android companion
> app, enabling the [Health Connect Sensors](https://companion.home-assistant.io/docs/core/sensors#health-connect-sensors).
> The Home Assistant developers have put in a lot of effort to link into Health Connect, and at the time of writing
> 12 sensors are supported. For further support, please raise an issue here:
> [https://github.com/home-assistant/android](https://github.com/home-assistant/android)
>
> #### Further Information
> 
> Due to the deprecation of the Fit API (see below), Google are blocking
> any new developers signing up to this API. As such, any **new user** wanting to use
> this integration who has *not already signed up to the Fit API* will not be able
> to. To anyone affected by this, I'm as annoyed and disappointed as you ☹️
>
> Additionally, Google is shutting down the [REST API on 30th June 2025](https://developers.google.com/fit).
> As this integration relies on this API, it will no longer work after this time.

This integration interfaces with the [Google Fit REST API][rest-api] to provide Google Fit
data within Home Assistant.

![Example](/res/example.png)

**This integration will set up the following platforms.**

Platform | Name | Description | Infrequent Update |
-- | -- | -- | -- |
`sensor` | `active_minutes_daily` | [Active Minutes][active-minutes]. Reset daily. | &#9744; |
`sensor` | `calories_burnt_daily` | [Calories burnt][calories-burnt] (kcal). Reset daily. | &#9744; |
`sensor` | `basal_metabolic_rate` | [Base Metabolic Rate][basal-metabolic-rate] (kcal). Calories per day based on weight and activity. | &#9744; |
`sensor` | `distance_travelled_daily` | [Distance travelled][distance-travelled] (metres). Reset daily. | &#9744; |
`sensor` | `heart_points_daily` | [Heart Points][heart-points] earned. Reset daily. | &#9744; |
`sensor` | `height` | [Height][height] (metres). | &#9745; |
`sensor` | `weight` | [Weight][weight] (kilograms). | &#9745; |
`sensor` | `body_fat` | [Body Fat][fat] (percentage). | &#9745; |
`sensor` | `body_temperature` | [Body Temperature][temperature] (celsius). | &#9745; |
`sensor` | `steps` | [Number of steps][steps] taken. Reset daily. | &#9744; |
`sensor` | `deep_sleep` | [Deep sleep][sleep] time over the past 24 hours. May not be available depending on sleep data provider. | &#9744; |
`sensor` | `light_sleep` | [Light sleep][sleep] time over the past 24 hours. May not be available depending on sleep data provider. | &#9744; |
`sensor` | `rem_sleep` | [REM sleep][sleep] time over the past 24 hours. May not be available depending on sleep data provider. | &#9744; |
`sensor` | `awake_time` | [Awake][sleep] time during a sleep session over the past 24 hours. Not overall daily awake time. May not be available depending on sleep data provider. | &#9744; |
`sensor` | `sleep` | [Overall sleep][sleep] time over the past 24 hours. | &#9744; |
`sensor` | `blood_pressure_diastolic` | Most recent Diastolic [blood pressure][blood-pressure] reading. | &#9744; |
`sensor` | `blood_pressure_systolic` | Most recent Systolic [blood pressure][blood-pressure] reading. | &#9744; |
`sensor` | `heart_rate` | Most recent [heart rate][heart-rate] measurement. | &#9744; |
`sensor` | `resting_heart_rate` | Most recent resting [heart rate][heart-rate] measurement. | &#9744; |
`sensor` | `blood_glucose` | Latest [blood_glucose][blood-glucose] measurement (mmol/L). | &#9744; |
`sensor` | `hydration` | Total [water][hydration] consumed. Reset daily. | &#9744; |
`sensor` | `oxygen_saturation` | The most recent [blood oxygen][blood-oxygen] saturation measurement. | &#9744; |

> Please note, there is a delay (roughly 30-60 minutes) between sensor measurements being recorded on the Google Fit
> app and the data then being available to query of the rest API. As such, although this integration polls the API
> more frequently than this it will take at least this length of time for your data to appear in Home Assistant.
> It is not instantaneous.

## Prerequisites

### Authentication Configuration

I will not try to duplicate what has already been documented countless times before as it will no doubt become out
of date.

Instead, follow the instruction in the Official Home Assistant Docs for [Google Mail][google-mail] under the
"Generate Client ID and Client Secret" section, replacing instructions for 'Gmail API' with 'Fit API'.

## Installation

### HACS (Recommend)

#### Prerequisites

HACS Installed in Home Assistant: [Home Assistant Community Store](https://hacs.xyz/)

#### Steps

1. Go to HACS
1. Go to Integrations
1. Click Explore & Download Repositories
1. Find the integration as `Google Fit`
1. Click install.
1. Restart Home Assistant.

### Manual

1. Using the tool of choice open the directory (folder) for your HA configuration (where you find `configuration.yaml`).
1. If you do not have a `custom_components` directory (folder) there, you need to create it.
1. In the `custom_components` directory (folder) create a new folder called `google_fit`.
1. Download _all_ the files from the `custom_components/google_fit/` directory (folder) in this repository.
1. Place the files you downloaded in the new directory (folder) you created.
1. Restart Home Assistant

## Setup

Setup is done completely in the UI.
Go to your integrations page by clicking the button below.

[![Open your Home Assistant instance and show your integrations.](https://my.home-assistant.io/badges/integrations.svg)](https://my.home-assistant.io/redirect/integrations/)

Click "+ Add Integration", search for "Google Fit" and follow the steps.
If you have set the app you created in the Credentials configuration to publish
(which you should've done to avoid re-authentication problems) then you
will probably see a warning step asking if you want to proceed.

You can hit "Advanced", then "Proceed" as *you* are the 'developer' of the app,
as you created the app and credentials in your own account.

### Setup Example Screenshots

![Add Integration](/res/add.png)
![Add Credentials](/res/add_credentials.png)
![Choose Google Account](/res/choose_account.png)
![Unverified Warning](/res/warning.png)
![Access Request with Scopes](/res/wants_access.png)
![Link Account](/res/link_account.png)
![Finished](/res/success.png)

## Configuration

The following options can be tweaked after setting up the integration:

Option | Description | Default
------------ | ------------- | -------------
Update interval | Minutes between REST API queries. Can be increased if you're exceeding API quota | 5 (minutes) |
Infrequent Sensor Multiplier | Multiply the update interval by this for less frequently updated sensors, e.g. height. This reduces unnecessary API queries. | 12 (so default 5 mins update interval changes to an hour) |

## Unknown Sensor Behaviour

All sensors in this integration can be grouped into two categories; cumulative or individual.
This is due to how data is reported in your Google Fit account. Every piece of data is only associated
with a time period and has no underlying logic for running totals.

For example, steps are reported like this in your account:

* 1st January 2023 9:32:01 - 495
* 2nd January 2023 11:54:03 - 34
* 2nd January 2023 13:02:40 - 1005
* 2nd January 2023 17:16:27 - 842

There is then some built-in logic in this integration to work out what
sensor we're dealing with, and to either sum up these values over a
logical time period, or to just take the latest known value, when the
sensor is something like height.

> If you're interested in all the underlying logic, it's contained in
> [api.py](/custom_components/google_fit/api.py).

The behaviour of this integration when there is *no* data available in your account differs
depending on the sensor category.

*Cumulative* sensors will use 0 as their base value and this will be their value in Home Assistant
if their is no data in your Google Fit account for that sensor.

*Individual* sensors will only report a value if they can find some data in your Google Fit account.
Otherwise, they will show `Unknown`.

### Unavailable

Besides `Unknown`, there is an additional Home Assistant sensor state; `Unavailable`.

This state has nothing to do with if there is or isn't data in your account. It indicates some error
in the fetching of the data. Your internet has stopped working, or maybe the Google servers are
down. In some cases, it may also indicate there is a bug with this integration. If that is the case,
please report it as a bug.

There are no plans to ignore these data fetching issues and retain the last known sensor
value. The reasoning for this is:

* This is the correct representation of the sensors state, and
* It indicates to the user that there is some underlying issue with the integration itself
and this should not be hidden.s

## Adding Multiple Accounts

![Multiple Account](/res/multiple_accounts.png)

Use the following steps to setup additional Google Fit accounts in Home Assistant.

1. Go through all the steps in [Authentication Configuration](#authentication-configuration) for the
second account, e.g. create a completely separate application cloud application, enable Fit API and create
new credentials.
1. Go to Integrations and click the 3 dots in the top right to go to Application Credentials.
1. Click "Add Application Credentials", select "Google Fit" as the integration and add in the newly created
credentials.
1. Go through the configuration flow, making sure to pick the *correct* new Google account when prompted.
1. Once completed you should now see multiple Google Fit integration credentials in the table.
1. Go back to integration, click "Add Integration", search for Google Fit and then choose the newly created
credentials as the implementation when prompted.

## Reconfiguring badly configured credentials

If you made a mistake somewhere when configuring your credentials, whether with the Google Cloud Console or
within Home Assistant, you need to delete not just the integration to be prompted to reconfigure but also the
*'bad'* credentials.

To do this, go to the Integrations page and click the 3 dots in the top right and go to "Application Credentials".
From there you can select the credentials and remove them.

![Application Credentials](res/application_credentials.png)

## Contributions are welcome!

If you want to contribute to this please read the [Contribution guidelines](CONTRIBUTING.md)

***

[buymecoffee]: https://www.buymeacoffee.com/yorkshireiot
[buymecoffeebadge]: https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow
[commits-shield]: https://img.shields.io/github/commit-activity/y/YorkshireIoT/ha-google-fit
[commits]: https://github.com/YorkshireIoT/ha-google-fit/commits/main
[github-sponsors]: https://github.com/sponsors/YorkshireIoT
[github-sponsors-badge]: https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86
[hacs]: https://github.com/hacs/integration
[hacsbadge]: https://img.shields.io/badge/HACS-Default-green
[discord]: https://discord.gg/Qa5fW2R
[discord-shield]: https://img.shields.io/discord/330944238910963714
[exampleimg]: example.png
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen
[downloads-shield]: https://img.shields.io/github/downloads/YorkshireIoT/ha-google-fit/total
[downloads]: https://tooomm.github.io/github-release-stats/?username=YorkshireIot&repository=ha-google-fit
[forum]: https://community.home-assistant.io/
[license-shield]: https://img.shields.io/github/license/YorkshireIoT/ha-google-fit
[installs]: https://img.shields.io/badge/dynamic/json?color=41BDF5&logo=home-assistant&label=integration%20usage&suffix=%20installs&cacheSeconds=15600&url=https://analytics.home-assistant.io/custom_integrations.json&query=$.google_fit.total
[maintenance-shield]: https://img.shields.io/badge/maintainer-%20%40YorkshireIoT-blue
[releases-shield]: https://img.shields.io/github/release/YorkshireIoT/ha-google-fit
[releases]: https://github.com/YorkshireIoT/ha-google-fit/releases
[rest-api]: https://developers.google.com/fit/rest
[google-mail]: https://next.home-assistant.io/integrations/google_mail/

<!--Links to Google Docs for detailed information on sensors -->
[active-minutes]: https://developers.google.com/fit/datatypes/activity#move_minutes
[calories-burnt]: https://developers.google.com/fit/datatypes/activity#calories_burned
[basal-metabolic-rate]: https://developers.google.com/fit/datatypes/activity#basal_metabolic_rate_bmr
[distance-travelled]: https://developers.google.com/fit/datatypes/location#distance_delta
[heart-points]: https://developers.google.com/fit/datatypes/activity#heart_points
[steps]: https://developers.google.com/fit/datatypes/activity#step_count_delta
[height]: https://developers.google.com/fit/datatypes/health#height
[weight]: https://developers.google.com/fit/datatypes/health#weight
[fat]: https://developers.google.com/fit/datatypes/health#body_fat_percentage
[temperature]: https://developers.google.com/fit/datatypes/health#body_temperature
[sleep]: https://developers.google.com/fit/datatypes/health#sleep
[heart-rate]: https://developers.google.com/fit/datatypes/health#heart_rate
[blood-pressure]: https://developers.google.com/fit/datatypes/health#blood_pressure
[blood-glucose]: https://developers.google.com/fit/datatypes/health#blood_glucose
[hydration]: https://developers.google.com/fit/datatypes/nutrition#hydration
[blood-oxygen]: https://developers.google.com/fit/datatypes/health#oxygen_saturation
