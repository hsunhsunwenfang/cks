

- General knowledge and online resources
    - CVE
        - https://nvd.nist.gov/vuln/search
        - Search for Kubernetes in the search bar
            - Track the CVEs : Known Vulnerabilities
    - CIS benchmarks
        - https://www.cisecurity.org/benchmark/kubernetes/
        - Recommendations for Kubernetes Security
    - CIS CAT
        - Download the CIS CAT Lite tool
            - Fill the form and get the tool in email sent by CIS
        - Scan by the downloaded tool
            - Get the report in html format

- Container image supply chain
    - Providers
        - Prisma Cloud, NeuVector, Twistlock, Aqua Security, Sysdig, StackRox, Anchore, Qualys, Tenable, Alcide, etc.
    - Grafeas
        - https://grafeas.io/
        - Open source artifact metadata API -> Normalized metadata from multiple providers

- Container runtime security
    - Providers
        - gVisor, Kata Containers, Nabla Containers, Firecracker, etc.
    - RuntimeClass
        - A k8s resource for api-server and kubelet to communicate with container runtime
    - Setup RuntimeClass
        - kubectl apply -f https://raw.githubusercontent.com/killer-sh/cka-resources/master/runtimeclass.yaml
        - Specify the runtimeClass in the pod spec
            - runtimeClassName: gvisor
    - gVisor
        - a golang sandbox used along with rule-based execution tools such as SELinux, AppArmor, 
        - runsc is the runtime entrypoint for gVisor to work with containerd or other OCI-compliant runtimes
        - Containers using gVisor has 2 process
            - Sentry
                - A process that runs as root and is responsible for managing the sandbox
            - Gofer
                - A process that runs as the container user and is responsible for executing the application
        - Sentry and Gofer communicate via 9p protocol
            - 9p is faster than VMs
        - As gVisor intercepts system calls, it causes a performance overhead and security risk
    - Keta
        - provides per-container kernel instead of per-node kernel
  
- Trusted Packages
    - TUF (The Update Framework)
        - https://theupdateframework.io/
        - A framework for securing software
    - Notary
        - verify the integrity and authenticity of container images

- Policy based tools
    - OPA (Open Policy Agent)
        - https://www.openpolicyagent.org/
        - A general-purpose policy engine
        - A declarative language called Rego
        - A policy is a set of rules and A rule is a boolean expression
    - Gatekeeper
        - validating admission controller handles connection from api-server to OPA
    - ContraintTemplate for OPA
        - CRD (Custom Resource Definition)



- kube-bench