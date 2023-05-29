# Eye-for-Blind
This deep learning model uses an attention mechanism on the Flickr8K dataset to explain image contents through speech caption generation. Designed for blind people, it enables image comprehension via speech. A CNN-RNN model generates captions, then a text to speech library converts them to speech

# Encoder-Decoder (CNN-RNN) based model

![244c4d7f-aa0b-4661-8bde-0b2e5a6bf80c-01](https://github.com/ABK9999/eye-for-blind/assets/104877550/f6e8b142-e200-4296-88a1-f72f77a7e0e4)
The image-to-caption generation part can be achieved with the help of encoder-decoder (CNN-RNN) based models. The encoder parts involve the convolution of the input image with the help of various convolution, max pooling, and fully connected layers. Since we are not dealing with the classification of the image, we have removed them from the end. The final output of the encoder part will be the generation of the feature vector.
![6792ca43-3019-4720-8736-680f895e89f9-02](https://github.com/ABK9999/eye-for-blind/assets/104877550/63125d15-2183-4f9c-9163-a1f197fbfe07)

The CNN-based encoder produces the feature vector which is the encoded representation of the input image. The resulting feature vector is static and does not change at each timestamp; therefore you pass this vector to the attention model along with the hidden state of the decoder to create the context vector. 
On the decoder front, you have the input sequence which is pre-processed and transformed such that all the samples have equal sequence length. This is then fed to the embedding layer present inside the decoder. The embedded layer transforms it into an embedded vector which is passed to a concatenation layer along with the context vector coming from the attention model. The output of the concatenation layer is fed as input to the GRU (the variant of the RNN considered here).
The GRU then produces an output along with the hidden state, which is fed to the RNN at the next timestep. The dense layer placed after the GRU does a linear transformation on the previous output and thus you get a list of probability values for all the words present in the vocabulary. The word with the highest probability is selected as the prediction for that timestep.
![9a79d531-079c-4f95-b876-235e8c68aaa6-image21](https://github.com/ABK9999/eye-for-blind/assets/104877550/fb4cad07-616c-4c5e-800a-77252dd980a9)

# Attention model

With an Attention mechanism, at each timestamp, we know where to look according to the previous predicted word.
Attention models use the feature vector and the decoder's hidden state to generate a context vector. This context vector keeps changing as per the previous prediction.
We are passing on, only that information that is necessary to predict the word at a particular timestamp.
![671b151d-fce9-4839-95e3-d30e1c455380-attention_final](https://github.com/ABK9999/eye-for-blind/assets/104877550/78d77ac0-998b-45a2-b583-233ed5201f15)

# Greedy vs Beam search

Greedy search is a simple approximation technique that selects the word with the highest probability at each step for the output caption.
For each time-step, the RNN predicts the most probable next value in the sequence, by considering the previous cell state. This process is repeated until the end token is sampled, signalling the end of the caption.
 The results through Greedy search are not accurate all the time and are suboptimal.
At each timestamp, instead of greedily selecting the most probable word, we select k most likely choices based on conditional probability. Here the k most likely words are chosen according to the k value or beam-width.
K values signify the number of parallel searches through the sequence of probabilities.
Beam search is a breadth-first search algorithm that explores the most promising nodes.
