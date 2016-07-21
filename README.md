# FinalProject

1.  

### Collection of data sequences

The collection of data sequences that we choדק is Harry Potter And The Chamber Of Secrets that was wrriten by by J. K. Rowling. This is the second book in Harry Potter's series

2.

### Description of data

This was actually not our first data that we tried to learn. We started this project working on a collection of speeches of Binyamin Netanyahu. We gathered and combined a lot of speeches, but when we tried to learn and train the model, we had difficulties to create sentences from it. We think the reason for that was because the collection of the speeches were no big enough. 
We then chose Harry potter topic because that data is very big and has variety in its content (number of words, number of unique words etc). We think that the model training can be much more efficent in thie context. 

The main challenge in dealing with this topic is the investigation of the words which are not "real" words. Harry Potter's books contain many phrases and words that describe magic, which can be challenging to learn.
In addition, in the past we never studied this kind of sequences analysys so this is also chanllenging for us. 

Data Analysis:
16238 sentences.
7605 unique words

3.

### Preprocessing

#### comment: Inilize variables. Replace all the words which are not in our vocabulary with "Unknown_Token". Append Setence_Start ans Sentence_End to the sentence - it is used becuase we want to "teach" the model which words open sentences and which one end them.

vocabulary_size = 7000

unknown_token = "UNKNOWN_TOKEN"

sentence_start_token = "SENTENCE_START"

sentence_end_token = "SENTENCE_END"

_HIDDEN_DIM = 50
_LEARNING_RATE = 0.005
_NEPOCH = 100




#### comment: Read the data to the memory and tokenize into sentences. 
with open('data/harrypotter.txt', 'rb') as f:

    reader = f.readlines()

    #### comment: Split into sentences

    sentences = itertools.chain(*[nltk.sent_tokenize(x.decode('utf-8').lower()) for x in reader])

    #### comment: Append SENTENCE_START and SENTENCE_END
    sentences = ["%s %s %s" % (sentence_start_token, x, sentence_end_token) for x in sentences]
print ("Parsed %d sentences." % (len(sentences)))

#### comment: Tokenize words in each sentence
tokenized_sentences = [nltk.word_tokenize(sent) for sent in sentences]



#### comment: Count the word freqencies
word_freq = nltk.FreqDist(itertools.chain(*tokenized_sentences))

print ("Found %d unique words tokens." % len(word_freq.items()))



#### comment: Get the most common words (vocabulary size) and build index_to_word and word_to_index vectors
vocab = word_freq.most_common(vocabulary_size - 1)

index_to_word = [x[0] for x in vocab]
index_to_word.append(unknown_token)

word_to_index = dict([(w, i) for i, w in enumerate(index_to_word)])

print ("Using vocabulary size %d." % vocabulary_size)

print ("The least frequent word in our vocabulary is '%s' and appeared %d times." % (vocab[-1][0], vocab[-1][1]))

#### comment: Replace all words not in our vocabulary with the unknown token
for i, sent in enumerate(tokenized_sentences):

    tokenized_sentences[i] = [w if w in word_to_index else unknown_token for w in sent]

#### comment: Create the training data
X_train = np.asarray([[word_to_index[w] for w in sent[:-1]] for sent in tokenized_sentences])

y_train = np.asarray([[word_to_index[w] for w in sent[1:]] for sent in tokenized_sentences])

#### comment: Init the model with random values for U, S, V. This function is attached to the project code.

model = RNNTheano(vocabulary_size, hidden_dim=_HIDDEN_DIM)

#### comment: Start Training the model
train_with_sgd(model, X_train, y_train, nepoch=_NEPOCH, learning_rate=_LEARNING_RATE)



