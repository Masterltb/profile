---
title: "Blog 2"
date: "2025-09-30"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Modernize fraud prevention: GraphStorm v0.5 for real-time inference

Fraud continues to inflict significant financial damage globally, costing US consumers alone $12.5 billion in 2024—a 25% increase from the previous year according to the Federal Trade Commission. This surge stems not from more frequent attacks, but from fraudsters’ growing sophistication. As fraudulent activities become more complex and interconnected, conventional machine learning approaches fall short because they analyze transactions in isolation, failing to capture the coordinated networks that characterize modern fraud rings.

Graph neural networks (GNNs) effectively address this challenge by modeling relationships between entities—such as users sharing devices, locations, or payment methods. By analyzing both network structure and entity features, GNNs excel at identifying sophisticated fraud rings where perpetrators mask individual suspicious activities but leave traces in their relationship networks. However, deploying GNN-based online fraud prevention in production environments presents unique challenges: achieving sub-second inference latency, scaling to billions of nodes and edges, and maintaining operational efficiency for model updates. In this post, we show you how to overcome these challenges using GraphStorm, specifically the new real-time inference capabilities of GraphStorm v0.5.

Previous solutions required tradeoffs between capability and simplicity. Our initial DGL approach offered comprehensive real-time capabilities but required complex service orchestration—involving manual updates of endpoint configurations and payload formats after retraining with new hyperparameters. This approach also lacked model flexibility, requiring custom GNN modeling and configuration when using architectures beyond relational graph convolutional networks (RGCN). Subsequent in-memory DGL implementations reduced complexity but faced scalability limitations with enterprise-grade data volumes. We built GraphStorm to bridge this gap, introducing distributed training and high-level APIs that simplify GNN development at enterprise scale.

In a recent blog post, we illustrated GraphStorm’s capability and simplicity in enterprise-scale GNN model training and offline inference. Although offline GNN fraud detection can identify fraudulent transactions after they occur—preventing financial loss requires intercepting fraud before it happens. GraphStorm v0.5 now makes this possible through native real-time inference support via Amazon SageMaker AI. GraphStorm v0.5 brings two improvements: streamlined endpoint deployment that reduces weeks of custom engineering—programming SageMaker entry point files, packaging model artifacts, and calling SageMaker deployment APIs—to a single command operation, and a standardized payload specification that simplifies client integration with real-time inference services. These capabilities enable sub-second node classification tasks like fraud prevention, helping organizations proactively combat fraud threats with scalable, operationally simple GNN solutions.

To showcase these capabilities, this post presents a fraud prevention solution. Through this solution, we show how a data scientist can transition a trained GNN model to production-ready inference endpoints with minimal operational overhead. If you are interested in deploying GNN-based models for real-time fraud prevention or similar business cases, you can adapt the methods presented here to create your own solution.

## Solution overview

Our proposed solution is a four-step pipeline as shown in the following figure. The pipeline starts in step 1 with exporting a transaction graph from an online transaction processing (OLTP) graph database to scalable storage (Amazon Simple Storage Service (Amazon S3) or Amazon EFS), followed by distributed model training in step 2. Step 3 is GraphStorm v0.5’s simplified deployment process, which creates SageMaker real-time inference endpoints with a single command. Once SageMaker AI has successfully deployed the endpoint, a client application integrates with the OLTP graph database to process live transaction streams in step 4. By querying the graph database, the client prepares subgraphs around the query transactions, converts the subgraph to a standardized payload format, and invokes the deployed endpoint for real-time prediction.

To provide concrete implementation details for each step in the real-time inference solution, we illustrate the complete workflow using the publicly available IEEE-CIS fraud detection task.

Note: This example uses a Jupyter notebook as the controller for the entire four-step pipeline for simplicity. For a more production-ready design, refer to the architecture described in Build a GNN-based real-time fraud detection solution.

## Prerequisites

