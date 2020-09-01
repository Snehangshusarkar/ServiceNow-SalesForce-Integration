# ServiceNow-SalesForce-Integration

This is a one-way integration between ServiceNow and Salesforce where Salesforce is the provider and ServiceNow is the receiver. ServiceNow instance is hosted outside the corporate network but Salesforce is inside corporate network. So, in order to build the communication b/w two systems, we need to setup a middleware. Every company has own gateway to their network and in our case, iPaas gateway is our middleware to communicate inside the corporate network. So, the dataflow is - as SFDC is the provider, so they will send a payload along with an api_key to the iPaas gateway and here, ServiceNow will act as a backend system who will receive the payload. As part of the gateway setup, make sure that the public IP Address of ServiceNow instance is whitelisted into the iPaas gateway.

1) Trigger Point: Once a new Sales Contract is signed by a customer in Salesforce, it changes the state of that contract to "Closed Booked" and immediately sent the whole Sales data in the form of JSON to the ServiceNow via iPaas gateway.

2) ServiceNow Data Processing: A scripted REST API is provided to the SFDC. So, whatever payload comes in, we will process the payload data first and then push it to a staging table.

3) Second Trigger Point: After contract signing, the purchased orders will go to Provisioning Hub to get this mapped and labelled with the Corporate standard. Once the mapping and labelling in done, Provisioning Hub sends data to ServiceNow via email as PH doesn't have API capabilities yet.

4) ServiceNow Data Processing: Inbound action is in place which parse the PH email and process the data and update it to the existing record in the stagging table. Once key point is PH send a contract pdf to ServiceNow and that pdf is also attached to the existing record in the stagging table.

5) ServiceNow Data Load: Once SFDC and PH data are updated in the stagging table and the pdf is also attached, we have a form button called "SFDC Data" to map and load the data in the various ServiceNow tables. In the backend, there is an UI action which does this work.

