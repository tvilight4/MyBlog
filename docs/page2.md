# LLM Utilisation- Annotation Tool AI Documentation

## Categorizaton


### Introduction

The codebase is designed to categorize/classify the columns present in the TSV input file into classes according to the existing categories present in the Neurobagel annotation tool, based on the column name and the content of the column. The LLM makes its predictions for a specific input string consisting of the column header and the column contents based on the examples provided to it beforehand in its prompt template. The various tasks carried out by this codebase mainly utilize LangChain, the json library from Python, and the LLM 'Gemma' from Ollama.


### Important Aspects:

#### 1. Choice of LLM
#### 2. Utilising Langchain and a well structured Prompt Template
#### 3. Obtaining the Output in the req format to pass it on to the parser:
        

# 
#

### 1.Choice of LLM:

The choice of LLM was a very crucial aspect as it was necessary to select an LLM that hallucinates the least and has the freedom to be used repeatedly without any limit.

1. The LLMs from hugging face:

Even though many LLMs like Flan-T5 are available for free on Hugging Face, it had an API request limit, and hence a conclusion was made that such an approach is not feasible for something to be sent to production. Therefore, we have been using LLMs from Ollama for the project.

2. Llama2/Llama3:

Even though Llama models appeared to work fine at first, hallucination was detected when tested further. The attached screenshots show the LLM response in the interval of about 10-20 seconds.



![Screenshot from 2024-08-26 14-52-13](https://github.com/user-attachments/assets/c3a5bb2a-790b-41ba-8e62-38f7449dd6b1)



![Screenshot from 2024-08-26 14-52-31](https://github.com/user-attachments/assets/e308d037-b7a0-405d-8401-1358c0d43a96)




3. Gemma Model: 

The Gemma model, also from Ollama, is working fine with no visible hallucinations so far. Further tasks are being carried out using the Gemma model.

However, an open mind has been kept regarding the choice of LLM, and we keep testing new LLMs that we come across because there is always room for improvement in this context.

4. Mistral/Mistral2:

Mistral2 was launched midway through our project, and we did try it out. This model worked better than Gemma for the other project related to the query tool, but the outputs given by Gemma were better than those of the Mistral models for our project. One of the possibilities for this could be that the prompt templates developed by us work most optimally with Gemma but not with Mistral. Since we were getting good results with Gemma, we stuck with it.

### 2. Utilising Langchain and a well structured Prompt Template

Utilizing LangChain’s components like PromptTemplate, ChatOllama, and implementing chains provides a robust framework for developing and deploying our application.

1. PromptTemplate:

Here, the PromptTemplate specifies the input examples for the LLM to base its response on and the form in which the output is expected. It also separately defines the input variables that will be given to the LLM.

![Screenshot from 2024-08-24 18-54-20](https://github.com/user-attachments/assets/021cda30-e331-4334-8ded-193b21358794)


![Screenshot from 2024-08-25 23-17-38](https://github.com/user-attachments/assets/456b421d-fd13-401d-9de0-2d6cc1b47292)



2. Chatollama

ChatOllama from langchain_community.chat_models was used to implement various LLMs from Ollama.


![Screenshot from 2024-08-26 14-55-04](https://github.com/user-attachments/assets/3f522bf0-1e36-42a1-aa37-551abe18959b)




3. The Chain

In LangChain, the concept of chains refers to a sequence or pipeline of operations applied to data. Chains in LangChain are flexible, allowing developers to incorporate various components (like ChatOllama, PromptTemplate, data parsers, etc.) into customized workflows. The chain.invoke(...) function executes each operation in the defined chain sequentially.
 

 ![Screenshot from 2024-08-26 14-56-00](https://github.com/user-attachments/assets/7a203fda-56df-4a03-96d1-5db02dc65a73)




### 3. Obtaining the Output in the req format to pass it on to the parser

This depends on the type of values present in the column, i.e., categorical, defined numeric indices, continuous, etc.

The LLM output is of the form of an 'AI_Model Object,' so it was required to convert it into a string object to further process the LLM output to obtain the required structured output from the code.

Functions like SexLevel(...) and AgeFormat(...) are defined for different types of columns that require extra identification other than the labeling done by the LLM. Additionally, the output has been structured in a way that is required by the parser using the json.dumps() function.

The key-value pairs that are not identified as "Participant_ID", "Session_ID", "Age", or "Sex" are then passed on to the llm_diagnosis_assessment function, where the column is categorized into its designated categories. The required structure to be sent to the parser code is obtained using the Diagnosis_Level and get_assessment_label functions for diagnosis and assessments, respectively.


![Screenshot from 2024-08-26 14-56-16](https://github.com/user-attachments/assets/677d0cfc-ecbb-4c19-9ca4-8d6a99a92c84)




### Basic Workflow:

In the main processing.py script, the llm_invocation function is called for each individual pair of key-value corresponding to a column name and a string of column entries, respectively. The decision-making process followed for every input is as follows:

![Copy of NB drawio](https://github.com/user-attachments/assets/92174fff-4230-40bd-9c35-033e6b5cb732)

