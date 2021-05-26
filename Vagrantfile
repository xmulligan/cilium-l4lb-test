# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant box for testing kind with cgroup v2
# Adopted from https://github.com/kubernetes-sig/kind
Vagrant.configure("2") do |config|
  config.vm.box = "fedora/33-cloud-base"
  memory = 2048
  cpus = 2
  config.vm.provider :virtualbox do |v|
    v.memory = memory
    v.cpus = cpus
  end
  config.vm.provider :libvirt do |v|
    v.memory = memory
    v.cpus = cpus
  end
  config.vm.provision "install-packages", type: "shell", run: "once" do |sh|
    sh.inline = <<~SHELL
    set -eux -o pipefail
    dnf install -y golang-go make kubernetes-client clang llvm

    # The moby-engine package (v19.03) included in Fedora 33 does not support cgroup v2.
    # So we need to install Docker 20.10 (or later) from the upstream.
    curl -fsSL https://get.docker.com | sh
    systemctl enable --now docker
    SHELL
  end
end