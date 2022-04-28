// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            // Or, to avoid YAML:
            // containerTemplate {
            //     name 'shell'
            //     image 'ubuntu'
            //     command 'sleep'
            //     args 'infinity'
            // }
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: registry.glams.com/glams/jenkins-agent:latest
    command:
    - sleep
    args:
    - infinity
    tty: true
    resources:
      requests:
        memory: "2Gi"
    volumeMounts:
     - name: docker-sock
       mountPath: /var/run/docker.sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock       
'''
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            defaultContainer 'shell'
        }
    }
    stages {
        stage('docker build') {
            steps {
                sh 'docker build -t sainikhil1999/myapp .'
            }
        }
        stage('docker login') {
            steps {
                sh 'docker login -u sainikhil1999 -p Akhil@1999'
            }
        }
         stage('docker push') {
            steps {
                sh 'docker push sainikhil1999/myapp'
            }
         }
        agent {
            kubernetes {
                // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            // Or, to avoid YAML:
            // containerTemplate {
            //     name 'shell'
            //     image 'ubuntu'
            //     command 'sleep'
            //     args 'infinity'
            // }
             yaml '''
             
             
 apiVersion: apps/v1

kind: Deployment

metadata:

  name: myapp

spec:

  selector:

    matchLabels:

      run: myapp

  replicas: 1

  template:

    metadata:

      labels:

        run: myapp

    spec:

      containers:

      - name: myapp

        image: sainikhil1999/myapp

        ports:

        - containerPort: 5000
             
             
            }
        }
            
       }
    }
}
