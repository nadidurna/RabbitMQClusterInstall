<H1>Adım 1: Erlang/OTP Yükleme</H1>

![main-page](https://github.com/nadidurna/RabbitMQClusterInstall/blob/master/images/Erlang_logo.webp?raw=true)


Erlang/OTP (Açık Telekom Platformu), yüksek düzeyde ölçeklenebilir ve hataya dayanıklı uygulamalar geliştirmek için bir çerçeve sağlamak üzere Erlang programlama dili üzerine inşa edilmiş bir kitaplıklar, tasarım ilkeleri ve araçlar kümesidir. OTP, diğerleri arasında dağıtılmış bir veritabanı, bir web sunucusu ve bir mesajlaşma sistemi gibi, çoğunlukla Erlang'da yazılmış, kullanıma hazır bir dizi bileşen içerir.

OTP özellikle gerçek zamanlı, dağıtılmış, hataya dayanıklı ve yüksek düzeyde kullanılabilir sistemler geliştirmek için çok uygundur. Geliştiricilerin hataları işleyebilen ve "çökmesine izin ver" felsefesi gibi hatalardan zarif bir şekilde kurtulabilen kodlar yazmasına yardımcı olan bir dizi tasarım ilkesi sağlar. Bu felsefe, geliştiricileri olası her hatayı önlemeye çalışmak yerine sistemlerini hataya dayanıklı olacak şekilde tasarlamaya teşvik eder.

Genel olarak Erlang/OTP, yüksek kullanılabilirlik, ölçeklenebilirlik ve hataya dayanıklılık gerektiren karmaşık, dağıtılmış uygulamalar geliştirmek için güçlü bir platformdur. Telekomünikasyon endüstrisinde olduğu kadar finans, oyun ve sağlık gibi diğer sektörlerde de yaygın olarak kullanılmaktadır.


<h2>Adım 1.1: Erlang GPG Anahtarını Çıkartma</h2>

Erlang deposu GPG anahtarını indirmek için aşağıdaki komutları çalıştırın:

     $> sudo apt update
     $> sudo apt install curl software-properties-common apt-transport-https lsb-release
     $> curl -fsSL https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/erlang.gpg

<h2>Adım 1.2: Ubuntu 20.04 Üzerine Erlang Deposunu Ekleme</h2>
Anahtarı indirdikten sonra aşağodaki komutu çalıştırarak Ubuntu 20.04 sisteminize depoyu ekleyin:

    $> echo "deb https://packages.erlang-solutions.com/ubuntu $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/erlang.list

<h2>Adım 1.3: Ubuntu 20.04 Üzerine Erlang Yükleme</h2>
Son adım ise Erlang' ın yüklenmesidir. Aşağıdaki komutlar ile sistem paket listesini güncelleyin ve Erlang' ı yükleyin:

    sudo apt update
    sudo apt install erlang
---
Erlang shell başlatmak için aşağıdaki komutu yazarak yüklemenizi doğrulayabilirsiniz:

    $ erl
--

    Erlang/OTP 24 [erts-12.1.4] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:1] [jit]
    Eshell V12.1.4  (abort with ^G)
    1> ^G
    --> q

<H1>Adım 2: RabbitMQ Yükleme</H1>

