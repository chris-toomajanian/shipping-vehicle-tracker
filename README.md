# shipping-vehicle-tracker

## Developer Notes

You will need to add an .env file to the project and add the google api key in order to see the app. I will supply this to you seperately for this demo. Once you have done that, simply run `npm install` and then `npm run serve` to boot up the Vue App.

## Functionality

This single page app has one main function - to show an admin the location and battery charge of a fleet of delivery trucks. I set up this app to display 4 trucks that are driving around the city of Atlanta. I chose VueJS as the Javascript framework to help handle componentization as well as manage state. The google map is powered by a VueJS integration and the UI is powered by Vuetify/Material Design. Using these helped to speed up the development process while still delivering a reliable front-end experience.

## Truck Data

The truck data is supplied as a static array. Each truck communicates it's location in `{ long, lat }` as well as it's battery charge level. I am using the Google Geocoding API to turn the `{ long, lat }` coordinates into a general region around Atlanta. This can help provide useful info to the admin when they may not know where a specific coordinate actually is. I decided to use a general region here but this API could easily enable a more specific location, like a street.

## Battery Charge

The battery status is it's own Vue component. This component recieves the battery status from the truck array and determines the proper battery icon and color to display (based on charge level). A charge level low warning appears on the cooresponding list item when a truck has less than 25%.

## Map

The map bounds are set by the trucks, which is checked when the app is mounted. Clicking on a truck list item will zoom the map into that truck's location. Clicking "recenter map" will zoom the map back out to see all trucks.

### If I had more time

I always like to note the things I would add if I had more time on this challenge:

- Global state to manage trucks: I'd import the truck data into the global app state instead of in to a singular component.
- Dynamic truck marker icons that can reflect the battery charge status in their color
- More data about the truck shown in an expanding window onClick
- Consolidate API calls for geocoding lookup
