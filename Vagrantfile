Vagrant.require_version ">= 1.8.0" # for ansible_local

# Check to determine whether we're on a windows or linux/os-x host,
# later on we use this to launch ansible in the supported way
# source: https://stackoverflow.com/questions/2108727/which-in-ruby-checking-if-program-exists-in-path-from-ruby
def which(cmd)
    exts = ENV['PATHEXT'] ? ENV['PATHEXT'].split(';') : ['']
    ENV['PATH'].split(File::PATH_SEPARATOR).each do |path|
        exts.each { |ext|
            exe = File.join(path, "#{cmd}#{ext}")
            return exe if File.executable? exe
        }
    end
    return nil
end

Vagrant.configure(2) do |config|

  config.vm.box = "debian/jessie64"
  config.vm.network :private_network, ip: "192.168.33.99"

  if which('ansible-playbook')
    config.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.skip_tags = "dhparam"
      ansible.extra_vars = {
        nginx_disable_ssl: true
      }
      ansible.playbook = "playbook.yml"
    end
  else
    config.vm.provision "ansible_local" do |ansible|
      ansible.install_mode = "pip"
      ansible.version = "2.3.0.0"
      ansible.verbose = "v"
      ansible.skip_tags = "dhparam"
      ansible.extra_vars = {
        nginx_disable_ssl: true
      }
      ansible.playbook = "playbook.yml"
    end
  end

  config.vm.synced_folder ".", "/vagrant", type: "nfs"
end
