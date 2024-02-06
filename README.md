# Desafio "Trabalhando com Machine Learning na Prática no Azule ML"

##### Como parte da resolução do desafio proposto pela DIO, estou descrevendo as etapas de como realizei a criação dos pontos de extremidades que usei no laboratório de Machine Learning.

##### Os passos seguidos foram:

###### 1. Foi criada uma conta no Portal do Azure (https://portal.azure.com/).
###### 2. Após a conta criada, foi realizada a criação do Resource Group.
###### 3. Com o Resource Group criado, foi criado uma Workspace no Azure Machine Learning.
###### 4. Após Workspace criada, foi criado um ML automatizado, com as seguintes configurações:

    Basic settings:
        Job name: mslearn-bike-automl
        New experiment name: mslearn-bike-rental
            Description: Automated machine learning for bike rental prediction
            Tags: none
        Task type & data:
            Select task type: Regression
            Select dataset: Create a new dataset with the following settings:
                Data type:
                    Name: bike-rentals
                    Description: Historic bike rental data
                    Type: Tabular
                Data source:
                    Select From web files
                    Web URL: https://aka.ms/bike-rentals
                    Skip data validation: do not select
                Settings:
                    File format: Delimited
                    Delimiter: Comma
                    Encoding: UTF-8
                    Column headers: Only first file has headers
                    Skip rows: None
                    Dataset contains multi-line data: do not select
                Schema:
                    Include all columns other than Path
                    Review the automatically detected types

###### 5. Após a criação do job "bike-rentals", foi realizada a sua configuração com os seguintes parâmetros:
        Task type: Regression
        Dataset: bike-rentals
        Target column: Rentals (integer)
        Additional configuration settings:
            Primary metric: Normalized root mean squared error
            Explain best model: Unselected
            Use all supported models: Unselected. 
            Allowed models: Select only RandomForest and LightGBM
        Limits:
            Max trials: 3
            Max concurrent trials: 3
            Max nodes: 3
            Metric score threshold: 0.085
            Timeout: 15
            Iteration timeout: 15
            Enable early termination: Selected
        Validation and test:
            Validation type: Train-validation split
            Percentage of validation data: 10
            Test dataset: None
##### 6. Na parte de Computação, foi escolhida a opção padrão:
        Compute:
            Select compute type: Serverless
            Virtual machine type: CPU
            Virtual machine tier: Dedicated
            Virtual machine size: Standard_DS3_V2*
            Number of instances: 1
##### 7. Após as configurações, foi realizado o treinamento da máquina.
##### 8. Máquina treinada, foi realizado o deploy, utilizando a opção "Web Service" e as seguintes configurações:
            Name: predict-rentals
            Description: Predict cycle rentals
            Compute type: Azure Container Instance
            Enable authentication: Selected
##### 9. Com as configurações prontas, foi realizado o teste com os seguintes parâmetros, conforme o arquivo "testeMlBike.json" anexado ao repositório:
        {
      "Inputs": {
        "data": [
          {
            "day": 20,
            "mnth": 10,
            "year": 2019,
            "season": 3,
            "holiday": 3,
            "weekday": 5,
            "workingday": 1,
            "weathersit": 2,
            "temp": 0.3,
            "atemp": 0.5,
            "hum": 0.4,
            "windspeed": 1.0
          }
        ]
      },
      "GlobalParameters": 2.0
    }
##### 10. O resultado do Teste informado foi este:
    {1 item
        "Results":[1 item
        0:float513.5999062541032
        ]
    }
##### 11. Após o término dos testes, foi realizada a remoção da Workspace e do Resource Group, para evitar gastos excedentes.
