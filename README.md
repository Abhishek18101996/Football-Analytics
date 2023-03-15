# Football Analytics

<img alt="Football2Vec" src="https://pbs.twimg.com/profile_banners/57356687/1630479357">


## Pre-Requisites
### Anaconda environment

### Dataset
The StatsBomb open dataset represents an invaluable resource for researchers seeking to explore the complexities of contemporary football. This freely available data offers a comprehensive overview of key performance indicators and enables users to engage in sophisticated data analysis to gain deeper insights into player and team performance. Interested parties are encouraged to visit the official GitHub repository for more information, where they can download the dataset and begin exploring its vast potential.

To ensure seamless integration with other data sources, it is recommended that the extracted 'statsbomb' directory be saved in <package_path>/data without any modifications to its original name. This will enable users to capitalize on the full range of features and functionalities offered by the StatsBomb open dataset and unlock new avenues for exploration and discovery.

### Pre-trained package: 
In order to effectively utilize the UI, it is imperative that users first create all necessary artifacts, including data objects and trained models. To accomplish this, simply run the main.py script manually, as detailed in the 'Manual Run' section.

Alternatively, users may choose to take advantage of pre-trained models that are available for download. Please refer to the information below for additional guidance on accessing these resources.

It is important to note that at present, data objects are not available for download due to licensing considerations. We appreciate your understanding and look forward to exploring additional avenues for making these resources available in the future.

 
## Manual Run
To initiate a manual run of the package, users may simply execute main.py using any Python-supported software or by utilizing the terminal. For terminal installation, navigate to the project directory and execute the command python main.py.

The package offers two primary processes:

build_data_objects(): This function is responsible for constructing all data objects necessary for both the models and the UI. These objects include enriched_events_data, matches_metadata, teams_metadata, players_metadata, players_metrics_df, and baselines.pickle.
The enriched_events_data DataFrame applies to_metric_centered_coordinates on the data and adds information such as shot types. The matches_metadata DataFrame adds information such as season_name and competition_name for each match in the dataset. The teams_metadata DataFrame adds columns such as nation, stadium, and gender for each team in the dataset. The players_metadata DataFrame combines player metadata given in the dataset and enriches it with events_data information, including player_name, team_name, and position_name per player. Finally, the players_metrics_df DataFrame builds a collection of stats for players, including xG, xA, lifts for each shot type, and more. The baselines.pickle object builds a dictionary that maps baselines_dimension to a DataFrame that is identical to players_metrics_df but contains baselines names instead of players.

build_language_models(): This function is responsible for building all models of Football2Vec, including Action2Vec and Player2Vec. It then exports the resulting artifacts for use by the package.

### Run configurations
There are some very basic configurations for each run, available to modify directly on `main.py`:
- `force_create = False`: Whether to force override all artifacts without trying load existing artifacts.
- `verbose = False` for Prints control.
- `plotly_export = False`: Whether to export <a href="http://plotly.com">Plotly</a> figures to <a href="https://chart-studio.plotly.com"> Plotly studio</a>. 
- `save_artifacts = False`: Whether to save the artifacts in to `params.PATHS.ARTIFACTS`. 
    - Pay attention that this is False by default, meaning NO ARTIFACTS WILL BE SAVED.

### Additional configurable parameters
params.py:
- `CONSTANTS.VOCABULARY` - holds all events types that will be considered by the language models.
- `CONSTANTS.HARD_XG` & `CONSTANTS.EASY_XG` - define the 'easy' and 'hard' probabilities thresholds for skill evaluation and the UI.

### Running times
Total run time: 106 minutes
- Total run time for `build_data_objects()`: 76 minutes
- Total run time for `build_language_models()`: 30 minutes<br>

With much older MacBook Pro (Retina, 15-inch, Late 2013):
- Machine processor i386
Total run time: 437 minutes
Total run time for build_data_objects: 108 minutes
Total run time for build_language_models: 329 minutes

## Running the Streamlit UI
To run the Streamlit UI, open a terminal/cmd window in the project directory and run:
<br>`streamlit run player_app.py`.

<br>
This will open a localhost on a browser. More on deploying Streamlit apps can be found <a href="https://docs.streamlit.io/en/stable/deploy_streamlit_app.html">here</a>.
<br>
Since the UI consumes all artifacts above, or creating them on the fly, it is highly recommended to either to download 
the pre-trained models or to run `main.py` before running the UI. When doing so, verify that `save_artifacts` is set to `True`.
The UI is a Streamlit dashboard which presents skill evaluation and representation of the selected player in the UI.<br>
During the first run (or any run, if `save_artifacts` is disabled) the app will create `players_metrics_by_seasons.csv` with the sie of 142KB.
- It is recommended to run step 1, 2 before building the UI, so the UI won't build all data objects and models on the fly.
- For best performance, enable save_artifacts in (see 'Run' section above). Streamlit will be able to load the data into its cache, allowing seamless experience.  

