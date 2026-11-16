## deployed on render link  :  https://doubthub-ml-api.onrender.com

# Smart College Doubt Solving Platform: ML API
 
An AI/ML-powered Q&A platform for college students to post, discover, and resolve technical doubts. This repository contains the **ML microservice** of the project.

## Project Overview
 
Students often face the same technical doubts again and again, and finding past answers is hard. The platform adds an ML layer on top of a Stack Overflow-style Q&A app. The ML service:
 
- classifies a question into a domain (Frontend, Backend, ML/AI, DSA, Cyber Security, Programming Language)
- recommends similar existing questions
- returns the best-matching stored answer
## How It Works
 
```
[Frontend] --> [Backend] --> [ML API (this repo)]
```
 
1. The student types a doubt in the frontend.
2. The backend sends the text to this ML API.
3. The API predicts the domain (TF-IDF + Logistic Regression, about 98% accuracy on a held-out test set).
4. Within that domain, questions are clustered (KMeans) and ranked by cosine similarity.
5. Similar questions or the best stored answer are returned to the backend.
## API Endpoints
 
All POST endpoints accept `{"Query": "your question"}`.
 
| Method | Endpoint | Description |
| --- | --- | --- |
| GET | `/` | Welcome message |
| POST | `/predict` | Predicts the domain of a query |
| POST | `/recommend` | Returns the 5 most similar questions |
| POST | `/solution` | Returns the closest question and its answer |
 
Example:
 
```bash
curl -X POST http://127.0.0.1:8000/predict \
  -H "Content-Type: application/json" \
  -d '{"Query": "What is linear regression?"}'
```
 
## Setup
 
```bash
git clone https://github.com/varunkumar-2107/DoubtHub_ML_api.git
cd DoubtHub_ML_api
pip install -r requirements.txt
 
cd ml_service
uvicorn app:app --reload
```
 
Run the server from inside `ml_service/`. Interactive docs are at http://127.0.0.1:8000/docs.
 
## Project Structure
 
```
DoubtHub_ML_api/
├── requirements.txt
└── ml_service/
    ├── app.py              # FastAPI app and endpoints
    ├── model/              # Saved classifier and vectorizer (.joblib)
    ├── model_3/            # Answer retrieval (solution.py) and Q&A data
    └── training/           # Notebooks, recommendation code, datasets
```
 
## Tech Stack
 
| Component | Stack |
| --- | --- |
| ML Service | Python, scikit-learn, pandas, NumPy |
| API | FastAPI, Uvicorn |
| Version Control | Git & GitHub |
 
## License
 
Developed by team **Gradient Descenders** for the final probation task evaluation.
 
