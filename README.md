# Escolha um diretório
cd Documentos
mkdir pos

# Clone os projetos
git clone git@github.com:parisprobr/pedraMouraSis.git
git clone git@github.com:parisprobr/apibcb.git

# Subas os containers
docker-compose up -d

# Acesse as aplicações
http://api.bcb.local

# Funcionamento
1) API com dois nós: 
docker logs -f pedraMoura-nginx-proxy

Acesso1: http://api.bcb.local (node1: IP:172.22.0.5)
```
nginx.1     | api.apibcb.local 172.22.0.1 - - [25/Nov/2023:08:46:39 +0000] "GET /favicon.ico HTTP/1.1" 200 0 "http://api.apibcb.local/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36" "172.22.0.5:80"
```

Acesso2: http://api.bcb.local (node2: IP:172.22.0.56)
```
nginx.1     | api.apibcb.local 172.22.0.1 - - [25/Nov/2023:08:46:51 +0000] "GET / HTTP/1.1" 200 7124 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36" "172.22.0.6:80"
```

2) Acessos dos dois nodes ao mesmos Indices do Redis:


