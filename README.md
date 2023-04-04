# Curso sobre integração contínua

## Baixando ferramentas
- Go pelo linux
```bash
    sudo apt install golang-go
```
- Docker pelo linux
```bash
    sudo apt-get update

    sudo apt-get install ca-certificates curl gnupg lsb-release

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    echo "deb [arch=$(dpkg --print-architecture)        signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update

    sudo apt-get install docker-ce docker-ce-cli containerd.io

    sudo apt  install docker-compose
```
---
## Fazendo a aplicação funcionar
- 1 - Suba o banco com 
```bash
    sudo docker-compose up
```
- 2 - Em outro terminar inicie o go
```bash
    go run main.go
```
- 3 - Para fazer os testes funcionarem
```bash
    go run main_test.go
```
- 4 - Caso queira saber os teste que foram passados
```bash
    go run -v main_test.go
```
---
## Configurando os actions no github

- Criando a rotina de teste
```yaml
test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        go_version: ['1.19','1.18','1.17','>=1.19']
    steps:
    - uses: actions/checkout@v3

    - name: Set up Go
      uses: actions/setup-go@v3
      with:
        go-version: ${{ matrix.go_version }}

    - name: Build-DB
      run: docker-compose build
      
    - name: Create-DB
      run: docker-compose up -d
    
    - name: Test
      run: go test -v main_test.go

``` 
- Criando a rotina de build
```yaml
build:
    needs: test
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Build
      run: go build -v main.go
    
``` 
## Criando chaves secretas do repositório

- 1 - Vá em settigns
- 2 - Ir em environments
- 3 - Ir em New environment


---
## Para saber mais
- Os principais teste e porque utiliza-los: <a href="https://www.alura.com.br/artigos/tipos-de-testes-principais-por-que-utiliza-los">Testes</a>