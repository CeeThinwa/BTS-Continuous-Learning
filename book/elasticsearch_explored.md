# ElasticSearch database setup notes

## To Do List
1. To set up the virtual machine (VM) aka. the private server for the database<br>
   1. Create of an appropriate VM in terms of compute and storage space
   2. Create a sudo user and restrict root access
   3. Create environment credentials to avoid exposure in code and control access to the VM
   4. Create app modules
   5. Fetch the data from GitHub and use the project structure to create metadata
2. To set up the web app to fetch data from the private Github repo
3. To run the ML on each fetched image and store results in the Elasticsearch database
4. To get text from the `raw` Elasticsearch database and display on the admin UI for validation
5. To store the clean data in the `clean` Elasticsearch database, ready for search
6. To display the resulting text and image upon request

## Resources:
* [Sample MLOps app](https://github.com/iusztinpaul/energy-forecasting)
* [Search for resources for using python to get a repo structure](https://duckduckgo.com/?t=ffab&q=get+project+directory+using+python&ia=web)
* [Personal Access Tokens on Github](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#keeping-your-personal-access-tokens-secure)
* [Access data from a private Github repository](https://blog.exploratory.io/extract-data-from-private-github-repository-with-rest-api-db804fa43d84)
* [Configuration of an elasticsearch database on a Virtual Machine](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-20-04)
