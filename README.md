# Client Response Demo Application

This repository contains an example implementation of the Bazaarvoice Client Response API for our clients to reference in building their own integrations.

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app). For details on how to perform some common tasks, refer to [this](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md).

## Getting Started

- Open the Command Line or Terminal.
- Run the following commands to get a copy of this repository on your PC and navigate into it.
  ```
  git clone https://github.com/bazaarvoice/clientresponse-demo.git
  cd clientresponse-demo
  ```
- Make sure you have `node 9.0.0` and `npm 5.5.1` installed.
  - For instructions on installing `node`, see [this](https://nodejs.org/en/download/package-manager/). When you install `node`, `npm` is automatically installed.
  - If you need to switch node versions, use [nvm](https://github.com/creationix/nvm)
  
## Local Deployment

* Modify `server/server-config.js` file to contain your backend credentials for different services.
* Modify `client/src/utils/config.js` file to contain your client-side credentials for different services.
* Make sure you are in the cloned directory and run following commands from your terminal:
  ```
  npm install
  cd client
  npm install
  cd ..
  npm run dev
  ```
  
## Application Architecture

This application is split into two components - a Node.js Express server and a client-side React app. You can read more about this kind of setup [here](https://github.com/fullstackreact/food-lookup-demo).

* The [server](server/server.js) makes calls to the Bazaarvoice OAuth2 service for authentication and exposes endpoints to interact with the Client Response API.
* On the client-side, [client.js](client/src/api/client.js) provides modular functions which are used by front-end components to interact with the application's back-end, and with Bazaarvoice Conversations API to fetch reviews.
* The core front-end consists of two pages which are composed from four React [components](client/src/components):
  * **[Search Page](client/src/components/searchPage.js):** This is a simple page with a search bar which expects user to enter a Review ID which leads them to the Review Page.
  * **[Review Page](client/src/components/reviewPage.js):** This page expects a Review ID from the query parameters. It then queries the Conversation's API to fetch the corresponding review. Further, it queries the Client Response API to fetch all client responses for that review and renders the [ClientResponsesSection](client/src/components/clientResponsesSection.js) component with that data. In turn, this section renders each client response as a [ClientResponse](client/src/components/clientResponse.js) component. 


## Application Limitations

* **This implementation only supports one client at a time**

   Since API passkeys are client-specific and this application reads API passkeys from configuration files - [server-config.json](server/server-config.json) and [config.js](client/src/utils/config.js), so it can only handle a single client at a time. In order to support multiple clients, you could store their API passkeys in your database and dynamically query your database for the current client's passkeys, use them to access Bazaarvoice APIs.
* **Current implementation demonstrates only a single Client Response API endpoint**

   By displaying all client responses for a review on the ReviewPage, only 1 Client Response API endpoint is being demonstrated. Your application could use the Client Response API in more sophisticated ways. 
* **Current server implementation uses a global variable to keep track of OAuth2 tokens**

   Server currently uses a global state variable - `data` for storing OAuth2 authentication tokens. Due to this, multiple users cannot use the application during a single deployment. You should maintain user sessions using cookies and session storage for storing these tokens.
* **Current server implementation does not explicitly handle error responses from API**
   All of the Bazaarvoice APIs send different error responses for invalid calls and a production application should handle and display them properly to the end user. Currently, this application assumes all calls to be valid and doesn't do explicit error handling. 

## License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

## Contributing

Found a bug or missing feature? Please open an issue!
Send your feedback. Send your pull requests. All contributions are appreciated.
