# Text-to-SQL RAG Chatbot 🤖

A natural language to SQL chatbot built to explore RAG (Retrieval-Augmented Generation) systems and learn how LLMs can understand database queries.

## 💡 What This Project Does

Ask questions in plain English, get SQL queries and natural language answers:

**You ask:** "How many customers do we have?"  
**Chatbot responds:** "According to our database, we have 96,096 unique customers."

The system:
- Converts natural language questions to SQL
- Retrieves relevant examples using semantic search (RAG)
- Validates queries for security
- Returns results in natural language

## 🎯 Why I Built This

I kept seeing Text-to-SQL systems everywhere and was curious how they actually work. This project helped me understand:
- How RAG improves LLM accuracy
- How to build semantic search with vector databases
- How to structure prompts for better SQL generation
- How to validate and secure AI-generated queries

## 🛠️ Tech Stack

- **LangChain** - RAG orchestration and prompt management
- **ChromaDB** - Vector database for semantic search
- **Ollama** - Local LLM (Qwen 2.5 Coder 14B)
- **PostgreSQL** - E-commerce database (~100K records)
- **Python** - Data processing and pipeline

## 📊 Dataset

Brazilian E-commerce dataset with 9 tables:
- 100K orders
- 96K customers  
- 32K products
- Order items, payments, reviews, sellers, geolocation

## 🏗️ How It Works
```
1. User Question → "How many customers spent over $1000?"
2. Semantic Search → Finds 7 similar examples from training set
3. Prompt Building → Combines schema + rules + examples + question
4. LLM Generation → Generates SQL query
5. Validation → Checks for security issues
6. Execution → Runs on database
7. Response → Returns natural language answer
```

## 📁 Project Structure
```
ecommerce-text-to-sql-rag/
├── data/
│   └── v2/
│       ├── metadata_v2.json      # Database schema and business rules
│       └── training_set.json     # 200 example queries
├── notebooks/
│   └── text_to_sql_chatbot.ipynb # Main implementation
├── .env.example                   # Environment variables template
└── requirements.txt               # Dependencies
```

## 🚀 Setup

### Prerequisites
- Python 3.11+
- PostgreSQL database
- [Ollama](https://ollama.ai/) with qwen2.5-coder:14b model

### Installation

1. Clone the repository:
```bash
git clone https://github.com/YOUR_USERNAME/ecommerce-text-to-sql-rag.git
cd ecommerce-text-to-sql-rag
```

2. Create virtual environment and install dependencies:
```bash
python -m venv .venv
source .venv/bin/activate  # On Mac/Linux
pip install -r requirements.txt
```

3. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your database credentials
```

4. Load the dataset into PostgreSQL (not included - use your own e-commerce data)

5. Pull the Ollama model:
```bash
ollama pull qwen2.5-coder:14b
```

6. Open and run the notebook:
```bash
jupyter notebook notebooks/text_to_sql_chatbot.ipynb
```

## 📝 Example Queries

The system can handle various types of questions:

**Simple:**
- "How many orders were placed?"
- "What's the total revenue?"

**Complex:**
- "Show me top 5 sellers by revenue with at least 100 orders"
- "What's the month-over-month revenue growth in 2017?"
- "Which customers made their first purchase in 2017 but haven't ordered since?"

**Analytics:**
- "What's the average delivery time by state for orders over R$500?"
- "Compare average review scores between credit card and boleto payments"

## 🎯 Results

Tested on 30 diverse queries:
- **80% success rate** on basic queries
- **80% success rate** on complex queries (window functions, subqueries, date calculations)
- **100% security blocking** on malicious queries (DROP, DELETE, etc.)

## 🔒 Security

Built-in validation prevents:
- DROP, DELETE, TRUNCATE operations
- INSERT, UPDATE statements  
- SELECT * without aggregations
- Any non-SELECT queries

## 📚 What I Learned

- **RAG is powerful** - Retrieving relevant examples dramatically improves accuracy
- **Prompt engineering matters** - Clear instructions and examples guide the LLM better
- **Few-shot learning works** - The model learns patterns from just 7 examples per query
- **Validation is essential** - Never trust AI-generated SQL without security checks
- **LangChain simplifies things** - Framework handles complex orchestration cleanly

## 🔄 How RAG Improved Accuracy

Without RAG: The model only sees the schema and generates generic SQL.

With RAG: The model sees 7 relevant examples similar to the user's question, learns the patterns, and generates accurate SQL that follows business rules.

Example: When asked "How many customers?", it retrieves examples showing `customer_unique_id` should be used (not `customer_id`), and the model learns from that.

## 🐛 Known Limitations

- Struggles with percentile calculations (NTILE, PERCENTILE_CONT)
- Requires specific examples in training set for best results
- Local LLM is slower than cloud APIs (10-15s per query)
- Dataset-specific - would need retraining for different schemas

## 🔮 Future Improvements

- Add more training examples for edge cases
- Implement query result caching
- Add support for multi-turn conversations
- Build a simple web interface (Streamlit/Gradio)
- Fine-tune a smaller model on this specific dataset

## 📖 Resources That Helped

- [LangChain Documentation](https://python.langchain.com/)
- [ChromaDB Docs](https://docs.trychroma.com/)
- [Ollama](https://ollama.ai/)
- Various YouTube tutorials on RAG systems

## 🤝 Contributing

This is a learning project, but feel free to:
- Open issues for bugs or suggestions
- Fork and experiment with your own datasets
- Share improvements or alternative approaches

## 📄 License

MIT License - feel free to use this for learning!

## 👤 Author

Built by [Your Name] as a learning project to understand RAG systems and Text-to-SQL.

---

⭐ If this helped you learn about RAG or Text-to-SQL, consider giving it a star!