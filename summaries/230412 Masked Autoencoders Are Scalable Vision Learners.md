
# GPT vs BERT
- GPT: autoregressive language modeling
  - Decoder에서 현재까지 inference output을 다시 input으로 활용하여 inference.  
- BERT: masked autoencoding
  - masked language modeling을 통해 pretraining. 
  - 몇 단어를 무작위로 마스킹하여 빈칸을 잘 추론하도록 Encoder를 학습하는 기법.
  - 일종의 self-supervised learning (+autoencoder)


# NLP에서 autoencoding이 잘 되는 이유?
- 
