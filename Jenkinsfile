pipeline {
  agent {
    node {
      label 'label'
    }

  }
  stages {
    stage('Build') {
      steps {
        withMaven(maven: 'M3') {
          sh 'mvn clean install'
        }

      }
    }
    stage('Results') {
      steps {
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'target/*.jar'
      }
    }
  }
  environment {
    label = 'mypod-${UUID.randomUUID().toString()}'
    podTemplate(label: label, 
  serviceAccount: 'jenkins-server',
  containers: [
    containerTemplate(
      name: 'docker', 
      image: 'docker:17.12.1-ce',
      ttyEnabled: true, 
      command: 'cat',
      envVars: [
        envVar(key: 'DOCKER_HOST', value: 'tcp://dind:2375')
      ]
    ),
    containerTemplate(
      name: 'helm', 
      image: 'lachlanevenson/k8s-helm:v2.8.1', 
      ttyEnabled: true, 
      command: 'cat'
    )
  ],
  volumes: [
    emptyDirVolume(mountPath: '/var/lib/docker', memory: false)
  ])
  }}
