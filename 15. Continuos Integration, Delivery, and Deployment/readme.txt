Continuous Integration, Delivery, and Deployment 


Continuous Integration

- a process where devs integrate code into a shared repo continuously 

- changes are verified by automatic builds and automated tests


A sample CI pipeline might be:

Code PR -> run unit tests -> lint -> code review -> deploy on dev server -> manually test -> merge -> deploy to QA


CI Tools For Hosted Servers:

- TravisCI 

- CircleCI 

For Managed Servers:

- Jenkins


Continuos Delivery 

- all configurations necessary so we can push to production whenever we want 

- works best with small, incremental changes to software 


Continuous Deployment 

- related to Continuos Delivery 

- we want to keep applications deployable at all times

- unlike Continuous Delivery, Continuous Deployment goes straight into production (no 
manual deployment once acceptance tests, like automation tests, are complete)

For example, Github Pages uses Continuous Deployment because anything in master is deployed.


A sample development workflow might be:


Lint, Run Unit Tests -> Push to Github, Code Review -> Push to dev server ->

Integration Tests and Feature Verification (Sanity Testing) -> Merge to QA -> automated tests -> 

Deploy to staging -> automated and acceptance tests -> deploy to prod -> sanity and performance check