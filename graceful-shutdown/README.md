# Graceful Shutdown

## Test

### Start console1 for a test server
+ Start a server test pod
    ```bash
    kubectl apply -f graceful-shutdown/
    ```

### Start Console2 for a test client
+ Start a client test pod
    ```bash
    kubectl run bcurl -it --image=yauritux/busybox-curl:latest  -- sh
    ```

+ Access the tested nginx in a long loop
    ```bash
    for var in `seq 1 1000000`; do curl -I http://mikeservice.miketest; done
    ```

### Test and expected Result
+ Delete pod1
    Its status will be changed to "Terminating", and after around 50 seconds, pod1 will be removed.
    > The "terminationGracePeriodSeconds" was set to 60s from default 30s, so the "preStop" hook can occupy 50s to do some clean up. 

+ Check the access in test client in console2
    There is no access disconnected, because the access has been redirected to pod2.

## Reference
+ https://learnk8s.io/graceful-shutdown
+ https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-terminating-with-grace


