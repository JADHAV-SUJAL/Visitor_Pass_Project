# Visitor_Pass_Project
Its an responsive and dynamic module for scheduling the pass for visitors coming to house/office and managing their data and records 

•	employees can plan for the family members and clients visit to their respective campuses on weekend  
•	Visitor pass access management system will aid the employees in raising a pass request which can be approved or rejected by the security head in their respective locations. 
•	. Each user can request for a maximum of 2 visitor passes in a month for family members  

•	This module will provide the following features 
1.	Employees in the system can raise a visitor pass request for the following types of visitors 
a.	Parent 
b.	Sibling 
c.	Spouse 
d.	Child 
e.	Clients 
2.	System should allow employees to raise a new pass request for above mentioned visitor types 
3.	The security head of the location will be responsible for approving/rejecting the visitor pass requests raised by the employees 
4.	Users can view their visitor pass request status 
5.	Use role toggling in the Front-end to show the functionalities of the roles, Login component not needed. For the Employee role, provide a textbox to capture the employee id

   
Stage 1: Database Implementation 

1.	Design a data base as per the following ER diagram provided.   
2.	Enforce the following constraints apart from primary and foreign keys 
a.	RequestStatus must only accept values as – pending, approved, rejected 
b.	IDProofTypes must be one of the these – Aadhar, VoterID, Passport 
c.	RequestRaisedOn must automatically take the today’s date 
Note: Visitor types table can be auto-populated with the following values as – Parents, Child, Sibling, Spouse, and Client. 


Stage 2: Data Access Layer Design 

1.	Created a library project and add ORM support into it.  
2.	Used the ORM to map the entities to database as per the ER diagram provided.  
3.	Used repository per entity pattern and generate the repositories to perform the following operations 
a.	Fetch the visitor types 
b.	Add a new visit request 
c.	Fetch pending visit request based on location 
d.	Approve/reject request 
e.	Fetch requests created by a user 

 
Stage 3: Business Logic Layer Development 

1.	Developed a library which reference the Data Access Library project created earlier 
2.	This class library will contain various service classes which will encapsulate the business logic for the application. 
3.	Use dependency injection to in service classes to inject the required repositories. 
4.	Create the service classes using the single responsibility principle which perform the given operations as follows  
a.	Return a collection of visitor types 
b.	Add a visitor request 
c.	Approve or reject a visitor request 
d.	Fetch request from a particular user 
e.	Return all pending request based on a location 
5.	Following business rules must be implemented as part of the business service class 
a.	A new visitor request must be raised 1 week in advance 
b.	Each employee can raise only 2 visitor request for their family members in a given calendar month 
c.	Visit request for family member can only be raised for weekend  
d.	For client visit there is no restriction on how many visit requests are placed.  
e.	When a request is rejected by the security head a valid reason must be provided 
f.	All visitor ID proofs must be uploaded into a local folder. The filename must be based on VisitorIDProof number 
g.	If visitor pass request has reached the maximum limit for a user in given month then throw a user-defined exception as “MaximumPassRequestLimitReached” 

 
Stage 4: Unit Testing 

1.	Created a new Unit test project to test the service classes created in business logic layers 
2.	Mock all the repositories using a mocking framework. 

 
Stage 5: Micro-service implementation 

1.	Created a API project which references the business logic layer created earlier 
2.	Implement service documentation using swagger 
3.	All exceptions in the micro-service must be handled and logged using a logging library 
4.	Create the following end-points and test them using postman and export the requests into a json file.
	
Table 1 : Visitor Pass – Endpoint – 1 
URL 	api/passrequests/<location> 
Request Type 	GET 
User Role 	SecurityHead 
Trigger 	Front end 
Description 	A security head must be able to fetch all the pending visitor pass requests 
Inputs 	Location 
Outputs 	VisitorPassRequestDTOs containing all visitor pass request details along with ID proof 
 
Table 2 : Visitor Pass – Endpoint – 2 
URL 	api/passrequests/new 
Request Type 	POST 
User Role 	Employee 
Trigger 	Front end 
Description 	A logged in employee should be able to successfully raise a visitor pass request 
Inputs 	VisitorPassRequestDTO containing all visitor pass request details  
Outputs 	Status code on success or validation errors 

Table 3 : Visitor Pass – Endpoint – 3 
URL 	api/vistortypes 
Request Type 	GET 
User Role 	Employee 
Trigger 	Front end 
Description 	An employees should be able to view the types of visitor available 
Inputs 	 
Outputs 	VisitorTypeDTOs 

Table 4 : Visitor Pass – Endpoint – 4 
URL 	api/passrequests/approvereject 
Request Type 	POST 
User Role 	Security Head 
Trigger 	Front end 
Description 	The location security head is responsible for updating the status of visitor pass request 
Inputs 	UpdatePassRequestDTO containing requestid, status and reason 
Outputs 	Status code 

Table 5 : Visitor Pass – Endpoint – 5 
URL 	api/passrequests/<requestid> 
Request Type 	GET 
User Role 	SecurityHead 
Trigger 	Front end 
Description 	Security head should be able to examine all details of a given visitor pass request 
Inputs 	RequestID 
Outputs 	VisitorPassRequestDTOs containing all visitor pass request details  
 
 
Stage 6: Front end design 
 
1.	Create the following components as per the specification provided below.  
a.	PassRequestListComponent 
1.	Create a component which can display all the visitor pass request into a tabular format 
2.	Against each table row a view details buttons must be available 
3.	Define a route for the component in the nav bar of the application 
4.	If an employee is accessing the component then only his pass request will be displayed 
5.	If a security head is accessing the component then pass request raised under his location will be displayed. 
 
b.	PassRequestDetailsComponent 
1.	An employee or the security head will be able to view details of a single pass request by clicking view details button in the PassRequestList component 
2.	All details related to a pass request must be displayed in this component in a definition list 
3.	If the user accessing the component is a security head then ApproveRejectRequest component must be rendered as a child of this component 
 
c.	ApproveRejectRequestComponent 
1.	Design a component which will allow security heads to approve or reject a pass request 
2.	The component should provide with 2 buttons one for approve and other for reject request. 
3.	Component should also contain a textbox for rejection reason in case user click on reject then reason must be provided 
4.	Once the operation is completed successfully then an appropriate message must be displayed 
 
d.	NewPassRequestComponent 
1.	Create a component which allows employees to raise a new pass request 
2.	The component should accept all the required details for the pass request 
3.	Visitor types must be selected from a dropdown list whose data must be populated based on VisitorType table. 
4.	Once the user fills all the data, proper validation must be in place to ensure it’s free of errors before submitting. 

 
Stage 7: Integration of Frontend and backend 

1.	Create a data service in the Front end application which will communicate with the Micro services. 
2.	Use the data service in the components to make them interact with the API 
3.	Valid error messages should be shown based on various response status codes received form the API 


