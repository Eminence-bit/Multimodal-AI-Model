# FusionQuery: Planning and Task Breakdown

This document outlines the development plan and task breakdown for **FusionQuery**, a multimodal AI query engine, over a 7-week timeline. It follows the milestones from the project specification, with tasks organized into sprints, deliverables for portfolio impact, and tracking for progress. The project uses a freemium tech stack: Next.js (Vercel), FastAPI (Render), LLaVA/BLIP-2 (Hugging Face Spaces), Cloudflare R2 (storage), and Pinecone (vector DB).

## Project Goal

Build a proof-of-concept AI system to process text, images, and optionally audio in unified queries (e.g., "What does this contract say about the warranty for the product in this photo?"). Target domains: education, e-commerce, research. Emphasize solving pain points (e.g., manual cross-referencing) for portfolio.

## Timeline Overview

- **Duration**: 7 weeks (part-time; ~10-15 hours/week).
- **Sprints**: 4 sprints (Weeks 1-2, 3-4, 5-6, 7).
- **Deliverables**: GitHub repo, demo video, screenshots, blog post, live demo URL.

## Sprint 1: Model Selection & Environment Setup (Weeks 1-2)

**Goal**: Establish foundation with model selection and local/cloud setup.

### Tasks Sprint1

1. **Model Selection**:
   - Benchmark LLaVA vs. BLIP-2 on sample data (e.g., VQA dataset).
   - Test inference speed/accuracy locally (use Hugging Face Transformers).
   - Select model (prefer LLaVA for VQA; fallback BLIP-2).
2. **Environment Setup**:
   - Initialize GitHub repo (`fusionquery/`), push structure.
   - Set up local dev with Docker Compose (backend, optional MinIO for testing).
   - Sign up for cloud services: Vercel, Render, Hugging Face Spaces, Cloudflare R2, Pinecone (all free tiers).
   - Configure `.env` files with API keys/URLs.
3. **Backend Skeleton**:
   - Create FastAPI app (`backend/app/main.py`) with `/health` endpoint.
   - Set up Pinecone index (2GB free tier) for embeddings.
   - Test R2 integration with `boto3` in `backend/app/services/storage.py`.
4. **Inference Script**:
   - Write basic script (`inference/app/multimodal.py`) for LLaVA/BLIP-2 inference.
   - Test image+text query locally (e.g., "Describe this image").

### Deliverables Sprint1

- GitHub repo with README (setup instructions, env vars).
- Working local inference script (e.g., `inference/app/multimodal.py`).
- Initial commits showing structure and early code.

### Success Metrics Sprint1

- Model selected (e.g., LLaVA achieves >80% VQA accuracy on sample).
- Backend API responds at `/health` (Render deployable).
- R2 and Pinecone accounts configured.

## Sprint 2: Backend API & File Handling (Weeks 3-4)

**Goal**: Implement core API logic and file handling.

### Tasks Sprint2

1. **API Development**:
   - Implement `/upload` endpoint (`backend/app/routers/upload.py`) for images/PDFs to R2.
   - Implement `/query` endpoint (`backend/app/routers/query.py`) to call HF Space API.
   - Implement `/history` endpoint (`backend/app/routers/history.py`) for query storage (use Pinecone metadata).
2. **File Handling**:
   - Integrate `boto3` in `backend/app/services/storage.py` for R2 uploads.
   - Add preprocessing (`inference/app/preprocess/embeddings.py`) for CLIP embeddings, upsert to Pinecone.
3. **Auth & Error Handling**:
   - Add JWT auth (`backend/app/services/auth.py`) for user sessions.
   - Implement error handling (e.g., "Invalid image format") with status codes.
4. **Testing**:
   - Write Pytest cases (`backend/tests/test_endpoints.py`) for endpoints (80% coverage).
   - Test multimodal fusion (e.g., image captioning + text Q&A).

### Deliverables Sprint2

- API docs (Swagger at `/docs` via FastAPI).
- Unit tests covering 80% of backend code.
- Commits showing endpoint implementation.
- Screenshot of API response for `/demo/screenshots/api-response.png`.

### Success Metrics Sprint2

- Uploads to R2 succeed (e.g., image/PDF stored).
- Queries return fused results (e.g., image+text response).
- Auth protects endpoints; errors return clear messages.

