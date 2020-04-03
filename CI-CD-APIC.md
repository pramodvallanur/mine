Development

Use API Connect Toolkit on your laptop: Designer (UI interface for API dev), + CLI (Command Line Utility that developer love to use) + Loopback (To create microservice based on the most popular node.js framework) + Local Test Environment (Data Plane environment for testing locally on the laptop)

1] Create a microservice from Loopback. This automatically generates OAI definition which could be imported into API Connect (bottom-up)
2] Import an OpenAPI definition from other microservices/middleware (bottom-up)
3] Create / Design an OpenAPI using API Connect and connect to a microservices/middleware.  (top-down)
4] Add security, enforcement, transformation policies, mashups required for making the API functional as per design
5] Debug the API assembly to understand the policy execution and fix any bugs
6] Generate the API behavior test cases (assertions) based on the OpenAPI design - Unit test cases
Perform these steps of Dev-Test iteratively using the toolkit and ensure you have the API working as designed.

7] Check-in the API definition into your SCM. Checkin the API test cases
8] Pre-commit hook runs set of linting/validation tools to ensure OAI definition is following enterprise standards

Every developer works on his laptop, continues to perform this task and check-in there API definition

System Environment
System testers use the unit test cases created by the dev and enhance it and validate if it is complete as per the requirement. Then CI/CD pipeline

1] Setup a provider organization, with the right set of resources created (q: If a resource needs to be created, how does the dev tell)
2] On each successful API commit, a new catalog is created, configured, a default product is created including the API and published to the catalog
3] Runs the test cases against the API, record the results and notify the stakeholders of the result in particular of any failure
4] un-publishes the API, the catalog is deleted 

This constantly happens for each developer for each commit. Which means, the same API could be getting tested in parallel based on the timing of commit.

Business Environment
1] On successful system testing, the API is made available in the business environment. Typically every night, updates are rolled out
2] Product Managers create products, include appropriate APIs, define plans, rate-limits and also ensure the right visibility and subscriability
3] This environment includes a portal, PM goes through a consumer on-boarding and consumption to validate product bundling
4] Checks-in the product definition into SCM


Integration Environment
Integration testers create test cases by incorporating the unit tests that include API sequence testing, logical constructs, negative test cases, sample data sets etc.
For this environment, the Provider Organization, Catalog remain static (If spaces are required those are enabled in this environment)
1] From the business environment, the new products or updated products are published in the Integration environment
2] App are created to mock real consumers
3] The integration test cases are executed to validate all business scenarios
4] If an existing product is updated, product hot replace functionality is used by increasing the product / API version
5] Perform any load testing 

Pre-production / UAT
Ops team leverages the integration test cases, updates it to ensure it meets their monitoring needs
1] Product Managers, mystery / blind users perform manual testing validating the UX and the API behavior
2] Provide sign-off for production roll-out
3] Ops teams perform monitoring checks and s

Production
1] Approved artifacts are moved/updated in production using the right lifecycle actions
2] Conduct a smoke test
3] Regular BAU Ops exercise of ensuring the API is healthy 
4] Setup an automated test to monitor the health and the behavior of the API.
5] Get alerts on any failure and ensure ops are the first to know





