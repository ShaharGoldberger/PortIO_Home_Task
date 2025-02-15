**Technical Writer Assignment**

**By- Shahar Goldberger**

**16/02/2025**


**Youtube Playlist & Videos Catalog Guide**
Port is an internal developer portal solution, allowing you to create compelling developer experiences using the building blocks you need, according to your stack, developer personas and culture.


**Overview-**

This guide will provide a **step-by-step** process for setting up a **YouTube playlist & videos catalog** in Port using a GitHub workflow. This document will cover:

1. <span style="text - decoration: underline;">Modeling the Data in Port-</span>

	- Defining blueprints for Playlists and Videos.

	- Setting relationships between them.

2. <span style="text - decoration: underline;">Fetching YouTube Data-</span>

	- Using the YouTube Data API to retrieve playlist and video information.

3. <span style="text - decoration: underline;">Ingesting Data into Port-</span>

	- Formatting and sending YouTube data to Port using its API.

	- Automating this process with a GitHub workflow.

4. <span style="text - decoration: underline;">Visualizing the Data in Port-</span>

	- Creating dashboards to track key components and find trends in your data.


**Setting Up Port**

To get started with Port, users need to create an account on the platform:

1. <span style="text - decoration: underline;">Go to Port’s Website</span>

	- Open a web browser and navigate to[ ](http://app.getport.io/)[app.getport.io](http://app.getport.io/).

2. <span style="text - decoration: underline;">Sign Up for an Account</span>

	- Click on "Sign Up" (or "Login" if you already have an account).

3. <span style="text - decoration: underline;">Set Up Your Workspace</span>

	- Once logged in, you’ll be asked to create a workspace.

	- Give it a name.

	- Choose the default settings.

4. <span style="text - decoration: underline;">Access the Port Dashboard</span>

	- After setting up, you’ll be directed to the Port dashboard ([https://app.getport.io/organization/home](https://app.getport.io/organization/home)) , where you can start creating blueprints and ingesting data.


**Blueprints**

Blueprints in Port define the structure of your data. They act like schemas in a database, describing the objects (entities) you want to catalog and their properties.


To learn more about Blueprints,[ ](https://docs.port.io/build-your-software-catalog/customize-integrations/configure-data-model/setup-blueprint/)[click here](https://docs.port.io/build-your-software-catalog/customize-integrations/configure-data-model/setup-blueprint/).


For this guide, we will create two blueprints:

<span style="text - decoration: underline;">Youtube Playlist Blueprint</span> – Represents a YouTube playlist.

<span style="text - decoration: underline;">Playlist Items </span>– Represents an individual an item within a playlist.


**Creating Blueprints in Port-**

To create a blueprint in Port:

1. Navigate to Port's[ ](https://app.getport.io/organization/home)[home page](https://app.getport.io/organization/home).

2. 2.	Click on[_](https://app.getport.io/settings/data-model)[Builder](https://app.getport.io/settings/data-model) button on the upper right corner-  ![image](https://github.com/user-attachments/assets/bdb2b6b3-d094-49ab-b6c7-1b99d7d61225)

3. Create a new Blueprint with "+Blueprint"-
![image](https://github.com/user-attachments/assets/276a323e-5fa5-47c5-84fa-3dccfc81b78e)

4. Fill the blueprint form or directly edit the JSON, examples:
	- Youtube playlist Form-  
![image](https://github.com/user-attachments/assets/d2791940-2ff4-4166-9d9c-b9c87172690e)

	- Playlist item Form- 
![image](https://github.com/user-attachments/assets/84becada-882c-496d-9983-4ff41dd5fc41)

**Create relations between the blueprints-**

YouTube playlist contains an array of playlist id's. Each playlist item contains the id of the YouTube playlist it belongs to.

In the Youtube playlist blueprint JSON, the relation looks like this:
```
  "relations": {

    "relation_yt_playlist_to_repo": {

      "title": "relation YT Playlist to Repo",

      "target": "githubRepository",

      "required": false,

      "many": true

}
```
In the Playlist item blueprint JSON, the relation looks like this:
```
"relations": {

    "belongs_to": {

      "title": "Belongs to Playlist",

      "target": "youtube_playlist",

      "required": false,

      "many": false

}

}
```
To learn more about blueprints relations,[ ](https://docs.port.io/build-your-software-catalog/customize-integrations/configure-data-model/relate-blueprints/)[click here](https://docs.port.io/build-your-software-catalog/customize-integrations/configure-data-model/relate-blueprints/).




**GitHub Workflow for Fetching and Ingesting YouTube Data**

**Fetching YouTube Data**
1. Using the **YouTube Data API** to retrieve playlist details. Be aware of the JSON that represents the list, in order to extract data from Youtube data API.
2. Go to[ ](https://console.cloud.google.com/welcome)[Google Cloud Console](https://console.cloud.google.com/welcome) and create the project that we will use for the API.
3. For the created project we need to enable **Youtube Data API v3** from[ ](https://console.cloud.google.com/apis/library)[here](https://console.cloud.google.com/apis/library).
4. Create an API key that will be used in the workflow from[ ](https://console.cloud.google.com/apis/credentials)[here](https://console.cloud.google.com/apis/credentials).
5. Using the JSON schema of the data that we can extract from the Youtube API. We will choose what relevant information to extract.
6. [ ](https://developers.google.com/youtube/v3/docs?hl=he#Playlists)[Youtube data API documentation](https://developers.google.com/youtube/v3/docs?hl=he#Playlists).

**How to fetch the data from GitHub workflow**
1. Go to your GitHub repo and create a folder called- ".github/workflows".
2. Go to settings -> secrets -> actions and then create New repository secret called: **YOUTUBE_API_KEY** with the API key you created in the Google Cloud Console.
3. Create/Use existing workflow yml file and used this call for Youtube API.
4. To fetch the data from Youtube you can use this example:
```
PLAYLIST_ID="PLTu_mo3y42N3OZ9y9C7FeWFWeaqCTJK56"
curl -s "https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&maxResults=50&playlistId=${PLAYLIST_ID}&key=${ YOUTUBE_API_KEY }" > test_playlist.json
```
**Ingesting Data into Port**
* Formatting the extracted data into **Port’s API schema-**

An example how to do this with a Youtube Playlist blueprint:
![image](https://github.com/user-attachments/assets/39c2ec54-c375-4408-bf51-e3dccb9f6163)

	- Sending the data to **Port using API requests**-

Example:
![image](https://github.com/user-attachments/assets/f21e12e9-057c-4d0c-85e2-15ff50c92fc4)

**Full GitHub workflow that automates this process-**

Is located here-[ ](https://github.com/ShaharGoldberger/PortIO_Home_Task)[https://github.com/ShaharGoldberger/PortIO_Home_Task](https://github.com/ShaharGoldberger/PortIO_Home_Task).



**Visualizing Data in Port**

Port provides powerful **visualization tools** that allow users to analyze and explore their structured data in a **user-friendly, interactive way**. These tools help users **track trends, monitor key metrics, and gain insights** into their data.

  - To learn more about dashboard widgets for visualization, Go to-[ ](https://docs.port.io/customize-pages-dashboards-and-plugins/dashboards/)[https://docs.port.io/customize-pages-dashboards-and-plugins/dashboards/](https://docs.port.io/customize-pages-dashboards-and-plugins/dashboards/).

In this guide, we will provide two visualizations on the Youtube playlist data:

1. Pie Chart with the number of videos in the playlist.
	- I wanted to create a pie chart that represents the spread of the different Privacy Statuses of each video in the playlist, but didn't find a way to access these from the Youtube Playlist Dashboard. This would help find what kind of videos tend to be added to playlists and that way better lists for offering the end user can be generated. \
![image](https://github.com/user-attachments/assets/3cee7e4e-90ae-4dfa-aeaa-85fe17cf59a4)

2. Table of Videos in the playlist.
	- The table offers a clean and easy to sort view of the videos by name, description, upload time, etc… What I originally meant to do was use the table (and a graph) to portrait the correlation between the location of the video in the playlist and it's number of views, however I didn't find a way to access this data.

![image](https://github.com/user-attachments/assets/7120682b-317d-4c3d-a6aa-940cebfc606b)


**Port Components-**

**Youtube playlist Blueprint JSON-**
```
{

  "identifier": "youtube_playlist",

  "title": "YouTube Playlist",

  "icon": "List",

  "schema": {

    "properties": {

      "kind": {

        "type": "string",

        "title": "Kind",

        "default": "youtube#playlist"

},

      "etag": {

        "type": "string",

        "title": "ETag"

},

      "id": {

        "type": "string",

        "title": "Playlist ID"

},

      "publishedAt": {

        "type": "string",

        "format": "date-time",

        "title": "Published At"

},

      "channelId": {

        "type": "string",

        "title": "Channel ID"

},

      "title": {

        "type": "string",

        "title": "Title"

},

      "description": {

        "type": "string",

        "title": "Description"

},

      "thumbnails": {

        "type": "object",

        "title": "Thumbnails"

},

      "channelTitle": {

        "type": "string",

        "title": "Channel Title"

},

      "defaultLanguage": {

        "type": "string",

        "title": "Default Language"

},

      "privacyStatus": {

        "type": "string",

        "title": "Privacy Status"

},

      "podcastStatus": {

        "type": "string",

        "title": "Podcast Status",

        "enum": [

          "PODCAST_STATUS_UNSPECIFIED",

          "PODCAST_STATUS_ENABLED",

          "PODCAST_STATUS_DISABLED"

]

},

      "itemCount": {

        "type": "number",

        "title": "Item Count"

},

      "embedHtml": {

        "type": "string",

        "title": "Embed HTML"

},

      "localizations": {

        "type": "object",

        "title": "Localizations"

}

},

    "required": [

      "id",

      "title"

]

},

  "mirrorProperties": {

    "item_published_at": {

      "title": "item_published At",

      "path": "videos_in_playlist.publishedAt"

},

    "privacy_status": {

      "title": "Privacy Status",

      "path": "videos_in_playlist.privacyStatus"

}

},

  "calculationProperties": {},

  "aggregationProperties": {},

  "relations": {

    "relation_yt_playlist_to_repo": {

      "title": "relation YT Playlist to Repo",

      "target": "githubRepository",

      "required": false,

      "many": true

},

    "videos_in_playlist": {

      "title": "videos in playlist",

      "description": "the array of the videos in the playlist",

      "target": "playlistItem",

      "required": false,

      "many": true

}

}

}
```


**Playlist item Blueprint JSON-**
```
{

  "identifier": "playlistItem",

  "title": "Playlist Item",

  "icon": "Google",

  "schema": {

    "properties": {

      "kind": {

        "type": "string",

        "title": "Kind",

        "description": "The type of the resource, in this case youtube#playlistItem"

},

      "etag": {

        "type": "string",

        "title": "ETag",

        "description": "The ETag of the item"

},

      "id": {

        "type": "string",

        "title": "ID",

        "description": "The ID that YouTube uses to uniquely identify the playlist item"

},

      "publishedAt": {

        "type": "string",

        "format": "date-time",

        "title": "Published At",

        "description": "The date and time that the item was added to the playlist"

},

      "channelId": {

        "type": "string",

        "title": "Channel ID",

        "description": "The ID that YouTube uses to uniquely identify the channel that published the playlist item"

},

      "title": {

        "type": "string",

        "title": "Title",

        "description": "The item's title"

},

      "description": {

        "type": "string",

        "title": "Description",

        "description": "The item's description"

},

      "thumbnails": {

        "type": "object",

        "title": "Thumbnails",

        "description": "A map of thumbnail images associated with the playlist item"

},

      "channelTitle": {

        "type": "string",

        "title": "Channel Title",

        "description": "Channel title for the channel that the playlist item belongs to"

},

      "videoOwnerChannelTitle": {

        "type": "string",

        "title": "Video Owner Channel Title",

        "description": "The channel title of the channel that uploaded the video"

},

      "videoOwnerChannelId": {

        "type": "string",

        "title": "Video Owner Channel ID",

        "description": "The channel ID of the channel that uploaded the video"

},

      "playlistId": {

        "type": "string",

        "title": "Playlist ID",

        "description": "The ID that YouTube uses to uniquely identify the playlist that the playlist item is in"

},

      "position": {

        "type": "number",

        "title": "Position",

        "description": "The order in which the item appears in the playlist"

},

      "videoId": {

        "type": "string",

        "title": "Video ID",

        "description": "The ID that YouTube uses to uniquely identify the video in the playlist item"

},

      "startAt": {

        "type": "string",

        "title": "Start At",

        "description": "The time at which the video should start playing"

},

      "endAt": {

        "type": "string",

        "title": "End At",

        "description": "The time at which the video should stop playing"

},

      "note": {

        "type": "string",

        "title": "Note",

        "description": "A user-generated note for this item"

},

      "videoPublishedAt": {

        "type": "string",

        "format": "date-time",

        "title": "Video Published At",

        "description": "The date and time that the video was published"

},

      "privacyStatus": {

        "type": "string",

        "title": "Privacy Status",

        "description": "The playlist item's privacy status"

}

},

    "required": []

},

  "mirrorProperties": {},

  "calculationProperties": {},

  "aggregationProperties": {},

  "relations": {

    "belongs_to": {

      "title": "Belongs to Playlist",

      "target": "youtube_playlist",

      "required": false,

      "many": false

}

}

}
```