## Sprint 3: Frontend Integration (Weeks 5-6)

**Goal**: Build user-facing Next.js UI and integrate with backend.

### Tasks Sprint3

1. **Frontend Development**:
   - Build UI (`frontend/app/page.tsx`) with drag-and-drop upload (`UploadForm.tsx`).
   - Create query form (`QueryInput.tsx`) and results display (`ResultsDisplay.tsx`).
   - Add query history UI (`HistoryList.tsx`) with loading states (`LoadingSpinner.tsx`).
2. **API Integration**:
   - Use `frontend/hooks/useApi.ts` for backend API calls (e.g., `/upload`, `/query`).
   - Display results with thumbnails (from R2 signed URLs) and highlighted text.
3. **Testing & Polish**:
   - Test UI responsiveness (TailwindCSS + Shadcn/UI).
   - Add accessibility (ARIA labels in `frontend/components/`).
   - Capture screenshots for `/demo/screenshots/` (e.g., `upload-ui.png`, `query-results.png`).
4. **Bonus (if time)**:
   - Add audio upload support (Whisper via HF Space) for transcription queries.

### Deliverables Sprint3

- End-to-end prototype (frontend → backend → inference → R2/Pinecone).
- Screenshots in `/demo/screenshots/` (UI, results, history).
- Demo video script drafted (record in Week 7).
- Commits showing frontend integration.

### Success Metrics Sprint3

- UI supports image+PDF upload and query submission.
- Results display correctly (e.g., 10s response time).
- History retrieves past queries from backend.

## Sprint 4: Hosting & Polish (Week 7)

**Goal**: Deploy to cloud, polish, and finalize portfolio assets.

### Tasks Sprint4

1. **Deployment**:
   - Deploy frontend to Vercel (`frontend/`) with GitHub integration.
   - Deploy backend to Render (`backend/`) with environment vars.
   - Deploy inference to Hugging Face Spaces (`inference/`) with LLaVA/BLIP-2.
   - Configure R2 bucket and Pinecone index; update backend env vars.
2. **Polish**:
   - Add HTTPS (Vercel/Render auto-TLS).
   - Optimize latency (e.g., async calls to HF Space, signed URLs for R2).
   - Ensure mobile responsiveness and accessibility (ARIA labels).
3. **Demo & Portfolio**:
   - Record 2-3 min demo video (show upload, query, results, history).
   - Upload video to YouTube; update `demo/README.md` and `README.md` with link.
   - Capture final screenshots for `/demo/screenshots/`.
   - Write blog post (Medium/personal site): Problem → Solution → Tech → Challenges → Results (e.g., “85% VQA accuracy, 10s analysis vs. 5min manual”) → Learnings.
4. **Monitoring**:
   - Use Vercel analytics and Render logs for basic metrics (e.g., latency).
   - Document metrics in blog post (e.g., “Processed 100+ queries”).

### Deliverables Sprint4

- Live demo URL (e.g., `fusionquery-frontend.vercel.app`).
- Demo video on YouTube (linked in `README.md`).
- Blog post draft (Medium or personal site).
- Final commits with polished code and docs.

### Success Metrics Sprint4

- App runs fully online (no local setup).
- Demo video shows all MVP features (image+text fusion, history).
- Blog post quantifies impact (e.g., reduced analysis time).

## Retrospectives

- **Weekly Retrospectives**:
  - Week 2: Reflect on model selection (e.g., “LLaVA faster but needs optimization”).
  - Week 4: Note API challenges (e.g., “R2 upload latency improved with async”).
  - Week 6: Discuss UI polish (e.g., “Added ARIA for accessibility”).
  - Week 7: Summarize learnings (e.g., “HF Spaces simplified ML hosting”).
- **Portfolio**: Include retrospectives in blog post and `README.md` for transparency.

## Notes

- **Freemium Constraints**: Stay within Vercel (100GB bandwidth), Render (512MB RAM), HF Spaces (free CPU/GPU), R2 (10GB), Pinecone (2GB) limits.
- **Portfolio Appeal**: Clean repo, demo video, screenshots in `/demo/`, and blog post emphasize real-world impact (e.g., “Bridging data silos”).
- **Stretch Goals**: If time allows, add real-time collaboration (e.g., share query sessions via Vercel API) or fine-tune LLaVA on a small dataset (HF Spaces).
