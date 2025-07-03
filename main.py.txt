# main.py

from fastapi import FastAPI
from pydantic import BaseModel
from azul_agi import AzulAGI, LLMBackend, EthicsKernel

app = FastAPI()

kernel = AzulAGI(LLMBackend(), EthicsKernel())

class Query(BaseModel):
    issue: str

@app.post("/reason")
def reason(query: Query):
    result = kernel.reason(query.issue)
    return {
        "input": query.issue,
        "output": result,
        "audit": kernel.audit_ledger
    }

@app.get("/")
def root():
    return {"message": "Azul AGI is running."}
