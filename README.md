# Eye-for-Blind
This deep learning model uses an attention mechanism on the Flickr8K dataset to explain image contents through speech caption generation. Designed for blind people, it enables image comprehension via speech. A CNN-RNN model generates captions, then a text to speech library converts them to speech

## Encoder-Decoder (CNN-RNN) based model
![244c4d7f-aa0b-4661-8bde-0b2e5a6bf80c-01](https://github.com/ABK9999/eye-for-blind/assets/104877550/f6e8b142-e200-4296-88a1-f72f77a7e0e4)
The image-to-caption generation part can be achieved with the help of encoder-decoder (CNN-RNN) based models. The encoder parts involve the convolution of the input image with the help of various convolution, max pooling, and fully connected layers. Since we are not dealing with the classification of the image, we have removed them from the end. The final output of the encoder part will be the generation of the feature vector.
![6792ca43-3019-4720-8736-680f895e89f9-02](https://github.com/ABK9999/eye-for-blind/assets/104877550/63125d15-2183-4f9c-9163-a1f197fbfe07)

## Attention model
Attention mechanism, at each timestamp, will look according to the previous predicted word. Attention models use the feature vector and the decoder's hidden state to generate a context vector. This context vector keeps changing as per the previous prediction. We are passing on, only that information that is necessary to predict the word at a particular timestamp.
![671b151d-fce9-4839-95e3-d30e1c455380-attention_final](https://github.com/ABK9999/eye-for-blind/assets/104877550/78d77ac0-998b-45a2-b583-233ed5201f15)

## Encoder-Decoder Architecture
1.The CNN-based encoder produces the feature vector which is the encoded representation of the input image. The resulting feature vector is static and does not change at each timestamp; therefore you pass this vector to the attention model along with the hidden state of the decoder to create the context vector.
![74751f7e-5e91-41df-9c1b-c5a3b76c9ea0-summary](https://github.com/ABK9999/eye-for-blind/assets/104877550/920232a2-1032-4b4a-ad4c-5c6e50a17dd8)
2.On the decoder front, you have the input sequence which is pre-processed and transformed such that all the samples have equal sequence length. This is then fed to the embedding layer present inside the decoder. The embedded layer transforms it into an embedded vector which is passed to a concatenation layer along with the context vector coming from the attention model. The output of the concatenation layer is fed as input to the GRU (the variant of the RNN considered here).

3.The GRU then produces an output along with the hidden state, which is fed to the RNN at the next timestep. The dense layer placed after the GRU does a linear transformation on the previous output and thus you get a list of probability values for all the words present in the vocabulary. The word with the highest probability is selected as the prediction for that timestep.

### Greedy
This method calculates the probability of the words according to their occurrence in the English vocabulary. It takes the sample of the words, finds the probability of each of the words, and then outputs the word with the highest probability. This makes the computational speed of the model fast, but the accuracy might not be up to the mark.

### Beam search
The alternative for greedy search is beam search. In beam search, the model finds the k most likely words instead of selecting a single word and these k words are passed on to the next timestamp. It works on the breadth-first search algorithm.

### BLEU score
Once you have done the prediction, the BLEU score is used as an evaluation metric for the predicted word. It determines the "difference" between the predicted sentence from the human-created sentence.
