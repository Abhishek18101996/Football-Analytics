# Football Analytics

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
The main.py file provides a few fundamental configurations that can be modified to suit your needs. These configurations are as follows:

force_create = False: This determines whether or not to override existing artifacts when running the program.
verbose = False: This determines whether or not to enable print statements for additional information during runtime.
plotly_export = False: This determines whether or not to export any Plotly figures to Plotly Studio.
save_artifacts = False: This determines whether or not to save the artifacts in the designated params.PATHS.ARTIFACTS directory.
Please note that the save_artifacts parameter is set to False by default, which means that no artifacts will be saved unless you explicitly change this configuration.

## Running the Streamlit UI
To run the Streamlit UI, open a terminal/cmd window in the project directory and run:
<br>`streamlit run player_app.py`.

<br>
This will open a localhost on a browser.
<br>
To ensure optimal performance when using the UI, it is strongly recommended that you either download the pre-trained models or run main.py beforehand. If you choose to run main.py, it is important to set save_artifacts to True to ensure that all necessary artifacts are saved. The UI itself is a Streamlit dashboard that provides skill evaluation and player representation features.

If you choose to run main.py before launching the UI, it is recommended that you complete steps 1 and 2 beforehand. This will prevent the UI from having to build all of the data objects and models on the fly. Additionally, enabling save_artifacts will allow Streamlit to load the necessary data into its cache, resulting in a seamless user experience.

Finally, it is worth noting that during the initial run (or any subsequent run with save_artifacts disabled), the app will create a players_metrics_by_seasons.csv file with a size of 142KB. 

### UI components
It is a simple Streamlit app with the following features:
1. The information section of the UI contains a sidebar where the user can select a team and a player. Once a player is selected, the sidebar displays the player's image and metadata such as their name, position, nationality, and date of birth. This information provides context for the user and helps them make informed decisions about which player to evaluate or represent.
<img src="https://cdn-images-1.medium.com/max/1600/1*IHUjltY_GltSnoPCZpTyqQ.png" alt="information section"><br><br>

2. Player skill analysis section: 
    - Analysis' parameters control panel.
    <img src="https://cdn-images-1.medium.com/max/1600/1*1-ecjk80BW0LqcZFXTPIHg.png"><br>
    The interface features a skills radar chart that displays a player's performance across multiple skills, along with baselines for comparison. Additionally, badges are awarded to players whose Lift value for each shot type exceeds the threshold of 1.1. The interface also includes a sidebar for team and player selection, as well as a player image and metadata.
    <img src="https://cdn-images-1.medium.com/max/1600/1*wv3LQrePZVKBRyDG-2FJsA.png">
   
3. Player evolution section (collapsable container)
<br>An analysis of the player's skills and performance over seasons is provided, which includes two Plotly animated charts. The first chart depicts the animation of the player's skills radar charts over seasons using the plot.py library's player_evolution function. This chart is equipped with integrated controls to provide an interactive experience.<br>
    <img src="https://cdn-images-1.medium.com/max/1600/1*FtCFSdY59ckv8wTb-0yN5Q.png"><br>
    
    This feature in the UI generates an animated heatmap that displays the frequency and location of a player's actions over multiple seasons. The heatmap is created using Plotly and can be accessed through the plot.py module's player_actions_heatmap_evolution function. The animation is interactive and comes with integrated controls, allowing users to toggle between seasons and view the player's actions over time.
    <img src="https://cdn-images-1.medium.com/max/1600/1*Po8zvZ4tya4xxR-hOxWwAA.png"><br><br>
 
4. xG evaluation (collapsable container)<br>
<br>The xG evaluation section presents two insightful charts that provide an in-depth analysis of the player's performance based on expected goals (xG).

The first chart is a shot conversions distribution plot that displays the player's xG conversion rate. This plot illustrates how often a player is expected to score based on the quality of the shots they take, and how often they actually score. By analyzing this chart, it is possible to evaluate the player's finishing ability and their effectiveness in converting scoring opportunities.

The second chart is called xG Lift by body part. This chart analyzes the player's performance based on their body part used to take the shot, such as their head or legs. By comparing a player's xG value to the average xG value for each body part, it is possible to see how much better or worse the player is at taking shots with specific body parts. This analysis provides valuable insights into a player's strengths and weaknesses and can help coaches and analysts identify areas for improvement. 
<img src="https://cdn-images-1.medium.com/max/1600/1*zjMxegpvNVCzp4c4FDIDcg.png"><br>
5. Player2Vec embeddings section<br>
We leverage the power of the Player2Vec language model to gain valuable insights into a player's performance. We begin by visualizing the player's UMAP embeddings using Plotly, allowing us to explore patterns and clusters within the data. Additionally, we identify the most similar players to the selected player based on cosine similarity and euclidean distance metrics, providing a comprehensive understanding of how the player compares to others in the league. These insights are invaluable for identifying strengths and weaknesses, and ultimately improving overall performance.
<img src="https://cdn-images-1.medium.com/max/1600/1*0_-lyiPn2kC0ofCm7ymRUQ.png">


## Language models
### Action2Vec
A <a href="https://radimrehurek.com/gensim/models/word2vec.html">Gensim Word2Vec</a> model which allows embedding the semantics of the football language in a 32-dimensional space. 
<br>The model utilizes an advanced approach to capture and encode the semantics of the football language, resulting in a 32-dimensional embedding space.

<img src="https://miro.medium.com/max/2000/0*OCSd2lLUVJV22-NA">
<em>UMAP projections of the complete 19K words Action2Vec vocabulary.</em>


### Player2Vec
PlayerMatch2Vec, a <a href="https://radimrehurek.com/gensim/models/doc2vec.html">Gensim Doc2Vec</a> The Action2Vec model is used to produce 32-sized vectors representing a player within a specific match. It takes into account the sequence of actions performed by the player in the match, along with the spatial and temporal features of the actions. These vectors can then be used to analyze the player's performance in the match, as well as to compare the player to other players in the dataset.<br> 
The Player2Vec representation for a player in a specific match is obtained by averaging the PlayerMatch2Vec representations of all the actions performed by that player in that match. This results in a single 32-dimensional vector representing the player's performance in that particular match. This process is repeated for all matches in which the player participated, resulting in a Player2Vec representation for the player across all matches.
Here it how it looks like:<br>
<a href="https://chart-studio.plotly.com/create/?fid=ofirmg:4#/"><img src="https://miro.medium.com/max/2000/0*EPo7_M0tZK-1-_ui"></a>
<em>UMAP (Uniform Manifold Approximation and Projection) is a dimensionality reduction technique used for visualization and clustering of high-dimensional data. In the context of Football2Vec, UMAP is used to visualize the embeddings of Player2Vec in a 2D space, allowing for interactive exploration of player similarities and differences.

The Plotly interactive UMAP projection of Player2Vec shows a scatter plot of all players in the dataset, where each player is represented by a single vector obtained by averaging all of their PlayerMatch2Vec representations. Players are colored by position, allowing for easy identification of different player types (e.g. defenders, midfielders, forwards).

The interactive plot allows users to hover over each point to see the player's name and position, and to zoom in and out for a closer look. The UMAP projection provides a high-level overview of the similarities and differences between players, and can be used as a starting point for further analysis and exploration. 
<a href="https://chart-studio.plotly.com/create/?fid=ofirmg:4#/">Interactive Plotly visualization.</a></em>
