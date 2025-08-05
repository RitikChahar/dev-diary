# FastAPI Background Tasks - Async API Pattern

## Why Use Background Tasks?
For long-running operations like sending emails, processing files, or generating reports - return immediately and process in background.

## Basic Setup
```python
from fastapi import FastAPI, BackgroundTasks
from pydantic import BaseModel
from typing import Dict, Optional
from enum import Enum

app = FastAPI()

class TaskStatus(str, Enum):
    PENDING = "pending"
    PROCESSING = "processing" 
    COMPLETED = "completed"
    FAILED = "failed"

task_status: Dict[str, Dict] = {}
```

## Background Task Function
```python
async def send_email_background(task_id: str, email: str, subject: str, body: str):
    """Background task to send email"""
    try:
        task_status[task_id]["status"] = TaskStatus.PROCESSING
        
        # Simulate email sending
        await asyncio.sleep(5)  # Replace with actual email service
        
        task_status[task_id]["status"] = TaskStatus.COMPLETED
        task_status[task_id]["result"] = {"message": "Email sent successfully"}
        
    except Exception as e:
        task_status[task_id]["status"] = TaskStatus.FAILED
        task_status[task_id]["error"] = str(e)
```

## API Endpoints
### Start Email Task
```python
class EmailRequest(BaseModel):
    email: str
    subject: str
    body: str

@app.post("/send-email")
async def send_email(request: EmailRequest, background_tasks: BackgroundTasks):
    """Start email sending in background"""
    import uuid
    task_id = str(uuid.uuid4())
    
    task_status[task_id] = {
        "status": TaskStatus.PENDING,
        "result": None,
        "error": None
    }
    
    background_tasks.add_task(
        send_email_background,
        task_id,
        request.email,
        request.subject,
        request.body
    )
    
    return {"task_id": task_id, "status": "started"}
```

### Check Task Status
```python
class TaskResponse(BaseModel):
    task_id: str
    status: TaskStatus
    result: Optional[Dict] = None
    error: Optional[str] = None

@app.get("/task/{task_id}/status", response_model=TaskResponse)
async def get_task_status(task_id: str):
    """Get task processing status"""
    if task_id not in task_status:
        raise HTTPException(404, "Task not found")
    
    info = task_status[task_id]
    return TaskResponse(
        task_id=task_id,
        status=info["status"],
        result=info["result"],
        error=info["error"]
    )
```

## Usage Flow
```bash
# 1. Start email task
POST /send-email
{
  "email": "user@example.com",
  "subject": "Hello",
  "body": "Test email"
}
# Returns: {"task_id": "abc-123", "status": "started"}

# 2. Check status
GET /task/abc-123/status
# Returns: {"task_id": "abc-123", "status": "completed", "result": {...}}
```

## Key Points
- **Immediate Response**: API returns instantly with task ID
- **Status Tracking**: Separate endpoint to check progress
- **In-Memory Storage**: Use Redis/DB for production
- **Error Handling**: Background tasks catch and store errors
- **Non-Blocking**: Main thread remains responsive
