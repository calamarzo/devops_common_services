


Use Kustomize to quickly deploy the 3 enviroments

If necessary we can create the Helm templates (but it is time consuming for this exercise)

no logging (fluent) neither monitoring (prometheus)

No security groups defined (security context, runas, etc.)

No priority class assigned

each enviroment in a different namespace, but ideally should be different cluster also

Deployments with constraints (https://kubernetes.io/docs/concepts/workloads/pods/pod-topology-spread-constraints/), and pod disruption 

Ingress with AWS ALB Ingress Controller, need to check the details for deployment, not used to work with AWS

Port 80 for web traffic (not recommended, we should redirect to 443, but that way we show the policy for DynamoDB), port 443 for DynamoDB API, port 3306 for RDS and port 53 for DNS




Jenkins role on kubernetes cluster to allow deployments (rbac.authorization.k8s.io/v1beta1)

Validate on changerequest and Deploy on merge

Pipeline has not been tested
