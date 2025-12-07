<p align="center">
  <img src="logo.png" alt="Memphora Logo" width="120" height="120">
</p>

<h1 align="center">Memphora Python SDK</h1>

<p align="center">
  <strong>Persistent memory layer for AI agents. Store, search, and retrieve memories with semantic understanding.</strong>
</p>

<p align="center">
  <a href="https://pypi.org/project/memphora/"><img src="https://img.shields.io/pypi/v/memphora.svg" alt="PyPI"></a>
  <a href="https://github.com/Memphora/memphora-sdk/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License"></a>
  <a href="https://memphora.ai"><img src="https://img.shields.io/badge/website-memphora.ai-orange.svg" alt="Website"></a>
</p>

## Installation

```bash
pip install memphora
```

## Quick Start

```python
from memphora import Memphora

# Initialize
memory = Memphora(
    user_id="user123",        # Unique identifier for this user (Admin can track data from dashboard)
    api_key="your_api_key"    # Required: Get from https://memphora.ai/dashboard
)

# Store a memory
memory.store("I love playing basketball on weekends")

# Search memories
results = memory.search("What sports do I like?")
print(results)

# Get context for a conversation
context = memory.get_context("Tell me about my hobbies")
```

## Features

- 🧠 **Semantic Search** - Find memories by meaning, not just keywords
- 🔄 **Auto-consolidation** - Automatically merges duplicate memories
- 📊 **Graph Relationships** - Link related memories together
- 🤖 **Multi-Agent Support** - Separate memory spaces for different agents
- 👥 **Group Memories** - Shared memories for teams
- 📈 **Analytics** - Track memory growth and usage

## API Reference

### Initialize

```python
memory = Memphora(
    user_id="user123",        # Unique identifier for this user (Admin can track data from dashboard)
    api_key="your_api_key"    # Required: API key from https://memphora.ai/dashboard
)
```

### Core Methods

#### `store(content: str, metadata: dict = None) -> dict`
Store a new memory.

```python
mem = memory.store("I work as a software engineer", {
    "category": "work",
    "importance": "high"
})
```

#### `search(query: str, limit: int = 10, **options) -> list`
Search memories semantically with optional external reranking.

```python
# Basic search
results = memory.search("What is my job?", limit=5)

# Search with Cohere reranking for better relevance
reranked_results = memory.search("headphone recommendations", limit=10,
    rerank=True,
    rerank_provider="cohere",  # or "jina" or "auto"
    cohere_api_key="your-cohere-api-key"  # Get from https://dashboard.cohere.com/api-keys
)

# Search with Jina AI reranking (multilingual support)
jina_results = memory.search("recomendaciones de auriculares", limit=10,
    rerank=True,
    rerank_provider="jina",
    jina_api_key="your-jina-api-key"  # Get from https://jina.ai/
)
```

#### `get_context(query: str, limit: int = 5) -> str`
Get formatted context for AI prompts.

```python
context = memory.get_context("Tell me about myself")
# Returns: "Relevant context from past conversations:\n- I work as a software engineer\n- ..."
```

### Advanced Methods

#### `store_conversation(user_message: str, ai_response: str) -> None`
Store a conversation and extract memories. Returns the extracted memories.

```python
memory.store_conversation(
    "What's my favorite color?",
    "Based on your memories, your favorite color is blue."
)
```

#### `list_memories(limit: int = 100) -> list`
List all memories for the user.

```python
all_memories = memory.list_memories(limit=100)
```

#### `update_memory(memory_id: str, content: str = None, metadata: dict = None) -> dict`
Update an existing memory.

```python
updated = memory.update_memory(memory_id, "Updated content", {"category": "work"})
```

#### `delete_memory(memory_id: str) -> bool`
Delete a memory.

```python
memory.delete_memory(memory_id)
```

#### `get_memory(memory_id: str) -> dict`
Get a specific memory by ID.

```python
mem = memory.get_memory(memory_id)
print(mem["content"])
```

#### `clear() -> bool`
Delete all memories for this user. Warning: This action is irreversible.

```python
memory.clear()
```

### Advanced Search

```python
# Advanced search with filters and options
results = memory.search_advanced("query",
    limit=10,
    filters={"category": "work"},
    min_score=0.7,
    sort_by="relevance"
)

# Optimized search for better performance
optimized = memory.search_optimized("query",
    max_tokens=2000,
    max_memories=20,
    use_compression=True
)
```

### Batch Operations

```python
# Store multiple memories at once
memories = memory.batch_store([
    {"content": "Memory 1", "metadata": {"category": "work"}},
    {"content": "Memory 2", "metadata": {"category": "personal"}}
])

# Merge multiple memories
merged = memory.merge([memory_id1, memory_id2], "combine")
```

