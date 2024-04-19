# SonarQube Scanning using Docker Compose

[![YouTube Video](https://img.youtube.com/vi/UoAfU5iAhl0/0.jpg)](https://youtu.be/UoAfU5iAhl0)
>**NOTE**: This image above is clickable, click it to look at the YouTube video!

## What is SonarQube?

SonarQube is an open-source platform designed for continuous inspection of code quality, security, and reliability. It performs static code analysis to identify bugs, vulnerabilities, code smells, and security issues in software projects. By analyzing source code, SonarQube provides developers and teams with actionable insights and recommendations to improve the overall quality and maintainability of their codebase.

Key features of SonarQube include support for multiple programming languages, customizable quality profiles, integration with CI/CD pipelines, security vulnerability detection, issue tracking and management, integration with IDEs, branch analysis and pull request decoration, customizable dashboards and reports, and APIs for automation and integration.

In summary, SonarQube helps developers ensure that their code meets established standards, follows best practices, and remains secure throughout the software development lifecycle, ultimately contributing to the delivery of high-quality and reliable software products. To learn more about SonarQube, check out this link: [SonarSource SonarQube Overview](https://www.sonarsource.com/products/sonarqube/)

## What is Docker Compose?

Docker Compose is a tool for defining and running multi-container applications. It is the key to unlocking a streamlined and efficient development and deployment experience.

Compose simplifies the control of your entire application stack, making it easy to manage services, networks, and volumes in a single, comprehensible YAML configuration file. Then, with a single command, you create and start all the services from your configuration file.

Compose works in all environments; production, staging, development, testing, as well as CI workflows. It also has commands for managing the whole lifecycle of your application. You can find more information about it here: [Docker Compose Overview](https://docs.docker.com/compose/)

## Instructions: Running SonarQube and Sonar Scanner

1. Make sure you have Docker installed and running.
1. For installing SonarQube, start Docker Compose and download all of the containers and such:

    ```bash
    $ docker-compose up
    ```

1. Log into the console: http://localhost:9001/sessions/new?return_to=%2F. Username and password is admin/admin
1. Update the password
1. Setup project manually and make sure you name it `test-vulnerable-app`
1. Setup local testing to analyze project
1. Generate your SonarQube Token

    > **NOTE** Take note of this - very important!

1. Run SonarScanner against the checked out project, TIWAP:

    ```text
    $ cd TIWAP
    $ docker run \
    --rm \
    --network=host \
    -e SONAR_HOST_URL="http://localhost:9001" \
    -v "<your_absolute_path>:/usr/src" \
    sonarsource/sonar-scanner-cli \
        -Dsonar.projectKey=test-vulnerable-app \
        -Dsonar.sonar.projectVersion=1.0 \
        -Dsonar.sonar.language=py \
        -Dsonar.sonar.sourceEncoding=UTF-8 \
        -Dsonar.login=<your_sonar_token> \
        -Dsonar.sonar.projectBaseDir=/root/src \
        -Dsonar.sources=. 
    ```

    This Docker command runs a container based on the `sonarsource/sonar-scanner-cli` image with the specified parameters. Here's a breakdown of each part:

    1. `docker run`: This command is used to run a Docker container.

    2. `--rm`: This flag ensures that the container is removed after it stops running. It helps in keeping your system clean by automatically removing the container once it's done executing.

    3. `--network=host`: This flag specifies that the container should share the network namespace with the Docker host, allowing it to access services running on the host's network. In this case, it's often used to allow the container to access services running on localhost.

    4. `-e SONAR_HOST_URL="http://localhost:9001"`: This sets an environment variable `SONAR_HOST_URL` with the value `http://localhost:9001`. It defines the URL of the SonarQube server that the Sonar scanner should connect to for analysis.

    5. `-v "<your_absolute_path>:/usr/src"`: This mounts a volume from the host machine to the container. It maps the local directory `<your_absolute_path>` to the directory `/usr/src` inside the container. This allows the Sonar scanner to access the project files located on the host machine.

    6. `sonarsource/sonar-scanner-cli`: This specifies the Docker image to be used for running the container. In this case, it's the official Sonar scanner CLI image provided by SonarSource.

    7. `-D` flags: These are parameters passed to the Sonar scanner CLI within the container. They provide configuration options for the SonarQube analysis:
        - `sonar.projectKey`: Specifies the unique key for the project in SonarQube.
        - `sonar.sonar.projectVersion`: Specifies the version of the project.
        - `sonar.sonar.language`: Specifies the programming language of the project (Python in this case).
        - `sonar.sonar.sourceEncoding`: Specifies the encoding of the source files.
        - `sonar.login`: Specifies the authentication token or credentials required to connect to the SonarQube server.
        - `sonar.sonar.projectBaseDir`: Specifies the base directory of the project within the container.
        - `sonar.sources=.`: Specifies the directory containing the source files to be analyzed. In this case, it's set to `.` which typically represents the current directory.

1. If you're feeling like you need to really clean up, or do a deep clean after all of this, run this command:

    ```text
    $ docker-compose down --volumes
    ```

    ```text
    $ docker system prune -a -f
    ```

    >**NOTE**: You'll thank me later!