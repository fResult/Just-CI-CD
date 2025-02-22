# Just-Github-Actions

This repository is for learning about the CI/CD with Github Actions from the course [Continuous Integration and Continuous Delivery (CI/CD)](https://www.coursera.org/learn/continuous-integration-and-continuous-delivery-ci-cd) on Coursera

## Continuous Integration and Continuous Delivery (CI/CD)

CI/CD is NOT one thing, it's 2 separate and distinct things.

**CI/CD processes:**

1. Plan
2. Code
3. Build
4. Test
5. Release
6. Deploy
7. Operate *(including Monitor)*

From process above, CI processes are 1-4, and CD processes are 5-7.\
But if we talk about *Automated CI* processes, they are 3-4.

## Continuous Integration

- CI is an automation process that helps developers continuously integrate code by using short-lived branches
- CI ensures that the software continuous to work properly after being pushed
- CI uses frequent pull requests which are easier to review and result in increased collaboration across teams
- Automated CI streamlines development by ensuring code works as intended by passing predefined tests

It is continuously integrating code back to the main/trunk branch.

**With CI/CD, you get the following:**

- Faster reaction times to changes
- Reduced code integration risk
- Higher code quality

### Main CI features

- Short-lived branches
- Frequent pull requests
- Automated CI tools

#### Short-lived branches

- A playground for developers to create and test new features
- Short-live branches will be deleted after being merged
- Benefits:
    - Reduce drift
    - Reduces parallel changes

#### Frequent pull requests

- Created for a specific feature
- Merged by maintainers/owners
- Small pieces of a bigger puzzle
- Benefits:
    - Frequent feedback from increased collaboration
    - Faster reaction to changes
    - Reduces changes management risk

#### Automated CI

- Uses webhooks to monitor events
- Ensures your code is doing what you want
- Streamlines development process
- Tools:
    - Github Actions
    - Circle CI
    - Jenkins CI
    - Travis CI
  They are standard CI tools, each has advantages and disadvantages. Choose wisely!

We use Github Actions (Workflow) to learn CI

**Workflow Composition consists of 5 components:**

- **Event:** Something that activates the execution of the workflow
- **Jobs:** A set of tasks that execute on the same runner (jobs are named whatever you want)
  Each job contains Runner, Services (Optional), Steps
- **Runner:** The environment where the job runs (e.g. Ubuntu, Fedora, Windows Server)
- **Steps:** A sequence of tasks that execute on the same runner
- **Actions:** Reusable commands that can be used in steps

## Continuous Delivery

CD is taking integrated code and deploy it somewhere.\
It is a software development discipline where you build software so what it can be released to production any time.

### Goals of Continuous Delivery

- Main or master branch must always be deployable
- Requires checks to ensure no bad code gets into main branch and breaks your product
- Continuous Integration runs tests on every pull request
- Ensures things work before merging back to main branch

### Continuous Delivery Best Practices

- Make every change releasable
- Embrace trunk-based development
- Deliver through an automated pipeline

**More best practices:**

- Automated as many processes as possible
- Eliminate downtime
- Release at the granularity of test'


### CI/CD Pipeline Requirements

- A code repository
- A build server
- An integration server/orchestrator
- A storage repository

### Deployment or Delivery?

- Continuous Deployment can be part of a Continuous Delivery pipeline
- Continuous Delivery describes the automated movement of code through the SDLC
- Continuous Deployment is taking that code and deployment it to production
- The need for Continuous Deployment depends on business needs

Continuous Delivery is the automated movement of code through the development lifecycle.\
While Continuous Deployment is taking delivered code and deploying it to production

### Tools of Continuous Delivery

#### Overview of CD Tools

- **Jenkins** - popular, supports many plugins, but not ideal for CD
- **Spinnakle** - dedicated cloud-agnostic CD tool, native support for load balancers and scaling clusters, but is not a build tool
- **Concourse CI** - mainly a CI tool (also included with CD) with containers mind, but can run in VMs too
- **GitLab** - implements CI and CD, easy to automate, also and SCM tool, supports major cloud platforms
- **Travis CI** - CI tool that has CD capabilities, but isn't feature-rich
- **Tekton** - can build, test, and deploy to Kubernetes, offers modularity
- **Go CD** - easy pipeline setup, native Docker and Kubernetes support, build pipelines with YAML, JSON, or GUI
- **Argo CD** - well designed UI, easy to use integrates with various CI tools
