# Weather App

In this repository, you will use _fetch_ to asynchronously retrieve data from an API.

🔗 **Live Demo**: [Weather App](http://weather-app-red-ten.vercel.app)

## 🎯 Objectives

- Use `fetch` to interact with an API.
- Use Vue's _state_ to manage asynchronous data.

## ⚙️ Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/papercri/weather-app.git
   ```
2. **Navigate to the project directory**:
   ```bash
   cd weather-app
   ```
3. **Install dependencies**:
   ```bash
   npm install
   ```
4. **Start the development server**:
   ```bash
   npm run dev
   ```

## 📌 Basic Requirements

### 1. Create a form.

- The form should include an input field for "location" and a "submit" button.
  - When clicking "submit," the `getWeather` method in your component should be called.
  - The _state_ `loading` should be updated to `true` when submitting.

### 2. Call the Open Weather Map API.

- Call the Open Weather API from the `getWeather` function.
  - You need to create an account on [Open Weather Map API](https://openweathermap.org/) and obtain an API key.
  - Use `fetch` inside the `getWeather` function to call the API and retrieve weather information based on the _location_.
  - Store the retrieved weather data in the _state_.

### 3. Display useful information to the user (make it visually appealing!)

- Show a loading indicator when data is being fetched (`loading`).
- Display the weather data once it has been successfully retrieved.
- Show an error message if the data could not be loaded correctly.

### 4. Optional: Add an additional button to switch to the 5-day weather forecast view.

- Create a second button that makes a slightly different API call to retrieve the 5-day forecast.
  - You can use the same function, modifying the URL in `fetch` to get the forecast data.

## 📚 Resources

- [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Open Weather Map API](https://openweathermap.org/)
- [Vue Documentation](https://vuejs.org/guide/introduction.html)

## 📝 Notes

_This is a student project created at [CodeOp](http://CodeOp.tech) during the Front End Development bootcamp in Barcelona._
