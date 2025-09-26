### EX5 Information Retrieval Using Boolean Model in Python
### DATE: 26/09/2025
### AIM: To implement Information Retrieval Using Boolean Model in Python.
### Description:
<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:
```python
import numpy as np
import pandas as pd

class BooleanRetrieval:
    def __init__(self):
        self.index = {}
        self.documents_matrix = None
        self.all_doc_ids = set()

    def index_document(self, doc_id, text):
        self.all_doc_ids.add(doc_id)
        terms = text.lower().split()
        print("Document -", doc_id, terms)

        for term in terms:
            if term not in self.index:
                self.index[term] = set()
            self.index[term].add(doc_id)

    def create_documents_matrix(self, documents):
        terms = list(self.index.keys())
        num_docs = len(documents)
        num_terms = len(terms)

        # Using sorted doc_ids to ensure consistent row order
        sorted_doc_ids = sorted(list(documents.keys()))
        self.documents_matrix = np.zeros((num_docs, num_terms), dtype=int)

        for i, doc_id in enumerate(sorted_doc_ids):
            text = documents[doc_id]
            doc_terms = text.lower().split()
            for term in doc_terms:
                if term in self.index:
                    term_id = terms.index(term)
                    self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self):
        df = pd.DataFrame(self.documents_matrix, columns=self.index.keys(), index=sorted(self.all_doc_ids))
        print("\n--- Document-Term Matrix ---")
        print(df)

    def print_all_terms(self):
        print("\nAll terms in the documents:")
        print(list(self.index.keys()))

    def boolean_search(self, query):
        parts = query.lower().split()
        
        # Handle cases with less than 3 parts (e.g., "python", "not python")
        if len(parts) == 1:
            # Single term query
            return self.index.get(parts[0], set())
        
        if len(parts) == 2 and parts[0] == 'not':
            # 'not term' query
            term = parts[1]
            term_docs = self.index.get(term, set())
            return self.all_doc_ids - term_docs

        # Assuming query format: "term1 operator term2"
        if len(parts) != 3:
            return "Invalid query format. Use 'term1 operator term2' (e.g., 'python and information')."

        term1, operator, term2 = parts[0], parts[1], parts[2]
        
        result_set1 = self.index.get(term1, set())
        result_set2 = self.index.get(term2, set())
        
        if operator == 'and':
            return result_set1.intersection(result_set2)
        elif operator == 'or':
            return result_set1.union(result_set2)
        elif operator == 'not':
            # This implements "term1 AND NOT term2" logic
            return result_set1.difference(result_set2)
        else:
            return "Unsupported operator. Use 'and', 'or', or 'not'."

if __name__ == "__main__":
    indexer = BooleanRetrieval()
    documents = {
        1: "Python is a programming language",
        2: "Information retrieval deals with finding information",
        3: "Boolean models are used in information retrieval"
    }

    for doc_id, text in documents.items():
        indexer.index_document(doc_id, text)

    indexer.create_documents_matrix(documents)
    indexer.print_documents_matrix_table()
    indexer.print_all_terms()

    query = input("\nEnter your boolean query (e.g., 'information and retrieval', 'python or models', 'retrieval not boolean'): ")
    results = indexer.boolean_search(query)
    
    if results:
        print(f"\nResults for '{query}': Document IDs {results}")
    else:
        print("\nNo results found for the query.")
```
### Output:
<img width="1339" height="385" alt="Screenshot 2025-09-26 104636" src="https://github.com/user-attachments/assets/8f898f0d-d1d1-4bb6-a805-8a9500166696" />

<img width="1385" height="382" alt="Screenshot 2025-09-26 104657" src="https://github.com/user-attachments/assets/7c8b3277-764c-4adb-9252-f1b6d9d86a27" />

<img width="1364" height="386" alt="Screenshot 2025-09-26 104721" src="https://github.com/user-attachments/assets/e216943e-9d3a-4717-8e81-da47dbf43544" />

### Result:
Thus, the python program to implement Information Retrieval Using Boolean Model is executed successfully.
