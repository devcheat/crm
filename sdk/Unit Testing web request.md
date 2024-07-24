# **Unit testing custom web requests**
---

One of the main points of all my unit testing posts is that unit tests should be written so that you can test your code without needing to rely on external resources, and, we've been able to use Moq as a stand-in for Dynamics CRM’s related services without too much trouble. Unfortunately there are some situations in which Moq can’t be used for mocking, and accessing RESTful web services in C# with the System.Net.WebRequest class is one of them. Today I will show you how to create your own factory object that you can use to simulate HTTP requests and responses in your unit tests.

For this example, let's say we are coding a custom workflow activity that retrieves the latitude and longitude for an address from the Bing Maps Locations REST service. The Locations service can take address details as part of a structured URL and return either an XML or JSON response that contains a variety of data attributes, including latitude and longitude. From the MSDN documentation, here is a URL that will return the latitude and longitude for Microsoft’s main campus: http://dev.virtualearth.net/REST/v1/Locations/US/WA/98052/Redmond/1%20Microsoft%20Way?o=xml&key=Bing...

You’ll note the URL is in the format of [base URL + / + country code + / + state abbreviation + / + postal code + / + city + / + address] with a query string appended to specify the output format and the calling application’s Bing Maps key. Because we’re not actually going to be calling the service, but rather we’ll be mocking a request and response, you don’t need a valid key to run the code in this example. Our example custom workflow activity will accept the country, state, postal code, city, address and Bing maps key as input parameters.

## Setting up the projects

I’m going to put this custom workflow activity class and corresponding test class in the same projects I used for the fifth post in this series, so if you went through that exercise, you can skip to the next section.

In Visual Studio, create a new class library project called DemoCrmWorkflowActivities and add references to the following assemblies:
```
System.Activities
System.ServiceModel
System.Runtime.Serialization
System.Text.RegularExpressions
Microsoft.Xrm.Sdk (browse to the .dll in the CRM SDK)
Microsoft.Xrm.Sdk.Workflow (browse to the .dll in the CRM SDK)
```
Once that's done, you need to add a new unit test project to the solution. Go to the add project menu, and select the test->unit test project type. Following the <PROJECT-BEING-TESTED>.Test naming convention, the test project should be called DemoCrmWorkflowActivities.Test. Once that project is created, add references to the following assemblies:

1. System.Runtime.Serialization
2. Moq (browse to the Moq.dll you downloaded for the earlier examples)
3. Microsoft.Xrm.Sdk (browse to the .dll in the CRM SDK)
4. Microsoft.Xrm.Sdk.Workflow (browse to the .dll in the CRM SDK)

Also, make sure to add a reference to the DemoCrmWorkflowActivities project. Otherwise you won't be able to call the methods you want to test in your test methods.

## The test code

Keeping with good testing practices, let’s write our test first. There are two things we want to test:

For a given set of input parameters, the custom workflow activity will call the correct URL.
From a given mock XML response, the custom workflow activity will extract the correct latitude and longitude attribute values.
As I mentioned at the beginning of this post, we can’t use Moq to mock a .Net WebRequest object, but because the WebRequest.Create method uses the factory pattern, we can  register our own factory object to be used for HTTP requests to a specific URL via the WebRequest.RegisterPrefix method.  This solution is described in greater detail in a blog post by Richard Willis, but essentially you just need to create three new classes to set up your factory object:

- A class that implements the System.Net.IWebRequestCreate  interface (TestWebRequestCreate)
- A class that inherits from the System.Net.WebRequest class (TestWebRequest)
- A class that inherits from the System.Net.WebResponse class (TestWebResponse)

For this example, I have basically copied the classes from Richard’s post, although I did modify his TestWebRequest class to include the ability to simulate errors. Going through these classes in detail is beyond the scope of what I want to discuss here, however a .CS file that contains all three classes is included in the attachment at the bottom of this post.

Using the TestWebRequestCreate class for mocking, we can set up our test code like so: 
```cs
//ARRANGE
string bingKey = "GOOD-KEY";
string country = "US";
string stateProvince = "WA";
string city = "Redmond";
string postalCode = "98052";
string address = "1 Microsoft Way";
 
//get new GetLocation object
GetLocation getLocation = new GetLocation();
 
//instantiate the workflowinvoker
var invoker = new WorkflowInvoker(getLocation);
 
/*************************************
detailed steps for arranging the invoker object are omitted for brevity
see the attached code samples at the bottom of this post for a complete sample
*************************************/
 
var inputs = new Dictionary<string, object> 
{
	{ "BingKey", bingKey},
	{ "Country", country},
	{ "StateProvince", stateProvince},
	{ "City", city},
	{ "PostalCode", postalCode},
	{ "Address", address}
};
string mockResponse = "";
using (System.IO.StreamReader sr = new System.IO.StreamReader("map-response-good.xml"))
{
	String line = sr.ReadToEnd();
	mockResponse += line;
}
 
string expectedUrl = "http://dev.virtualearth.net/REST/v1/Locations/US/WA/98052/Redmond/1%20Microsoft%20Way?o=xml&key=GOOD...
WebRequest.RegisterPrefix(expectedUrl, new TestWebRequestCreate());
TestWebRequest request = TestWebRequestCreate.CreateTestRequest(mockResponse);
 
//ACT
var outputs = invoker.Invoke(inputs);
string lat = Convert.ToString(outputs["Latitude"]);
string lon = Convert.ToString(outputs["Longitude"]);
 
//ASSERT
Assert.AreEqual("47.640120461583138", lat);
Assert.AreEqual("-122.12971039116383", lon);
```

