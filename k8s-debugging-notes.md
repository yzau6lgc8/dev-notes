# Kubernetes Debugging Notes

Quick commands and heuristics for diagnosing failing pods/services.

## Pod stuck in Pending
- Check scheduler events: `kubectl describe pod <pod> -n <ns>`
- Common causes: insufficient CPU/memory, node affinity, taints, or unbound PVC.
- Inspect node resources: `kubectl top nodes`

## CrashLoopBackOff
- View logs: `kubectl logs <pod> --previous`
- If logs are empty, check `kubectl describe pod` for failed readiness/liveness probes.
- Verify ConfigMap/Secret values are mounted correctly.

## Service not resolving
- Confirm endpoints exist: `kubectl get endpoints <service>`
- Check selector labels match pod labels: `kubectl get pods --show-labels`
- Use a temporary busybox pod to test DNS: `kubectl run -it --rm debug --image=busybox -- sh`

## ImagePullBackOff
- Check events: `kubectl describe pod`
- Validate image name/tag and registry credentials.
- For private registries, ensure the imagePullSecrets are attached to the Deployment.

## Useful aliases
```bash
alias k='kubectl'
alias kd='kubectl describe'
alias kg='kubectl get'
alias kl='kubectl logs'
```