### UI components
It is a simple Streamlit app with the following features:
1. Information section: Sidebar for team & player selection, the player image and player metadata.
<img src="https://cdn-images-1.medium.com/max/1600/1*IHUjltY_GltSnoPCZpTyqQ.png" alt="information section"><br><br>

2. Player skill analysis section: 
    - Analysis' parameters control panel.
    <img src="https://cdn-images-1.medium.com/max/1600/1*1-ecjk80BW0LqcZFXTPIHg.png"><br>
    - Skills radar chart with baselines
    - Badges - each shot type has a unique icon for players with a Lift value greater than the threshold [=1.1].
    <img src="https://cdn-images-1.medium.com/max/1600/1*wv3LQrePZVKBRyDG-2FJsA.png">
   
3. Player evolution section (collapsable container)
<br>Analyzing the player's skills and performance over seasons. Contain two Plotly animated charts:<br>
    - Animation of player's skills radar charts over seasons (see `plot.py > player_evolution`), with integrated controls.<br>
    <img src="https://cdn-images-1.medium.com/max/1600/1*FtCFSdY59ckv8wTb-0yN5Q.png"><br>
    
    - Animated actions heatmaps over seasons (see `plot.py > player_actions_heatmap_evolution`), with integrated controls. It describes the frequency and location of actions over seasons.
    <img src="https://cdn-images-1.medium.com/max/1600/1*Po8zvZ4tya4xxR-hOxWwAA.png"><br><br>
 
4. xG evaluation (collapsable container)<br>
<br>The xG evaluation section presents two charts:
    - Shot conversions distribution plot (on top)- Plots the xG conversion for a single-player.
    - xG Lift by body part - analyzes the players' head and legs performance. 
<img src="https://cdn-images-1.medium.com/max/1600/1*zjMxegpvNVCzp4c4FDIDcg.png"><br>
5. Player2Vec embeddings section<br>
This section holds all insights origin from the language models listed below.
    - Player2Vec UMAP embeddings plot with coloring and presentation configurations.
    - Most similar players to the selected player, by cosine similarity, as well as by euclidean distance.
<img src="https://cdn-images-1.medium.com/max/1600/1*0_-lyiPn2kC0ofCm7ymRUQ.png">


## Language models
### Action2Vec
A <a href="https://radimrehurek.com/gensim/models/word2vec.html">Gensim Word2Vec</a> model which allows embedding the semantics of the football language in a 32-dimensional space. 
<br>
Read more: <a href="https://towardsdatascience.com/embedding-the-language-of-football-using-nlp-e52dc153afa6">Embedding the Language of Football Using NLP</a>.
<img src="https://miro.medium.com/max/2000/0*OCSd2lLUVJV22-NA">
<em>UMAP projections of the complete 19K words Action2Vec vocabulary.</em>

#### Extending the models
For extending the model, you may edit the following functions:
1. `data_processing.py -> FootballTokenizer -> def tokenize_action`. This function receives an event and produce a word out of it.
2. `data_processing.py -> FootballTokenizer -> def build_corpus`. This function controls the building process of the corpus.
3. Model hyper-parameters: `models.py -> def train_Word2Vec`

### Player2Vec
PlayerMatch2Vec, a <a href="https://radimrehurek.com/gensim/models/doc2vec.html">Gensim Doc2Vec</a> model that produces 32-sized vectors representing a player within a specific match.<br> 
Player2Vec representation is achieved by simply averaging all the player PlayerMatch2Vec representations.
Read more: <a href="https://towardsdatascience.com/embedding-the-language-of-football-using-nlp-e52dc153afa6">Embedding the Language of Football Using NLP</a>
Here it how it looks like:<br>
<a href="https://chart-studio.plotly.com/create/?fid=ofirmg:4#/"><img src="https://miro.medium.com/max/2000/0*EPo7_M0tZK-1-_ui"></a>
<em>Plotly interactive UMAP projection of Player2vec where all player’s matches are averaged to a single vector. Players are colored by position. 
<a href="https://chart-studio.plotly.com/create/?fid=ofirmg:4#/">Interactive Plotly visualization.</a></em>

#### Extending the model
For extending the model, you may edit the following functions:
1. `data_processing.py -> FootballTokenizer -> def tokenize_action`. This function receives an event and produce a word out of it.
2. `data_processing.py -> FootballTokenizer -> def build_corpus`. This function controls the building process of the corpus.
3. Model hyper-parameters: `models.py -> def train_Doc2Vec`


### Explainers
NOTICE: this module, unlike others, is mostly hard-coded and very strict in its input support.
It requires full compliance to the pre-trained models' format and vocabulary naming conventions.<br>
These requirements are strict: when broken, no meaningful outputs will be achieved, and errors will be raised.

