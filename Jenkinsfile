pipeline {
  environment {
        DOCKERHUB_CREDENTIALS=credentials('haleema-dockerhub')
    }
  agent none 
  stages {
 
        stage('On-RPI') {
          options {
                timeout(time: 20, unit: "SECONDS")
            }
        agent {label 'linuxslave1'}
          
          steps {
            script { 
            try {
           sh 'echo "collecting data from sound sensor" '
           git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-RPI-sound-sensor.git'
           //sh 'docker build -t haleema/docker-rpi-sound:latest .'
           sh 'docker run --privileged -t haleema/docker-rpi-sound'

            sh 'echo "collecting data from flame sensor" '
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-RPI-flame-sensor.git'
            //sh 'docker build -t haleema/docker-rpi-flame:latest .'
            sh 'docker run --privileged -t haleema/docker-rpi-flame'
            
            sh 'echo "collecting data from DHT11 sensor" '
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-RPI-DHT11-sensor.git'
            //sh 'docker build -t haleema/docker-rpi-dht:latest .'
            sh 'docker run --privileged -t haleema/docker-rpi-dht'
              sleep(time: 4, unit: "SECONDS")
               } catch (Throwable e) {
                        echo "Caught ${e.toString()}"
                        currentBuild.result = "SUCCESS" //currentBuild.result = 'SUCCESS'
                    }

            }
    }
          
     stage('On-Edge-rec-data') {
      agent any
        steps {
            sh 'echo "edge-rec-3con-data"'
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge-rec.git'
            sh 'docker build -t haleema/docker-edge1:latest .'
            echo "Started stage A"
            //sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge1'
            }
        }
           
    }
}

        
