# SonarQube Integration Guide

## Introduction

This guide provides step-by-step instructions for integrating SonarQube into your projects using both Node.js and Maven.

## Installation Steps

### Helm Chart Installation

1. Add the SonarQube Helm repository:
   ```
helm repo add sonarqube https://SonarSource.github.io/helm-chart-sonarqube
helm repo update
kubectl create namespace sonarqube-lts
helm upgrade --install -n sonarqube-lts sonarqube sonarqube/sonarqube-lts
```

### Port forward to access SonarQube dashboard locally

```
kubectl port-forward service/sonarqube-sonarqube 9000:9000 -n sonarqube
```

If you get the error of max_map_count then run this

```
sudo sysctl -w vm.max_map_count=262144
```

## Node.js Setup

1. Add packages for SonarQube:

```
"jest-sonar-reporter": "^2.0.0",
"sonarqube-scanner": "^2.9.1"
```
Alternatively, install SonarQube Scanner globally:

```
npm install -g sonarqube-scanner
```

2. Create a sonar-project.properties file:

```
sonar.projectKey=sonar-example
sonar.projectName=A Sonarqube project
sonar.sources=src
sonar.host.url=http://localhost:9000
sonar.token=sqp_e1df7b16175a43424b9865848b1fbeacb9358e15
```

3. Run SonarQube analysis using Scanner

```
sonar-scanner
```

## Maven Setup

1. Add the SonarQube plugin in the pom.xml:

```
<build>
  <pluginManagement>
    <plugins>
      <plugin>
        <groupId>org.sonarsource.scanner.maven</groupId>
        <artifactId>sonar-maven-plugin</artifactId>
        <version>3.7.0.1746</version>
      </plugin>
    </plugins>
  </pluginManagement>
</build>
```

2. Run the command for SonarQube analysis: 

```
mvn clean install
mvn clean verify sonar:sonar  -Dsonar.projectKey=gfdf  -Dsonar.projectName='gfdf'  -Dsonar.host.url=http://localhost:9000  -Dsonar.token=sqp_d23fcc815be0656ad8adfe5e0bf9b1421b467652
```

3. Additional Resources

Watch this SonarQube Tutorial - Integration with Jenkins & Maven

```
https://www.youtube.com/watch?v=WXhQHG3zjjk

https://raw.githubusercontent.com/rchidana/calcwebapp/master/Jenkinsfile

https://github.com/rchidana/calcwebapp/tree/master
```
