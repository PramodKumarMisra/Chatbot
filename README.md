# Chatbot
It is chatbot for everyone 
This Chatbot run into Conda platform


{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "import nltk"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "import numpy as np"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "import random"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [],
   "source": [
    "import string # to process standard python strings"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [],
   "source": [
    "f=open('C:/Users/R D vaidya/Desktop/DS_Intern/python/Project_Code/chatbot.txt','r',errors = 'ignore')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "raw=f.read()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [],
   "source": [
    "raw=raw.lower()# converts to lowercase"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "[nltk_data] Downloading package punkt to C:\\Users\\R D\n",
      "[nltk_data]     vaidya\\AppData\\Roaming\\nltk_data...\n",
      "[nltk_data]   Package punkt is already up-to-date!\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "nltk.download('punkt') # first-time use only"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "[nltk_data] Downloading package wordnet to C:\\Users\\R D\n",
      "[nltk_data]     vaidya\\AppData\\Roaming\\nltk_data...\n",
      "[nltk_data]   Package wordnet is already up-to-date!\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "nltk.download('wordnet') # first-time use only"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [],
   "source": [
    "sent_tokens = nltk.sent_tokenize(raw)# converts to list of sentences "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [],
   "source": [
    "word_tokens = nltk.word_tokenize(raw)# converts to list of words"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "['a', 'chatbot', '(', 'also', 'known']"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "sent_tokens[:2]\n",
    "['a chatbot (also known as a talkbot, chatterbot, bot, im bot, interactive agent, or artificial conversational entity) is a computer program or an artificial intelligence which conducts a conversation via auditory or textual methods.',\n",
    " 'such programs are often designed to convincingly simulate how a human would behave as a conversational partner, thereby passing the turing test.']\n",
    "word_tokens[:2]\n",
    "['a', 'chatbot', '(', 'also', 'known']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [],
   "source": [
    "lemmer = nltk.stem.WordNetLemmatizer()\n",
    "#WordNet is a semantically-oriented dictionary of English included in NLTK.\n",
    "def LemTokens(tokens):\n",
    "    return [lemmer.lemmatize(token) for token in tokens]\n",
    "remove_punct_dict = dict((ord(punct), None) for punct in string.punctuation)\n",
    "def LemNormalize(text):\n",
    "    return LemTokens(nltk.word_tokenize(text.lower().translate(remove_punct_dict)))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [],
   "source": [
    "GREETING_INPUTS = (\"hello\", \"hi\", \"greetings\", \"sup\", \"what's up\",\"hey\",)\n",
    "GREETING_RESPONSES = [\"hi\", \"hey\", \"*nods*\", \"hi there\", \"hello\", \"I am glad! You are talking to me\"]\n",
    "def greeting(sentence):\n",
    " \n",
    "    for word in sentence.split():\n",
    "        if word.lower() in GREETING_INPUTS:\n",
    "            return random.choice(GREETING_RESPONSES)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.feature_extraction.text import TfidfVectorizer"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.metrics.pairwise import cosine_similarity"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [],
   "source": [
    "def response(user_response):\n",
    "    robo_response=''\n",
    "    sent_tokens.append(user_response)\n",
    "    TfidfVec = TfidfVectorizer(tokenizer=LemNormalize, stop_words='english')\n",
    "    tfidf = TfidfVec.fit_transform(sent_tokens)\n",
    "    vals = cosine_similarity(tfidf[-1], tfidf)\n",
    "    idx=vals.argsort()[0][-2]\n",
    "    flat = vals.flatten()\n",
    "    flat.sort()\n",
    "    req_tfidf = flat[-2]\n",
    "    if(req_tfidf==0):\n",
    "        robo_response=robo_response+\"I am sorry! I don't understand you\"\n",
    "        return robo_response\n",
    "    else:\n",
    "        robo_response = robo_response+sent_tokens[idx]\n",
    "        return robo_response"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "ROBO: My name is Robo. I will answer your queries about Chatbots. If you want to exit, type Bye!\n",
      "hi\n",
      "ROBO: I am glad! You are talking to me\n",
      "yes\n",
      "ROBO: "
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "c:\\python36\\lib\\site-packages\\sklearn\\feature_extraction\\text.py:300: UserWarning: Your stop_words may be inconsistent with your preprocessing. Tokenizing the stop words generated tokens ['ha', 'le', 'u', 'wa'] not in stop_words.\n",
      "  'stop_words.' % sorted(inconsistent))\n"
     ]
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "I am sorry! I don't understand you\n",
      "how are you\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "c:\\python36\\lib\\site-packages\\sklearn\\feature_extraction\\text.py:300: UserWarning: Your stop_words may be inconsistent with your preprocessing. Tokenizing the stop words generated tokens ['ha', 'le', 'u', 'wa'] not in stop_words.\n",
      "  'stop_words.' % sorted(inconsistent))\n"
     ]
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "ROBO: I am sorry! I don't understand you\n",
      "what is your name\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "c:\\python36\\lib\\site-packages\\sklearn\\feature_extraction\\text.py:300: UserWarning: Your stop_words may be inconsistent with your preprocessing. Tokenizing the stop words generated tokens ['ha', 'le', 'u', 'wa'] not in stop_words.\n",
      "  'stop_words.' % sorted(inconsistent))\n"
     ]
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "ROBO: I am sorry! I don't understand you\n",
      "hi\n",
      "ROBO: *nods*\n",
      "hellow\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "c:\\python36\\lib\\site-packages\\sklearn\\feature_extraction\\text.py:300: UserWarning: Your stop_words may be inconsistent with your preprocessing. Tokenizing the stop words generated tokens ['ha', 'le', 'u', 'wa'] not in stop_words.\n",
      "  'stop_words.' % sorted(inconsistent))\n"
     ]
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "ROBO: I am sorry! I don't understand you\n",
      "thanks\n",
      "ROBO: You are welcome..\n"
     ]
    }
   ],
   "source": [
    "flag=True\n",
    "print(\"ROBO: My name is Robo. I will answer your queries about Chatbots. If you want to exit, type Bye!\")\n",
    "while(flag==True):\n",
    "    user_response = input()\n",
    "    user_response=user_response.lower()\n",
    "    if(user_response!='bye'):\n",
    "        if(user_response=='thanks' or user_response=='thank you' ):\n",
    "            flag=False\n",
    "            print(\"ROBO: You are welcome..\")\n",
    "        else:\n",
    "            if(greeting(user_response)!=None):\n",
    "                print(\"ROBO: \"+greeting(user_response))\n",
    "            else:\n",
    "                print(\"ROBO: \",end=\"\")\n",
    "                print(response(user_response))\n",
    "                sent_tokens.remove(user_response)\n",
    "    else:\n",
    "        flag=False\n",
    "        print(\"ROBO: Bye! take care..\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.2"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
