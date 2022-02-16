# compile the binary
local_resource('hello-tilt-go-compile',
  'CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build',
  dir='hello-tilt-go',
  deps=['./hello-tilt-go/main.go'],
  labels=['Go'])

# build the Go image
docker_build('lreimer/hello-tilt-go', 'hello-tilt-go', 
  dockerfile='hello-tilt-go/Dockerfile',
  only=['./hello-tilt-go', "./favicon.ico"])

# deploy and port forward to pod
k8s_yaml(['hello-tilt-go/k8s/deployment.yaml', 'hello-tilt-go/k8s/service.yaml'])
k8s_resource(workload='hello-tilt-go', port_forwards='19090:9090', labels=['Go'])

# build the WAR file
local_resource('hello-tilt-java-build',
  './gradlew assemble',
  dir='hello-tilt-java',
  deps=['./hello-tilt-java/build.gradle', './hello-tilt-java/src/'],
  labels=['Java'])

# build the Java image
docker_build('lreimer/hello-tilt-java', 'hello-tilt-java', 
  dockerfile='hello-tilt-java/Dockerfile',
  only=['./build/libs/'])

# example of using JIB build
# custom_build('lreimer/hello-tilt-java',
#   'cd hello-tilt-java && ./gradlew jibDockerBuild --image $EXPECTED_REF',
#   deps=['./hello-tilt-java/build.gradle', './hello-tilt-java/src/'])

# deploy and port forward to microservice and database
k8s_yaml(kustomize('hello-tilt-java/k8s/overlays/dev'))
k8s_resource(workload='hello-tilt-java', port_forwards=[port_forward(18080, 8080, 'REST API', '/api/message')], labels=['Java'])

k8s_resource(workload='database', port_forwards='5432:5432', labels=['Utility'])
k8s_resource(workload='hoverfly', port_forwards=[port_forward(8888, 8888, 'Admin')], labels=['Utility'])
# a serving command as local resource
local_resource('hello-tilt-go-svc', serve_cmd='kubectl port-forward -n default svc/hello-tilt-go 9090:9090',
  labels=['Utility'])
