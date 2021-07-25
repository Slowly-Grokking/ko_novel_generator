This is a quick and dirty translate of https://github.com/IllgamhoDuck/ko_novel_generator
I run this with python3.8, but have not tested other versions or dug into any requirements. 

I have only used the train, multi_train, vocab_generator, and generate.py as described in the howto below.


# en_novel_generator
### Deep learning model writing korean novel
### Deep learning model to generate Korean novels

Lablup - [Just model it event](https://events.backend.ai/just-model-it/) This is a project that I participated in.


### Black Oribanana Team


Based on the website, it is a project that aims to learn artificial intelligence that people can participate in and write novels, and to use this to write relay novels with artificial intelligence. And we want to build a deep learning model for writing Korean novels, which is a key part of this project.

#### 1. Human participation - selection of learning data and creation of results

> People participate in the process of selecting novels to use as data, and finally utilize the artificial intelligence created to select keywords and sentence lengths to be selected when the artificial intelligence writes a novel.   
> Multiple people present various options, and the final choice is decided by people's votes.  
> Through this, it is a project in which people complete a novel together by participating in all processes, from the process of selecting data to the process of using the learned artificial intelligence to produce results.

#### 2. Relay novel writing method - Human -> AI turn method

> Like a relay novel, you write a part of the novel and then write the novel with new keywords, repeating it, and gradually completing the novel.  
> In addition, we want to learn new novels in the middle of writing novels so that AI can digest various styles of novels.  

We do not aim to create the best performing model.  
It aims to enable humans who do not know artificial intelligence to experience and enjoy artificial intelligence.  

## How did you make it?
Although the term artificial intelligence has long been familiar to humans, the places where artificial intelligence can be directly applied are limited. Artificial intelligence technology is embedded in our daily life, but it is difficult to recognize it. There are representative AI-enabled websites, such as https://experiments.withgoogle.com/, but they can only use learned AI. Of course, it is easy to learn artificial intelligence to your own taste.

With the advent of various deep learning frameworks, artificial intelligence technology is now in an era where it is easier than ever for humans to use it. However, unless you are a developer, it is also difficult to use it directly. Since there was no place where even ordinary people who had no knowledge of development could directly learn and utilize artificial intelligence, I thought of making it.

I wanted to make it possible to create and experience artificial intelligence directly by participating continuously, rather than enjoying it for a while. Among various topics, I thought that artificial intelligence for novel writing was appropriate in that it allowed people to continuously participate in the AI ​​writing project of a participatory novel. has been planned.

## How to make it
#### 1. Currently applied model
The model currently in use is the 'Gated Recurrent Neural Network' (GRU) model. It is a very simple principle by receiving Hangul input one character at a time and predicting the next Hangul. This is a model that is used temporarily, and we plan to build a better model in the future to increase the performance.

![lstm](https://github.com/IllgamhoDuck/DLND/blob/master/intro-to-rnns/assets/charRNN%400.5x.png)

We use various pre-made RNN models to understand the structure and test the performance, and then we want to create our own custom model.

#### 2. Data
About 1,000 novels that exist on the Internet were crawled and used as learning data.

#### 3. Results
Learning proceeds normally and begins to replicate the novel as-is. If you study several novels, you expect to learn various styles, but you start to adapt to the style of the novel you are learning right now. We have not yet created a model that responds appropriately to input sentences, and we would like to find a way to do this by studying the gpt-2 model of Open ai.

## file configuration
1. `data/` - folder to put novel data to be trained (your .txts)
2. `generate/` - folder where the generated novel data is saved
3. `save/` - Folder to save the learned model parameter values (.pths)
4. `vocab/` - Folder to save the created novel dictionary data
5. `data_loader` - After preprocessing data, it is created in batch units.
6. `generate.py` - generate a novel based on the trained model
7. `model.py` - Pytorch GRU model code
8. `multi_train.py` - Used to train multiple novels in succession. Based on `train.py`.
9. `opt.py` - Abbreviation for Option, which stores important variables related to overall deep learning learning
10. `train.py` - novel learning
11. `vocab_generator.py` - create a dictionary based on a novel
  

## How to use
How to use is very simple. Except for the novel preprocessing process. The novel preprocessing code has not been uploaded yet, so you have to do it yourself.

1. Create a folder
`mkdir save generate save vocab`
2. Novel Pretreatment
You must put a delimiter `` between every sentence and sentence in the novel. If you're having trouble with this, you're right. It's time to start that infamous regular expression.

- The following sites are recommended. https://www.regexpal.com/
- https://mikedombrowski.com/2017/04/regex-sentence-splitter/

3. Put the novel text in the data folder   (orignal source said to put in save folder, unless i'm missing something it should be in data)
- Put the desired novel in the `data` folder. (orignal source said to put in save folder, unless i'm missing something it should be in data)
4. Create a word dictionary
`python vocab_generator.py`
5. Start learning
`python multi_train.py`
6. Fiction Generation
`python generate.py --epoch [int] --prime [str] --len [int] --resume [bool]`
- `epoch` - Determines which epoch model to use among the trained models.
- `prime` - Set the input text. If you enter a phrase, the appropriate phrase will be generated in succession.
- `len` - Specifies the length of the sentence to be generated.
- `resume` - If set to True, it will be generated after the previously generated phrase.

#### A more detailed description of the novel creation process
1. Input - `python generate --epoch 20 --prime ""Hello I'm a duck"" --len 10`
2. Output - `"I'm a goose!"`
3. Input - `python generate --epoch 20 --prime ""goose?"" --len 10 --resume True`
4. Output - `""Yes I'm a goose""`

For resume, you do not need to worry about it because the default is False for the first input. If you want to continue with the previous phrase when creating the second phrase, set `--resume True`. Here, a total of four sentences were written in succession.

- "Hello, I'm a duck."
- "I'm a goose!"
- "goose?"
- "Yeah, I'm a goose"

The generated output text file is stored in the `result.txt` in the `generate` folder.



## API (Flask)
API server for using ko_novel_generator in web (python Flask)

#### API LIST
- `put_Human_txt (get, post)`  
After further learning by inputting the user's text, the text is then generated.  
  ###### parameters
  1. `contents_id` : ID of the contents entered by the user on the web
  2. `is_first` : Whether to write first, if True, learn new, if False, learn continuously    
  ###### result
  Save the created text in DB, no return value
  
#### How to use the API
######
  - `flask` ​​/ `flask_restful` / `flask_script` / `flask_migrate`
  - `sqlalchemy`
  - `marshmallow`
  - `pytorch`  
###### execute
1. `config.py`  
  Enter DB information in `SQLALCHEMY_DATABASE_URI`
2. `run.py`  
  Execute after entering host information in host(`YOUR_LOCAL_HOST`)
  
  

## Other models tested
#### l2w(Learning to Write) - https://github.com/ari-holtzman/l2w
> It is a `GRU` model that learns `word unit`. When ‘Adaptive softmax’ is applied and a decoder is used to generate a result, a score is scored through **4 discriminators** in ‘Beam search’ to create a sentence that is not repetitive and that matches the context and existing novel style. It is an outgoing model.

**4 discriminators**
- `Repetition`
- `Entailment`
- `Relevance`
- `Lexical Style`

 **Entailment using [SNLI](https://nlp.stanford.edu/projects/snli/) and [MultiNLI](https://www.nyu.edu/projects/bowman/multinli/)** Discriminator` should be learned, but since it is English data, we gave up using the model because we could not find Korean data for a similar purpose. Except for applying the discriminator in Beam search, I tested the rest.

#### Seq2seq attention - https://github.com/IBM/pytorch-seq2seq
> This is a model in which `attention` is applied to `seq2seq`, which receives a sentence and predicts and outputs the sentence. Although it uses RNNs, it is suitable for tasks such as translation because memories are passed on a sentence-by-sentence basis.

The seq2seq model is not suitable for continuous writing tasks such as novels because it does not transmit hidden state (memory) while continuing to learn. To make it into a form suitable for novels, we need to modify the hidden state to pass between the seq2seq models. seq2seq attention wrote an appropriate novel for pre-learned sentences, but only presented phrases with unknown meaning in sentences they saw for the first time.

#### Transformer - https://github.com/JayParks/transformer
![transformer](https://cdn-images-1.medium.com/max/1200/0*plM2xPXX4TppwUeQ.)
> This is 'transformer', which has helped Google a lot in making a high-performing translator. It is not the 'RNN' method and the 'CNN' method, and the model was created by acquiring the excellent advantages of both. This algorithm is different from the existing `attention` called `Self-attention`. Like 'Seq2Seq', long-term memory does not exist, so it is suitable for tasks that require instantaneous judgment, such as translators and review analysis, rather than tasks that require long-term memory such as novels.

As a result of application, learning proceeds, but gives meaningless results in the stage of generating results. It is expected that long-term memory will be added to function normally.

## What part do you want to do more?
#### Open AI GPT-2 - https://github.com/openai/gpt-2
It is the GPT-2 model, which has become a hot topic recently as OpenAI was released. Because of its excellent writing performance, it was decided to be kept private and only a simple model was released for fear of being misused like fake news.

By studying this model, we want to create an artificial intelligence model that is as natural as writing a relay novel as humans give and receive.
