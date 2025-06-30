# Week 5 - Monday: Introduction to Sequence-to-Sequence Models

## Daily Overview

Today marks the beginning of our journey into sequence-to-sequence (seq2seq) models - a revolutionary approach in deep learning that enables us to transform input sequences into output sequences of potentially different lengths. This is the foundation for machine translation, text summarization, chatbots, and many other NLP applications.

## Learning Objectives

- Understand the fundamental problem of sequence-to-sequence modeling
- Learn the limitations of traditional RNNs for variable-length input/output
- Implement encoder-decoder architectures from scratch
- Train and evaluate basic seq2seq models
- Debug common issues in sequence-to-sequence learning

## Schedule

- **10:00-11:30**: Introduction to Sequence-to-Sequence Models
- **11:30-12:30 & 13:00-14:30**: Building First Encoder-Decoder
- **14:30-16:00**: Training Encoder-Decoder Models
- **16:00-17:30**: Testing and Evaluation

## Key Concepts Covered

### 1. Sequence-to-Sequence Problem Definition

- **Variable Length Challenge**: Unlike traditional neural networks where input and output dimensions are fixed, seq2seq models handle sequences of different lengths
- **Real-world Example**: Translating "Hello" (5 characters) to "Bonjour" (7 characters)
- **Applications**: Machine translation, text summarization, question answering, code generation

### 2. Encoder-Decoder Architecture

- **Encoder**: Processes the input sequence and creates a fixed-size context vector (thought vector)
- **Decoder**: Takes the context vector and generates the output sequence step by step
- **Context Vector**: The bottleneck that contains all information about the input sequence

### 3. Training Methodology

- **Teacher Forcing**: During training, use the actual target sequence as input to the decoder
- **Inference**: During testing, use the decoder's own predictions as input for the next step
- **Sequence Padding**: Handle variable-length sequences using padding tokens

### 4. Common Challenges

- **Information Bottleneck**: Single context vector may not capture all necessary information
- **Repetitive Outputs**: Model might get stuck generating the same tokens
- **Short Translations**: Model might terminate sequences too early
- **Exposure Bias**: Difference between training (teacher forcing) and inference behavior

## Notebooks Created

1. **seq2seq_fundamentals.ipynb**: Introduction to sequence-to-sequence problems and basic concepts
2. **encoder_decoder_implementation.ipynb**: Step-by-step implementation of encoder-decoder architecture
3. **training_seq2seq_models.ipynb**: Training procedures, data preparation, and optimization
4. **evaluation_debugging.ipynb**: Testing models, debugging common issues, and evaluation metrics

## Technical Implementation Details

### Encoder Architecture

```python
class Encoder(nn.Module):
    def __init__(self, vocab_size, embed_dim, hidden_dim):
        super().__init__()
        self.embedding = nn.Embedding(vocab_size, embed_dim)
        self.lstm = nn.LSTM(embed_dim, hidden_dim, batch_first=True)

    def forward(self, x):
        embedded = self.embedding(x)
        outputs, (hidden, cell) = self.lstm(embedded)
        return hidden, cell  # Return final hidden state as context
```

### Decoder Architecture

```python
class Decoder(nn.Module):
    def __init__(self, vocab_size, embed_dim, hidden_dim):
        super().__init__()
        self.embedding = nn.Embedding(vocab_size, embed_dim)
        self.lstm = nn.LSTM(embed_dim, hidden_dim, batch_first=True)
        self.output_projection = nn.Linear(hidden_dim, vocab_size)

    def forward(self, x, hidden_state, cell_state):
        embedded = self.embedding(x)
        output, (hidden, cell) = self.lstm(embedded, (hidden_state, cell_state))
        predictions = self.output_projection(output)
        return predictions, hidden, cell
```

## Datasets Used

- **Simple English-French pairs**: For basic translation experiments
- **Synthetic data**: Number sequence transformations (e.g., sorting, reversing)
- **Name translation**: Converting names between different writing systems

## Key Insights Discovered

1. **Context Vector Limitation**: Single vector struggles to capture long sequences
2. **Teacher Forcing Trade-off**: Speeds up training but creates exposure bias
3. **Sequence Length Impact**: Longer sequences are harder to encode effectively
4. **Vocabulary Handling**: Out-of-vocabulary words require special treatment

## Performance Metrics

- **BLEU Score**: For translation quality evaluation
- **Perplexity**: Measures how well the model predicts sequences
- **Accuracy**: Token-level prediction accuracy
- **Sequence Accuracy**: Complete sequence match percentage

## Debugging Strategies

1. **Start Simple**: Begin with short sequences and small vocabularies
2. **Visualize Outputs**: Monitor what the model generates during training
3. **Check Convergence**: Ensure loss decreases and doesn't plateau too early
4. **Gradient Analysis**: Monitor gradient flow through the sequence

## Tomorrow's Preview

Tomorrow we'll address the major limitation of basic encoder-decoder models - the information bottleneck problem. We'll introduce attention mechanisms that allow the decoder to "look back" at all encoder outputs, not just the final context vector.

## Personal Learning Notes

- The encoder-decoder paradigm is intuitive but has fundamental limitations
- Teacher forcing is a clever training trick but creates train/test mismatch
- Context vector acts as a bottleneck - this will be crucial for understanding attention
- Debugging seq2seq models requires patience and systematic approach

## Resources and References

- Sutskever et al. (2014): "Sequence to Sequence Learning with Neural Networks"
- Cho et al. (2014): "Learning Phrase Representations using RNN Encoder-Decoder"
- PyTorch Seq2Seq Tutorial: https://pytorch.org/tutorials/intermediate/seq2seq_translation_tutorial.html
