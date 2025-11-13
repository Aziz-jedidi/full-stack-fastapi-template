# Agent Instructions: Knowledge Graph Visualization Tool

## Project Overview
Build a **Knowledge Graph Visualization Tool** for SEO experts during a 24-hour hackathon. The tool integrates Google Knowledge Graph, Wikidata, and NLP to visualize semantic relationships around keywords and analyze URL content.

## Core Mission
Create a functional MVP that allows users to:
1. **Analyze keywords** → Generate interactive semantic graphs
2. **Audit URLs** → Extract entities and calculate semantic coverage scores

---

## Technical Stack (Leverage Existing FastAPI Template)

### Backend (Python/FastAPI - Already Present)
- **Framework**: FastAPI (existing in `/backend`)
- **Database**: Neo4j (graph database) - to be integrated
- **NLP**: spaCy for entity extraction
- **APIs**: 
  - Google Knowledge Graph API
  - Wikidata SPARQL endpoint

### Frontend (To Be Added)
- **Framework**: Next.js + React + Tailwind CSS
- **Graph Visualization**: Cytoscape.js or Sigma.js
- **Location**: `/frontend` (replace existing or adapt)

---

## Development Priorities (24h Timeline)

### Phase 1: Backend Foundation (Hours 1-8)
1. **Setup Neo4j Integration**
   - Add Neo4j Docker service to `docker-compose.yml`
   - Create graph database connection in `backend/app/core/db.py`
   - Add neo4j Python driver to `backend/pyproject.toml`

2. **Implement API Endpoints** (`backend/app/api/routes/`)
   ```python
   # New route file: knowledge_graph.py
   GET /api/knowledge-graph/keyword?query={keyword}
   GET /api/knowledge-graph/audit?url={url}
   ```

3. **Data Source Integrations** (`backend/app/services/`)
   - `google_kg_service.py`: Google Knowledge Graph API client
   - `wikidata_service.py`: SPARQL query builder and executor
   - `nlp_service.py`: spaCy entity extraction
   - `graph_fusion_service.py`: Merge data from all sources

4. **Graph Models** (`backend/app/models.py`)
   - Entity model (id, name, type, description, aliases, source)
   - Relation model (source_entity, target_entity, relation_type, weight)
   - Graph response schemas

### Phase 2: Data Integration Logic (Hours 8-14)
1. **Google Knowledge Graph Integration**
   - API key configuration in `.env`
   - Entity extraction and normalization
   - Map to Wikidata IDs when available

2. **Wikidata SPARQL Queries**
   - Taxonomic relations (P31: instance of, P279: subclass of)
   - Structural relations (P361: part of, P527: has part)
   - Property enrichment

3. **Graph Fusion Algorithm**
   - Merge entities by ID/alias matching
   - Weight relations by source and frequency
   - Build unified graph structure in Neo4j

4. **NLP Pipeline for URL Audit**
   - Fetch URL content
   - Extract entities with spaCy
   - Compare with ideal semantic graph
   - Calculate coverage score

### Phase 3: Frontend Development (Hours 14-20)
1. **Next.js Setup** (if not using existing React frontend)
   - Initialize Next.js in `/frontend` or adapt existing
   - Configure Tailwind CSS
   - Setup API client for FastAPI backend

2. **Core UI Components**
   - `SearchBar.tsx`: Main input field + "Analyze" button
   - `GraphVisualization.tsx`: Interactive graph (Cytoscape.js/Sigma.js)
   - `Sidebar.tsx`: Entity details, relations list, SEO recommendations
   - `OnboardingModal.tsx`: Quick usage guide

3. **Pages/Routes**
   - `/keyword-analysis`: Keyword input and graph display
   - `/url-audit`: URL input, entity extraction results, coverage score
   - Dashboard layout with tabs

4. **Graph Interaction**
   - Node click → Show entity details in sidebar
   - Zoom, pan, node dragging
   - Color coding by entity type
   - Relation weight visualization (edge thickness)

### Phase 4: Integration & Polish (Hours 20-24)
1. **API-Frontend Connection**
   - Connect search inputs to backend endpoints
   - Display loading states
   - Error handling and user feedback

2. **SEO Recommendations Engine**
   - Identify missing entities in URL audit
   - Suggest related entities from graph
   - Display in sidebar with actionable insights

3. **Testing & Demo Preparation**
   - Test keyword analysis flow
   - Test URL audit flow
   - Prepare demo keywords and URLs
   - Screenshot/video capture for presentation

4. **Documentation**
   - Update README with setup instructions
   - API documentation (FastAPI auto-docs)
   - Usage guide for presentation

---

## Critical Implementation Details

### Backend API Response Format
```json
{
  "query": "artificial intelligence",
  "entities": [
    {
      "id": "kg:/m/0b2x9",
      "name": "Artificial Intelligence",
      "type": ["Thing", "Field"],
      "description": "Intelligence demonstrated by machines",
      "source": "google_kg",
      "wikidata_id": "Q11660"
    }
  ],
  "relations": [
    {
      "source": "kg:/m/0b2x9",
      "target": "kg:/m/019_z5",
      "type": "related_to",
      "label": "Machine Learning",
      "weight": 0.9
    }
  ],
  "recommendations": [
    "Include entities: Machine Learning, Neural Networks",
    "Cover subtopics: Deep Learning, Natural Language Processing"
  ]
}
```

### Neo4j Graph Schema
```cypher
// Nodes
(:Entity {id, name, type, description, source, wikidata_id})

// Relationships
(:Entity)-[:RELATED_TO {weight, source}]->(:Entity)
(:Entity)-[:INSTANCE_OF]->(:Entity)
(:Entity)-[:SUBCLASS_OF]->(:Entity)
(:Entity)-[:PART_OF]->(:Entity)
```

