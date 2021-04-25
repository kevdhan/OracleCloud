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

![2_OverallArchitecture]()

The image above shows a high level architecture of this tutorial. You the user, will interact with the front-end (built out on Visual Builder) of the Weather Application to get and visually see the weather data. In the backend, the front-end will call on the integration we created, which in return calls the Weather API to get the necessary weather data.

Let's go ahead and get started!

### (2a) Connections - Creating REST-based Connections
We will create 2 REST Connections:
* 1 for the Integration itself - this connection will be used as the Trigger connection to invoke the integration
* 1 for the Weather API - this connection will be used as the Invoke Connection to invoke the Weather API to get the weather data

We will begin with the integration connection!

#### (2a.1) Integration Connection - "GetCurrentWeather"
1. The Integration Connection will be called "GetCurrentWeather". Go to Integrations -> Connections -> Create (new).

![2a1_CreateGetCurrentWeatherConnection]() **GET IMAGE FOR THIS**

3. Give the name "GetCurrentWeather" and choose the role "Trigger and Invoke"
   1. The Trigger and Invoke role allows you to invoke or trigger this connection

![2a1_ConnectionType]() **GET IMAGE FOR THIS**

4. In the Connection Properties, select the Connection Type: REST API Base URL, and enter the OIC Service Console URL (the one collected in the prerequisites)

![2a1_SecurityType]() **GET IMAGE FOR THIS**

6. In the Security section, select: Basic Authentication, and enter your OIC Username/Password

![2a1_TestSave]() **GET IMAGE FOR THIS**

8. Click Test to test the connection
9. Once the connection was tested successfully, click Save.

#### (2a.2) Weather API Connection - "APIXUWeather"
Now, we will create the connection for the Weather API.

1. Create another REST connection (like in Section 2a.1). Call it "APIXUWeather" with the Trigger and Invoke role.
2. In the Connection Properties, selecth the Connection Type: REST API Base URL, and for the Connection URL, enter: http://api.weatherstack.com/
   1. You can see that http://api.weatherstack.com/ is the base URL for the Weather API (go to the Weather API documentation link, noted in the Prerequesite Section).
3. In the Security section, select "No Security Policy"
   1. The API (Weather API) will manage the security for the connection, so there is no need to add a form of security from the OIC side/perspective.
4. Test the connection, then once verifying it ran correctly, save the connection.


Now that we have set up the connections for OIC and the Weather API, we can now go ahead and create an integration that will let us use these connections and work with one another.

### (2b) Integration - Creating a REST-based Integration
#### (2b.1) Add Trigger and Invoke Connections
#### (2b.2) Map the Data (aka Parameters) between the Connections
#### (2b.3) Add Business Identifier Fields for Tracking
#### (2b.4) Activate The Integration

### (2c) Visual Builder - Creating a Web Application
#### (2c.1) Create a Service Connection to the OIC Integration
#### (2c.2) Define the User Interface
#### (2c.3) Configure the Action Chain to Invoke the Integration
#### (2c.4) Run the Application


