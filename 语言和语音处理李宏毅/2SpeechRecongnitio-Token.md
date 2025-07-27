

语音识别中，声音和文字要表示成什么样子呢？语音是一个序列向量，长度为T，维度为d。文字是一个序列token，长度为N，内容id为它在词表中的序号。通常T是要远远大于N的。

<img width="661" height="465" alt="image" src="https://github.com/user-attachments/assets/5149d071-637b-479f-9949-05ca8437d5bd" />


我们会把文字想成是一个个的token。这个token可以是发音的基本单位Phoneme。比如说WAHN PAHNCH MAEN。这其中每一个符号对应到一种声音。接着我们要有一个词表Lexicon来记录单词与Phonemes的映射。通过这个词表我们就能转成单词。使用Phonemes作为基本单元的缺点是，你需要一个额外的词表来记录映射。这个词表的构建要由专家来构建。而且它对同音字不大友好。第二种是书写的基本单位Grapheme。比如英文的最小书写单位就是26个英文字母。再加上标点符号和其它特殊符号，词表也很小。中文可能才不到1万种字，一拳超人才不到4个字符。使用最小书写单位的好处是词表很小，不需要考虑OOV的问题，也不需要额外的Lexicon。但使用Grapheme的难点在，它对模型的能力要求比较高。模型必须要自己学会一个词汇中有哪些Grapheme。

比Grapheme更大的单位就是Word，也就是我们一般意义上的词。对英文而言，英文的词是非常明确的，用空格分开。英文的词也非常地多，有十万个以上。但对中文而言，要讨论词汇这件事就比较困难。因为中文并没有用空白来进行词的分割。所以一个句子中有几个词汇，我们很难说清楚。用词汇来当作一个语音辨识的单位，往往不是一个非常好的选择。因为对某些语言来说，它词汇的数量可能会非常地大。中文有这个问题，其它的语言也有这个问题。比如土耳其文的词汇是没有办法穷举的，它永远可以创造出新的词汇。

<img width="641" height="499" alt="image" src="https://github.com/user-attachments/assets/a231b413-df2f-4992-b68c-e8631de719eb" />

<img width="644" height="305" alt="image" src="https://github.com/user-attachments/assets/6fbcb567-1738-442a-8bf5-eef3ae520859" />

<img width="648" height="269" alt="image" src="https://github.com/user-attachments/assets/c263f41b-634e-4de6-bd05-b84378611e8b" />


我们也可以用可以传达意思的最小单位Morpheme来作为最小单位。它是一个比词要小，但是比Grapheme要大的单位。比如英文中，unbreakable可以被分成un-break-able。但是我们要如何知道一个语言中表示意思的最小单位呢？通常我们会用语言学知识和统计去确认经常出现的模式。这些模式很大可能就是Morpheme。还有一种更小的粒度是使用Bytes当作单位。我们直接把语言的字符转码成二进制的UTF-8。这种方法的好处是能统一融合各种不同语言的符号。而且token的大小不会超过256。也就是说词表可以很小很小。

<img width="688" height="503" alt="image" src="https://github.com/user-attachments/assets/00dcd410-63e2-49d2-ae10-d89e3e940b2e" />

<img width="681" height="499" alt="image" src="https://github.com/user-attachments/assets/0048e559-6d97-40a9-a924-16f8779a87ea" />

我们要怎样选择最小单元呢？统计了2019各大期刊顶会年超过100篇文章发现，语音识别大家最喜欢用的是grapheme，其次是phoneme，大家用的最少的是word。

语音识别还有很多有趣的论文。比如输入声音讯号，直接输出词嵌入。再比如输入英文语音，直接输出中文语音。还有的把语音识别和意图识别结合在一起，或者是把语音识别和槽位填充结合在一起。

传统的语音识别是怎么做的呢？一段声音进来，我们会取一个窗口，它的长度通常为25ms。这个窗口内的声音信号会用一个向量来表示，被描述为frame。我们可以从中采样400个点（16kHz），但这种方式不大常用。过去我们会用MFCC来把它变成一个39维的向量。但近年来非常流行的是用filter bank output方法转换为80维的向量。接下来我们会让窗口往右移动10ms，再用同样的方法取向量。窗口之间是有重叠的。1秒钟的声音讯号，我们会有100个frames。向量与向量之间的信息其实是很相近的，这是一个可以改进模型，节省运算量的方式。

<img width="673" height="498" alt="image" src="https://github.com/user-attachments/assets/b1e9659c-58a4-4cbc-995a-052a5fd3d78a" />


一个窗口的声音信号我们会用傅里叶变换映射到频域空间。频谱和声音之间的关联性是非常强的。我们人类都可以通过看出来猜测什么频谱对应什么声音。但是光看波形却做不到。这便是要映射到频域空间的原因。接着我们会用一些根据人发声器官发声原理设计的filter bank来对频谱过滤，取log后再做DCT就可以得到MFCC向量。这些我们叫作声学特征。

由图可见，filter bank output最为流行，其次是MFCC

我们需要多少有标注文字的声音数据，才能做出一个足够好的语音识别系统呢？看图可知，MNIST数据集可以等价于49分钟时长的语音。CIFAR-10可以等价于2小时40分钟。现有评测数据集ISLVRC有4096个小时的语音数据。文献上，谷歌语音搜索，他们会用超过1万小时的语音数据去训练模型。而实际产业中的商用系统，使用的数据量大小会远远超过以上这些。



# speech recognition
speech：a sequence of vector(length t,dimension d)

text:a sequence of token(lengN,V diffrenet tokens)

t>n

输出

就是token

<img width="888" height="449" alt="image" src="https://github.com/user-attachments/assets/946798fc-3611-4237-b56c-50435166c204" />


哪些词汇的？

grapheme书写的基本单位的


所有的英文

空白分割的

26engish alphabet
+{punctuation marks}

+{_}{space}

word🦴

one punch man

N = 3

词汇当作语音辨识的单位不是最好的选择

没有拌饭穷尽所有的ci
morpheme



<img width="867" height="666" alt="image" src="https://github.com/user-attachments/assets/39e3e567-7801-4b91-9c79-3ab48601778f" />

the smallest meaningful unit(<word,>grapheme)

bytes

the system can be language independent

utf-8

v is always 256


<img width="842" height="673" alt="image" src="https://github.com/user-attachments/assets/a1961df5-8388-438a-bc6a-a001e503589f" />

acooustic feature

frame

simple拿出来的，


<img width="881" height="631" alt="image" src="https://github.com/user-attachments/assets/dd6310a6-845b-4b89-a9cf-1c44a34c004c" />

deep learning

TIMIT

WSJ

Switchboard

Librispeech

Fisher



<img width="888" height="603" alt="image" src="https://github.com/user-attachments/assets/df16ae99-d901-4fe2-affa-8752ea97fb69" />



sqeunce-to-sequence

HMM


<img width="800" height="621" alt="image" src="https://github.com/user-attachments/assets/01c89b1f-ca2d-4113-b191-5304fb8e4f81" />



Listen，Attend，and speel

LAS

CTC




