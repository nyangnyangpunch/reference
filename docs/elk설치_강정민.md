Elasticsearch download
==========================
! 다운로드에 앞서 다음 패키지들(명령어)이 필요할 수 있음.


1.apt-transport-https


2.dpkg


3.wget


<공개 서명키 다운로드>


wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -


<저장소 설정??>


생략 가능하지만 출력해보는지 모르겠음.->echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list


! 다운로드 방식이 2가지 방식이 존재한다.(선택해서 하면됨)


<elasticsearch 다운로드>


(1)


sudo apt-get update && sudo apt-get install elasticsearch


(2)


wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.2.0-amd64.deb


wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.2.0-amd64.deb.sha512


shasum -a 512 -c elasticsearch-7.2.0-amd64.deb.sha512 


sudo dpkg -i elasticsearch-7.2.0-amd64.deb


! elasticsearch, logstash, kibana 유사하다.


링크 참조-> https://www.elastic.co/guide/en/elasticsearch/reference/7.2/deb.html#deb
