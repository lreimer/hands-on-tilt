# compile the binary
local_resource('hello-tilt-go-compile',
  'CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build',
  dir='hello-tilt-go',
  deps=['./hello-tilt-go/main.go'])

# build the image
docker_build('lreimer/hello-tilt-go', 'hello-tilt-go', 
  dockerfile='hello-tilt-go/Dockerfile',
  only=['./hello-tilt-go', "./favicon.ico"])

# deploy and port forward to pod
k8s_yaml(['hello-tilt-go/k8s/deployment.yaml', 'hello-tilt-go/k8s/service.yaml'])
k8s_resource(workload='hello-tilt-go', port_forwards='19090:9090')

local_resource('hello-tilt-go-svc', serve_cmd='kubectl port-forward -n default svc/hello-tilt-go 9090:9090')