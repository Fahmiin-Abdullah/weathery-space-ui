# Weathery Space UI

In fulfillment of Spacebox's hiring challenge requirements for RoR engineer role. You can access the live demo [here](https://weathery-space.herokuapp.com)

## Scope of work

It has a very simple product requirement. A weather forcasting app that connects with our [custom](https://github.com/Fahmiin-Abdullah/weathery-space-api) API to perform 2 main functions

1. As a user, I can select a location and query for the forecast for the next 7 days
2. As a user, I can view location's weather report based on a date range

The app is built as an SPA and made to be responsive on mobile. Initial mockup design can be found [here]()

## Setting up the app

1. Clone this repo
2. Run `npm install`
3. Create a `.env.local` file with the following content `VUE_APP_API_URL=http://localhost:3000`
4. Run `npm run serve`
5. Make sure that you have the API running locally, otherwise, you can set `VUE_APP_API_URL=https://young-cliffs-56595.herokuapp.com` to work with the live API.
