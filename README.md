## 2. DevOps on AWS: Code, Build, and Test 
Credit goes to to the course [DevOps on AWS](https://www.udemy.com/course/devops-aws-code-build-test)

### Code, build, and test
- Explore AWS services that help application service delivery
- AWS code commit
- AWS Code Build
- AWS Code Pipeline
- Are the pipelines fast, can we get a competitive advantage from better work flows?
- Adapt the culture of devOps: this part focuses on Code, Build, Test 
- Later: Release, deploy, operate, monitor
- Best practice for continous integration
- With automation comes confidence and better quality software

### Code, build, test
- Development and production environments may differ
- How can developer and operations team work better together: DevOps

### Thinking DevOps
- DevOps is not equal to Infrastructure as Code, agile, automated testing and delivery, ...
- DevOps: Enables development teams and operations teams to work better together and ultimately share the responsibility for the softare they build
- DevOps bridges the gap of development and operations
- Development and operations need to increase their cooperation and transparency
- DevOps cultural philosophies: Increase collaboration, trasparency, and communication
- DevOps cultural philosophies: Take on responsibilites outside the scope of current position
- DevOps cultural philosophies: Define expectations for job roles and collaboration
- Tooling: take the application of engineering practices and tools and apply to operations tasks
- DevOps tools and practices: Perfrom frequent, but small, updates to an applications = less risk
- DevOps tools and practices: Automate everything, reversible states
- DevOps tools and practices: Continous intergration and continous delivery (CI/CD)
- DevOps loop: Code, Build, Test, Release, Deploy, Operate, Monitor, Code, ...

### Example Application
- Static react frontend
- Browser (users)
- Browser talks to API Gateway Backend (websocket APIs)
- API Gateway Backend (websocket APIs) talks to Lambda Functions
- Lambda Functions talk to DynamoDB table
- Lambda Functions talk to the state machine of the running game with five states
- State machine of the running game: Question, QuestionWait, Scores, IsGameOver, GameOver, End
- Question: Lambda 
- QuestionWait: Websocket to the API, Lambda updates DynamoDB with the answer
- Scores: Lambda function loops over the answers in the DynamoDB and sends scores to the players
- IsGameOver: State machine executes
- In use: AWS Services, Lambda, DynamoDB, Step Functions

### The Code
- Project managers want to add features
- Developers want to develop quickly and secure
- Questions for the new team
- Question 1: Where is the code?
- Question 2: Where should I be commiting code?
- Question 3: How often should I be commiting and integrating?
- Question 4: When do changes get deployed for end users?

### Answers
- Where is the code: changes availabe to all team members with AWS CodeCommit is a secure, highly scalable, fully managed source control service that hosts private Git repositories
- Where should I be commiting my code updates: E.g. branching strategy and do a merge with the main branch
- How often should I be commiting and implementing: Push to the cloud, its safe
- When do changes get deployed and get infront of end users: Often, testers can quickly validate, quickly deploying new features is supported with automation
- CodeCommit authentication: IAM generated credentials, SSH key, Signed credentials 
- CodeCommit authentication: CLI to access, then use commands as in git (add, commit, push)

### AWS Sam
- CLI to create services
- Creates AWS Lambda, Amazon API Gateway, AWS Step Functions, Amazon DynamoDB
- React application in an S3 bucket, S3 is the "webserver"

### The Build
- Successful build: outputs a package artefact ready for deployment e.g. a docker image that can be passed on to the deployment process to roll an update
- A build usually integrates tests e.g. unit tests
- Commiting something that brakes the source control is a braking the build
- Successful build on a clean environment on the local laptop? Why not do it on every commit
- A broken commit is problematic because it blocks others from integrating their source code
- Code build is a service that scales up, you pay when you are using it
- CodeBuild need a build project and a build spec file
- Build Project: Source, Environment, IAM role, Logging

### buildspec.yml
#### Amazon CodeBuild Buildspec.yml README

This README provides an overview and explanation of the `buildspec.yml` file used in Amazon CodeBuild projects. The `buildspec.yml` file is a YAML formatted configuration file that defines the build and deployment stages for a project within Amazon CodeBuild. This file is stored in the root directory of your source code repository and serves as a blueprint for the build process.

#### What is Amazon CodeBuild?

Amazon CodeBuild is a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready for deployment. It supports various programming languages, build tools, and integrations with other AWS services, making it a powerful tool for automating your build, test, and deployment processes.

#### The Buildspec.yml File

The `buildspec.yml` file is at the heart of the CodeBuild process. It defines the sequence of build and deployment actions that CodeBuild will perform when it runs a build on your project. The file contains a series of build commands, environment variables, settings, and other configuration options that instruct CodeBuild on how to build your project.

#### Basic Structure

The `buildspec.yml` file follows a hierarchical YAML structure. Here's a basic outline of the possible sections:

```yaml
version: 0.3  # The version of the build spec format, currently 0.3 is the latest version.
phases:       # The build phases and their respective commands.
  install:    # Install phase (optional)
    commands:
      - command1
      - command2
  pre_build:  # Pre-build phase (optional)
    commands:
      - command3
      - command4
  build:      # Build phase (required)
    commands:
      - command5
      - command6
  post_build: # Post-build phase (optional)
    commands:
      - command7
      - command8
artifacts:    # Defines the output artifacts to be produced by the build.
  files:
    - path/to/artifact1
    - path/to/artifact2
  name: build-output-name
```

#### Build Phases
The `phases` section defines the various phases of the build process. These phases include:

1. **Install (optional)**: Allows you to install any necessary dependencies or prerequisites needed for the build.
2. **Pre-build (optional)**: Useful for preparing the build environment or setting up any additional configurations before the actual build process.
3. **Build (required)**: This phase contains the primary build commands that compile the source code, run tests, and generate the output artifacts.
4. **Post-build (optional)**: Provides the ability to perform any final steps or cleanup tasks after the build is complete.

Each phase can contain one or more commands, which are executed sequentially.

#### Artifacts
- The `artifacts` (section specifies the **output artifacts** that CodeBuild should produce after the build completes successfully. These artifacts can include build artifacts, compiled binaries, test reports, and any other files generated during the build process.

#### Environment Variables
- You can define environment variables directly in the `buildspec.yml` file. These variables can be used in your build commands and provide flexibility for passing dynamic values into the build process.

#### Example
- Here's a simple example of a `buildspec.yml` file for a Node.js application:

```yaml
version: 0.3
phases:
  install:
    commands:
      - npm install
  build:
    commands:
      - npm run build
artifacts:
  files:
    - 'app/**/*'
```

In this example, the build process installs Node.js dependencies, runs the build script to compile the application, and then generates an artifact containing all files within the `app` directory.

#### Integration with CodeBuild
- To use the `buildspec.yml` file with CodeBuild, you simply include the file in the root directory of your source code repository. When you start a build in CodeBuild, it will automatically detect and use the `buildspec.yml` configuration to execute the defined build steps.

#### Conclusion
- The `buildspec.yml` file is a powerful tool that enables you to define and customize your build and deployment process in Amazon CodeBuild. By creating a well-structured `buildspec.yml` file, you can ensure a smooth and efficient continuous integration workflow for your projects.

### branching strategy
- Branch in Git is a pointer to a commit
- The commits form a DAG
- A commit may have multiple parents
- A merge commit has two (or more) parents
- Everything in main is in production
- Code commit supports branching strategies, using various branches, pull requests, reviews, just like Git, squash commit (changes from a branch will appear as one commit), three way merges
- Hot fixes in the production system: create a hotfix branch from the main production branch, e.g. for a security fix, run tests to make sure nothing broke
- `git rebase main` is used for incorporating changes from the `main` branch into the current branch. It allows you to update your branch with the latest changes from the `main` branch and maintain a cleaner commit history by integrating your changes on top of the latest `main` branch instead of merging it.
- Rebases are used to:
- 1. Keep a cleaner commit history: Rebasing allows you to squash or reorganize your commits before incorporating them into the main branch, resulting in a linear and cleaner commit history.
- 2. Integrate latest changes: It helps you incorporate the latest changes from the main branch into your feature branch before merging it back, ensuring your code is up-to-date.
- 3. Resolve conflicts early: Rebasing forces you to deal with conflicts as you apply your changes on top of the main branch, making it easier to resolve conflicts early on.
- 4. Facilitate easier code reviews: A cleaner commit history makes code reviews easier to follow and understand.
- 5. Avoid unnecessary merge commits: Rebasing avoids creating additional merge commits, making the commit history more straightforward.
- However, it's important to note that rebasing rewrites the commit history, and it's not recommended to use it on public branches shared with others, as it can lead to conflicts for other collaborators. It's generally more suitable for private feature branches where you can reorganize and update your changes before merging them into the main branch.

