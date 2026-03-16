pipeline {
    agent {
        kubernetes {
            defaultContainer 'kubectl'
            yaml '''
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins
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
        stage('Run Hello Batch') {
            steps {
                sh '''
                    set -euxo pipefail
                    echo "START"
                    date
                    whoami
                    pwd
                    ls -la
                    which kubectl
                    kubectl version --client
                    echo "BEFORE GET NS"
                    kubectl get ns --request-timeout=5s
                    echo "AFTER GET NS"
                    kubectl auth can-i create jobs.batch -n ${K8S_NAMESPACE}
                    echo "BEFORE DELETE JOB"
                    kubectl delete job hello-batch -n ${K8S_NAMESPACE} --ignore-not-found=true
                    echo "AFTER DELETE JOB"
                    kubectl apply -n ${K8S_NAMESPACE} -f config/k8s/hello-batch-job.yaml
                    echo "AFTER APPLY JOB"
                    kubectl wait -n ${K8S_NAMESPACE} --for=condition=complete job/hello-batch --timeout=120s
                    echo "AFTER WAIT JOB"
                    kubectl logs -n ${K8S_NAMESPACE} job/hello-batch
                    echo "END"
                '''
            }
        }
        // stage('Apply NetworkPolicy') {
        //     steps {
        //         container('kubectl') {
        //             sh 'kubectl apply -n ${K8S_NAMESPACE} -f config/k8s/network-policy.yaml'
        //         }
        //     }
        // }

        // stage('Run Hello Batch') {
        //     steps {
        //         container('kubectl') {
        //             sh '''
        //                 kubectl delete job hello-batch -n ${K8S_NAMESPACE} --ignore-not-found
        //                 kubectl apply -n ${K8S_NAMESPACE} -f config/k8s/hello-batch-job.yaml
        //                 kubectl wait -n ${K8S_NAMESPACE} --for=condition=complete job/hello-batch --timeout=120s
        //                 kubectl logs -n ${K8S_NAMESPACE} job/hello-batch
        //             '''
        //         }
        //     }
        // }
    }
}
