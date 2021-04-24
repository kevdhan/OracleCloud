# OIC - Rain or Shine Tutorial (Weather Data/API)

Topics this tutorial will go over:
* Creating REST Connections
* Creating an Integration (putting our REST connections in action)
* Creating a Web Application through Visual Builder (a built-in low code development platform) to call and visualize our created Integration.

The goal of this tutorial is to show the basics of Oracle Integration Cloud: Connections, Integrations, and Visual Builder

We will be using the Weather REST API from Weatherstack (previously known as APIXU Weather API). So first, we will create an account from Weatherstack to obtain the API token needed to use their API. Next, if not already done, create an Oracle Integration Cloud Instance (at [cloud.oracle.com](https://www.oracle.com/cloud/sign-in.html?redirect_uri=https%3A%2F%2Fcloud.oracle.com%2F)). Then, we will give the right access to ourselves (policies to users on Oracle Cloud) to use all the appropriate Oracle Integration and Visual Builder features needed for this tutorial. Then in the Integration instance, create 2 REST APIs (1 for the Weather API and 1 for the Integration itself), use these connections to create an integration (we trigger the Integration API to invoke the Weather API), and use the integration in a simple web application (that we'll create using Visual Builder) to showcase the Weather Data.

## (1) Prerequesites
Before we dive into the integration aspect of this tutorial, we need to do some prerequesite steps.

### (1a) Weatherstack
First, we need to gather the necessary information to perform a REST API call to get weather data. Go to [https://weatherstack.com/](https://weatherstack.com/) and create a FREE account. Once created, go to your dashboard. From here, in a separate note/file, take note of:

![1a_WeatherAPI]()

* Your API Access Key - The API Access Key will authenticate you and allow you to make the API Call and get the requested weather data
* The documentation link - The documentation link is helpful to see how to use the API (base + relative URLs, required/optional parameters, etc.)

### (1b) Oracle Integration Cloud + Policies
First, if not done already, an Oracle Integration Instance needs to be created.
Follow these steps to create an Integration Instance:
* [Policies needed to have ability to create an Integration Instance](https://docs.oracle.com/en/cloud/paas/integration-cloud/oracle-integration-oci/iam-policy-permissions.html)
* [Actual Steps on Creating an Oracle Integration Instance ](https://docs.oracle.com/en-us/iaas/integration/doc/creating-oracle-integration-instance.html#GUID-930F40E8-5149-4091-9CDA-8E05C8449BA6)

Second, to use all the features needed in the Integration and Visual Builder platforms, we need certain roles:

* Oracle Integration: ServiceDeveloper and ServiceDeployer roles
* Visual Builder: ServiceDeveloper roles

More info on Access to OIC: [Configuring Access to Oracle Integration Instances](https://docs.oracle.com/en-us/iaas/integration/doc/configuring-access-oracle-integration-instances.html)

More info on User Roles: [Oracle Integration Roles and Privileges](https://docs.oracle.com/en/cloud/paas/integration-cloud/integration-cloud-auton/oracle-integration-cloud-roles-and-privileges.html#GUID-356C84F7-3AE7-4CB0-A5E4-40A4731873EE)

![1b_UserRoles]()

Go to Identity -> Federation -> OracleIdentityCloudService -> your username -> Manage Roles.
Within Manage Roles, give yourself admin roles for both Integration and Visual Builder (more info about Roles in links and image above)

![1b_ServiceConsoleURL]()

Lastly, go to your Oracle Integration Details page. From there, copy and save the Service Console URL. This URL will act as the base URL for the Integration Instance connection (more info on this later).

Third, if not done already, on the Oracle Integration Details Page, enable the Visual Builder functionality.
* [Enabling Visual Builder for OIC](https://docs.oracle.com/en/cloud/paas/integration-cloud/visual-admin/administering-oracle-visual-builder-oracle-integration.pdf#unique_15)

This will allow you to utilize the Visual Builder Platform.

## (2) Steps
Now that we've got everything setup, we are ready to create connections, integrations, and even a Web Application.
