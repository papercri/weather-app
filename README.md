# Fetch

In this repository, you will use _fetch_ to asynchronously retrieve data from an API.

## Objectives

- Use Fetch to interact with an API.
- Use Vue's _state_ to manage asynchronous data.

## Setup

- Run `npm install` to install the packages.
- Run `npm run dev` to start the development server.

## Basic Requirements

### 1. Create a form.

- This form should have an input for "location" and a "submit" button.
  - When you click "submit," you should call the "getWeather" method in your component.
  - When you click "submit," you should also update the _state_ "loading" to true.

### 2. Call the Open Weather Map API.

- Call the Open Weather API from the "getWeather" function.
  - You will need to create an account on [Open Weather Map API](https://openweathermap.org/) and obtain your API key to make requests.
  - Use "fetch" inside the "getWeather" function to call the Open Weather API and retrieve weather information based on the _location_.
  - Store the retrieved weather data in the _state_.

### 3. Display useful information to the user (make it visually appealing!)

- Create a visual element to indicate when the data is loading (_loading_).
- Display the weather data once it has been loaded.
- Show an error if the data could not be loaded correctly.

### 4. Optional: Add an additional button to switch to the 5-day weather forecast view.

- To achieve this, you will need to create a second button that makes a slightly different API call.
  - You can use the same function; you just need to modify the URL used in _fetch_ to retrieve the 5-day forecast data.

## Resources

- [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Open Weather Map API](https://openweathermap.org/)
- [Vue Documentation](https://vuejs.org/guide/introduction.html)

## Notes

_This is a student project created at [CodeOp](http://CodeOp.tech), during the Front End Development bootcamp in Barcelona._
