**HOW TO CHOOSE YOUR NEW HOME AT VANCOUVER AS A UBC STUDENT (OVERCOMPLICATED)**


**Introduction**
I created this project as way to apply what I learned during my Master of Business analytics study. I grew interest in linear programming subjects, and how it can optimally help us decide on the best listings line up that satisfies trade off between customer preferences and budgets.


**Application**
This methodology can be applied to numerous other business applications, for example e-commerce listings. The MILP can be expanded to how e-commerce websites recommend product assortments while balancing profit margin as part of the constraints.

**HOW TO / DISCLAIMER**
Gurobi is a paid mathematical solver tools, in case my lisence is only for academic purpose, not commercial. For showcase purpose, I have attached the Gurobi Code and MILP formula in the repository (HousingOptimizer.ipynb). It can be run locally on your machine.


**🔍 Project Overview**

Helping UBC students find the best balance between rent, commute, and safety
Finding affordable and safe housing near UBC is a persistent challenge for students. Our project builds a data-driven web application that scores every postal code in Greater Vancouver on key factors [...]
The tool helps students answer the question:
“Given my budget, which listing offers the best overall living value for me?”


<img width="468" height="282" alt="image" src="https://github.com/user-attachments/assets/d8de95c1-526f-4755-85aa-709595f3613a" />


Methods

	Step 1) Scrapping from Craiglist data on Vancouver Rent, and clean for duplications.
	I decided to scrap my own data, since publicly available data i.e. Kaggle, doesn't have the required data quality I have. I used website scrapper called 			https://dashboard.scraperapi.com/home, and clean it up using my ChatGPT. 

	Of course, craiglist data might have some fake listings or invalid ones, but at least for the purpose of this model it generates price, number of rooms, and locations, which are good enough for minimum viable product (MVP) launch.


	Step 2) Visualization: mapping each neighbourhood price and crime rate
	
	<img width="2134" height="1446" alt="image" src="https://github.com/user-attachments/assets/79f79a88-6c1b-4eb9-a3b6-7715939a2f18" />
	
<img width="935" height="658" alt="image" src="https://github.com/user-attachments/assets/2439b28b-b686-48b9-bb71-da6216dfbf6a" />



	Step 3) Optimization: MILP to maximize safety/affordability under a commute constraint.
	**Linear Programming Formulation**

<img width="497" height="654" alt="image" src="https://github.com/user-attachments/assets/b94f450b-c6a6-4ccc-8fae-4e961de619f0" />

<img width="420" height="409" alt="image" src="https://github.com/user-attachments/assets/4d00355c-0129-485b-96f9-9136984c72c8" />

	Step 4) Interactive Web Dashboard + API
		Enter budget → get Top 5 postal codes ranked by score
		Enter tradeoff appetite (transport modes, safety preference etc)
		Explore results on user input with filters (budget, commute, safety priority)
		Optimal Decision Results

<img width="1046" height="629" alt="image" src="https://github.com/user-attachments/assets/383776c1-0b6e-4805-bee6-3247e77204ac" />


**Results**
 
The crime hard constraint (must be < 50 incidents per 1,000) knocks out only Downtown (~128) and Strathcona (~145). Every other neighbourhood in the table is “eligible” on safety. Within the eligi[...] 

Commute and price show the real trade-off: commute ranges from ~14–15 mins (Dunbar-Southlands, West Point Grey) up to 84 mins (Hastings-Sunrise), and average price ranges from about $620–$625 (Sun[...] 

<img width="836" height="519" alt="image" src="https://github.com/user-attachments/assets/5999acef-abb0-4fed-a0a7-6c7515b5b289" />


The optimizer’s Top-5 under the crime cap (<50/1,000) are: Dunbar-Southlands, West Point Grey, Shaughnessy, Kitsilano, and South Cambie. All five are 1-mode routes (so no modes penalty kicking in), [...]
The pattern is clear: the model is prioritizing short commutes and safety, then using price to break ties. Dunbar-Southlands wins because it’s very safe and the fastest commute (14 mins) at a reason[...]