### Analytics

```python
# Get user statistics
stats = memory.get_statistics()
print(stats)
# {"totalMemories": 42, "avgMemoryLength": 85, ...}
```

### Graph Features

```python
# Get memory context with related memories
context = memory.get_context_for_memory(memory_id, depth=2)

# Get memories related to a specific memory
related = memory.get_related_memories(memory_id, limit=10)

# Find contradictions
contradictions = memory.find_contradictions(memory_id, threshold=0.7)

# Link two memories in the graph
memory.link(
    memory_id,
    target_id,
    "related"  # or "contradicts", "supports", "extends"
)

# Find shortest path between two memories
path = memory.find_path(source_id, target_id)
print(path["distance"])  # Number of steps
```

### Context Methods

```python
# Get optimized context (26% better accuracy, 91% faster)
optimized_context = memory.get_optimized_context(
    "user preferences",
    max_tokens=2000,
    max_memories=20,
    use_compression=True,
    use_cache=True
)

# Get enhanced context (35%+ accuracy improvement)
enhanced_context = memory.get_enhanced_context(
    "programming languages",
    max_tokens=1500,
    max_memories=15,
    use_compression=True
)
```

### Version Control

```python
# Get all versions of a memory
versions = memory.get_versions(memory_id, limit=50)

# Compare two versions
comparison = memory.compare_versions(version_id1, version_id2)
print(comparison["changes"])
print(comparison["similarity"])

# Rollback to a specific version
memory.rollback(memory_id, target_version)
```

### Conversation Management

```python
# Record a full conversation
conversation = [
    {"role": "user", "content": "I need help with Python"},
    {"role": "assistant", "content": "I'd be happy to help!"}
]
recorded = memory.record_conversation(conversation, "web_chat", {
    "session_id": "sess_123"
})

# Get all conversations
conversations = memory.get_conversations("web_chat", limit=50)

# Get a specific conversation by ID
conv = memory.get_conversation(conversation_id)

# Get rolling summary of all conversations
summary = memory.get_summary()
print(summary["total_conversations"])
print(summary["topics"])
```

### Multi-Agent Support

```python
# Store a memory for a specific agent
memory.store_agent_memory(
    "agent_123",
    "User prefers Python for backend development",
    "run_001",  # optional run_id
    {"category": "preference"}
)

# Search memories for a specific agent
agent_memories = memory.search_agent_memories(
    "agent_123",
    "What does the user prefer?",
    "run_001",  # optional run_id
    limit=10
)

# Get all memories for a specific agent
all_agent_memories = memory.get_agent_memories("agent_123", limit=100)
```

### Group/Collaborative Features

```python
# Store a shared memory for a group
memory.store_group_memory(
    "team_alpha",
    "Team decided to use React for the frontend",
    {"priority": "high"}
)

# Search memories for a group
group_memories = memory.search_group_memories(
    "team_alpha",
    "What framework did we choose?",
    limit=10
)

# Get context for a group
group_context = memory.get_group_context("team_alpha", limit=50)
```

### Analytics

```python
# Get user statistics
stats = memory.get_statistics()
print(stats)
# {"totalMemories": 42, "avgMemoryLength": 85, ...}

# Get user's memory statistics and insights
analytics = memory.get_user_analytics()
print(analytics)

# Track memory growth over time
growth = memory.get_memory_growth(days=30)  # last 30 days
print(growth)
```

### Image Operations

```python
# Store an image memory
image_mem = memory.store_image(
    image_url="https://example.com/photo.jpg",
    description="A photo of the Golden Gate Bridge",
    metadata={"location": "San Francisco", "type": "landmark"}
)

# Search image memories
image_results = memory.search_images("bridge", limit=5)

# Upload an image from file
with open("product_photo.jpg", "rb") as f:
    image_data = f.read()
uploaded = memory.upload_image(
    image_data,
    "product_photo.jpg",
    {"category": "product", "product_id": "prod_123"}
)
```

### Export & Import

```python
# Export all memories
export_data = memory.export("json")
# or
csv_data = memory.export("csv")

# Import memories
memory.import_memories(export_data["data"], "json")
```

### Text Processing

```python
# Make text more concise
concise_result = memory.concise("This is a very long text that needs to be made more concise...")
print(concise_result["concise_text"])
```

### Health Check

```python
# Check API health
health = memory.health()
print(health["status"])
```

## Type Hints

Full type hint support included.

