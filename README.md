ko_novel_generator
Deep learning model writing korean novel
Deep learning model to create Korean novels
Lablup - A project that participated in a Just model it event.

Black Oribana Team


It is a project aimed at learning ai based on websites and writing novels, and using them to write relay novels with ARTIFICIAL INTELLIGENCE. And we want to build a deep learning model that writes Korean novels, which is a key part of this project.

1. Human participation - selection of learning data and creation of results
People will participate in the process of selecting a novel to use as data, and then participate in the number of keywords and sentence lengths that AI will choose when writing a novel, using the final AI.
When multiple people present different options, the final choice is decided by people's votes.
This is a project that involves people picking their own data and participating in all the processes of creating results using learned ARTIFICIAL INTELLIGENCE to complete a novel together.

2. Relay novel writing method - human-> ai turn method
Like a relay novel, it is a way to write a part of a novel, then write a novel with a new keyword, and repeat it to gradually complete the novel.
We also want to learn new novels as we write them, allowing AI to digest novels in different styles.

We don't aim to create the best performing model.
We aim to be able to experience and enjoy ARTIFICIAL INTELLIGENCE by humans who do not know artificial intelligence.

What an occasion you made
The term artificial intelligence has long been familiar to humans, but it's limited where it can be used directly. Artificial intelligence technology is a melting force in everyday life, but it's hard to recognize it. https://experiments.withgoogle.com/ like The New Year, but you can only use learned AI. Of course, it's easy to learn AI directly on your own taste buds.

With the advent of various deep learning frameworks, artificial intelligence technology is in an era where it is easier than ever for humans to use. But it's also hard to use it yourself, as long as you're not a developer. Even the public who had no idea of development thought that artificial intelligence should be created because they didn't see a place to learn and use it.

Instead of enjoying and ending up enjoying it for a while, I wanted to be able to continue participating and create and experience ARTIFICIAL INTELLIGENCE, and I was planning a participating novel AI writing project, which I see as appropriate in that among various topics, novel writing AI can continue to engage people.

How you made it
1. Currently applied model
The model currently in GRU(Gated Recurrent Neural network) model. It's a very simple principle by typing a letter by letter and predicting the next, and we want to build a better model in the future to improve performance with a temporary model.

lstm

We want to use a variety of pre-built NN models to understand structure, test performance, and create our own custom models.

2. Data
We crawled over 1,000 novels on the Internet and used them as learning data.

3. Results
The learning progresses normally and begins to replicate the novel as it is. As I learned different novels, I expected to learn a variety of stylistics, but I started to adapt to the stylistics of the novels I was learning right now. We haven't created a model that responds appropriately to the sentences we type, and we want to find a way to do that while studying the gpt-2 model of Open ai.

File configuration
data/ - A folder that contains the novel data to be trained
generate/ - The folder where the generated fiction data is stored
save/ - Folder that stores trained model parameter values
vocab/ - Folder that stores generated novel dictionary data
data_loader - Preprocesses data and generates it in batch units
generate.py Create a novel based on a trained model
model.py - Pytorch GRU Model Code
multi_train.py - used when you want to learn multiple novels in a row. train.pyon the data.
opt.py - An option that saves important variables related to overall deep learning learning
train.py - Learning Fiction
vocab_generator.py Pre-generated based on the novel
How to use it
How to use it is very simple. Except for the pretreatment of the novel. The preprocessing code for the novel has not yet been uploaded, so you have to do it yourself.

Create mkdir save generate save vocab
Preprocessing a novel A delimiter must be inserted </s>between every sentence and sentence. If you've felt hassled by this point, you're right. It's time to start that infamous regular expression.
We recommend the following sites: https://www.regexpal.com/
Put novel text in save folder
save you want in the save folder.
Word python vocab_generator.py
Start python multi_train.py
python generate.py --epoch [int] --prime [str] --len [int] --resume [bool]
epoch - Decides how many of the trained models to use.
prime - Set the input phrase. When you type a phrase, it generates the appropriate phrases.
len the sentence you want to create.
resume resume - True, it is subsequently generated by a phrase that you previously created.
More details on the process of creating a novel
Input - python generate --epoch 20 --prime ""안녕 난 오리라고 해."" --len 10
"난 거위라 해!"
input - python generate --epoch 20 --prime ""거위?"" --len 10 --resume True
Output - ""맞아 난 거위야""
The resume does not need to be cared for separately because default is False for the first input. If you want to continue with the phrase before you create the second --resume True Here, a total of four sentences were followed.

"Hi I'm a duck."
"I'm a goose!"
"Goose?"
"That's right, I'm a goose"
The generated output generateis result.txtin the generate folder.

API(Flask)
ko_novel_generator API server (python Flask) for use on the web

API LIST
put_Human_txt (get, post)
Enter the user's text to further learn, followed by generate text
paramters
contents_id The id of the contents that the user entered on the web
is_first : Original creation, new learning if True, followed by False
result
Save generated text to DB, no return value
How to use the API
depengency
flask / flask_restful / flask_script / flask_migrate
sqlalchemy
marshmallow
pytorch
execute
config.py
SQLALCHEMY_DATABASE_URIDB information in the database
run.py
EnterYOUR_LOCAL_HOSTin host (1000000000000000000000000000000000000
Other models I've tested
l2w(Learning to Write) - https://github.com/ari-holtzman/l2w
word 단위로 학습the GRU method to train in GRU units. Adaptive softmaxapply An Applitive Beam searchsoftmax and use decoder to create results by scoring through four discriminatorsto create results, creating sentences that are not repetitive, contextual, and conform to existing novel styles.

4 discriminators

Repetition
Entailment
Relevance
Lexical Style
You need to learn the Entertainment discriminator using SNLI and MultiNLI, which is English data, and we have not found any Korean data for similar purposes, so we have abandoned the use of the model. Entailment discriminator We tested the rest except for discriminator application in Beam search.

Seq2seq attention - https://github.com/IBM/pytorch-seq2seq
A model with attention applied to seq2seqwhich attentionby using sentences. It is suitable for translation, because memory is passed on a sentence-to-sentence basis, even though NN is used.

The seq2seq model does not convey the hidden state while continuing to learn, so it is not suitable for continuous writing, such as fiction. To make it suitable for fiction, the hidden state must be modified to pass between seq2seq models. seq2seq attention wrote a proper novel for pre-ed sentences, but the first sentences contained phrases that did not know their meaning.

Transformer - https://github.com/JayParks/transformer
transformer

This is a transformer that has helped Google create transformertranslator. RNNthe CNNand CNN approaches, and we've acquired the best of both worlds to create a model. Self-attentionthan traditional attentioncalled Self-attention. Seq2Seqit is suitable for making decisions on the fly, such as translators and review analysis, rather than long-term support, such as fiction, because there is no long-term support.

As a result, learning progresses, but it produces nonsensical results in the step of generating the result. We need to add long-term support, which is expected to work normally.

What parts do you want to do?
Open AI GPT-2 - https://github.com/openai/gpt-2
It is the GPT-2 model which has become a hot point with the recent release of OpenAI. The writing performance is so good that we're concerned that it will be mis-utilized like fake news, so we're only releasing a privately determined and simple model.

By studying this model, we want to create an ARTIFICIAL INTELLIGENCE model where relay fiction is as natural as humans send and receive.