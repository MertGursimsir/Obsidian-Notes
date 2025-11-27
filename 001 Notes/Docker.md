docker run --rm --name benim_konteynerim ubuntu
    verilen isimde yeni bir container çalıştırır
    --rm --> container kapandığında çöpe at, iz bırakmasın

docker exec -it benim_konteynerim bash
    çalışan containerda komut çalıştırır
    -i --> keeps STDIN open
    -t --> human readable

docker run --rm --name benim_konteynerim -it ubuntu bash
    Hem container başlatır hem bash içine sokar



Docker Compose = Birden fazla container’ı birlikte yönetme
Bir docker-compose.yml dosyası yaparsın
İçine dersin ki:
    "Bir tane Postgres çalıştır"
    "Bir tane Backend ayağa kaldır"
    "Bir de Frontend ver"
Sonra bir komutla hepsine seda edersin:
    docker-compose up -d
        -d --> detached mod, ekranı meşgul etmeden arka planda çalıştırır

docker-compose down
docker-compose logs -f
docker compose ps
docker-compose restart

Container internetini kapatıp açmak:
    docker network disconnect bridge <CONTAINER_ID>
    docker network connect bridge <CONTAINER_ID>


docker tag eski_imaj_adi:eski_tag yeni_imaj_adi:yeni_tag
docker rmi eski_imaj_adi:eski_tag

docker load < FILE.tar
docker save busybox > busybox.tar


JENKINS
-------
agent {dockerfile true}
    Bunu dersen dockerfile'dan imaj üretilip çalıştırılır.
    Dockerfile nerede? ---> in repository

her stage farklı agent'ta çalışabilir:
    pipeline {
        agent none
        stages {
            stage ('First stage') {
                agent {
                    docker { image 'ubuntu:22.04'}
                }
                steps {
                    sh 'gcc --version'
                }
            }
            stage ('Second stage') {
                agent {
                    docker { image 'ubuntu:24.04'}
                }
                steps {
                    sh 'gcc --version'
                }
            }
        }
    }