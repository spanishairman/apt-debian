# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.
  config.vm.define "Debian12" do |srv|

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
    srv.vm.box = "/home/max/vagrant/images/debian12"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  #  config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "private_network", ip: "192.168.33.10", adapter: "5", type: "ip"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.
  # system("virsh net-create /home/max/vagrant/vagrant-libvirt-mgmt.xml")
    srv.vm.provider "libvirt" do |lv|
      lv.memory = "2048"
      lv.cpus = "2"
      lv.title = "Debian12"
      lv.description = "Виртуальная машина на базе дистрибутива Debian Linux"
      lv.management_network_name = "vagrant-libvirt-mgmt"
      lv.management_network_address = "192.168.121.0/24"
      lv.management_network_keep = "true"
      lv.management_network_mac = "52:54:00:27:28:83"
      lv.storage :file, :size => '1G', :device => 'vdb', :allow_existing => false
    end
    srv.vm.provision "shell", inline: <<-SHELL
      brd='*************************************************************'
      echo "$brd"
      echo 'Изменим ttl для работы через раздающий телефон'
      echo "$brd"
      sysctl -w net.ipv4.ip_default_ttl=66
      echo "$brd"
      echo 'Если ранее не были установлены, то установим необходимые  пакеты'
      echo "$brd"
    apt install -y apache2 dpkg-dev build-essential zlib1g-dev libpcre3 libpcre3-dev unzip git cmake reprepro
    echo "$brd"
    echo 'Создаём директорию для работы с исходниками'
    echo "$brd"
    mkdir /home/vagrant/custom-nginx && cd $_
    echo "$brd"
    echo 'Текущая директория' $(echo $(pwd))
    echo "$brd"
    sleep 10
    echo "$brd"
    echo 'Загружаем исходники пакета nginx'
    echo "$brd"
    sleep 10
    apt source nginx
    echo "$brd"
    echo 'Создаём директорию для модуля brotli и переходим в неё'
    echo "$brd"
    mkdir nginx-1.22.1/debian/modules && cd $_
    echo "$brd"
    echo 'Текущая директория' $(echo $(pwd)) 
    echo "$brd"
    sleep 10
    echo "$brd"
    echo 'Клонируем репозиторий ngx_brotli'
    echo "$brd"
    git clone --recurse-submodules -j8 https://github.com/google/ngx_brotli
    echo "$brd"
    echo 'Код завершения загрузки репозитория' $(echo $?)
    echo "$brd"
    sleep 10
    echo "$brd"
    echo 'Создаём директорию out для сборки модуля brotli и переходим в неё'
    echo "$brd"
    mkdir ngx_brotli/deps/brotli/out && cd $_
    echo "$brd"
    echo 'Текущая директория' $(echo $(pwd)) 
    echo "$brd"
    sleep 5
    echo "$brd"
    echo 'Запускаем копмпиляцию модуля'
    echo "$brd"
    cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=OFF -DCMAKE_C_FLAGS="-Ofast -m64 -march=native -mtune=native -flto -funroll-loops -ffunction-sections -fdata-sections -Wl,--gc-sections" -DCMAKE_CXX_FLAGS="-Ofast -m64 -march=native -mtune=native -flto -funroll-loops -ffunction-sections -fdata-sections -Wl,--gc-sections" -DCMAKE_INSTALL_PREFIX=./installed ..
    echo "$brd"
    echo 'Код завершения для сборки' $(echo $?)
    echo 'Текущая директория' $(echo $(pwd)) 
    echo "$brd"
    sleep 10
    cmake --build . --config Release --target brotlienc
    echo "$brd"
    echo 'Код завершения для сборки' $(echo $?)
    echo "$brd"
    sleep 10
    cd /home/vagrant/custom-nginx/nginx-1.22.1/
    echo "$brd"
    echo 'Текущая директория' $(echo $(pwd)) 
    echo "$brd"
    sleep 10
    echo "$brd"
    echo 'Добавим строку --add-module=/path_to_brotli/ngx_brotli в секцию configure файла rules'
    echo "$brd"
    sleep 10
    sed -i.bak 's/--with-threads/--with-threads --add-module=\\/home\\/vagrant\\/custom-nginx\\/nginx-1.22.1\\/debian\\/modules\\/ngx_brotli/' /home/vagrant/custom-nginx/nginx-1.22.1/debian/rules
    echo "$brd"
    echo 'Код выполнения модификации' $(echo $?)
    echo "$brd"
    cat /home/vagrant/custom-nginx/nginx-1.22.1/debian/rules | grep "add-module"
    sleep 10
    echo "$brd"
    echo 'Установим зависимости, необходимые для сборки пакетов'
    echo "$brd"
    sleep 10
    apt install -y $(dpkg-checkbuilddeps 2>&1 | sed 's/dpkg-checkbuilddeps:\sошибка:\sUnmet build dependencies: //g' | sed 's/[\(][^)]*[\)] //g')
    echo "$brd"
    echo 'Код выполнения установки' $(echo $?)
    echo "$brd"
    echo "$brd"
    echo 'Собираем пакеты'
    echo "$brd"
    sleep 10
    dpkg-buildpackage -b
    echo 'Код завершения сборки' $(echo $?)
    sleep 10
    echo "$brd"
    echo "$brd"
    echo "$brd"
    echo 'Создание репозитория с пакетом nginx'
    echo "$brd"
    echo "$brd"
    echo "$brd"
    sleep 10
    echo "$brd"
    echo 'Создаём структуру каталогов'
    echo "$brd"
    mkdir -p /var/www/repo/conf && cd $_
    echo "$brd"
    echo 'Текущая директория' $(echo $(pwd))
    echo "$brd"
    sleep 10
    echo "$brd"
    echo 'Заполняем файл конфигурации'
    echo "$brd"
    echo 'Origin: Debian' >> distributions
    echo 'Codename: nginx-brotli' >> distributions
    echo 'Suite: stable' >> distributions
    echo 'Version: 12' >> distributions
    echo 'Architectures: amd64' >> distributions
    echo 'Components: main contrib non-free' >> distributions
    echo 'UDebComponents: main' >> distributions
    echo 'cat distributions'
    cat distributions
    echo "$brd"
    sleep 10
    echo "$brd"
    echo 'Сначала сбрасываем, затем инициализируем репозиторий'
    echo "$brd"
    reprepro -b /var/www/repo/ clearvanished
    reprepro -b /var/www/repo/ export
    echo "$brd"
    echo 'Добавляем ранее созданные пакеты в репозиторий'
    echo "$brd"
    reprepro -b /var/www/repo/ includedeb nginx-brotli /home/vagrant/custom-nginx/*.deb
    echo "$brd"
    echo 'Настраиваем веб сервер, изменяем значение корневой директории'
    echo 'Было'
    echo "$brd"
    cat /etc/apache2/sites-enabled/000-default.conf | grep "www"
    sleep 5
    sed -i.bak 's/html/repo/' /etc/apache2/sites-enabled/000-default.conf
    echo "$brd"
    echo 'Стало'
    echo "$brd"
    cat /etc/apache2/sites-enabled/000-default.conf | grep "www"
    sleep 5
    apache2ctl graceful
    echo "$brd"
    echo 'Редактируем источники и обновляем информацию о пакетах'
    echo "$brd"
    echo 'deb [trusted=yes] http://192.168.121.10/ nginx-brotli main contrib non-free' > /etc/apt/sources.list.d/nginx-brotli.list
    apt update
      SHELL
  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
    end
  config.vm.define "Debian12c" do |clnt|
    clnt.vm.box = "/home/max/vagrant/images/debian12"
    clnt.vm.provider "libvirt" do |lv|
    lv.memory = "2048"
    lv.cpus = "2"
    lv.title = "Debian12c"
    lv.description = "Виртуальная машина на базе дистрибутива Debian Linux"
    lv.management_network_name = "vagrant-libvirt-mgmt"
    lv.management_network_address = "192.168.121.0/24"
    lv.management_network_keep = "true"
    lv.management_network_mac = "52:54:00:27:28:84"
    lv.storage :file, :size => '1G', :device => 'vdb', :allow_existing => false
  end
  clnt.vm.provision "shell", inline: <<-SHELL
    brd='*************************************************************'
    echo "$brd"
    echo 'Изменим ttl для работы через раздающий телефон'
    echo "$brd"
    sysctl -w net.ipv4.ip_default_ttl=66
    echo "$brd"
    echo 'Если ранее не были установлены, то установим необходимые  пакеты'
    echo "$brd"
    echo "$brd"
    echo 'Редактируем источники пакетов на клиентской машине'
    echo "$brd"
    echo "$brd"
    echo "$brd"
    sed -i.bak 's/^deb/#deb/' /etc/apt/sources.list
    echo 'deb [trusted=yes] http://192.168.121.10/ nginx-brotli main' > /etc/apt/sources.list.d/nginx-brotli.list
    echo "$brd"
    cat /etc/apt/sources.list.d/nginx-brotli.list
    sleep 10
    echo "$brd"
    SHELL
  end

  # config.vm.define "ArchLinux1" do |srv1|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
    # srv1.vm.box = "/home/max/vagrant/images/debian12"
  # config.vm.provider "libvirt" do |lv|
  #   lv.memory = "1024"
  #   lv.cpu = "2"
  # end
  # end
end
