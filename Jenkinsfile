#!/bin/env groovy

pipeline {
    agent any
    stages {
        stage('Compile') {
			dir('complete'){
				steps {
					sh './mvnw clean compile'
				}
			}
        }
		stage('Test') {
			dir('complete'){
				steps {
					sh './mvnw test'
				}
			}
		}
		stage('Dependency Check') {
			dir('complete'){
				steps {
					sh './mvnw org.owasp:dependency-check-maven:check'
				}
			}
		}
		stage('Integration Test') {
			dir('complete'){
				steps {
					sh './mvnw verify'
				}
			}
		}
		stage('') {
			dir('complete'){
				steps {
					sh './mvnw jib:dockerBuild'
				}
			}
		}
    }
}