There are two things I want to point out in this code. First, we have a hard-coded XML response for testing in a file called "map-response-good.xml" that is part of our project. The test class opens it and reads it into memory so that the XML can be passed to our mock REST service to use for its response. This lets us validate the latitude and longitude data are properly extracted (item #2 from our test criteria).

Second, the WebRequest.RegisterPrefix step sets up our mock REST service to only respond to calls to a specific URL pattern. You could set up a mock service that handles all calls to a particular domain, but in this example I have a complete URL specified. This lets us implicitly validate the URL syntax is correct (item #1 from our test criteria) because if the custom workflow activity doesn’t create this exact URL, a real request will be made to the Bing Maps location service, which would result in an error and cause the test to fail.

## The method to test

Now that we’ve constructed our test, we can write a custom workflow activity class that will pass the test.

First we set up our input and output parameters to communicate with the calling process and also set up a global variables to hold the base Bing Maps URL:
```cs
[Input("Bing key")]
public InArgument<String> BingKey { get; set; }
 
[Input("Country")]
public InArgument<String> Country { get; set; }
 
[Input("State province")]
public InArgument<String> StateProvince { get; set; }
 
[Input("City")]
public InArgument<String> City { get; set; }
 
[Input("Postal code")]
public InArgument<String> PostalCode { get; set; }
 
[Input("Address")]
public InArgument<String> Address { get; set; }
 
[Output("Latitude")]
public OutArgument<String> Latitude { get; set; }
 
[Output("Longitude")]
public OutArgument<String> Longitude { get; set; }
 
private string _webAddress = "http://dev.virtualearth.net/REST/v1/Locations/";
 
Next we build the correct URL based on the input parameters:
 
string country = Country.Get(executionContext);
string stateProvince = StateProvince.Get(executionContext);
string address = Address.Get(executionContext);
string postalCode = PostalCode.Get(executionContext);
string city = City.Get(executionContext);
string bingKey = BingKey.Get(executionContext);
 
_webAddress += country + "/" + stateProvince + "/" + postalCode + "/" + city + "/" + address + "?o=xml&key=" + bingKey;
 

Finally we call the Bing Maps location service using a method named CallBing and extract the latitude and longitude from the response:

XmlDocument bingDoc = CallBing(_webAddress);
 
//retrieve lat/long
XmlNamespaceManager nsmgr = new XmlNamespaceManager(bingDoc.NameTable);
nsmgr.AddNamespace("rest", "http://schemas.microsoft.com/search/local/ws/rest/v1");
 
//set lat/long
XmlNodeList latElements = bingDoc.SelectNodes("//rest:Latitude", nsmgr);
XmlNodeList lonElements = bingDoc.SelectNodes("//rest:Longitude", nsmgr);
 
Latitude.Set(executionContext, latElements[0].InnerText);
Longitude.Set(executionContext, lonElements[0].InnerText);
 

Here’s the CallBing method:

/// <summary>
/// method to call a RESTful Bing service using HTTP GET and return the XML response to the calling method
/// </summary>
/// <param name="svcUri">full URL</param>
/// <returns>XML response</returns>
XmlDocument CallBing(string svcUri)
{
	try
	{
		WebRequest req = WebRequest.Create(svcUri);
		req.GetResponse().GetResponseStream();
 
		XmlDocument xmlDoc = new XmlDocument();
		xmlDoc.Load(req.GetResponse().GetResponseStream());
 
		
		return xmlDoc;
	}
	catch (WebException exception)
	{
		string str = string.Empty;
		if (exception.Response != null)
		{
			using (StreamReader reader =
				new StreamReader(exception.Response.GetResponseStream()))
			{
				str = reader.ReadToEnd();
			}
			exception.Response.Close();
		}
		if (exception.Status == WebExceptionStatus.Timeout)
		{
			throw new InvalidPluginExecutionException("The timeout elapsed while attempting to issue the request.",
				exception);
		}
		throw new InvalidPluginExecutionException(String.Format(CultureInfo.InvariantCulture,
			"A Web exception ocurred while attempting to issue the request. {0}: {1}",
			exception.Message, str), exception);
	}
	catch (Exception e)
	{
		throw new InvalidPluginExecutionException(String.Format(CultureInfo.InvariantCulture,
			"Exception: {0}", e.ToString()));
	}
}
```
