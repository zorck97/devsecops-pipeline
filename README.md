# Example DevSecOps pipeline

[![Build Status](https://travis-ci.org/zorck97/gs-rest-service.svg?branch=master)](https://travis-ci.org/zorck97/gs-rest-service)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=com.example%3Arest-service&metric=alert_status)](https://sonarcloud.io/dashboard?id=com.example%3Arest-service)

## Introduction

This is an example DevSecOps pipeline which uses [spring-guides/gs-rest-service](https://github.com/spring-guides/gs-rest-service). The referenced project was forked to use an existing application as a baseline for a devsecops pipeline.

## Goals

Overall goals

1. As a developer I should be able to start the build and security checks locally
2. Master first development with no pull requests (this is only an example)
3. Build a docker image containing the application
4. The docker image should not be pushed to a registry

DevSecOps related goals

1. Check the imported libraries for existing, known vulnerabilities. Let the build fail, if vulnerability with CVSS Score >= 8 is found.
2. Scan the created docker image and let the build fail, if critical a critical vulnerability was found.

## Prerequisites

The following tools have to be installed or must be available:

- Java JDK 8
- Sonarcloud or SonarQube server (local possible)

## Process

The process consists of two major parts. Maven is used to schedule the build and additional security related tasks. Travis is the CI software / pipeline scheduler. 

### Maven build-lifecycle

The following table shows a selection of the build-phases and list what additional plugins are bind to which phase.

Phase | Description
----- | -------
clean | Cleanup all artifacts and start from the beginning
validate | Validate the project is correct and all necessary information is available
compile | Compile all source code files
test | Run JUnit tests
package | Build docker image
verify | Start the sonar scanner, publish test artifacts to Sonarcloud and run OWASP dependency check

### Travis CI lifecycle

Action | Step
------ | ---- 
Checkout | Checkout repo or branch
Install trivy | Get latest version
, | Download archived executable
, | Unpack executable in current directory
Maven Goals | Run goals clean and compile
Maven Goal | Run verify goal
Docker security scan (identify) | Identify medium and high vulnerabilities and save report
Docker security scan (block) | Identify critical vulnerabilities, fail pipeline if any found

Right now the project is configured not to fail, if critical vulnerabilites are found. This can be changed by uncommenting the specific configurations.