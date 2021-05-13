# Mapping Forward-Looking Mitigation Studies at Country Level

# Repository information
This repository contains the replication code and data for the paper *Mapping Forward-Looking Mitigation Studies at Country Level* (Claire Lepault and Franck Lecocq, 2021).

For copyright reasons, the abstracts from Scopus and Web of Science can not be published. To reproduce the analysis, it would then be necessary to download the abstracts of the 4691 references, either using the Scopus or Web of Science IDs provided, or the query presented in [1_Search_equation.ipynb](1_Constitution_database/1_Search_equation.ipynb). To reproduce exactly the download process, we refer you to an older tutorial on systematic search on Scopus and Web of Science, available on Github : https://github.com/ClaireLepault/systematic-search-wos-scopus.

All the code is presented in Jupyter notebook format, with detailed comments to ensure transparency and reproducibility of the analysis.

### Content

There are seven folders presenting all the steps of the analysis, from the constitution of the database to the creation of the figures :

* [0_Reference_files](0_Reference_files): Contains the reference files used for the analysis <br>
    * **country_shapefile** : folder containing the country shapefile from [Natural Earth](https://www.naturalearthdata.com/)
    * **CAIT_GHG_2016.csv** : historical greenhouse gas emissions at country level in 2016 (all gases, including emissions of the Land-Use Change and Forestry sector) from the Climate Watch Historical GHG Emissions (2021. Washington, DC: World Resources Institute). Available online at: https://www.climatewatchdata.org/ghg-emissions
    * **Models_list.csv**: the model names that we search in the metadata (title, abstract, author keywords) of each publication. This list results from the association of models reviewed in [Allen et al (2016)](https://www.sciencedirect.com/science/article/abs/pii/S1462901116306712) and the list of models of the Integrated Assessment Modeling Consortium (available online at https://www.iamconsortium.org/resources/models-documentation/, consulted on April 30, 2021). We finally added to the list the "integrated assessment model" and the "computable general equilibrium" model. 
    * **UNSD_database.csv**: list of countries from the United Nations Statistics Division. Available online at:https://unstats.un.org/unsd/methodology/m49/overview/. Demonyms have been manually added.

In each following folder (corresponding to a stage of the analysis), the files being used in the following stages are in the *output* folder. The files being intermediaries within each stage are the *interm* folder.

* [1_Constitution_database](1_Constitution_database) : presents the search query creation, the fusion of publications from the different batchs downloaded respectively on Scopus (batch of maximum 2000 papers metadata at one time) and WoS (batch of maximum 500 papers metadata at one time) and finally the database merging both databases respectively from WoS and Scopus. Inputs are the batchs of metadata obtained on Scopus and WoS. Intermediary files are the complete databases coming respectively from Scopus and WoS. Output is the database merging metadata from WoS and Scopus. 


* [2_Treatment_database](2_Treatment_database) : presents additional treatment to the original database from WoS and Scopus. Country names, demonyms and acronyms (present in the title) are associated to each publication. Horizon year horizon year in [2025; 2100] and model names (present in the title, abstract or keywords) are then added to the database. Input is the output database of [1_Constitution_database](1_Constitution_database)). Output databases are : 
    * **database_multi_rows_each_paper.csv** : contains one row for each publication-country-model association
    * **database_one_row_each_paper.csv** : each row corresponds to one publication, if numerous countries or models are related to the paper, the column "Country" and "model" present the first one by alphabetical order


* [3_Topic_modeling](3_Topic_modeling) : seven notebooks detail  step-by-step the Non-Negative Matrix Factorization (NMF) approach and its parameters selection. For this step, thanks to Derek Greene, who proposes a great [tutorial](https://github.com/derekgreene/topic-model-tutorial) on topic modeling with NMF including the selection of the parameter *k* (number of topics) using topic coherence. We extend here the method to the selection of the regularization parameters \alpha and $l_1$. Due to its size (16.8 GB), the file "topic_models.pkl" containing the 16,731 matrix decompositions for the different combinations (k, \alpha, l_1) could not be uploaded in the *interm* folder.


* [4_Manual_treatment_topic_table](4_Manual_treatment_topic_table) : Input is the [table counting topic-papers association](3_Topic_modeling/output/Table_topics_count.csv). Topic "final names" are added manually. 


* [5_Final_databases](5_Final_databases) : contains one Jupyter notebook (R language) merging the databases with additional treatments from [2_Treatment_database](2_Treatment_database), the topic modeling classification from [3_Topic_modeling](3_Topic_modeling) and the topic names from [4_Manual_treatment_topic_table](4_Manual_treatment_topic_table). The three resulting final databases are:
    * **database_multi_rows_each_paper.csv** : contains one row for each publication-country-model association
    * **database_multi_rows_each_paper_one_per_country.csv** : contains one row for each publication-country association, if numerous models are related to the paper, the column "model" present the first one by alphabetical order
    * **database_one_row_each_paper.csv** : each row corresponds to one publication, if numerous countries or models are related to the paper, the column "Country" and "model" present the first one by alphabetical order


* [6_Figures](6_Figures) : presents the code producing all the figures (except Figure A6, coming from the [interactive visualization](3_Topic_modeling/7.Online_Visualization_topics.ipynb) showing relationship across topics). Inputs are the final databases as well as the reference files. Outputs are the figures.

### Dependencies

#### Python 3.7 
The core package requirements are:

* pandas (version 1.0.3)

* numpy (version 1.16.4)

* stemming (version 1.0.1)

* nltk (version 3.5)

* joblib (version 0.15.1)

* sklearn (version 0.0)

* gensim (version 3.8.3)

* pyLDAvis (version 2.1.2)

* matplotlib (version 3.1.0)

* seaborn (version 0.10.1)

and modules :

* operator

* string

* itertools

#### R 3.6.3 
The core package requirements are :

* tidyverse (1.3.0)
* dplyr (0.8.4)
* sf (0.9.7)
* ggrepel (0.8.1)
* scales (1.1.1)
* hrbrthemes (0.6.0)
* ggpubr (0.2.4)
* magick (2.5.2)
* ggpattern (0.1.3)
* ggplot2 (3.3.3)
* stargazer (5.2.2)
* viridis (0.5.1)
* extrafont (0.17)
* RColorBrewer (1.1.2)
