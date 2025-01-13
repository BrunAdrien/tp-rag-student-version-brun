# TP - Retrieval Augmented Generation

Dans ce TP, vous allez mettre mettre la technique du _Retrieval Augmented Generation (RAG)_ pour répondre à des questions en utilisant une base de connaissance externe.

En sortie de ce module, vous serez capables de :
- Calculer l'_embedding_ d'un texte, c'est à dire sa représentation sémantique. En fonction du modèle choisi pour calculer les embeddings, ces derniers peuvent même être multilangues !
- Rechercher des documents de manière plus pertinente grâce à la recherchs sémantique ;
- Mettre en oeuvre un système de Question / Réponse en utilisant la méthologie _Retrieval Augmented Generation (RAG)_
- Mettre en place un système multi agents pour répondre à des questions complexes grâce à LangGraph.

**Intruction pour le TP**
- Réaliser le TP sous la forme d'un notebook exécutable sous Google Colab
- Inclure les dépendances nécessaires : fichier `requirements.txt` ou installation des dépendances directement dans le notebook
- La qualité de votre code sera prise en compte dans la notation
- Faire en sorte que les données soient directement accessibles par le notebook sur google colab. Pour ce faire, vous pouvez cloner votre dépôt contenant les données grâce aux lignes ci-dessous

```
import os

# Vérifie si le code est exécuté sur Google Colab
if 'COLAB_GPU' in os.environ:
    # Commandes à exécuter uniquement sur Google Colab
    !git clone https://github.com/GITHUB_ACCOUNT/tp-rag-student-version.git
    %cd tp-rag
    !pip install -r requirements.txt
else:
    # Commandes à exécuter si ce n'est pas sur Google Colab
    print("Pas sur Google Colab, ces commandes ne seront pas exécutées.")
```

## Etape 1. - Indexation des documents

La première étape consiste à indexer un corpus documentaire dans un _vector store_. Les documents doivent être découpés en paragraphes. Ensuite, sur chaque paragraphe on calcule l'embedding son embedding que l'on stocke dans le _vector store_. Des pré-traitements sur les documents peuvent être réalisés avant l'indexation.

**Exercice 1 : indexation**

Le corpus de documents est disponible dans le dossier `data`.

A partir du tutoriel https://python.langchain.com/docs/tutorials/rag/, mettre en place l'indexation des documents en respectant les consignes ci-dessous :
- _ChromaDB_ comme vector store.
- https://huggingface.co/intfloat/multilingual-e5-base comme mododèle d'embeddings.


**Exercice 2 : interrogation**

- Créer une fonction permettant d'interroger la base à partir d'une requête `query` et qui retourne la liste des documents répondant à la requête ainsi que les scores associés.
- Tester la fonction sur la requête de votre choix.

## Etape 2. - RAG

Dans cette partie, vous allez mettre en oeuvre un chatbot en utilisant un _Large Language Model_ exploiter les patagraphes trouvés dans la BDD vectorielle pour synthétiser les informations et construire une réponse adequate.

Nous pourrions utiliser GPT3.5 ou GPT4 mais pour des raisons de coût (il faut un abonnement payant à OpenAi), nous allons utiliser un 'petit' modèle open source [Qwen2.5](https://github.com/QwenLM/Qwen2.5).

**Exercice 3. : prompt template**

- Créer un template de prompt (_PromptTemplate_) comportant les éléments nécessaires pour que le système réponde à la question de l'utilisateur en utilisant le contexte récupéré de la base de connaissance. Vous spécifierez _a minima_ deux variables : `context` et `question`. 

**Exercice 4. : chaîne RAG**

- A partir de cet exercice, nous allons utiliser le serveur d'inférence `Olama`que nous allons installer dans Google Colab. Pour ce faire, exécuter les commandes suivantes

```
%xterm
curl https://ollama.ai/install.sh | sh
ollama serve &
ollama run qwen2.5:14b
```

Puis pour utiliser le LLM, inspirez-vous de cet exemple : 

```
llm = ChatOllama(
    model="qwen2.5:14b",
    temperature=0,
)
```
https://python.langchain.com/docs/integrations/chat/ollama/

- Créer une fonction prenant en paramètre la base de connaissance, le template de prompt et la question de l'utilisateur et retournant la réponse à la question.

- Tester la fonction sur la question de votre choix.

**Exercice 5. : mémoire**

- Ajouter une fonctionnalité de mémoire des conversations précédente à votre chaîne RAG en utilisant le tutorial https://python.langchain.com/docs/tutorials/qa_chat_history/ et en l'adaptant pour utiliser le modèle servi par Ollama.

**Exercice 6 : nouveaux outils**

- Créer un nouvel outil pour permettre de récupérer un document complet pour en faire un résumé. 
- Tester avec l'article de votre choix.

## Etape 3 - IHM

- A l'aide de [gradio](https://www.gradio.app/guides/quickstart), mettre en place une IHM permettant d'interroger le chatbot.
