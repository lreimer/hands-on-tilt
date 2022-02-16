# Hands-on Tilt

Demo repository for IX magazine article on Tilt and microservice development without pain. 

First, you need to install Tilt as described here: https://docs.tilt.dev/install.html

Create a `Tiltfile` in the project root and add the following content:
```python
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
k8s_resource(workload='hello-tilt-go', port_forwards='18080:8080')
```

The run `tilt up` in the console and get into the flow. Once finished do a `tilt down`.

## Maintainer

M.-Leander Reimer (@lreimer), <mario-leander.reimer@qaware.de>

## License

This software is provided under the MIT open source license, read the `LICENSE`
file for details.