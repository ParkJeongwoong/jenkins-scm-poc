pipeline {
    agent {
        kubernetes {
            defaultContainer 'kubectl'
            yaml '''
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins
  securityContext:
    runAsUser: 1000
    runAsGroup: 1000
    fsGroup: 1000
  containers:
    - name: kubectl
      image: bitnami/kubectl:latest
      command:
        - /bin/sh
        - -c
        - cat
      tty: true
'''
        }
    }

    environment {
        K8S_NAMESPACE = 'jenkins'
    }


    stages {
        stage('Apply NetworkPolicy') {
            steps {
                container('kubectl') {
                    sh 'kubectl apply -n ${K8S_NAMESPACE} -f config/k8s/network-policy.yaml'
                }
            }
        }

        stage('Run Hello Batch') {
            steps {
                container('kubectl') {
                    sh '''
                        kubectl delete job hello-batch -n ${K8S_NAMESPACE} --ignore-not-found
                        kubectl apply -n ${K8S_NAMESPACE} -f config/k8s/hello-batch-job.yaml
                        kubectl wait -n ${K8S_NAMESPACE} --for=condition=complete job/hello-batch --timeout=120s
                        kubectl logs -n ${K8S_NAMESPACE} job/hello-batch
                    '''
                }
            }
        }
    }
}
