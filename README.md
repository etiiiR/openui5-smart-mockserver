# openui5-smart-mockserver
An extended and smarter version of UI5 MockServer for meaningful mock data.

OpenUI5 Smart MockServer uses the open source library Faker.js to generate meaningful fake data.

Regular MockServer: just a bunch of texts and numbers
[<img src="mockserver.png">](https://raw.githubusercontent.com/mauriciolauffer/openui5-smart-mockserver/master/mockserver.png)

Smart MockServer: meaningful data, better for tests and presentations to customer
[<img src="smartmockserver.png">](https://raw.githubusercontent.com/mauriciolauffer/openui5-smart-mockserver/master/smartmockserver.png)


## Demo
You can check out a live demo here:
https://rawgit.com/mauriciolauffer/openui5-smart-mockserver/master/demo/webapp/index.html


## Project Structure
* demo - Demo site for the library
* dist - Distribution folder that contains the library ready to use
* src  - Development folder
* test - Testing framework for the library


## Getting started

### Installation
Install openui5-smart-mockserver as an npm module
```sh
$ npm install openui5-smart-mockserver
```

### Configure manifest.json
Add the library to *sap.ui5/dependencies/libs* and set its path in *sap.ui5/resourceRoots* in your manifest.json file, as follows:

```json
"sap.ui5": {
  "dependencies": {
    "libs": {
      "openui5.smartmockserver": {}
    }
  },
  "resourceRoots": {
    "openui5.smartmockserver": "./FOLDER_WHERE_YOU_PLACED_THE_LIBRARY/openui5/smartmockserver/"
  }
}
```

### How to use
Import openui5-smart-mockserver to your UI5 controller using *sap.ui.require*:

```javascript
sap.ui.define([
    'openui5/smartmockserver/SmartMockServer'
  ], function (SmartMockServer) {
    'use strict';

    return {
      init: function() {
        const metadataUrl = 'local/path/to/the/metadata.xml/file';
        const mockServerUrl = 'odata/service/url';
        const mockServer = new SmartMockServer({ rootUri: mockServerUrl });

        // This is where everything starts.
        // It's the only extra method you need to call to have meaningful mock data.
        // That's it!
        mockServer.setSmartRules(this._getSmartRules());

        SmartMockServer.config({
          autoRespond: true,
          autoRespondAfter: 1
        });

        mockServer.simulate(metadataUrl, {
          bGenerateMissingMockData: true
        });

        mockServer.start();
      },


      //Here we define the smart rules for entity's properties
      _getSmartRules: function() {
        const smartRules = [];
        smartRules.push({
          entityName: 'Employee', //Entity Name
          properties: [
            {
              name: 'FirstName', // Entity's property name
              fakerMethod: 'name.firstName' //Faker method which will be used for this property
            },
            {
              name: 'Address',
              fakerMethod: 'address.streetAddress'
            },
            {
              name: 'Phone',
              fakerMethod: 'phone.phoneNumber'
            },
            {
              name: 'Email',
              fakerMethod: 'internet.email'
            }
          ]
        });
        return smartRules;
      }
    };
  }
);
```


## Author
Mauricio Lauffer

 - LinkedIn: [https://www.linkedin.com/in/mauriciolauffer](https://www.linkedin.com/in/mauriciolauffer)

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details