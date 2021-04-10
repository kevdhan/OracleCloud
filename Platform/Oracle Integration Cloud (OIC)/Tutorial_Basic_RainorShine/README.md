# OIC - Rain or Shine Tutorial (Weather Data/API)

Topics this tutorial will go over:
* Creating REST Connections
* Creating Integrations (putting 
* Creating a Web Application through Visual Builder (a built-in low code development platform)

The goal of this tutorial is to show the basics of Oracle Integration Cloud: Connections, Integrations, and Visual Builder

We will be using the Weather REST API from Weatherstack (previously known as APIXU Weather API). So first, we will create an account from Weatherstack to obtain the API token needed to use their API. Next, if not already done, create an Oracle Integration Cloud Instance (at [cloud.oracle.com](https://www.oracle.com/cloud/sign-in.html?redirect_uri=https%3A%2F%2Fcloud.oracle.com%2F)). Then, we will give the right access to ourselves (policies to users on Oracle Cloud) to use all the appropriate Oracle Integration and Visual Builder features needed for this tutorial. Then in the Integration instance, create 2 REST APIs (1 for the Weather API and 1 for the Integration itself), use these connections to create an integration (we trigger the Integration API to invoke the Weather API), and use the integration in a simple web application (that we'll create using Visual Builder) to showcase the Weather Data.

## (1) Prerequesites
Before we dive into the integration aspect of this tutorial, we need to do some prerequesite steps.

### (1a) Weatherstack