As demonstrated in <a href="https://towardsdatascience.com/a-deep-dive-into-the-language-of-football-2a2984b6bd21#744c">A Deep Dive into the Language of Football</a>, 
this package includes four explainability methods, both local and global: representation-based explainers, analogies, 
similarities, and creating players' variations:
1. *ActionAnalogies*: an object that allows actions analogies using analogies equations: Word A1 → Word A2 ~ Word B1 → 
Word B2. <a href="https://towardsdatascience.com/a-deep-dive-into-the-language-of-football-2a2984b6bd21#744c">Read more 
here </a>.<br>An example for pass direction analogy:
<img alt="Pass direction" src="https://miro.medium.com/max/1400/1*4quayQwivRb6jru_P7JxcQ.png">
<em>Illustrative analogy plot for learning pass direction. B1/2/3 are the best actions to fit the analogy equation: 
A - A’ + B’ =?. Solid lines represent A or B, while dashed lines represent A’ or B’. Green colors are for A, A’, reds 
for B, B’. The pass distance (short/med/long) is represented by the arrow length. Here, A’ is the same pass as A, but 
with the opposite direction (left). B’ is the same as A’ from one position behind. B1/2 are mirrored passes to B with variations of height and length. B3 is exactly the mirrored pass. Image by Author.</em>

2. *PlayersAnalogies*: an object that allows players analogies using analogies equations: Word A1 → Word A2 ~ Word B1 → Word B2. <a href="https://towardsdatascience.com/a-deep-dive-into-the-language-of-football-2a2984b6bd21#b8ab">Read more here </a>.<br>Examples:
<img alt="Players analogies" src="https://miro.medium.com/max/2000/1*Pbh5EWpdrXRUwLWSHbHaDw.png">
<em>Illustrative players analogies plots. In each figure, B values are the top players to fit the analogy equation: A - A’ + B’ =?. Solid lines represent A and B, while dashed lines represent A’ and B’. Green colors are for A, A’, reds for B, B’.</em>

3. *PlayerSkillsExplainer*: an object that allows combining players with actions, generating endless local variations 
for a player, across one or more skills. For example, creating offensive variations with more shots or crosses, or 
enhancing defensive skills by replacing bad tackles with successful ones. These variations can serve as explainers. 
<a href="https://towardsdatascience.com/a-deep-dive-into-the-language-of-football-2a2984b6bd21#c674">Read more here </a>
and interact with the <a href="https://chart-studio.plotly.com/create/?fid=ofirmg:110#/">full Plotly chart here.</a>

4. *LinearDocExplainer* - an object that allows summing collection of actions and players representation, creating 
players variations to serve as explainers. <a href="https://towardsdatascience.com/a-deep-dive-into-the-language-of-football-2a2984b6bd21#5c82">Read more here</a>.
<br>Example:
    - Most similar player to _Neymar_: _Ronaldinho_
        - _Neymar — dribbling (all locations) ~ Thierry Henry (in Barcelona)_
        - _Neymar — flank dribbling ~ Philippe Coutinho_
    - Most similar player to _Griezmann_: _Carlos Vela_
        - _Griezmann + dribble (all locations) ~ Arjen Robben_
        - _Griezmann + flank dribble ~ Mikel Oyarzabal_    
5. `Player2Vec_std_analysis` - a function that allows analyzing Player2Vec variance, plotting it using Plotly. <a href="https://towardsdatascience.com/a-deep-dive-into-the-language-of-football-2a2984b6bd21#cb52">Read more here </a> and 
interact with the <a href="https://chart-studio.plotly.com/create/?fid=ofirmg:114">full Plotly figure here.</a>

6. `analyze_vector_dimensions_semantics` - a function that analyzes each dimension of the representation by returning 
players with the highest and lowest corresponding dimension values. <a href="https://towardsdatascience.com/a-deep-dive-into-the-language-of-football-2a2984b6bd21#b83c">Read more here</a>. 


### default_run 
This method is defined for all explainers in `explainers.py`. It runs the explainer, executing all analogies and actions done <a href="https://towardsdatascience.com/a-deep-dive-into-the-language-of-football-2a2984b6bd21">here.</a>
You can use the `default_run` function as a convenient benchmark.<br>

## Artifacts
The package outputs various artifacts, both data-related and models-related.<br>
You can enable / disable the save of artifacts using the `save_artifacts` configuration mentioned above.<br>
You can control all paths in this package using the `param.py` module.<br>
Model artifacts naming is dynamic, according to the model name. Hence, to change model artifacts path, change `params.py -> MODELS_ARTIFACTS`.<br> 

