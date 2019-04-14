# PSCalculatorRepo
Notes:

Did some initial reading into overview of both WS02's API Manager and Enterprise Integrator then downloaded WS02 API Manager and WS02 Enterprise Integrator. Downloaded WS02 Developer Studio (used additional tools link given after download of EI)

Booted up both AM and EI to have an initial look - found there was a clash of ports - did some research and realised I needed to change an <Offset> element value in repository/conf/carbon.xml - changed this to 1 in API Manager to bump up all default ports by 1 and rebooted both to find 'port issue' resolved. Closed both AM and EI and booted up Dev Studio to start new project. Initialised the project root directory as a GIT repo for later upload to Github.
	
From DS Dashboard selected 'ESB Solution Project' and created an initial empty project with no additional projects added.
From DS Dashboard selected 'Proxy Service' and created 'Pass through Proxy' type and set 'Save in' to name of parent ESB project and specifying endpoint of calculator SOAP service.
Reviewed the Design pane of proxy and added a few Log mediators to indicate reception of request message, receipt of response message and one for the fault sequence to log an error condition (see ProxyDesign.PNG).

Created a 'Composite Application Project' and added the ESB project as a dependency.      

Added a WS02 server from within DS and selected WS02 Enterprise Integrator 6.4.0. Added the project to the server and then started it. 

After the server booted up I was automatically directed to the carbon admin console - from here I looked at Manager/Service/List and saw my project listed there. I clicked on the WSDL1.1 column to review the WSDL and captured the wsdl address http://localhost:8280/services/PSCalculatorProxy?wsdl and then imported this into SOAPUI to run some manual requests to see if it was working properly (see ProxySoapUIProject.PNG). 

Started up API Manager to build API to call new service. This did not automatically put me into the API Manager console. Checked console logs and noted the URL for this and pasted it into chrome to get to API console. Signed in as admin.  

Clicked Add API. Added SOAP endpoint from SOAPUI project. Published and then went to API Store. Went to Applications link. Added new application and subscribed to Gold level. Generated Production/Sandbox keys. Went back to API link and into API Console tag to test the API Post request. 

Pasted in request sample from SOAPUI to add two numbers. Added mime type of request/response and 'mediate' as Soap Action. At first got back a 202 but no response body. Took me a while to realise that the namespace I was using for xmlns:soapenv: of  "http://schemas.xmlsoap.org/soap/envelope/" from my SOAPUI version was SOAP1.1 and that WS02 expected a SOAP1.2 compliant header of "http://www.w3.org/2003/05/soap-envelope". This was not at all obvious. (see APIConsole.PNG)  

Created some soapui mocks on the original soap Caculator service and tested the mocked responses against the proxy service requests. 

