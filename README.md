# govtech traffic app

The website waits for the user to select a date and time. Upon selecting a date and time, the eligibility of the date and time would be checked. If the date and time inputted is ahead of the current date and time, an error message would be returned, having the user input a correct date and time.

Upon inputting an appropriate date and time, the traffic api is called and data is retrieved. The weather api would also be called, returning all the locations in Singapore with their coordinates. Cameras that have a latitude and longitude that is within 0.01 of the coordinates of these locations would be stored under the area. Areas with at least one camera would be shown, allowing the user to click on a particular area.

Upon clicking on the area, the weather and screenshot components are shown. The weather component shows the current weather in that area at this point of time. The screenshot component returns all the cameras under the area. If a user were to choose another location or change the date and time, a loading overlay would run, showing the user that the website is loading the new content. While the overlay is running, the user is unable to click on any other component. This ensures that the website loads the right data. It also improves the user-friendliness of the app, as the user would know whether the app is retrieving the new data. 

Animations are also included for the components, making the app more user-friendly. 

## Project Setup
```sh
npm install
```

## Compile and Hot-reload for Development
```sh
npm run dev
```

## Compile and Minify for Production
```sh
npm run build
```