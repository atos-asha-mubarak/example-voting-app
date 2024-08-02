# Senario 1

1. Discuss & justify potential deployment options in terms of infrastructure for development environment of above application 
deployment to K8S
2. Demonstrate example infrastructure set-up and application deployment model (CD)

In terms of development enviorntment, we can have two development environtment which are Dev and testing. these two env can be handle by github by two differnt env and the actuact deployment of dev environtment from the main branch in dev 

deployment in a seprete cluster

# senario 2
Scenario 2 - production set-up: 
Discuss and justify below points: 
1. Recommendation as for the application environments (how many and why?) and how are they going to be separated 
 atleast beter to have minmum 2 env prod and dev howevwe my recommandation is to have 3 env in order to make sure test everything before PROD, SO 3 would be ideal. 
 A development environment is typically used by developers to try their changes in, and to quickly iterate on their work.

Test: A test environment is designed to run manual or automated tests against your changes.

Production: Your production environment is the one that end users of the application use. It's your live environment that you want to protect and keep up and running as much as possible.

 i chooce these 3 environtments its because reduce more complex infrasture by mutiple env, however 

we can have 3 env, one for Development, Staging ( to build a test environment to deploy newer versions of code for testing and allow developers to automatically deploy to both environments when code is changed in the repository.)and Prod.  if we choose GithubAction as a ci/cd tool then we first needs to set up the all 3 environtments in github along with its protection rules and secrets ( the secerets can be stored either in the env level or repo level, if the secret sepesfic to env we can store it env otherwisw github action will use the defult repo level secret- Federated credentials for environments) 

since we setup all three env github we canhave only one CI/CD pipeline which is responsible for the deployment in these three enviroment. in order to do that then A workflow job can reference an environment to use that environment's rules and secrets. . we can seprete these 3 env by Github env setups otions, then in the ci/cd piplline under the Jobs we can define the corresponding environtment such as DEV or Test or PROD by the keyword environtment, which will make sure the jobs will run to the correcponding env. in order to maksure the test env run after dev envorment run, we can add need keyword. 

prod can setup manual review 

and in the cicd pipeline we can have env options that can   repos and branches. 

Create one AWS CodeCommit repository for each of the applications. Use AWS CodeBuild to build one Docker image for each application in Amazon Elastic Container Registry (Amazon ECR). Use AWS CodeDeploy to deploy the applications to Amazon Elastic Container Service (Amazon ECS) on infrastructure that AWS Fargate managesants
isolation of resources between env. 

Branch-Specific Deployments: Ensures that only changes in the specified branches trigger deployments to the corresponding environments.
  if: github.ref == 'refs/heads/main'

  This setup uses GitHub Actions to create a CI/CD pipeline that deploys specific branches to designated environments, ensuring that each environment receives the appropriate code changes. You can further customize the workflow steps to suit your application’s build and deployment process

2. Which tooling will you use for CI/CD and why? How to ensure no downtime during deployments? 
Github Action track the history of the deployments of the environtment, didpolyment history help to track what hapend to the environtment over the time, even allow you to track the deployment to a commit in your repository. 
GithubAction. will makesure nodown time by selecting different deployment stratergy such as canary or blue green 
3. What are the additional tooling you need to supplement the application with to ensure it runs smoothly on production? (e.g. from observability) 

4. Networking & dns-records management and networking protection rules 
ISTIO
