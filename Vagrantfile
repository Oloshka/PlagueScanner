system('docker network ls | grep plaguescanner-net > /dev/null ; if [ $? -eq 1 ]; then docker network create --subnet=172.18.1.0/24 plaguescanner-net; fi')

docker_nodes = [
  { :name       => 'core', 
    :ports      => ["8080:80"],
    :build_args => ["-t", "plaguescanner/core:vagrant-build"],
    :run_args   => ["--net=plaguescanner-net", "--ip=172.18.1.5"],
    :volumes    => ["/opt:/mnt"],
    :dockerfile => 'Dockerfile.core'
  },
  { :name       => 'clam',
    :ports      => [],
    :build_args => ["-t", "plaguescanner/clam:vagrant-build"],
    :run_args   => ["--net=plaguescanner-net", "--ip=172.18.1.10"],
    :volumes    => [],
    :dockerfile => 'Dockerfile.clam'
  },
  { :name       => 'bitdefender',
    :ports      => [],
    :build_args => ["-t", "plaguescanner/bitdefender:vagrant-build"],
    :run_args   => ["--net=plaguescanner-net", "--ip=172.18.1.11"],
    :volumes    => [],
    :dockerfile => 'Dockerfile.bitdefender'
  },
]

Vagrant.configure("2") do |config|
  docker_nodes.each do |node|
    config.vm.define node[:name] do |nodeconfig|
      nodeconfig.vm.provider "docker" do |d|
        d.volumes     = node[:volumes]
        d.ports       = node[:ports]
        d.build_args  = node[:build_args]
        d.create_args = node[:run_args]
        d.build_dir   = "."
        d.has_ssh     = false
        d.name        = node[:name]
        d.dockerfile  = node[:dockerfile]
      end
    end
  end
end

