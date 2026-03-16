# jenkins-scm-poc

Jenkins SCM 기반 PoC를 위한 최소 예제입니다.

추가된 구성:

- `config/k8s/network-policy.yaml`: `hello-batch` Pod에 대해 ingress 전부 차단, DNS egress만 허용
- `config/k8s/hello-batch-job.yaml`: `busybox` 기반 Hello World Kubernetes Job
- `Jenkinsfile`: NetworkPolicy 적용 후 Job 실행 및 로그 확인

수동 실행 예시:

```sh
kubectl apply -f config/k8s/network-policy.yaml
kubectl apply -f config/k8s/hello-batch-job.yaml
kubectl logs job/hello-batch
```
