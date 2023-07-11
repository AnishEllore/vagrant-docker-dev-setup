Vagrant.configure(2) do |config|

# ... your existing config

  # Custom configuration for docker
  config.vm.provider "docker" do |docker, override|
    # docker doesnt use boxes
    override.vm.box = nil

    # this is where your Dockerfile lives
    docker.build_dir = "."

    # Make sure it sets up ssh with the Dockerfile
    # Vagrant is pretty dependent on ssh
    override.ssh.insert_key = true
    docker.has_ssh = true
    docker.privileged = true
    # Setting docker args to run. Gives us a lot of power!
    docker.create_args = ["--hostname=goplay", "--cap-add", "NET_ADMIN", "-m", "2g", "--cpus=2"]
    # Configure Docker to allow access to more resources
    
  end
  config.vm.synced_folder "..", "/project"
  config.vm.provision "shell", inline: <<-SHELL
    apt update
    apt install -y net-tools build-essential
    ln -s /project /home/vagrant/project
    sed -i "1s/^/127.0.0.1 goplay \n/" /etc/hosts
    apt install vim sudo -y
    apt-get install -y curl git cmake tmux vim wget
    wget https://go.dev/dl/go1.20.6.linux-amd64.tar.gz
    rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.6.linux-amd64.tar.gz
    export PATH=$PATH:/usr/local/go/bin
    go version
    echo "All installtion done."
  SHELL

# ...

end