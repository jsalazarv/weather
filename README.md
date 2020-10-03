# Weather App

This project allows you to check the weather of any selected location on the map.

## Installation

Clone the project using the following command:

```bash
git clone https://github.com/jsalazarv/weather.git
```

Install project dependencies

```
npm install
```

You need to create an .env file at the root level of the project as shown in the .env.example file and set the environment variables for the project to work properly.

```bash
VUE_APP_GOOGLE_MAPS_API_KEY=MY_GOOGLE_MAPS_API_KEY
VUE_APP_OPEN_WEATHER_MAP_API_KEY=MY_OPEN_WEATHER_MAP_API_KEY
```

Compiles and hot-reloads for development

```
npm run serve
```

Compiles and minifies for production
```
npm run build
```

Lints and fixes files
```
npm run lint
```

### API Keys
You can get your keys in the following links:

[Google Maps](https://console.cloud.google.com)

[Open Weather Map](https://openweathermap.org/api)

Note: you must create an account to get your keys.