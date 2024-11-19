
# Knic Installation
### 1. install minconda on your computer (for mac M1/M2)
  ```bash
  curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
  bash Miniconda3-latest-MacOSX-arm64.sh
  conda init
  ```
### 2. install knic engine (basically follow the knic engine readme file : https://github.com/usc-isi-i2/knic-engine)
#### Create knic-engine virtual environment:
    ```bash
    conda create -n knic-engine python=3.10
    conda activate knic-engine
    ```
#### Docker:
    ```bash
    docker build --tag ghcr.io/usc-isi-i2/knic-engine .
    docker compose up --detach
    docker compose down
    ```
#### Running RabbitMQ Docker
    ```bash
    docker pull rabbitmq:3.12.13-management-alpine
    docker run --detach --rm --name knic-rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12.13-management-alpine

### 3.install knic companion
#### 3.1 install api  
##### create a conda environment:
    ```bash
    conda create --name knic-companion python=3.9.18
    conda activate knic-companion
    ```
##### install project requirements: 
    ```bash
    pip install -r requirements.txt -c constraints.txt
    conda install -c anaconda mongodb
    ```
##### start mongodb
    ```bash
    mkdir -p mongo_dbpath
    mongod --dbpath mongo_dbpath
    ```
#### 3.2 install app 
    ``` bash
    npm install
    ```

### 4.install knic jupyter
    ```bash
    git clone git@github.com:usc-isi-i2/knic-jupyter.git
    cd knic-jupyter
    conda create --name knic-jupyter python=3.8 -y
    conda activate knic-jupyter
    ```

### 5. finally run 5 command lines at the same time (should be run in 5 different terminals)
#### 1. RUN mongo db:
      ```bash
      mongod --dbpath mongo_dbpath
      ```
#### 2. RUN the companion api server:
      ```bash
      python server.py
      ```
#### 3. RUN knic jupyter
      ```bash
      ../knic-engine/knic-jupyter/run-jupyter-lab.sh
      ```
#### 4. RUN knic_engine:
      ```bash
      python knic_application.py
      ```
#### 5. RUN the companion api server
      ```bash
      npm start
      ```