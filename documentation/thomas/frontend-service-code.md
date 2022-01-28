# Frontend service code

The frontend web client displays a catalog. You need to authenticate and then you can navigate to that catalog. The data for the catalog is provided by a backend microservice.

The frontend needs to be integrated with App ID for the authentication and authorization to invoke the backend microservice.

The frontend is build on following components/frameworks. 

* [Vue.js](https://vuejs.org/) to build the frontend
* [Material Design](https://material.io/) to define the content elements of the website like buttons menus and so on.
* [App ID client JavaScript SDK for single WebPage](https://github.com/ibm-cloud-security/appid-clientsdk-js) to setup the integration for the App ID service usage.
* [axios](https://github.com/axios/axios) to invoke HTTPS endpoints.

Useful to know:

* [How to use axios to invoke HTTPS endpoints?](https://github.com/axios/axios)
* [How to implement `await` in JavaScript](https://basarat.gitbook.io/typescript/future-javascript/async-await), when we use the App ID JavaScript client SDK?
* [How to get the User with the App ID client JavaScript SDK?](https://ibm-cloud-security.github.io/appid-clientsdk-js/AppID.html#getUserInfo)