### Artifacts Directory and configurations
- Artifacts directory can be modified via `params.py -> ARTIFACTS`
- Models' artifacts directory can be modified via `params.py -> MODELS_ARTIFACTS`. It contains to following objects:
    - Word2Vec / Doc2Vec model object: `<model_name>.model`
    - Word2Vec model's wordvectors object: `<model_name>.wordvectors` (read more 
    <a href="https://radimrehurek.com/gensim/models/word2vec.html">here</a> and <a href="https://radimrehurek.com/gensim/models/doc2vec.html">here</a>).
    - Corpus object, in which all words and sentences are processed and their mappings are saved:  `<model_name>_corpus.pickle`
    - Embeddings dictionary object, in which all words and documents vectors are saved: `<model_name>_embeddings.pickle`
    - Similarity db object, keeps all cosine similarities values across documents: `<model_name>_docs_similarities.pickle`
    - UMAP figure as HTML, created by models -> `plot_embeddings, <model_name>_umap_plot.html`
* Pay attention `MODELS_ARTIFACTS` includes the `ARTIFACTS` path in it.

#### Output directories
*!* It is recommended to modify the `ARTIFACTS` and `MODELS_ARTIFACTS` rather than the following paths. *!*
1. Events data outputs:
    - Path of all processed enriched events data: `params.py -> ENRICH_PLAYERS_METADATA_PATH`

2. Metadata and metrics paths:
    - Path of players metadata: `params.py -> PATHS.PLAYERS_METADATA_PATH`
    - Path of teams metadata: `params.py -> PATHS.TEAMS_METADATA_PATH`
    - Path of matches metadata: `params.py -> PATHS.MATCHES_METADATA_PATH`
    - Path of players skill evaluation metrics: `params.py -> PATHS.PLAYERS_METRICS_PATH`
    - Path of baselines skill evaluation metric: `params.py -> PATHS.BASELINE_PLAYERS_METRICS_PATH`
    - Path of skill evaluation metric by seasons: `params.py -> PATHS.PLAYERS_METRICS_BY_SEASON`

3. Analyses paths
    - Path of explainers' outputs: `params.py -> EXPLAINERS`
    - Path of skill analysis: `params.py -> PATHS.EXPLAINERS`

### Data Artifacts
This includes the following files:
- matches_metadata.csv - 434KB
- players_metadata.csv - 1.4MB
- players_metrics_df.csv - 2.5MB
- baselines_metrics.pickle - 20KB
- enriched_events_data.pickle - 1.65GB

### Models artifacts
These include the following files:
- Action2Vec 
    - `Action2Vec.model` (<a href="https://radimrehurek.com/gensim/index.html">Gensim</a> Word2Vec object) - 257KB (3.9MB for the pre-trained)
    - `Word2Vec Action2Vec.wordvectors` - 138KB (2.2MB for the pre-trained)
    - `Action2Vec_corpus.pickle` - 8MB (14.3MB for the pre-trained)
    - Plotly HTML file `Action2Vec_umap_plot.html` - 4.4MB
- Player2Vec
    - `Player2Vec.model` (Gensim Doc2Vec object. In fact, it is PlayerMatch2Vec) - 5.1MB (13.7MB for the pre-trained) 
    - `Doc2Vec Player2Vec.wordvectors` - 4.7MB
    - `Player2Vec_embeddings.pickle` - 1MB
    - `Player2Vec_corpus.pickle` - 8.4MB (13.6MB for the pre-trained)
    - Plotly HTML file `Player2Vec_umap_plot.html` ~ 5MB

### Analyses artifacts
These include the following files:
- `explain.py`
    - `Player2Vec Variance_umap_plot.html` (in `MODELS_ARTIFACTS` directory)
    - `PlayersAnalogies` object outputs Players analogies results (if `export_artifacts` argument sent as `True`). It produces a csv file for each analogy. 
    <br> Naming format: `Analogy/<analogy name>/ <A1> - <A2> + <B2> ~ ?.csv`
    - `PlayerSkillsExplainer` outputs a csv with most similar results to given query.<br> 
    Naming format: `most_similar_<player_name>_<variation_action>_<skill_name>.csv`
    - A Plotly UMAP projection figure will be opened via the browser, for each given query.
- `skill_analysis.py`: no artifacts. Plotly figures are opened in the browser.

### UI artifacts
 - `players_metrics_by_seasons.csv`: Builds a DataFrame of metrics `players_metrics_df`, by also aggregated by season, for evolution plots.
 - `team_2_players.pickle` - dict for fast access to all players of each time.

### Plotly export
In order to allow export to <a href="https://chart-studio.plotly.com"> Plotly studio</a>, please fill `PLOTLY_USERNAME` and `PLOTLY_API_KEY` in `params.py`.

## Contacts and communication:
- <a href="https://github.com/ofirmg/football2vec">Github repository</a>
- <a href="https://twitter.com/Magdaci">Twitter</a><br> 