![main-page](https://github.com/nadidurna/RabbitMQClusterInstall/blob/master/images/rabbitmq-logo.webp?raw=true)


<p>
RabbitMQ bir mesaj kuyruğu sistemidir. Benzerleri Apache Kafka, Msmq, Microsoft Azure Service Bus, Kestrel, ActiveMQ olarak sıralanabilir. Amacı herhangi bir kaynaktan alınan bir mesajın, bir başka kaynağa sırası geldiği anda iletilmesidir. Mantık olarak Redis Pub/Sub’a benzemektedir. Ama burada yapılacak işler bir sıraya alınmaktadır. Yani iletimin yapılacağı kaynak ayağa kalkana kadar, tüm işlemler bir quee’de sıralanabilir. Fakat aynı durum Redis Pub’Sub için geçerli değildir. RabbitMQ çoklu işletim sistemine destek vermesi ve açık kaynak kodlu olması da en büyük tercih sebeplerinden birisidir.

Peki neden kullanılmalıdır: Bazı işlemlerin anlık yapılmasına ihtiyaç yoktur. Örnek vermek istenir ise sisteme yeni bir haber girildiğinde, ya da var olan bir haberin güncellenmesi anında cache’in düşürülmesi, bir başka örnek de upload edilen”Gif” dosyalarının scale işleminin yapılması gibi düşünülebilir. Hatta zaman ayarlı message ve otomatik mailler de yine RabbitMq’ya güzel bir örnek olabilir. Sıraya alınan bu işlemlerin asenkron bir şekilde yapılması, hem çalışan uygulamanın boş yere bekletilmemesinden hem de sunucu üzerindeki işlem maliyetinin minimuma indirilmesinden dolayı RabbitMQ iyi bir tercih sebebi olabilir. Ayrıca scalable olmasından dolayı da değişen trafikli yapılarda ayrıca tercih edilebilir.
</p>

<h2>Adım 2.1: Ubuntu' ya RabbitMQ Deposunu Ekleme</h2>

Aşağıdaki komut ile Ubuntu sisteminize RabbitMQ deposunu ekleyin:

    $> curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.deb.sh | sudo bash

<h2>Adım 2.2: Ubuntu 20.04 Üzerinde RabbitMQ Server Yükleme</h2>

Ubuntu 20.04 üzerinde RabbitMQ Server yüklemek için öncelikle sistemin apt listesini güncelleyin:

    $> sudo apt update

Sonrasında rabbitmq-server paketini yükleyin:
    
    $> sudo apt install rabbitmq-server

Yüklemeyi devam ettirmek için **y** tuşuna basın:

    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    The following additional packages will be installed:
    socat
    The following NEW packages will be installed:
    rabbitmq-server soca
    0 upgraded, 2 newly installed, 0 to remove and 27 not upgraded.
    Need to get 12.3 MB of archives.
    After this operation, 15.3 MB of additional disk space will be used.
    Do you want to continue? [Y/n] y

Yükleme sonrasında aşağıdaki ilk komut ile RabbitMQ servisinin sistem başlangıcında çalışmasını aktif edin, sonrasında ise servisin durumunu görüntüleyin:

    $> systemctl enable rabbitmq-server.service
    $> systemctl status rabbitmq-server.service
    ● rabbitmq-server.service - RabbitMQ broker
        Loaded: loaded (/lib/systemd/system/rabbitmq-server.service; enabled; vendor preset: enabled)
        Active: active (running) since Thu 2022-05-12 23:57:22 EAT; 40s ago
    Main PID: 5631 (beam.smp)
        Tasks: 28 (limit: 9460)
        Memory: 95.0M
            CPU: 3.177s
        CGroup: /system.slice/rabbitmq-server.service
                ├─5631 /usr/lib/erlang/erts-12.2.1/bin/beam.smp -W w -MBas ageffcbf -MHas ageffcbf -MBlmbcs 512 -MHlmbcs 512 -MMmcs 30 -P 1048576 -t 5000000 -stbt db -zdbbl 128000 -sbwt none -sbwtdcpu>
                ├─5642 erl_child_setup 32768
                ├─5672 /usr/lib/erlang/erts-12.2.1/bin/epmd -daemon
                ├─5699 inet_gethost 4
                └─5700 inet_gethost 4

    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Doc guides:  https://rabbitmq.com/documentation.html
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Support:     https://rabbitmq.com/contact.html
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Tutorials:   https://rabbitmq.com/getstarted.html
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Monitoring:  https://rabbitmq.com/monitoring.html
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Logs: /var/log/rabbitmq/rabbit@ubuntu22.log
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:         /var/log/rabbitmq/rabbit@ubuntu22_upgrade.log
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:         <stdout>
    Mei 12 23:57:21 ubuntu22 rabbitmq-server[5631]:   Config file(s): (none)
    Mei 12 23:57:22 ubuntu22 rabbitmq-server[5631]:   Starting broker... completed with 0 plugins.
    Mei 12 23:57:22 ubuntu22 systemd[1]: Started RabbitMQ broker.

Aşağıdaki komutu kullanarak servisin sistem başlangıcında çalışmak için konfigüre edildiğini doğrulayın:

    $> systemctl is-enabled rabbitmq-server.service 
    enabled

<h2>Adım 2.3: RabbitMQ Yönetim Ara Yüzünü Aktif Etme</h2>

İsteğe bağlı olarak, kolay yönetim için RabbitMQ web ara yüzünü de aktif edebilirsiniz:

    $> sudo rabbitmq-plugins enable rabbitmq_management
    Enabling plugins on node rabbit@ubuntu:
    rabbitmq_management
    The following plugins have been configured:
    rabbitmq_management
    rabbitmq_management_agent
    rabbitmq_web_dispatch
    Applying plugin configuration to rabbit@ubuntu...
    The following plugins have been enabled:
    rabbitmq_management
    rabbitmq_management_agent
    rabbitmq_web_dispatch

    started 3 plugins.

Eğerk aktif olarak UFW firewall ayarına sahipseniz 5672 ve 15672 portlarını aşağıdaki komutlar ile açın:

    $> sudo ufw allow 5672
    $> sudo ufw allow 15672

http://[server IP|Hostname]:15672 URL' ini açarak ara yüze erişin:


![login-page](https://github.com/nadidurna/RabbitMQClusterInstall/blob/master/images/rabbitmq-loginpage.png?raw=true)




Varsayılan olarak **guest** kullanıcısı mevcuttur ve sadece localhost üzerinden erişilebilir. Local olarak kullanıcı adı ve şifre **guest** yazarak giriş yapabilirsiniz.

Ağ üzerinde giriş yapabilmek için aşağıdaki komutlar ile **admin** kullanıcı oluşturabilirsiniz:

    $> sudo rabbitmqctl add_user admin StrongPassword
    $> sudo rabbitmqctl set_user_tags admin administrator

**admin** kullanıcı adı ve atanan parola ile giriş yapabilirsiniz.

<h1>Adım 3: RabbitMQ Cluster Kurulumu</h1>

**Kurulum Gereksinimleri**
<p>-Ubuntu 20.04 server</p>
<p>-En az 2 farklı server üzerinde çalışan RabbitMQ Servisi</p>
<p>-sudo ayrıcalıklarına sahip bir kullanıcı</p>
<p>-Serverların internet erişimi</p>


Bu örnekte RabbitMQ Cluster kurulumu için 3 adet Ubuntu 20.04 olarak çalışan server kullanılmıştır ve ağ bilgileri aşağıdaki tablodaki gibidir:

| Server       | Hostname    | IP Adresleri   |
| ------------ | ----------- | -------------- |
| rabbitmq-1   | mq1.com     | 4.231.106.167  |
| rabbitmq-2   | mq2.com     | 74.234.187.167 |
| rabbitmq-3   | mq2.com     | 74.234.240.177 |


<h2>Adım 3.1: Hostname and DNS Ayarları</h2>

The first Adım in the installation of the RabbitMQ cluster on Ubuntu 20.04 is to configure correct hostnames and DNS, you can add the records to the **/etc/hosts** file.
Rabbitmq Cluster kurulumu için gerekli olan ilk adım sunucular üzerinde DNS ve hostname ayarlarını doğru olarak konfigüre etmektir. Tablodaki bilgiler doğrultusunda kayıtları **/etc/hosts** dosyasına ekleyebilirsiniz.

Server, hostname ve ip adresi bilgilerini aşağıdaki şekilde tüm sunucularda ekleyin:

    sudo echo "4.231.106.167 mq1.com rabbitmq-1" >> /etc/hosts
    sudo echo "74.234.187.167 mq2.com rabbitmq-2" >> /etc/hosts
    sudo echo "74.234.240.177 mq3.com rabbitmq-3" >> /etc/hosts

Sonrasında sisteminizi güncelleyin:

    sudo apt update
    sudo apt -y upgrade

<h2>Adım 3.2: Sunuculara Erlang ve RabbitMQ Yükleme</h2>

Birinci ve ikinci adımlarda anlatılan Erlang ve RabbitMQ kurulumlarınızı tüm sunucularınızda uygulayın.

<h2>Adım 3.3: RabbitMQ Server-1 Cookie Bilgisini Server-2 ve Server-3' e Kopyalama</h2>

RabbitMQ cluster çalışması için cluster' a katılan tüm node'ların aynı Cookie' ye sahip olması gerekmektedir. Bu nedenle ilk serverdaki Cookie bilgisini diğer serverlara kopyalayın.

Bunun için rabbitmq-1 sunucunda aşağıdaki komutları çalıştırın:

    $> sudo scp /var/lib/rabbitmq/.erlang.cookie rabbitmq-2:/var/lib/rabbitmq/.erlang.cookie
    $> sudo scp /var/lib/rabbitmq/.erlang.cookie rabbitmq-3:/var/lib/rabbitmq/.erlang.cookie

<h2> Adım 3.4: Node-2 Üzerinde RabbitMQ Servisini Yeniden Ayarlama</h2>

Node-2 üzerindeki RabbitMQ servisini yeniden yapılandırın ve bu node'u cluster'a dahil edin.

RabbitMQ servisini yeniden başlatın:

    sudo systemctl restart rabbitmq-server

Uygulamayı Durdurun:

    $ sudo rabbitmqctl stop_app
    Stopping rabbit application on node rabbit@mq2 ...

Rabbitmq uygulamasını yeniden başlatın:

    $ sudo rabbitmqctl reset
    Resetting node rabbit@mq2 ...

Node' u cluster' a dahil edin:

    $ sudo rabbitmqctl join_cluster rabbit@rabbitmq-1
    Clustering node rabbit@mq2 with rabbit@rabbitmq-1

Start the application process

    $ sudo rabbitmqctl start_app
    Starting node rabbit@mq2 ...  completed with 0 plugins.

Cluster Statüsünü Kontrol Edin:

    root@rabbitmq-1:~# rabbitmqctl cluster_status

<h2>Adım 3.5: RabbitMQ HA İlkesi Yapılandırma</h2>

Cluster'daki tüm node'lara yansıtılan kuyruklara izin veren bir ilke oluşturun:


    $ sudo rabbitmqctl set_policy ha-all "." '{"ha-mode":"all"}'

    Setting policy "ha-all" for pattern "." to "{"ha-mode":"all"}" with priority "0" for vhost "/" …

Aşağıdaki komut ile yapılandılan ilkeleri listeleyebilirsiniz:

    root@mq1:~# sudo rabbitmqctl list_policies
    Listing policies for vhost "/" …
    vhost    name    pattern apply-to    definition  priority
    /    ha-all  .*  all {"ha-mode":"all"}   0
Bir ilkeyi kullanmayı bırakmak için aşağıdaki komutu kullanabilirsini:


    sudo rabbitmqctl clear_policy <policyname>

<h2>Adım 3.6: RabbitMQ Cluster Kontrolü</h2>

Aşağıdaki resimde de görüldüğü üzere gerekli yapılandırmalar sonrasında cluster'ımızın 3 node ile çalışıyor durumda olduğunu shell üzerinden kontrol ettiğimiz gibi ara yüz üzerinden de kontrol edebiliriz.

![main-page](https://github.com/nadidurna/RabbitMQClusterInstall/blob/master/images/cluster.jpg?raw=true)



**RabbitMQ Cluster Kurulumu**