```python
from memphora import Memphora
from typing import List, Dict
import os

memory: Memphora = Memphora(
    user_id="user123",                    # Unique identifier for this user (Admin can track data from dashboard)
    api_key=os.environ["MEMPHORA_API_KEY"]  # Required: API key from dashboard
)

results: List[Dict] = memory.search("query")
```

## Error Handling

```python
try:
    memory.store("My memory")
except Exception as e:
    if "401" in str(e):
        print("Invalid API key")
    elif "429" in str(e):
        print("Rate limit exceeded")
    else:
        print(f"Error: {e}")
```

## Examples

### Chatbot with Memory

```python
from memphora import Memphora
import os

memory = Memphora(
    user_id="user123",                    # Unique identifier for this user (Admin can track data from dashboard)
    api_key=os.environ["MEMPHORA_API_KEY"]  # Required: Get from dashboard
)

def chat(user_message: str) -> str:
    # Get relevant context
    context = memory.get_context(user_message)
    
    # Generate AI response with context
    ai_response = generate_ai_response(user_message, context)
    
    # Store the conversation
    memory.store_conversation(user_message, ai_response)
    
    return ai_response
```

### Multi-Agent System

```python
memory = Memphora(
    user_id="user123",
    api_key="your_api_key"
)

# Store memories for different agents
memory.store_agent_memory("coder", "User prefers Python", "run_001")
memory.store_agent_memory("writer", "User likes technical writing", "run_001")

# Search memories for a specific agent
coder_memories = memory.search_agent_memories("coder", "What does the user prefer?")

# Get all memories for an agent
all_coder_memories = memory.get_agent_memories("coder", limit=100)
```

### Group Collaboration

```python
memory = Memphora(
    user_id="user123",
    api_key="your_api_key"
)

# Store shared memories for a team
memory.store_group_memory("team_alpha", "Team decided to use React", {
    "priority": "high",
    "decision_date": "2024-01-15"
})

# Search group memories
team_memories = memory.search_group_memories("team_alpha", "What framework?")

# Get group context
team_context = memory.get_group_context("team_alpha", limit=50)
```

### Memory Linking and Path Finding

```python
memory = Memphora(
    user_id="user123",
    api_key="your_api_key"
)

# Store related memories
mem1 = memory.store("User works at Google")
mem2 = memory.store("User is a software engineer")
mem3 = memory.store("User lives in San Francisco")

# Link memories together
memory.link(mem1["id"], mem2["id"], "related")
memory.link(mem1["id"], mem3["id"], "related")

# Get related memories
related = memory.get_related_memories(mem1["id"], limit=10)

# Find path between memories
path = memory.find_path(mem1["id"], mem3["id"])
print(f"Path distance: {path['distance']} steps")
```

### Version Control and Rollback

```python
memory = Memphora(
    user_id="user123",
    api_key="your_api_key"
)

# Store and update a memory multiple times
mem = memory.store("User works at Microsoft")
memory.update_memory(mem["id"], "User works at Google")
memory.update_memory(mem["id"], "User works at Meta")

# Get all versions
versions = memory.get_versions(mem["id"], limit=10)
for v in versions:
    print(f"Version {v['version']}: {v['content']}")

# Compare two versions
comparison = memory.compare_versions(versions[0]["id"], versions[1]["id"])
print("Changes:", comparison["changes"])
print("Similarity:", comparison["similarity"])

# Rollback to a previous version
memory.rollback(mem["id"], 1)
```

### Using Optimized Context

```python
memory = Memphora(
    user_id="user123",
    api_key="your_api_key"
)

# Get optimized context (best for production)
optimized_context = memory.get_optimized_context(
    "user preferences",
    max_tokens=2000,
    max_memories=20,
    use_compression=True,
    use_cache=True
)

# Use in your AI prompt
prompt = f"""Context about user:
{optimized_context}

User query: What are my preferences?
Assistant:"""

response = your_ai_model(prompt)
```

### Decorator Pattern

```python
from memphora import Memphora

memory = Memphora(
    user_id="user123",
    api_key="your_api_key"
)

@memory.remember
def chat(user_message: str, memory_context: str = "") -> str:
    # memory_context is automatically injected with relevant memories
    ai_response = generate_response(user_message, memory_context)
    return ai_response

# The decorator will:
# 1. Search for relevant memories
# 2. Add them to kwargs as 'memory_context'
# 3. Store the conversation after response
```

## License

MIT

## Links

- [Documentation](https://memphora.ai/docs/sdk-python)
- [Dashboard](https://memphora.ai/dashboard)
- [GitHub](https://github.com/Memphora/memphora-sdk-python)
- [PyPI](https://pypi.org/project/memphora/)

## Support

- Email: info@memphora.ai
- [Issues](https://github.com/Memphora/memphora-sdk-python/issues)
