
## Tree

```
.
├── [ 14M]  books_combined.csv
├── [3.1M]  books.csv
├── [ 15M]  book_tags.csv
└── [672K]  tags.csv

  33M used in 0 directories, 4 files
```

### Creating `books_combined.csv`

```python
import pandas as pd
books = pd.read_csv("books.csv", encoding="ISO-8859-1")
# Load tags book_tags data from csv
book_tags = pd.read_csv("book_tags.csv", encoding="ISO-8859-1")
tags = pd.read_csv("tags.csv", encoding="ISO-8859-1")
# Merge book_tags and tags 
tags_join = pd.merge(book_tags, tags, left_on='tag_id', right_on='tag_id', how='inner')
# Merge tags_join and books
books_with_tags = pd.merge(books, tags_join, left_on='book_id', right_on='goodreads_book_id', how='inner')
# Store tags into the same book id row
temp_df = books_with_tags.groupby('book_id')['tag_name'].apply(' '.join).reset_index()
# Merge tag_names back into books
books = pd.merge(books, temp_df, left_on='book_id', right_on='book_id', how='inner')
books.to_csv('books_combined.csv')
```