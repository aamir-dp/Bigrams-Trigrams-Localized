# Documentation: Extract Keywords with N-grams in Localized Text

This script extracts keywords with their respective counts from localized text. It supports generating unigrams, bigrams, and trigrams by adjusting the `ngram_range` parameter.

---

## Prerequisites

### Libraries Required:
1. **nltk**: For tokenizing words and fetching stopwords for various languages.
2. **re**: For text preprocessing (removing punctuation).
3. **collections**: For counting occurrences of words or n-grams.

Install the necessary libraries:
```bash
pip install nltk
```

### NLTK Stopwords
Ensure that the NLTK stopwords corpus is downloaded:
```python
import nltk
nltk.download('stopwords')
nltk.download('punkt')
```

---

## Function Overview

### `extract_keywords_with_counts(text, language='english', ngram_range=1)`

#### Parameters:
- **`text`** (str): Input text in the desired language.
- **`language`** (str): Language for stopwords. Default is `'english'`. Supported languages include `'french'`, `'spanish'`, `'german'`, etc.
- **`ngram_range`** (int): Specifies the type of n-grams to generate. Default is `1` (unigrams).
  - `1`: Unigrams (single words).
  - `2`: Bigrams (two-word combinations).
  - `3`: Trigrams (three-word combinations).

#### Returns:
- A list of tuples containing the top 20 keywords or n-grams and their counts.
  - Format: `[(keyword, count), ...]`

---

## Usage

### Example Code:
```python
import re
from collections import Counter
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.util import ngrams

def extract_keywords_with_counts(text, language='english', ngram_range=1):
    # Fetch stopwords for the specified language
    stop_words = set(stopwords.words(language))

    # Normalize text
    text = re.sub(r'[\^\w\s]', '', text)  # Remove punctuation
    words = word_tokenize(text.lower())  # Tokenize and convert to lowercase

    # Remove stopwords and non-alphabetic tokens
    filtered_words = [word for word in words if word not in stop_words and word.isalpha()]

    # Generate n-grams
    if ngram_range > 1:
        ngram_words = [' '.join(ng) for ng in ngrams(filtered_words, ngram_range)]
        keywords = Counter(ngram_words).most_common(20)  # Top 20 n-grams
    else:
        keywords = Counter(filtered_words).most_common(20)  # Top 20 words

    # Format output
    return [(keyword, count) for keyword, count in keywords]

# Example usage
localized_text = """
Editor de vídeo con música
Éditeur vidéo avec musique
Videoleap: Éditeur vidéo
VideoShow: Editor de vídeos
Historias animadas con música
"""

# Extract bigrams (2-grams) for Spanish
keywords_with_counts = extract_keywords_with_counts(localized_text, language='spanish', ngram_range=2)

# Print output
for keyword, count in keywords_with_counts:
    print(f"{keyword}: {count}")
```

---

## Adjusting N-grams
To switch between unigrams, bigrams, and trigrams, modify the `ngram_range` parameter when calling the function:

- **Unigrams:**
  ```python
  extract_keywords_with_counts(text, language='french', ngram_range=1)
  ```

- **Bigrams:**
  ```python
  extract_keywords_with_counts(text, language='spanish', ngram_range=2)
  ```

- **Trigrams:**
  ```python
  extract_keywords_with_counts(text, language='german', ngram_range=3)
  ```
