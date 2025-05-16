
# Knic Installation
### 1. install miniconda on your computer (for mac M1/M2)
  ```
  curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
  bash Miniconda3-latest-MacOSX-arm64.sh
  conda init
  ```
  
### 2. git clone all github repos on your computer
  ```
  git clone git@github.com:usc-isi-i2/knic-notebooks.git
  git clone git@github.com:usc-isi-i2/knic-jupyter.git
  git clone git@github.com:usc-isi-i2/knic-engine.git
  git clone git@github.com:usc-isi-i2/knic-companion.git
  ```
### 3. install knic engine (basically follow the knic engine readme file : https://github.com/usc-isi-i2/knic-engine)
#### Create knic-engine virtual environment:
    
    conda create -n knic-engine python=3.10
    conda activate knic-engine
    go to the file 'knic-engine/knic/engine_config.py', change experiment parameter from 'LINEAR_4' to 'ICT_5' (line 12). 
    In the file, reset TAD_LLM_API_KEY (line 64). 
    
#### Docker:
    
    docker build --tag ghcr.io/usc-isi-i2/knic-engine .
    Optional:
    docker compose up --detach
    docker compose down
    
#### Running RabbitMQ Docker
    
    docker pull rabbitmq:3.12.13-management-alpine
    docker run --detach --rm --name knic-rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12.13-management-alpine
    

### 4.install knic companion
#### 4.1 install api  
##### create a conda environment:
    
    conda create --name knic-companion python=3.9.18
    conda activate knic-companion
    
##### install project requirements: 
    
    pip install -r requirements.txt -c constraints.txt
    conda install -c anaconda mongodb
    
##### start mongodb
    
    mkdir -p mongo_dbpath
    mongod --dbpath mongo_dbpath
    
#### 4.2 install app 
    
    npm install
    

### 5.install knic jupyter
    
    git clone git@github.com:usc-isi-i2/knic-jupyter.git
    cd knic-jupyter
    conda create --name knic-jupyter python=3.8 -y
    conda activate knic-jupyter
    pip install -ve .
    export KNIC_COMPANION=http://localhost:3000
    

### 6. finally run 5 command lines at the same time (should be run in 5 different terminals)
###### Before running each command, making sure to activate corresponding conda environment. The command is: conda activate your-env-name
#### 1. RUN mongo db:
      
      cd knic-companion/api/
      conda activate knic-companion
      mongod --dbpath mongo_dbpath
      
#### 2. RUN the companion api server:
      
      cd knic-companion/api/
      conda activate knic-companion
      unset HOST
      export KNIC_COMPANION=http://localhost:3000
      python server.py
      
#### 3. RUN knic jupyter
      
      cd knic-jupyter
      conda activate knic-jupyter
      unset HOST
      export KNIC_COMPANION=http://localhost:3000
      sh run-jupyter-lab.sh
      
#### 4. Running the RabbitMQ Docker
      cd knic-engine
      docker run --detach --rm --name knic-rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12.13-management-alpine
###### Note: Can continue running the step 5 in this terminal   

#### 5. RUN knic_engine:
      
      conda activate knic-engine
      export KNIC_COMPANION=http://localhost:3000
      python knic_application.py
      
#### 6. RUN the companion app server
      
      cd knic-companion/app/
      conda activate knic-companion
      export KNIC_COMPANION=http://localhost:3000
      npm start
      

### other comments:
go to the file 'knic-engine/knic/engine_config.py', in line 6, change experiment parameter from 'LINEAR_4' to 'ICT_5', making sure we use the same experiment. 
