# LLM Utilisation- Annotation Tool AI Documentation

## Categorizaton


### Introduction

The codebase is created to categorize/classify the columns present in the tsv input file into classes according to the already existing categories present in the neurobagel annotation tool, based on the column name and the content of the column. The LLM makes it predictions for a specific input string consisting of the column header and the column contents based on the examples provided to it beforehand in its promptemplate.
The various tasks carried out by this codebase mainly utilise Langchain,
the json library from python and the LLM 'Gemma' from Ollama.


### Important Aspects:

#### 1. Choice of LLM
#### 2. Utilising Langchain and a well structured Prompt Template
#### 3. Obtaining the Output in the req format to pass it on to the parser:
        

# 
#

### 1.Choice of LLM:

The choice of LLM was a very crucial aspect as it was necessary to select an LLM that hallucinates the least and has a freedom to be used enumerably without any limit.

1. The LLMs from hugging face:
Even though we get to use  many LLMs like Flant-T5 for free on Hugging face it had an API request Limit and hence a conclusion was made that such an approach is not feasible for something to be sent to production.Thereafter we have been using LLMs from Ollama for the project.

2. Llama2/Llama3:
Even though Llama models appeared to work fine at first, hallucination was detected when tested further 
The attatched Screenshots show the LLM response in the interval of abt 10-20 seconds
![Hallucination](https://hackmd.io/_uploads/Bk9wR-ILR.png)

![Hallucination](https://hackmd.io/_uploads/SkMjCWIUR.png)


3. Gemma Model: 
The gemma model also from Ollama is working  fine with no visible hallucinations until now. And further tasks are being carried out using the Gemma model.

However an open mind has been kept in context of the choice of LLM and we keep on testing new LLMs that we come across because there can always be a scope of improvement in this context.

4. Mistral/Mistral2:
Mistral2 was launched midway during our project and we did try it out.
This model did work better than gemma for the other project related to the query tool but the outputs given by gemma were better than that of the mistral models for our project, one of the possiblities for this nature  can be that the prompt templates developed by us work in most optimum manner with gemma but not with mistral as we were getting good results with gemma we  sticked with it.

### 2. Utilising Langchain and a well structured Prompt Template

Utilizing LangChain’s components like PromptTemplate, ChatOllama  and implementing chains provides a robust framework for developing and deploying  our application.

1. PromptTemplate:

Here the PromptTemplate specifies the input form examples for the LLM to base the response on and the form in which the output is expected.
It also seperately defines the input variables that will be given to the LLM.


![image](https://hackmd.io/_uploads/S1UAGGUUA.png)

2. Chatollama

Chatollama from langchain_community.chat_models was used to implement various LLMs from Ollama.

![image](https://hackmd.io/_uploads/SkOUEzIIA.png)

3. The Chain
In LangChain, the concept of chains refers to a sequence or pipeline of operations applied to data. Chains in LangChain are flexible, allowing developers to incorporate various components (like ChatOllama, PromptTemplate, data parsers, etc.) into customized workflows. The ' chain.invoke(...)' function executes each operation in the defined chain sequentially.
 

 
 ![image](https://hackmd.io/_uploads/ByIn4MILC.png)

### 3. Obtaining the Output in the req format to pass it on to the parser

This depends on the type of values present in the column ie categorical, defined numeric indices, Continuous etc

The LLM output is of the form of 'AI_Model Object' so it was required to convert it into string object in order to further process the LLM output to obtain the required structered output from the code.

So funtions (SexLevel(...) and AgeFormat(....) ) are defined for different types of columns which require extra identification other than the labelling done by the LLM also the output has been structured in a way that is required by the parser using the 'json.dumps()' function.

The key value pairs which are not identified as "Participant_ID" "Session_ID" "Age" "Sex" are then passed on to the llm_diagnosis_assessment function,where the column is categorized in its designated categories and the required structure to be sent to the parser code is obtained using the Diagnosis_Level, get_assessment_label function for diagnosis and assessments respectively.

![image](https://hackmd.io/_uploads/BkUCnQMsR.png)


### Basic Workflow:

In the main processing.py script the llm_invocation function is called for each individual pair of key value corresponding to a the column name and a string of column entries respectively. The decision making process which is followed for every input is as follows:

![Copy of NB.drawio](https://hackmd.io/_uploads/Hk5poXfiA.png)
