# Cilium

## Securing Cluster

### Network Policy

```shell
apiVersion: cillium.io/v2
kind: CilliumNetworkPolicy
metadata:
  name: ingress-policy
  namespace: endor
spec:
  endpointSelector: # Who Policy Applies to
    matchLabels:
      class: deathstart
      org: empire
  ingress: # Traffic Direction: Ingress = Traffic to the pods this policy applies to
    - fromEndpoints: # From which source endpoints, over which ports
        - matchLabels:
            class: tiefighter
            org: empire
        toPorts:
          - ports:
            - port: "80"
```

```shell
apiVersion: cillium.io/v2
kind: CilliumNetworkPolicy
metadata:
  name: egress-policy
  namespace: endor
spec:
  endpointSelector: # Who Policy Applies to
    matchLabels:
      class: tiefighter
      org: empire
  egress: 
    - toFQDNs:
        - matchName: disney.com
      toPorts:
        - ports:
          - port: "443"
    - toFQDNs:
        - matchName: swapi.dev
      toPorts:
        - ports:
          - port: "443"
      toEndpoints:
      - matchLabels:
          class: deathstar
          org: empire
      toPorts:
      - ports:
          - port: "80"
```

## Gateway Api

[Documentation](https://docs.cilium.io/en/stable/network/servicemesh/gateway-api/gateway-api/)

```yaml
# Source: cilium/templates/cilium-gateway-api-class.yaml
apiVersion: gateway.networking.k8s.io/v1
kind: GatewayClass
metadata:
  name: cilium
spec:
  controllerName: io.cilium/gateway-controller

```

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: robot-shop-gateway
  namespace: robot-shop
spec:
  gatewayClassName: cillium
  listeners:
    - protocol: HTTP
      port: 80
      name: http-listener
      allowedRoutes:
        namespaces:
          from: Same
```

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: web-http-route
  namespace: robot-shop
spec:
  parentRefs:
    - name: robot-shop-gateway
#  hostnames:
#    - "example.com"
  rules:
    - backendRefs:
        - name: web
          port: 8080
```

### LoadBalancer IPAM

```yaml
apiVersion: "cilium.io/v2alpha1"
kind: CiliumLoadBalancerIPPool
metadata:
name: "pool"
spec:
cidrs:
- cidr: "20.0.10.0/24"
```

### Layer 2

```yaml
apiVersion: "cilium.io/v2alpha1"
kind: CiliumL2AnnouncementPolicy
metadata:
  name: l2announcement-policy
spec:
  nodeSelector:
    matchExpressions:
      - key: node-role.kubernetes.io/control-plane
        operator: DoesNotExist
  interfaces:
    - eth1
  externalIPs: true
  loadBalancerIPs: true
```

# Hubble

```shell
cilium hubble port-forward&
hubble observe --last 20
hubble observe -f
hubble observe -f --output json | jq .
hubble observe -f --port 53 -t l7
```

## Hubble UI

```shell
cilium hubble ui
```