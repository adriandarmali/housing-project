HOW TO CHOOSE YOUR NEW HOME AT VANCOUVER AS A UBC STUDENT (OVERCOMPLICATED)<img width="468" height="105" alt="image" src="https://github.com/user-attachments/assets/d2fd05e8-a23f-4c1a-aba1-6b22c2ef390c" />

Introduction
I created this project as way to apply what I learned during my Master of Business analytics study. I grew interest in linear programming subjects, and how it can optimally help us decide on the most mundane things in our life. In this case, it would be how we best choose our place as a UBC student. Combining with data visualisation class and web scrapping programming method, I created this dashboard as a way for me to apply what I have learnt in class to solve real life problem.
<img width="468" height="147" alt="image" src="https://github.com/user-attachments/assets/9f442ceb-e96e-4498-bab0-176fdbbe57e8" />

🔍 Project Overview

Helping UBC students find the best balance between rent, commute, and safety
Finding affordable and safe housing near UBC is a persistent challenge for students. Our project builds a data-driven web application that scores every postal code in Greater Vancouver on key factors that matter most — rent affordability, commute time, and neighborhood safety.
The tool helps students answer the question:
“Given my budget, which area offers the best overall living value for me?”

Category	Source	Example Fields
Rent Data	Craigslist	Average rent by room type, neighbourhood
Safety Data	City of Vancouver Open Data	Crime rate per 1,000 residents
Transit Data	TransLink GTFS	Commute time from postal code centroid to UBC campus
Spatial Context	Vancouver zoning, census population	Density, land use, demographics

Methods
	Multi-criteria scoring model
 Normalize and weight each factor (rent, commute, safety) → compute composite “Liveability Score.”


	Normalization via min–max scaling or winsorization to reduce outliers


	Visualization: mapping each neighbourhood price and crime rate


	Optimization: Linear programming to maximize safety/affordability under a commute constraint.

Interactive Web Dashboard + API
	Enter budget → get Top 5 postal codes ranked by score
	Enter tradeoff appetite (transport modes, safety preference etc)
	Explore results on user input with filters (budget, commute, safety priority)
	“How this score works” info page for transparency
	Optimal Decision Results

<img width="1046" height="629" alt="image" src="https://github.com/user-attachments/assets/383776c1-0b6e-4805-bee6-3247e77204ac" />


Week	Focus Area	Key Deliverables
W1 — Data & Design Foundations	• Finalize scoring metrics and weighting framework (safety, affordability, commute).• Set up repository, database schema, and environment (Python + Gurobi + PostGIS/SQLite).• Ingest and clean base datasets: City of Vancouver crime data, Craigslist, TransLink GTFS.• Define spatial unit (postal code or neighborhood).	Integrated raw dataset + project data dictionary.
W2 — Normalization & Market Insight Integration	• Apply winsorization and min–max normalization for each quantitative metric.• Design and distribute a student housing survey (Google Forms or Qualtrics) capturing: – Housing priorities (price vs. safety vs. commute) – Preferred neighborhoods – Tolerance levels for rent and commute.• Incorporate survey aggregates as weight priors or a “student sentiment” layer influencing (w_s, w_a, w_c).• Generate preliminary visualization (heatmap of preferences).	Clean, normalized dataset + survey summary + first version of weighted scoring model.
W3 — Optimization & API Layer	• Implement Gurobi binary optimization model: – Hard safety constraint (s_i \ge s_{\min}) – Soft budget & commute penalties (only for excess values) – Top-K recommendations.• Build backend API (Flask/FastAPI) allowing real-time solver runs based on user inputs.• Write unit tests for score computation + solver logic.	Functional optimization backend returning Top-K shortlist.
W4 — Dashboard & User Testing	• Develop interactive web dashboard (map + sliders for budget, commute, and weights).• Integrate backend API for live optimization.• Conduct user acceptance testing with students, analyze feedback, and fine-tune λ-penalties and scoring weights.• Deploy MVP (e.g., Streamlit or web app) + documentation + refresh scripts.	Deployed MVP + testing report + update pipeline guide.

Linear Programming Formulation
<img width="478" height="468" alt="image" src="https://github.com/user-attachments/assets/79696a11-231f-4f0c-b7a5-c759ed289fbe" />
<img width="478" height="251" alt="image" src="https://github.com/user-attachments/assets/6ea0f84a-fda2-4e39-9249-5b34076156f7" />
<img width="508" height="629" alt="image" src="https://github.com/user-attachments/assets/ff322c84-1ac7-48cc-86de-65cb4f97c050" />


Results
 
 
 
The crime hard constraint (must be < 50 incidents per 1,000) knocks out only Downtown (~128) and Strathcona (~145). Every other neighbourhood in the table is “eligible” on safety. Within the eligible set, the safest-looking areas are Dunbar-Southlands (~10), Killarney (~12), Victoria-Fraserview (~12), West Point Grey (~14), Shaughnessy (~16), and Oakridge (~17), while West End (~46) is close to the cap and should be treated as borderline.
Commute and price show the real trade-off: commute ranges from ~14–15 mins (Dunbar-Southlands, West Point Grey) up to 84 mins (Hastings-Sunrise), and average price ranges from about $620–$625 (Sunset, South Cambie) up to $1,321–$1,313 (West End, West Point Grey). Most neighbourhoods have 1 mode, but West End (3 modes) and several 2-mode areas (e.g., Sunset, Marpole, Victoria-Fraserview) will be penalized under your modes preference; combined with your high value on time, long-commute areas like Hastings-Sunrise (84 mins) will usually lose even if they look cheap.

<img width="468" height="588" alt="image" src="https://github.com/user-attachments/assets/3ddcab86-e8d8-4e51-95ee-cae424ef7c7d" />

 
The optimizer’s Top-5 under the crime cap (<50/1,000) are: Dunbar-Southlands, West Point Grey, Shaughnessy, Kitsilano, and South Cambie. All five are 1-mode routes (so no modes penalty kicking in), and all have crime well below the threshold (roughly 10–34 per 1,000). Commute times are mostly tight (14–25 mins) except South Cambie (35 mins), which still makes the cut because it’s much cheaper.
The pattern is clear: the model is prioritizing short commutes and safety, then using price to break ties. Dunbar-Southlands wins because it’s very safe and the fastest commute (14 mins) at a reasonable price, while West Point Grey stays in despite being expensive because its commute is also extremely short (15 mins) and crime is low. South Cambie is the “value” pick—its commute is longer and crime is higher than the others in the list, but the low rent (~$625) compensates enough to keep it in the Top-5.
<img width="468" height="551" alt="image" src="https://github.com/user-attachments/assets/88b327f0-2ed5-4ed6-aac0-a8110150d5fa" />