To run this example, you need an [AWS account](https://signin.aws.amazon.com/signup?request_type=register) where the example’s [AWS Cloud Development Kit (AWS CDK)](https://aws.amazon.com/cdk/) code creates the required resources, including an [Amazon Virtual Private Cloud (Amazon VPC)](https://aws.amazon.com/vpc/), [Amazon Neptune](https://aws.amazon.com/neptune/) database, Amazon SageMaker AI, [Amazon Elastic Container Registry (Amazon ECR)](https://aws.amazon.com/ecr/), Amazon S3, and relevant roles and permissions.

Note: These resources incur costs during execution (approximately $6 per hour with default settings). Monitor usage carefully and review pricing pages for these services before proceeding. Follow the cleanup instructions at the end to avoid ongoing charges.

## Walkthrough: Real-time fraud prevention with the IEEE-CIS dataset

The complete implementation code for this example, including Jupyter notebooks and supporting Python scripts, is available in our public repository. This repository provides a complete end-to-end implementation that you can directly execute and adapt for your own fraud prevention use cases.

### Dataset and task overview

The example uses the IEEE-CIS fraud detection dataset, containing 500,000 anonymized transactions with approximately 3.5% fraud cases. The dataset includes 392 categorical and numerical features, with key attributes like card type, product type, address, and email domains forming the graph structure shown in the following figure. Each transaction (with an isFraud label) connects to Card type, Location, Product type, and Purchaser and Recipient email domain entities, creating a heterogeneous graph that enables GNN models to detect fraud patterns through entity relationships.

Unlike our previous post that demonstrated GraphStorm with [Amazon Neptune Analytics](https://aws.amazon.com/neptune/) for offline analytics workflows, this example uses a Neptune database as an OLTP graph store, optimized for the fast subgraph extraction required during real-time inference. Based on the graph design, the tabular IEEE-CIS data is transformed into a set of CSV files compatible with Neptune database format, allowing direct loading into both the Neptune database and GraphStorm’s model training pipeline with a single file set.

### Step 0: Environment setup

Step 0 sets up the required runtime environment for the four-step fraud prevention pipeline. Complete setup instructions are available in the deployment repository.

To run the example solution, you need to deploy an [AWS CloudFormation stack](https://aws.amazon.com/cloudformation/) via the AWS CDK. The stack creates the Neptune DB instance, the VPC to put it in, and appropriate roles and security groups. It additionally creates a SageMaker AI notebook instance, from which you run the example notebooks included with the repository.

```bash
git clone [https://github.com/aws-samples/amazon-neptune-samples.git](https://github.com/aws-samples/amazon-neptune-samples.git)

cd neptune-database-graphstorm-online-inference/neptune-db-cdk

# Ensure you have CDK installed and have appropriate credentials set up

cdk deploy
Once deployment is complete (taking approximately 10 minutes for the necessary resources to be ready), the AWS CDK prints several outputs, one of which is the name of the SageMaker notebook instance you use to run through the notebooks:

Bash

# Example output

NeptuneInfraStack.NotebookInstanceName = arn:aws:sagemaker:us-east-1:012345678912:notebook-instance/NeptuneNotebook-9KgSB9XXXXXX
You can navigate to the SageMaker AI notebook UI, find the corresponding notebook instance, and select its Open Jupyterlab link to access the notebook.

Alternatively, you can use the AWS Command Line Interface (AWS CLI) to get a pre-signed URL to access the notebook. You will need to replace <notebook-instance-name> with the actual notebook instance name.

Bash

aws sagemaker create-presigned-notebook-instance-url --notebook-instance-name <notebook-instance-name>
Once you are in the notebook web console, open the first notebook, 0-Data-Preparation.ipynb, to start walking through the example.

Step 1: Graph construction
In Notebook 0-Data-Preparation, you transform the tabular IEEE-CIS dataset into the heterogeneous graph structure shown in the figure at the beginning of this section. The provided Jupyter Notebook extracts entities from transaction features, creating Card Type nodes from card1–card6 attributes, Purchaser and Recipient nodes from email domains, Product Type nodes from product codes, and Location nodes from geographic information. This transformation establishes relationships between transactions and these entities, producing graph data in Neptune import format for direct ingestion into the OLTP graph store. The create_neptune_db_data() function orchestrates this entity extraction and relationship creation across all node types (taking approximately 30 seconds).

Python

GRAPH_NAME = "ieee-cis-fraud-detection"

PROCESSED_PREFIX = f"./{GRAPH_NAME}"

ID_COLS = "card1,card2,card3,card4,card5,card6,ProductCD,addr1,addr2,P_emaildomain,R_emaildomain"

CAT_COLS = "M1,M2,M3,M4,M5,M6,M7,M8,M9"

# Lists of columns to keep from each file

COLS_TO_KEEP = {

"transaction.csv": (

ID_COLS.split(",")

+ CAT_COLS.split(",")

+

# Numerical features without missing values

[f"C{idx}" for idx in range(1, 15)]

+ ["TransactionID", "TransactionAmt", "TransactionDT", "isFraud"]

),

"identity.csv": ["TransactionID", "DeviceType"],

}

create_neptune_db_data(

data_prefix="./input-data/",

output_prefix=PROCESSED_PREFIX,

id_cols=ID_COLS,

cat_cols=CAT_COLS,

cols_to_keep=COLS_TO_KEEP,

num_chunks=1,

)
The notebook also generates the JSON configuration file required for GraphStorm’s GConstruct command and executes the graph construction process. This GConstruct command converts the Neptune-formatted data into a distributed binary graph format optimized for GraphStorm’s training pipeline, partitioning the heterogeneous graph structure across compute nodes to enable scalable model training on industrial-scale graphs (measured in billions of nodes and edges). For the IEEE-CIS data, the GConstruct command takes 90 seconds to complete.

In Notebook 1-Load-Data-Into-Neptune-DB, you load the CSV data into the Neptune database instance (taking approximately 9 minutes), which makes them available for online inference. During online inference, upon picking a transaction node, you query the Neptune database to get the graph neighborhood of the target node, retrieving the features of every node in the neighborhood and the subgraph structure around the target.

Step 2: Model training
Now that you have transformed the data to distributed binary graph format, it is time to train a GNN model. GraphStorm provides command-line scripts for training a model without writing code. In Notebook 2-Model-Training, you train a GNN model using GraphStorm’s node classification command with configuration managed through YAML files. The basic configuration defines a two-layer RGCN model with 128-dimensional hidden layers, training for 4 epochs with a learning rate of 0.001 and batch size of 1024, taking approximately 100 seconds for 1 epoch of training and model evaluation on an ml.m5.4xlarge instance. To improve fraud detection accuracy, the notebook provides more advanced model configurations like the command below.

Bash

!python -m graphstorm.run.gs_node_classification \

--workspace ./ \

--part-config ieee_gs/ieee-cis.json \

--num-trainers 1 \

--cf ieee_nc.yaml \

--eval-metric roc_auc \

--save-model-path ./model-simple/ \

--topk-model-to-save 1 \

--imbalance-class-weights 0.1,1.0
The arguments in this command address the dataset’s label imbalance challenge, where only 3.5% of transactions are fraudulent, by using AUC-ROC as the evaluation metric and using class weights. This command also saves the best performing model along with essential configuration files required for endpoint deployment. Advanced configurations can further enhance model performance through techniques like HGT encoders, multi-head attention, and class-weighted cross entropy loss, though these optimizations increase computational requirements. GraphStorm enables these changes through run time arguments and YAML configurations, reducing the need for code modifications.

Step 3: Real-time endpoint deployment
In Notebook 3-GraphStorm-Endpoint-Deployment, you deploy the real-time endpoint via GraphStorm v0.5’s simplified launch script. The deployment requires three artifacts generated during training: the saved model file containing weights, the updated graph construction JSON with feature transformation metadata, and the runtime-updated training configuration YAML. These artifacts allow GraphStorm to reconstruct the exact training and model configurations for consistent inference behavior. Notably, the updated graph construction JSON and training configuration YAML files contain critical configurations essential for restoring the trained model on the endpoint and processing incoming request payloads. Using the updated JSON and YAML files for endpoint deployment is crucial.

GraphStorm uses SageMaker AI’s bring your own container (BYOC) feature to deploy a consistent inference environment. You need to build and push the GraphStorm real-time Docker image to Amazon ECR using the provided shell scripts. This containerized approach provides consistent runtime environments compatible with SageMaker AI managed infrastructure. The Docker image contains the necessary dependencies for GraphStorm’s real-time inference capabilities on the deployment environment.

To deploy the endpoint, you can use the launch_realtime_endpoint.py script provided by GraphStorm, which helps you gather the necessary artifacts and creates the required SageMaker AI resources to deploy an endpoint. The script accepts the Amazon ECR image URI, IAM role, model artifact paths, and S3 bucket configuration, automatically handling endpoint provisioning and configuration. By default, the script waits for endpoint deployment to complete before exiting. Upon completion, it prints the deployed endpoint name and AWS Region for subsequent inference requests. You will need to replace the fields enclosed in <> with actual values of your environment.

Bash

!python ~/graphstorm/sagemaker/launch/launch_realtime_endpoint.py \

--image-uri <account_id>.dkr.ecr.<aws_region>[.amazonaws.com/graphstorm:sagemaker-endpoint-cpu](https://.amazonaws.com/graphstorm:sagemaker-endpoint-cpu) \

--role arn:aws:iam::<account_id>:role/<your_role> \

--region <aws_region> \

--restore-model-path <restore-model-path>/models/epoch-1/ \

--model-yaml-config-file <restore-model-path>/models/GRAPHSTORM_RUNTIME_UPDATED_TRAINING_CONFIG.yaml \

--graph-json-config-file <restore-model-path>/models/data_transform_new.json \

--infer-task-type node_classification \

--upload-tarfile-s3 s3://<cdk-created-bucket> \

--model-name ieee-fraud-detect
Step 4: Real-time inference
In Notebook 4-Sample-Graph-and-Invoke-Endpoint, you build a basic client application that integrates with the deployed GraphStorm endpoint to perform real-time fraud prevention on incoming transactions. The inference process accepts transaction data via standardized JSON payloads, performs node classification predictions in hundreds of milliseconds, and returns fraud probability scores enabling immediate decision-making.

An end-to-end inference call for an existing node in the graph takes three distinct stages:

Graph sampling from the Neptune database. For a given target node existing in the graph, retrieve its k-hop neighborhood with a fanout limit, i.e. cap the number of neighbors retrieved at each hop by a threshold.

Prepare payload for inference. Neptune returns graphs using GraphSON, a specialized JSON-like data format used to describe graph data. In this step, you need to convert the returned GraphSON to GraphStorm’s own JSON specification. This step is performed on the client performing inference, in this case a SageMaker notebook instance.

Model inference by a SageMaker endpoint. Once the payload is prepared, you send an inference request to a SageMaker endpoint that has loaded a pre-trained model snapshot. The endpoint receives the request, performs any necessary feature transformations (such as converting categorical features to one-hot encoding), creates the in-memory binary graph representation, and makes a prediction for the target node using the graph neighborhood and trained model weights. The response is encoded into JSON and sent back to the client.

An example response from the endpoint looks like this:

JSON

{
"status_code": 200,

"request_uid": "877042dbc361fc33",

"message": "Request processed successfully.",

"error": "",

"data": {

"results": [

{

"node_type": "Transaction",

"node_id": "2991260",

"prediction": [0.995966911315918, 0.004033133387565613]

}

]

}

}
The data of interest for the single transaction you predicted is in the prediction key and corresponding node_id. The prediction result gives you the raw scores the model produced for class 0 (legitimate) and class 1 (fraudulent) at the 0th and 1st indexes of the predictions list respectively. In this example, the model flags this transaction as highly likely to be legitimate. You can find the full GraphStorm response specification in GraphStorm documentation.

Complete implementation examples, including client code and payload specifications, are provided in the (repository) to guide integration with production systems.

Clean up
To stop incurring costs on your account, you need to delete the AWS resources you created with AWS CDK at Environment Setup step.

First, you must delete the SageMaker endpoint created in Step 3 so the cdk destroy command can complete. See Delete Endpoints and Resources for options to delete an endpoint. Once done, you can run the following command from the root folder of the repository:

Bash

cd neptune-database-graphstorm-online-inference/neptune-db-cdk

cdk destroy
See AWS CDK documentation for more information on how to use cdk destroy, or CloudFormation documentation on how to delete a stack from console UI. By default, the cdk destroy command does not delete the model artifacts and processed graph data stored in S3 bucket during the training and deployment. You must delete them manually. See Deleting a general purpose bucket for information on how to empty and delete an S3 bucket AWS CDK created.

Conclusion
Graph neural networks address complex fraud prevention challenges by modeling entity relationships that traditional machine learning methods miss when analyzing transactions in isolation. GraphStorm v0.5 simplifies GNN real-time inference deployment with single-command endpoint creation, which previously required multi-service orchestration, and a standardized payload specification that simplifies client integration with real-time inference services. Organizations can now deploy enterprise-scale fraud prevention endpoints through streamlined commands, reducing engineering customization from weeks to single-command operations.

To implement GNN-based fraud prevention with your own data:

Review GraphStorm documentation for model configuration options and deployment specifications.

Adapt this IEEE-CIS example for your fraud prevention dataset by modifying the graph construction and feature engineering steps, using the source code and complete tutorials available in our GitHub repository.

Visit the step-by-step implementation guide to build production-ready fraud prevention solutions with GraphStorm v0.5’s advanced capabilities using your enterprise data.

Author Jian Zhang — Senior Applied Scientist who has used machine learning techniques to help customers solve various problems, such as fraud detection, decor image generation, and more. He successfully developed graph-based machine learning solutions, especially graph neural network, for customers in China, the US, and Singapore. As an evangelist of AWS graph capabilities, Zhang has given many public talks about GraphStorm, GNN, Deep Graph Library (DGL), Amazon Neptune, and other AWS services.

Author Theodore Vasiloudis — Senior Applied Scientist at AWS, where he works on distributed machine learning systems and algorithms. He led the development of GraphStorm Processing, the distributed graph processing library for GraphStorm and is a core developer for GraphStorm. He received his PhD in Computer Science from KTH Royal Institute of Technology, Stockholm, in 2019.

Author Xiang Song — Senior Applied Scientist at AWS AI Research and Education (AIRE), where he develops deep learning frameworks including GraphStorm, DGL, and DGL-KE. He led the development of Amazon Neptune ML, a new capability of Neptune that uses graph neural networks for graphs stored in a graph database. He is currently leading the development of GraphStorm, an open source graph machine learning framework for enterprise use cases. He received his PhD in computer systems and architecture at Fudan University, Shanghai, in 2014.

Author Florian Saupe — Senior Technical Product Manager at AWS AI/ML research, supporting science teams such as the graph machine learning, and ML Systems teams working on large scale distributed training, inference, and fault tolerance. Prior to joining AWS, Florian led technical product management for automated driving at Bosch, was a strategy consultant at McKinsey & Company, and worked as a scientist in control systems and robotics—a field he holds a PhD in.

Author Ozan Eken — Product Manager at AWS, passionate about building cutting-edge Generative AI and Graph Analytics products. With a focus on simplifying complex data challenges, Ozan helps customers unlock deeper insights and drive innovation. Outside of work, he enjoys trying new foods, exploring different countries, and watching football.
```
