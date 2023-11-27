# Sistema Distribuído: Arquitetura e Implementação

## Arquitetura do Sistema

Este sistema adota uma **arquitetura cliente-servidor**, composta por várias aplicações interconectadas e um balanceamento de carga eficiente.

### Componentes do Sistema

1. **API Laravel (`apibcb1` e `apibcb2`)**: Dois nós de uma aplicação Laravel, atendendo às requisições e processando os dados. Ambos compartilham um sistema de cache Redis centralizado.

2. **Sites (`site1` e `site2`)**: Dois nós de um servidor web Apache, configurados com o mesmo `VIRTUAL_HOST`, facilitando o balanceamento de carga entre eles.

3. **Banco de Dados MySQL e phpMyAdmin**: Um servidor MySQL para armazenamento de dados e phpMyAdmin para administração do banco de dados.

## Marshaling e Unmarshaling de Dados

O **Laravel** gerencia o marshaling e unmarshaling de dados, convertendo automaticamente requisições HTTP JSON em arrays ou objetos PHP, e vice-versa para as respostas.

## Paradigma de Comunicação

Utiliza-se a **invocação remota** via protocolo requisição-resposta HTTP. A API Laravel processa solicitações e retorna dados em formato JSON, típico em APIs RESTful.

## Cache Distribuído com Redis

A aplicação utiliza `Cache::remember` do Laravel integrado a um sistema de cache **Redis centralizado**, compartilhado entre os nós `apibcb1` e `apibcb2`, garantindo eficiência e consistência no acesso aos dados.

## Balanceamento de Carga com Nginx-Proxy

O balanceamento de carga é gerenciado pelo **nginx-proxy**, distribuindo o tráfego de forma eficaz entre os nós da API e os servidores web Apache, otimizando a disponibilidade e a resiliência.

## Banco de Dados e Administração

O **MySQL** é utilizado para o armazenamento de dados, enquanto o **phpMyAdmin** proporciona uma interface web para a administração do banco de dados.

## Conclusão

Esta configuração do sistema distribuído, incluindo Laravel, Redis, Apache, MySQL, phpMyAdmin e nginx-proxy, cria uma arquitetura robusta e bem adaptada às necessidades de um sistema distribuído. A estrutura promove eficiência operacional, consistência de dados, resiliência e performance em um ambiente distribuído.

# AMBIENTE LOCAL

# Mapeie em seu host local:
127.0.0.1 site.pedramoura.local api.apibcb.local 


# Escolha um diretório
cd Documentos;<BR>
mkdir pos

# Clone os projetos
git clone git@github.com:parisprobr/pedraMouraSis.git <BR>
git clone git@github.com:parisprobr/apibcb.git

# Subas os containers
docker-compose up -d

# Acesse as aplicações
http://api.apibcb.local

# Funcionamento
## 1) API com dois nós: 
docker logs -f pedraMoura-nginx-proxy

Acesso1: http://api.bcb.local (node1: IP:172.22.0.5)
```
nginx.1     | api.apibcb.local 172.22.0.1 - - [25/Nov/2023:08:46:39 +0000] "GET /favicon.ico HTTP/1.1" 200 0 "http://api.apibcb.local/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36" "172.22.0.5:80"
```

Acesso2: http://api.bcb.local (node2: IP:172.22.0.56)
```
nginx.1     | api.apibcb.local 172.22.0.1 - - [25/Nov/2023:08:46:51 +0000] "GET / HTTP/1.1" 200 7124 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36" "172.22.0.6:80"
```