### Environment Variables Required
```bash
# .env
GOOGLE_KG_API_KEY=your_api_key
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=your_password
WIKIDATA_SPARQL_ENDPOINT=https://query.wikidata.org/sparql
```

---

## Key Files to Create/Modify

### Backend
- `backend/app/api/routes/knowledge_graph.py` ✨ NEW
- `backend/app/services/google_kg_service.py` ✨ NEW
- `backend/app/services/wikidata_service.py` ✨ NEW
- `backend/app/services/nlp_service.py` ✨ NEW
- `backend/app/services/graph_fusion_service.py` ✨ NEW
- `backend/app/core/neo4j.py` ✨ NEW
- `backend/pyproject.toml` (add: neo4j, spacy, requests, SPARQLWrapper)
- `docker-compose.yml` (add Neo4j service)

### Frontend
- `frontend/src/components/SearchBar.tsx` ✨ NEW
- `frontend/src/components/GraphVisualization.tsx` ✨ NEW
- `frontend/src/components/Sidebar.tsx` ✨ NEW
- `frontend/src/components/OnboardingModal.tsx` ✨ NEW
- `frontend/src/routes/_layout/keyword-analysis.tsx` ✨ NEW
- `frontend/src/routes/_layout/url-audit.tsx` ✨ NEW
- `frontend/package.json` (add: cytoscape, @types/cytoscape)

---

## Success Criteria (MVP Checklist)

### Must Have ✅
- [ ] Keyword analysis endpoint functional
- [ ] Google KG + Wikidata integration working
- [ ] Interactive graph visualization
- [ ] Sidebar showing entity details
- [ ] Basic SEO recommendations
- [ ] URL audit with entity extraction
- [ ] Semantic coverage score calculation

### Nice to Have 🎯
- [ ] Co-occurrence analysis from NLP
- [ ] Relation weighting/filtering
- [ ] Export graph as image
- [ ] Dark/light theme
- [ ] Caching for repeated queries

### Presentation Demo 🎤
- [ ] Demo keyword: "Artificial Intelligence"
- [ ] Demo URL: Wikipedia article or major blog post
- [ ] Show graph interaction
- [ ] Explain SEO value proposition
- [ ] Highlight multi-source fusion

---

## Development Commands

### Setup
```bash
# Backend
cd backend
pip install -e .
alembic upgrade head

# Frontend  
cd frontend
npm install
npm run dev

# Docker (with Neo4j)
docker-compose up -d neo4j
docker-compose up backend frontend
```

### Testing
```bash
# Backend tests
cd backend
pytest

# Frontend tests
cd frontend
npm run test

# API manual testing
curl http://localhost:8000/api/knowledge-graph/keyword?query=seo
```

---

## Time-Saving Strategies

1. **Reuse Existing Template Structure**: The FastAPI template already has auth, DB setup, Docker configs → focus on KG-specific features
2. **Use FastAPI Auto-Docs**: Leverage `/docs` endpoint for API testing instead of building custom test UI
3. **Mock Data Initially**: Start with hardcoded graph responses, swap for real API calls later
4. **Simplify NLP**: Use spaCy's pre-trained model (en_core_web_sm) without custom training
5. **Wikidata Query Templates**: Prepare SPARQL queries in advance, parameterize them
6. **Graph Library**: Choose Cytoscape.js (better docs, easier for MVP)

---

## Risk Mitigation

| Risk | Mitigation |
|------|------------|
| API rate limits | Cache responses in Neo4j, implement request throttling |
| SPARQL timeout | Set query timeout, limit result size, use simpler queries |
| Graph rendering performance | Limit nodes displayed (max 50), implement lazy loading |
| NLP accuracy | Use entity types as fallback, display confidence scores |
| Time overrun | Cut URL audit if needed, focus on keyword analysis first |

---

## Agent Behavioral Guidelines

1. **Prioritize MVP Features**: Don't over-engineer; working > perfect
2. **Test Incrementally**: Run endpoints after each service implementation
3. **Document as You Go**: Add docstrings to all functions/classes
4. **Keep UI Simple**: Minimalist design, focus on graph clarity
5. **Handle Errors Gracefully**: Return meaningful error messages, don't crash
6. **Think SEO Value**: Every feature should answer "How does this help SEO?"

---

## Final Deliverable Structure

```
/home/aziz/workspace/full-stack-fastapi-template/
├── backend/
│   ├── app/
│   │   ├── api/routes/knowledge_graph.py
│   │   ├── services/ (KG, Wikidata, NLP, Fusion)
│   │   ├── core/neo4j.py
│   │   └── models.py (Graph schemas)
│   └── pyproject.toml (updated dependencies)
├── frontend/
│   ├── src/
│   │   ├── components/ (SearchBar, Graph, Sidebar)
│   │   └── routes/ (keyword-analysis, url-audit)
│   └── package.json (updated with graph libs)
├── docker-compose.yml (Neo4j service added)
├── .env (API keys, Neo4j config)
└── README.md (updated with KG tool instructions)
```

---

## Presentation Talking Points

1. **Problem**: SEO experts struggle to understand semantic relationships at scale
2. **Solution**: Visual knowledge graph merging Google, Wikidata, NLP
3. **Innovation**: Multi-source fusion with weighted relations
4. **Demo**: Live keyword analysis + URL audit
5. **Impact**: Faster content optimization, data-driven SEO strategy
6. **Tech**: FastAPI + Neo4j + Next.js + Cytoscape.js

---

**Remember**: This is a 24h hackathon. Velocity > perfection. Ship the MVP, iterate if time allows.

**Good luck at Hacktogone! 🚀**
