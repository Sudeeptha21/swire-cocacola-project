**Predicting Cart Abandonment on MyCoke360**

**🏢 Project Overview**

The goal of this project was to help Swire Coca-Cola predict cart abandonment on their B2B ordering platform MyCoke360.

Many customers start adding products to their carts but fail to complete their orders before their order window ends, resulting in lost revenue and missed engagement opportunities.

We built a machine learning solution to identify at-risk customers early within the first 30% of their order window, so the sales team can intervene before the purchase is lost.

**🎯 Business Impact**

•	14.6% cart abandonment rate → approximately $2.87M in annual lost revenue.

•	Restaurants, Stores, and Hotels had the highest abandonment (15–17%).

•	Early intervention at just 5–15% success rate could recover up to $300K+ in revenue.

**⚙️ Methodology**

**1. Data Integration**

Combined multiple data sources:

•	Google Analytics events

•	Orders, Sales, Visit Plans, Cutoff Times, and Customer information

•	Each table provided event-level, transactional, and customer context

**2. Rolling Window Creation**

•	Created customer-specific order windows using ANCHOR_DATE, FREQUENCY, and CUTOFFTIME__C.

•	Defined cutoff periods = 30% of window frequency to flag early abandonment behaviour. Example:

Window Frequency (Days)	Cutoff (Days)
7	                        2
14	                        3
	
**3. Labelling Logic**

•	A window is labelled ABANDONED_CART = 1 if a customer adds items to the cart but no order is placed before the cutoff period.

•	If an order (CREATED_DATE_UTC) exists within the window → ABANDONED_CART = 0

**4. Modelling**

Tested multiple algorithms:

•	Logistic Regression (baseline)'

•	Random Forest

•	XGBoost (best performer with AUC ≈ 0.73, Recall ≈ 0.55)

Model tuned for recall, ensuring maximum detection of likely abandoners for early intervention.

**💡 Key Insights**

•	Restaurants and Stores are most likely to abandon carts.

•	Longer shipping windows (72 hours) correlate with higher abandonment (~22%)

•	Manual distribution modes (Tell Sell) underperform automated ones (OFS, Rapid Delivery).
•	Events like remove_from_cart, update_cart, and login show strong predictive power.

**📈 Results**

Metric	Accuracy	Recall (Abandoners)	AUC

Logistic Regression	 
81.8%	 71.2%	 0.69
XGBoost	
74.1%	 55.0%	 0.73

Best performing model: XGBoost tuned for recall effective for triggering early reminders and recovering lost orders.

**🔍 Preventing Data Leakage**

•	Removed CUSTOMER_ID and ABANDONED_CART from training features.

•	Dropped post-outcome event columns (e.g., purchase, add_payment_info, begin_checkout) to ensure the model only uses pre-checkout signals.

•	This guarantees true predictive validity and prevents the model from “cheating.”

**Business Value of the Solution**

Our predictive model provides measurable business value for Swire Coca-Cola:

•	Reduces Lost Revenue: With cart abandonment causing an estimated $2.87M in annual losses, even modest intervention success (5–15%) can recover $140K–$430K

•	Enables Early, Targeted Intervention: Detecting abandonment within the first 30% of the order window allows the sales team to reach out when it matters most.

•	Improves Customer Engagement: By identifying customers who are delaying checkout, Swire can provide support, leading to lower cart abandonment rates.

•	Optimizes Sales Team Effort: Instead of contacting all accounts, sales teams receive prioritized, high-risk customer lists, making outreach more efficient.

**🚧 Difficulties Our Group Encountered**

Throughout the project, we faced several challenges that shaped our approach and improved our problem-solving:

•	Complexity of Order Windows: Customers had varying order frequencies and anchor dates, making it difficult to define rolling windows consistently. Creating the 30% cutoff logic required multiple iterations and validation cycles.

•	Data Integration Issues: Combining Google Analytics data with transactional and customer tables required careful cleaning, timestamp alignment, and deduplication.

•	Class Imbalance: Abandonment events were relatively rare compared to non-abandonment, requiring advanced balancing techniques like SMOTE and class weighting.

•	Avoiding Data Leakage: Ensuring that no post-checkout events or future information leaked into the training set forced us to rebuild several feature pipelines.

•	Model Performance Trade-Offs: We needed a high recall to capture as many likely abandoners as possible, which meant sacrificing some accuracy. Choosing the right balance across models (Logistic Regression vs. XGBoost) required careful evaluation.

•	Ambiguity in Customer Behaviour Interpretation: Some customers add items “for later” or use carts as planning tools. This required thoughtful labeling rules and collaboration with business stakeholders.

**🎓 What We Learned from the Project**

This project provided several important technical and business lessons:

•	How Real-World Data Differs from Clean Academic Datasets: We learned how messy, inconsistent, and incomplete enterprise data can be and how to manage it.

•	Importance of Domain Knowledge: Understanding Swire’s order windows, distribution modes, and customer workflows was essential to building meaningful features.

•	Value of Early Prediction in Business Settings: Unlike traditional predictive modeling, the goal was not pure accuracy, it was enabling timely intervention. This shifted our evaluation focus to recall and real-world usefulness.

•	Feature Engineering Creativity: Turning raw user events into meaningful behavioral signals was one of the most valuable parts of the experience.
•	Ethical & Technical Care with Data Leakage: We gained a deeper understanding of why leakage invalidates models and how to prevent it.
