# jenkins-scm-poc

Jenkins SCM 기반 PoC를 위한 최소 예제입니다.

추가된 구성:

- `config/k8s/network-policy.yaml`: `hello-batch` Pod에 대해 ingress 전부 차단, DNS egress만 허용
- `config/k8s/hello-batch-job.yaml`: `busybox` 기반 Hello World Kubernetes Job
- `Jenkinsfile`: Kubernetes Plugin 기반 Jenkins Agent Pod에서 `kubectl` 컨테이너로 NetworkPolicy 적용 후 Job 실행 및 로그 확인

Jenkins 전제 조건:

- Kubernetes Plugin이 설치되어 있어야 함
- `jenkins` ServiceAccount가 대상 namespace에 대해 `Job`, `Pod`, `NetworkPolicy`를 생성/조회할 권한이 있어야 함
- Jenkins가 Kubernetes Cloud에 연결되어 있어야 함

수동 실행 예시:

```sh
kubectl apply -f config/k8s/network-policy.yaml
kubectl apply -f config/k8s/hello-batch-job.yaml
kubectl logs job/hello-batch
```
