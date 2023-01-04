# REDİS-NGİNX-DOCKER

**Redis, verileri anahtar-değer çiftleri biçiminde depolayan ünlü, ücretsiz ve açık kaynaklı bir bellek içi veritabanıdır. Yerleşik komutları ve CLI arabirimi sayesinde Redis'in kullanımı ve yönetimi kolaydır.**

- Veri güvenliği ve veri tutarlılığı için bir kontrol mekanizması
  yoktur.

- Redis, diğer mesaj brokerlerinden biraz farklıdır. Redis, özünde,  
  yüksek performanslı bir anahtar/değer deposu veya bir ileti aracısı  
  olarak kullanılabilen bir bellek içi veri deposudur

- Redis'in persistent olmaması, bunun yerine belleğini bir Disk/DB'ye  
  boşaltmasıdır.

- Aynı zamanda gerçek zamanlı veri işleme için mükemmeldir.

- Redis'in bellek içi veritabanı, kalıcılığın gerekli olmadığı kısa  
  ömürlü mesajların olduğu kullanım durumları için neredeyse mükemmel  
  bir uyum sağlar.
- Son derece hızlı hizmet ve bellek içi yetenekler sağladığı için  
  Redis, kalıcılığın çok önemli olmadığı ve bir miktar kaybı tolere  
  edebileceğiniz kısa saklama mesajları için mükemmel bir adaydır.

Projemizde nginx de kullanılmıştır.Kullanım amacı ;

Web sunucusu YÜK DENGELEME(load balancing) -STATİK DOSYALARI KULLANICILARA SERVİS ETME - CACHE LEME işlemlerini gerçekleştirir.

Nginx => web sunucuları gelen istekleri işleyip cevap üretemez - cevap üretecek sisteme gönderir.

Proxy=> Bağlantı, ara sunucunun(nginx) IP adresi üzerinden yapılarak, sizin IP adresiniz bağlantı için kullanılmaz. Güvenli ve hızlı bağlantı isteyenler için oldukça avantajlıdır

**_Docker-compose işlemleri_**

    $docker rm node-app -f
    $docker build -t node-app-image .
    $docker run -v $(pwd):/app -p 3000:3000 -d --name node-app node-app-image || $docker run -v $(pwd):/app -v /app/node_modules -p 3000:3000 -d --name node-app node-app-image
    || $docker run -v $(pwd):/app -v /app/node_modules --env PORT=4000 -p 3000:4000 -d --name node-app node-app-image

    $docker exec -it node-app bash
    $printenv

    $docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d
    $docker-compose -f docker-compose.yml -f docker-compose.dev.yml down -v

**_Mongo kurmak için_**
**mongodb yükleyip docker içine koymak istediğimde**

    $wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | apt-key add -

    $echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-6.0.list
    $deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse
    $apt-get update
    $apt-get install -y mongodb-mongosh
    $docker exec -it 228f32195ee5 bash
    $mongosh "mongodb://Dilan:dilan123@localhost:27017/admin?serverSelectionTimeoutMS=5000&connectTimeoutMS=10000"
    $docker-compose up

**_Mongo ya clı üzerinden işlem yapabilmek için_**

    $docker exec -it redis_mongo_docker_mongo_1 bash || docker exec -it redis_mongo_docker_mongo_1 mongosh -u "Dilan" -p "dilan123"
    $mongosh
    $use admin
    $db.auth("Dilan",passwordPrompt())
    $show dbs

_mongoose u indirmek istediğimde Error: EACCES: permission denied hatası alırsanız.Sebebi kullanıcı yetkisinin root da olması bunu düzeltmek için_

    $ npm install -g --unsafe-perm=true --allow-root
    $ sudo chown -R dilan /home/dilan/
    $ sudo chmod -R 777 /home/dilan/Desktop/redis_mongo_docker/node_modules
    $ npm install mongoose
    $docker logs redis_mongo_docker_mongo_1 -f => mongoose kurduktan sonra connect olmak için kullanıyoruz.

**_Ping atmak istediğimizde_**

    $docker exec -it redis_mongo_docker_node-app_1 bash
    $ping mongo

**_Yeni bir paket etkiledikten sonra_**

    $docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build -V

**_Redis i kurmak için_**

    $ docker run -d --name redis-stack-server -p 6379:6379 redis/redis-stack-server:latest
    $ docker run -d --name redis-stack -p 6379:6379 -p 8001:8001 redis/redis-stack:latest
    $ docker exec -it redis-stack redis-cli
      $keys *
      $Get <keys>

Docker compose dosyalarımızın içerisine redis bilgilerini eklemeyi unutmuyoruz.!
Nodejs projemiz içerisinde aktif edebilmek için
$`npm install redis connect-redis express-session` ekliyoruz.

Nginx için compose yaml içerisine eklememizi yaptıktan sonra build ediyoruz entegre oluyor.

_Proxy ile kullanıcının mevcut ıp adresi yerine proxy proxy nin ıp adresini kullanıyoruz._

_Yeni bir container açıp_

    $docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d --scale node-app=2
    $docker logs redis_mongo_docker_node-app_2 -f

postman den de istek atarak işlev kazandığını görüyoruz.

Nginx kullanarak proxy üzerinden elde ettiğim ıp adresi ile artık web sitesinde yer alıyorum.Bu şekilde güvenli bir dolaşım sağlıyorum. Web sitem içerisinde başka web sitelerine erişim sağlayabilmek adına cors kullanıyoruz.
Web tarayıcısı tarafından yönetilen ve ek HTTP başlıkları kullanılarak, bir kökende çalışan web uygulamasının, farklı bir kökende yer alan web uygulamasına erişim izni kontrolünü sağlayan mekanizmadır.

_Port ile alakalı hata aldığımızda mongosh daki hata_

    sudo kill $sudo kill -9  $(sudo lsof -t -i:7101)^Csof -ti:2181)
    sudo kill -9 $(sudo lsof -t -i:3000)
    netstat -tulpn
