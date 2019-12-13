# Status

[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)

# Qlik Sense Extension Usage Dashboard
#### Download Latest [Here](https://github.com/eapowertools/qs-extension-usage-dashboard/releases/latest)
The Qlik Sense Extension Usage Dashboard is a Qlik application that parses application meta-data to uncover which applications use extension objects. The sources for this data are the meta-data fetch from the [Qlik Sense Telemetry project](https://github.com/eapowertools/qs-telemetry-dashboard) and QRS API calls. By combining these, we can see which apps use extensions, where those extensions are used, who are the users who use extensions, which extensions are _not_ used, as well as which extension usages could be replaced by a bundled visualization from Qlik.

## Questions answered by the app

* Which applications in my Qlik Sense Enterprise site use extensions?
* For apps that use extensions, on what sheets are they?
* Which extensions are _unused_? 
* Which apps use extensions which can be migrated to bundled visualization objects

## Screenshots

![1](../assets/qs-extension-usage-1.png)
![2](../assets/qs-extension-usage-2.png)
![3](../assets/qs-extension-usage-3.png)

## Requirements

* Qlik Sense Telemetry Dashboard installed
* Successful reload of the `TelemetryDashboard-1-Generate-Metadata` task
* An operational data connection using Windows authentication to the Qlik Repository Service (QRS)

## Installation

* Have [Qlik Sense Telemetry project](https://github.com/eapowertools/qs-telemetry-dashboard) installed
* Import Extension Usage Dashboard.qvf

## Configuration

* Import the qvf into the QMC
* Grant 'Read' access to the `monitor_apps_REST_app` data connection in the QMC (this is a default data connection shipped with the product) to the user that will be reloading the application if it is planned to reload the app from the Hub. Ensure that the user can see this connection in the Data Load Editor. Occasionally this takes a services restart to take effect.
* Grant 'Read' access to the `TelemetryMetadata` data connection in the QMC to the user that will be reloading the application if it is planned to reload the app from the Hub. Ensure that the user can see this connection in the Data Load Editor. Occasionally this takes a services restart to take effect.
* Ensure that the Qlik Sense service account has `RootAdmin` priveleges. The `monitor_apps_REST_app` data connection is reloaded by the service account, and that account needs access to specific resources via the QRS API that are not available otherwise.
* Open up the application and navigate to the Data Load Editor. Navigate to `Config` tab.
* Ensure that `vCentralHostname` contains the hostname of the Central Node and is directed to a virtual proxy using Windows authentication. If it has a virtual proxy prefix, be sure to include it (e.g. `mycentralnode/myprefix`):

![4](../assets/qs-extension-usage-data_load_editor.png)

* Reload the application either via a Task in the QMC or as a user other than the service account from the Hub.

## Analysis

The application will provide insight into what Qlik apps use extensions and on what sheet inside of that application the extension resides. The sheets are divided into:
* **KPIs**: High level KPIs of the use of Extensions across the Qlik site
* **Extension Overview**: A detail sheet to allow the user to see extension use across sheets. Included is a filter for extensions which are not used in any Qlik app.
* **Migration Sheet**: A mapping of apps which use extensions which have been added to Qlik’s bundles. These apps can be easily edited to use objects which are now supported by Qlik.
  * The Dashboard Bundle, first released in Qlik Sense Enterprise November 2018
  * The Visualization Bundle, first released in Qlik Sense Enterprise February 2019