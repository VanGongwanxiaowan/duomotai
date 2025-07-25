```
## 1.分词
import tensorflow as tf
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow import keras
sentences = [
    'I love my dog',
    'I love my cat',
    'You love my dog!',
    'Do you think my dog is amazing?'
]
tokenizer = Tokenizer(num_words=100)
tokenizer.fit_on_texts(sentences)
word_index = tokenizer.word_index
print(word_index)
sequences = tokenizer.texts_to_sequences(sentences)
# 每个句子的词序列
# 将文本转换成为数据的
## oov_token将其设置为代替语料库当中无法识别的内容
tokenizer = Tokenizer(num_words=100,oov_token='<OOV>')
tokenizer.fit_on_texts(sentences)
sequences = tokenizer.texts_to_sequences(sentences)
print(sequences)
# 借助不规则的张量
# ragged tensor

from tensorflow.keras.preprocessing.sequence import pad_sequences
padded = pad_sequences(sequences,padding='post')
print(padded)
#前面补充0，确保所有的句子序列的长度都是一致的
#maxlen=5
#truncation="post"
# 打造识别情感的模型的
[
    {"article_link":"https://politics.theonion.com/trump-to-visit-michigan-to-promote-his-new-book-1850686000"},
    {"article_link":"https://www.nytimes.com/2025/07/24/us/politics/trump-michigan-book-tour.html"},
    {"article_link":"https://www.washingtonpost.com/politics/2025/07/24/trump-michigan-book-tour-2024-election/"}
]



import json
with open("sarcasm.json",'r') as f:
    datastore = json.load(f)
sentences = []
labels = []
urls=[]
for item in datastore:
    sentences.append(item['headline'])
    labels.append(item['is_sarcastic'])
    urls.append(item['article_link'])
print(sentences[0])
print(labels[0])
print(urls[0])
# 分词
tokenizer = Tokenizer(num_words=10000,oov_token='<OOV>')
tokenizer.fit_on_texts(sentences)
word_index = tokenizer.word_index
print(word_index)

model = tf.keras.Sequential([
    tf.keras.layers.Embedding(10000,16,input_length=10),
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(24,activation='relu'),
    tf.keras.layers.Dense(1,activation='sigmoid')
])
model.compile(loss='binary_crossentropy',optimizer='adam',metrics=['accuracy'])
model.summary()
model.fit(padded,labels,epochs=10)
#bit.ly/tfw-sarcembed
sentence = [
    "granny starting to fear spiders in the garden might be real",
    "game of thrones season finale showing this sunday night"
]
sequences = tokenizer.texts_to_sequences(sentence)
padded = pad_sequences(sequences,maxlen=10,padding='post',truncating='post')
print(model.predict(padded))
# 使用LSTM
model = tf.keras.Sequential([
    tf.keras.layers.Embedding(10000,16,input_length=10),
])
# 用循环神经网络进行机器学习

# 生产文本，单词顺序很重要的
# 非序列数据转换成为序列数据的
# LSTM
#cellstate
model = tf.keras.Sequential([
    tf.keras.layers.Embedding(10000,16,input_length=10),
    tf.keras.layers.LSTM(150,return_sequences=True),
    tf.keras.layers.LSTM(10),
    tf.keras.layers.Dense(1,activation='sigmoid')
])
model.compile(loss='binary_crossentropy',optimizer='adam',metrics=['accuracy'])
model.summary()
model = tf.keras.Sequential([
    tf.keras.layers.Embedding(tokenizer.vocab_size,16,input_length=10),
    tf.keras.layers.Bidirectional(tf.keras.layers.LSTM(150,return_sequences=True)),
    tf.keras.layers.Bidirectional(tf.keras.layers.LSTM(10)),
    tf.keras.layers.Dense(1,activation='sigmoid')
])
model.compile(loss='binary_crossentropy',optimizer='adam',metrics=['accuracy'])
model.summary()
model.fit(padded,labels,epochs=10)
# 使用双向LSTM
model = tf.keras.Sequential([
])
# 打造一个会写诗的ai
tokenizer = Tokenizer()
data="in the twon of Athu one Jeremy Lanigan \n Matered away..."
corpus = data.lower().split("\n")
tokenizer.fit_on_texts(corpus)
total_words = len(toeknizer.word_index)+1
# 生成文本的时候这个实例不需要验证集
# 利用所有的字节来找出单词在哪里出现以及如何出现的模式的
# 从完整语料库的开始，填充子句
# 为此我们需要一个[0]token
ys = tf.keras.utils.to_categorical(labels,num_classes=total_words)
model = Sequential()
modal.add(Embedding(total_words,100,input_length = max_sequence_len-1))
model.add(Bidrectional(LSTM(150)))
model.add(Dense(total_words,activation='softmax'))
adam = Adam(lr=0.01)
model.compile(adam,loss='categorical_crossentropy',metrics=['accuracy'])
history = model.fit(xs,ys,epoches= 100,verbose=1)
seed_text='I MADE A POETRY MACHINE'
next_words = 100
for _ in range(next_ords):
    token_list=tokenizer.texts_to_sequences([seed_text])[0]
    token_list=pad_sequences([token_list],maxlen=max_sequence_len-1,padding='pre')
    predicted = model.predict_class(token_list,verbose=0)
    output_word=""
    for word,index in tokenizer.word_index.items():
        if index == predicted:
            output_word = word
            break
    seed_text += " "+output_word
print(seed_text)
# 使用LSTM生成文本
model = tf.keras.Sequential([
    tf.keras.layers.Embedding(total_words,100,input_length=max_sequence_len-1),
])
```
