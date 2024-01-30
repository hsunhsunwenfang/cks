
# Image Supply Chain Security

- Trow, Prisma Cloud, NeuVector, Twistlock, Aqua Security, Sysdig, StackRox, Anchore, Qualys, Tenable, Alcide, etc.

# Try out Trivy

```bash
# Install Trivy
sudo curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sudo sh -s -- -b /usr/local/bin v0.37.1
# Test a know flawwed image with "trivy image"
trivy image knqyf263/vuln-image:1.2.3
# Test nginx image -> still has some flaws
trivy image nginx
```

# OPA (Open Policy Agent)

# Sandbox tools

- gVisor, kata containers, Nabla containers, Firecracker, etc.

# Secure RuntimeClass

```bash
# setup a containerd cluster
cd cks/lfs260/labs/use-first-node/LFS260/SOLUTIONS/s_04
# install with containerd-setup.txt
```