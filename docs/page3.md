# Details of codebase and categorization for every category

The project was divided into 2 parts:

1)[Parsing](#parsing)

2)[Categorization](#categorization)

## Parsing

This part was taken care of by my colleuge while I was working on the Categorization.

## Categorization

Key-value pairs corresponding to the column header and a string consisting of the concatenation of the column header and column entries were used as a basis to classify each column.

The prompt template for classifying Participant_ID, Session_ID, Age, and Sex was developed using input-output examples, referring to which the LLM could predict the category of each key-value pair input.

The prompt template for assessments and diagnosis included descriptions of the respective tools, and the prompt included a question for the LLM to return a 'yes' or 'no' based on the reasoning if the key-value pairs fit the description. Based on the 'yes' and 'no' answer, the code returned the TermURL and other required information after further processing.

![Screenshot from 2024-08-25 23-17-38](https://github.com/user-attachments/assets/c7d3812c-43f0-4598-9fb6-18697ac87ea4)

**Diagnosis Levels**

The diagnosis levels were to be returned according to the full forms provided by the Neurobagel API for the corresponding acronyms. However, each acronym consisted of multiple full forms, and simply fetching the most probable full form wouldn't always have been correct. So instead, my colleague prepared a JSON file consisting of acronyms and their corresponding full forms from the existing Neurobagel API. I converted that JSON file into a dictionary with the acronym as the key and a list of corresponding full forms as the value field, as I noticed that the retrieval process was comparatively faster when using the process dictionary. Currently, once a column is identified as diagnosis, all the unique entries present in the column are listed, and then the full forms corresponding to each term are fetched from the dictionary. The output format is:

```
   {
    "TermURL": "nb:Diagnosis",
    "Levels": {
        "MDD": ["Major depressive disorder"," "....],
        "HC": ["healthy control","  "...],
        }
   }

```

The user then needs to select one of the listed full forms from the entire list via a dropdown provided by the React UI, and a new updated JSON file is created with the single diagnosis selected by the user.

![Screenshot from 2024-08-21 19-40-23](https://github.com/user-attachments/assets/8ffd3edc-1ed3-406a-ac09-a47052443178)


**Test Code**

One of the things I learned during my GSOC journey is the importance of writing tests.

To maintain code reliability, check if individual components are working correctly, and ensure that updating code doesn't hamper previous functionality, we made sure to add test codes for every new feature that we added.

We used Pytest for writing our tests and mocked the LLM responses as GitHub does not run Ollama on its own. The test codes written by us were used for automating testing by implementing GitHub **Continuous Integration Workflow**.

**FastAPI integration and React UI**

My colleague worked on the development of the FastAPI backend. We went through the entire code using VSCode Liveshare and then later worked on the React UI. My colleague worked on the initial part of the React UI while I worked on the final parts of the Diagnosis, where I faced some issues related to Pydantic validations. Since the plan of returning a list was not decided earlier, there were a few declarations that had to be changed. I then worked on developing the dropdown and updating the JSON file according to the selection from the dropdown.