Learn With Us : The Process Behind the Build 
A Technical Overview of the Architecture, AI 
Integration, Prompt Design, Performance, and 
Limitations 
1. Implementation Architecture 
Learn With Us (LWU) was designed as a two-part system consisting of a React + Vite 
frontend and an Express 5 backend API. Both services exist inside a pnpm monorepo, 
allowing shared code such as database schemas and API types to be reused across 
the entire project. This setup helped maintain consistency between the frontend and 
backend while reducing development errors. 
The frontend was built using React with shadcn/ui components to create a clean and 
responsive interface suitable for teachers, tutors, and students. Routing is handled 
using Wouter, while TanStack Query manages caching and API state. API hooks are 
automatically generated from the OpenAPI specification using Orval, which removes the 
need for manually written fetch logic and keeps the frontend aligned with backend 
changes. 
The backend runs on Node.js with Express 5 and handles two main responsibilities: 
generating AI content and managing database operations. PostgreSQL was selected as 
the database solution because of its reliability and structured data support. Drizzle ORM 
was used for type-safe database queries without adding unnecessary complexity. 
In production, the frontend compiles into static files that can be served directly, while the 
backend processes only API-related requests. This keeps deployments lightweight and 
improves loading performance. 
2. API Selection Rationale 
LWU uses OpenAI’s GPT models through Replit AI Integrations. OpenAI was selected 
because it consistently produced the most reliable and structured educational outputs 
during testing. With the project requiring lesson plans, study guides, and academic 
content with strict formatting rules, consistency was really important. 
The application uses Server-Sent Events (SSE) to stream AI responses in real time 
instead of waiting for the full output before displaying anything. Educational content can 
often take 30–60 seconds to generate, especially for university level material. Streaming 
allows users to see text appearing almost immediately, creating a much smoother 
experience. 
Another advantage of using Replit AI Integrations is security. The application does not 
expose raw API keys because authentication and billing are hanndled through Replit’s 
managed proxy system. This makes the project safer to share and easier to deploy 
publicly. 
3. Prompt Engineering Methodology 
Prompt engineering became the most important part of the project. Basic prompts made 
for inconsistent and generic outputs, so each template was carefully designed using 
structured prompting techniques. 
Every template begins with role setting instructions that position the AI as an 
experienced South African educator or academic specialist. Examples include 
curriculum designers, university lecturers, and subject tutors. This made the generated 
content feel more realistic and contextually appropriate. 
South African context was intentionally included throughout the prompts. Local educator 
names such as Dr. Nomsa Dlamini, Sipho Nkosi, and Prof. Kefilwe Sithole were used to 
ground the responses within familiar educational settings. Examples and topics related 
to South African history, CAPS curriculum standards, and local classroom environments 
were also incorporated where relevant. 
To improve grade level accuracy, each prompt included strict “ABSOLUTE 
CONSTRAINTS” covering: 
 Age range  
 Reading level  
 Vocabulary complexity  
 Cognitive difficulty  
This reduced the tendency of the AI to default to a primary-school writing style 
regardless of audience. 
The prompts also enforced strict output formatting using predefined markdown 
headings. Educational frameworks such as Bloom’s Taxonomy, the Feynman 
Technique, and Socratic questioning were embedded into different templates depending 
on the content type being generated. 
4. Performance Optimisation Techniques 
The main performance improvement came from SSE streaming. Instead of forcing 
users to wait for a completed AI response, the backend streams content token-by-token 
to the frontend in real time. This reduced perceived waiting time significantly and made 
the application feel far more interactive. 
TanStack Query was also used to cache frequently accessed data such as generation 
history and saved outputs. This allowed pages to load instantly when revisited while still 
checking for updates in the background. 
Another optimisation involved generating API hooks automatically from the OpenAPI 
specification. This reduced duplicated code, simplified maintenance, and ensured better 
type safety between the frontend and backend. 
Finally, deploying the frontend as static files improved loading speed and reduced 
server overhead because only API requests require backend processing. 
5. Limitation Management Strategies 
Although LWU performs well overall, several limitations still exist. 
One issue is formatting inconsistency on very long or highly specialised topics. 
Occasionally, the AI may skip sections or combine headings incorrectly. Strong 
formatting constraints in the prompts reduced this problem, but it can still happen in 
edge cases. 
Another challenge involves niche topics. When prompts are too broad or unclear, the AI 
may produce generic educational content instead of highly specific material. To address 
this, the system includes an “Additional Context” field where users can provide extra 
detail. 
Grade level drift is also a minor issue. Despite strict constraints, some advanced 
university content may become oversimplified, while younger-grade content may 
occasionally include slightly advanced vocabulary. 
Language support is another limitation. South Africa has 11 official languages, but LWU 
currently works best in English. Users can manually request Afrikaans or isiZulu outputs 
through the prompt, but proper multilingual support has not yet been fully implemented. 
Lastly, LWU currently functions mainly as a teacher-facing content generator. Students 
cannot yet interact directly with quizzes, automated marking, or annotations. Expanding 
the platform into a more interactive learning environment would be a strong future 
improvement. 
Conclusion 
LWU was developed as a functional AI-powered educational content generator 
designed for primary school, high school, and university use. The project combines 
modern web technologies with carefully engineered prompts to produce structured and 
curriculum-aware educational materials. 
While the coding architecture was important, the quality of the outputs depended heavily 
on prompt engineering, formatting control, and educational design principles. The 
project demonstrates how AI can assist educators by reducing preparation time and 
generating adaptable learning resources while still highlighting the importance of human 
oversight and instructional design. 
