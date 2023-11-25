# Sistema Distribuído: Arquitetura e Implementação

## Arquitetura do Sistema

O sistema adota uma arquitetura **cliente-servidor**. Os clientes, como navegadores web ou aplicações que consomem a API, interagem com o servidor, que é uma aplicação PHP executada em containers Docker. Esta arquitetura é marcada pela comunicação direta entre os clientes e o servidor, com o servidor processando e respondendo às solicitações dos clientes.

## Marshaling e Unmarshaling de Dados

O framework **Laravel** é utilizado para o marshaling e unmarshaling de dados. Ao receber requisições HTTP:

- **Unmarshaling**: O Laravel converte automaticamente dados JSON em arrays PHP ou objetos de requisição.
- **Marshaling**: Ao enviar respostas, o Laravel transforma arrays ou objetos PHP de volta para JSON, cuidando dos cabeçalhos de resposta HTTP apropriados.

## Paradigma de Comunicação

A comunicação se baseia na **invocação remota**, operando através do protocolo requisição-resposta HTTP. A API, desenvolvida com Laravel, atende a solicitações HTTP, processando-as e retornando dados em formato JSON.

## Cache Distribuído com Redis

Um aspecto importante é o uso de `Cache::remember` do Laravel integrado ao **Redis**. O Redis funciona como um sistema de cache distribuído, armazenando índices que são acessíveis e compartilhados entre os dois nós da API (`apibcb1` e `apibcb2`). Isso permite:

- Compartilhamento eficiente de informações de cache entre os nós.
- Gerenciamento de acessos concorrentes e condições de corrida.
- Melhoria de desempenho ao reduzir leituras e escritas repetitivas no banco de dados.

## Conclusão

A estrutura da aplicação, incluindo o uso eficiente do Laravel para o processamento de dados e do Redis para cache distribuído, é bem adaptada às necessidades de um sistema distribuído. Esta abordagem facilita a eficiência operacional, reforça a consistência dos dados e melhora o desempenho em um ambiente distribuído.



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
http://api.bcb.local

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

## 2) Acessos dos dois nodes ao mesmos Indices do Redis:


