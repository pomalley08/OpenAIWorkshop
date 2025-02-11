# Overview
This application showcases the concept of a smart analytical agent designed to assist in answering business inquiries by executing advanced data analytics tasks on a business database. The agent is endowed with a learning capability; for problems it has never encountered before, it undergoes comprehensive reasoning steps including researching the database schema and interpreting the definitions of business metrics before proceeding to write code. However, for problems similar to those it has previously solved successfully (in memory recall mode), it quickly leverages its knowledge and reuses the code. The LLM that powers the full reasoning mode is GPT-4, while the LLM that facilitates the memory recall mode can be GPT-3.5-Turbo.

To determine the quality of the solution or answer to a question, the application provides users with a feedback mechanism. Answers that receive positive feedback are memorized and subsequently utilized by the agent, enhancing its efficiency and accuracy over time.

![Sample scenario](./data/plot1.png)

For a detailed explanation of the concept, visit: [Toward Learning-Capable LLM Agents](https://medium.com/data-science-at-microsoft/toward-learning-capable-llm-agents-72db3737e1c2)

Examples of questions are:
- Simple: 
    - What were the total sales for each year available in the database?
    - Who are the top 5 customers by order volume, and what is the total number of orders for each?
- More difficult: Is that true that top 20% customers generate 80% revenue?
- Advanced: Forecast monthly revenue for next 12 months

The application supports Python's built-in SQLITE .
# Installation 
## Azure Open AI setup
1. Create an Azure OpenAI deployment in an Azure subscription with a GPT-35-Turbo deployment and preferably a GPT-4 deployment.
Here we provide options to use both but GPT-4 should be used to address difficult & vague  questions.
We assume that your GPT-4 and CHATGPT deployments are in the same Azure Open AI resource.
## [Create Azure AI Search Service](https://learn.microsoft.com/en-us/azure/search/search-create-service-portal)

## Install the application locally 
1. Clone the repo (e.g. ```git clone https://github.com/microsoft/OpenAIWorkshop.git``` or download). Then navigate to ```cd scenarios/natural_language_query```
2. Provide settings for Open AI and Database.You can either create a `secrets.env` file in the root of this folder (scenarios/incubations/automating_analytics) as below or do it using the app's UI later on. 
    - use built-in SQLITE.
        ```txt
        AZURE_OPENAI_ENDPOINT="https://YOUR_OPENAI.openai.azure.com/"
        AZURE_OPENAI_API_KEY=""
        AZURE_OPENAI_EMB_DEPLOYMENT="text-embedding-ada-002"
        AZURE_OPENAI_GPT4_DEPLOYMENT1="gpt-4-1106"
        AZURE_OPENAI_CHAT_DEPLOYMENT2="gpt-35-turbo-1106" #this can be gpt-35-turbo
        AZURE_OPEN_AI_VISION_DEPLOYMENT=gpt-4-vision
        AZURE_SEARCH_SERVICE_ENDPOINT="https://.search.windows.net"
        AZURE_SEARCH_INDEX_NAME=sql_query_caches #name of the Azure search index that cache the SQL query
        AZURE_SEARCH_ADMIN_KEY=
        USE_SEMANTIC_CACHE=True
        SEMANTIC_HIT_THRESHOLD=0.02
        ```
4. Create a python environment with version from 3.8 and 3.10
    - [Python 3+](https://www.python.org/downloads/)
        - **Important**: Python and the pip package manager must be in the path in Windows for the setup scripts to work.
        - **Important**: Ensure you can run `python --version` from console. On Ubuntu, you might need to run `sudo apt install python-is-python3` to link `python` to `python3`. 
5. Import the requirements.txt `pip install -r requirements.txt`
6. Run the ```python create_cache_index.py``` to create index for the SQL query cache
7. To run the application from the command line: `streamlit run copilot.py`
8. If you are a Mac user, please follow [this](https://learn.microsoft.com/en-us/sql/connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos?view=sql-server-ver16) to install ODBC for PYODBC
## Deploy the application to Azure 
This application can be deployed to an Azure subscription using the Azure Developer CLI. 
There is no need to have any coding experience to deploy this application but you will need permissions to create resources in an Azure Subscription
To deploy to Azure:
- Install [Azure Developer CLI](https://aka.ms/azure-dev/install)   
- Use either `git clone https://github.com/microsoft/OpenAIWorkshop.git` to clone the repo or download a zip
- Go to the local directory of the OpenAIWorkshop
    > 💡 NOTE: It is very important to be in the root folder of the project
- Authenticate to Azure by running `azd auth login`
- Create a local environment `azd env new`
    > 💡 NOTE: If deploying to the same Subscription as others, use a unique name
- Deploy the app and infrastructure using `azd up`




