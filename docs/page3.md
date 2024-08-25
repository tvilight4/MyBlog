# Details of codebase and categorization for every category

The project was divided into 2 parts:

1)[Parsing](#parsing)

2)[Categorization](#categorization)


## Parsing

This part was taken care of by my colleuge while I was working on the Categorization.

## Categorization

Key Value pairs corresponding to the column header and a string consisiting of concatenation of column header and colum entries were used as a basis to classify each column.

The prompt template for classifying Participant_ID, Session_id, Age and Sex was developed using input output examples refering to which the llm could predict the category of each key-value pair input.

The prompt template for assessments and diagnosis included description of the respective tools and theprompt included a question for the llm to return a 'yes' or 'no' based on the reasoning if the key-value pairs fit the description.
Based on the 'yes' and 'no' answer the code returned the termurl and orther required information after further processing.

**Diagnosis Levels**

The diagnosis levels were to be returned according to the full forms provided by the neurobagel api for the corresponding acronyms. But Each acronymn consisted of multiple full forms and just fetching the most probable full form would not always have been correct.So instead my colleuge prepared a json file which consisted of acronyms and their corresponding full forms from the existing Nuerobagel API. I converted that JSON file into a dictionary consistinf of key as the acronym and a list of corresping full forms as the value field as I noticed that the retrieval process while using the process dictionary was comparitively faster. So currently once a column is identified as diagnosis all the unique entries present in the column are listed down and then the full forms corresponding to each term is fetched from the dictionary. So the Output is of format:

```
   {
    "TermURL": "nb:Diagnosis",
    "Levels": {
        "MDD": ["Major depressive disorder"," "....],
        "HC": ["healthy control","  "...],
        }
   }

```

The User then needs to select one of the listed full forms from the entire list via a droppable provided by the React UI and then a new updated JSON file is created with the single diagnosis selected by the user.


**Test Code**

One of the things which I learnt during my GSOC journey is the importance of writing tests.

In order to maintain code reliability and to check if individual components are working correctly and to ensure that updating code doesnt hamper the previous functionality we ensured to add test codes for every new fature that we added. 

We used Pytest for writting our tests and mocked the LLM responses as GitHub does not run Ollama on its own and the test codes written by us were used for automating testing by implementing GitHub **Continuos Integration Workflow**.

**FastAPI integration and React UI**

My colleuge worked on the development of the FastAPI backend we went through the entire code using VSCode Liveshare and then Later Worked on the React UI. My colleuge worked on the initial part of the React UI while I was working on the Final parts of the Diagnosis where I had faced some issues related to pydantic validations as The plan of returning a list was not decided earlier so there were a few declarations tha had to be changed. I then worked on developing the droppable and working on the updation of the JSON file according to the selection from the Droppable.