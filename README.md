# Building Parks in King County

## Business Question

The King County Parks Department has approached our team to help build an argument for investing more in public green spaces and to identify opportunities to build community parks and gardens. They want an alternative to building in existing public space and have come to us for a unique approach. Our proposed solution is to build a linear regression model to help the client evaluate home listings to find undervalued properties in priority ZIP codes. The client can then approach these homeowners to work out a deal for converting the home into a park.

## The Data

Three unique datasources were used in the modeling process. The first is a record of house sales in King County for the years 2021-2022 (source: King Country Department of Assessments https://info.kingcounty.gov/assessor/DataDownload/default.aspx). This dataset includes details about the homes being sold, including prices, sale dates, home features (bedrooms, bathrooms etc.) and locations. The second dataset includes metrics evaluating the quality of the different school districts in King County (source: Washington Office of Superindendent of Public Instruction https://washingtonstatereportcard.ospi.k12.wa.us/ReportCard/ViewSchoolOrDistrict/100117). These metrics included standardized test scores, investment per student and graduation rates. The final dataset used included statistics around crime in neighborhoods in King County (source: The King County Sheriff’s Office https://data.kingcounty.gov/Law-Enforcement-Safety/KCSO-Offense-Reports-2020-to-Present/4kmt-kfqf).

## Data Cleaning 

Prior to merging any data together or creating models, the King County Housing Data was cleaned to remove any irrelevant or incomplete data. The largest change to the dataset came from removing homes with ZIP codes outside of King County. Additionally, the dataset was checked for duplicate IDs and addresses. The record with the most recent sale date was the one chosen to remain in the dataset. Any outliers with over 10 bedrooms or exactly 0 bedrooms were also removed from the dataset. The end sample included 28,971 houses which could be linked to education or data and be used for regression. 

## Exploratory Data Analysis (EDA)

As part of the EDA process, heatmaps and pairplots were created to identify any relationships between variables that need to be addressed in the upfront. This revealed that the target variable was not normally distributed. To address this, a log transformation was applied to the price column and log_price (price) became the new target.  

## Baseline Model

The baseline model used was a linear regression using a single varibale to predict price. EDA revealed that sqft_living was the most highly correlated feature with the target variable, so it was chosen for use in the baseline model. The resulting model had a R-squared value of 0.356 and would be the comparison point for our subsequent models.

<img width="747" alt="Screenshot 2023-02-17 at 4 00 26 PM" src="https://user-images.githubusercontent.com/66101132/219792423-36d92cba-e447-40f9-ba46-5faf019131da.png">

## Model Progression

After the baseline model, 6 additonal models were ran. In each model, features were added or removed to try to maximize the R-squared value. Model progression:

Model 1: The remainder of the numeric features in the King County Housing dataset were added to the baseline model. The resulting regression produced a R-squared value of 0.439

Model 2: New interaction features were created for numeric variables that had high multicolinearity. Features that were non-influential in model 1 were removed. The resulting regression produced a R-squared of 0.445

Model 3: Ordinal (including binary) categorical features from the King County Housing dataset were introduced into the model. The resulting regression produced a R-squared of 0.519

Model 4: Nominal categorical features from the King County Housing dataset were introduced into the model. Some features such as City, Bedrooms, Bathrooms, heat_source and sewer_system were removed from the model since they were either non-influential or contributed to a high condition number. This is the first model where a train-test split was used. The model was trained using 66% of the data while the test was run using 33% of the data. The resulting regression produced a test R-squared of 0.7201

Model 5: School data from the education dataset was added. The resulting regression produced a test R-squared of 0.7265

Model 6: Crime rate data from the Crime dataset was added. The resulting regression produced a test R-squared of 0.7052


## Final Model

Models 5 & 6 were chosen as the final models for consideration. To measure the accuracy of the model, the mean absolute error (MAE) of both models were calculated. The MAE calculates the average distance of the model's predicted price from the actual observed price in our test sample. The MAE of model 5 test was $192,704. The MAE of model 6 test was $198,955. Since model 5 was both a stronger predictor of price and had less error, it was chosen as the final model. 

The distribtuion of the predicted home prices vs. actual home prices has a strong linear relationship and the residuals are tightly clustered around each other. This shows that the model is both a good predictor of home prices, but also stays consistent with its predictions for all home prices.


![Test_graph](https://user-images.githubusercontent.com/66101132/219791871-be76869f-8db0-436f-b86e-ca5ef5b97c4d.png)


## Recommendations 

Among the coefficients of our final model, two that are worth highlighting are ZIP code and greenbelt. ZIP code was the largest coefficient, meaning it has the largest influence on home price. Greenbelt, which encodes whether the property is adjacent to a greenbelt, also has a positive influence on home price. These two coefficients were used in combination to make the following recommendations to our stakeholders:

* Model results validate that proximity to green space increases property values. This can be used as a supporting piece of evidence when applying for funding

* The town’s parks department should prioritize ZIP codes with low access to greenbelts and low property values relative to the rest of King County

* Our model can be used to accurately evaluate home listings to determine whether a home is being sold below value and may be a potential target to convert into a public greenspace 

From the inital EDA of the data set, the ZIP codes 98188, 98198, 98023, 98001, 98003, 98032, 98354, 98168, 98047, 98002 were identified as the bottom ten zipcodes in terms of average house price. These ZIP codes should be the client's priority targets. 

![download](https://user-images.githubusercontent.com/66101132/219791947-ae7a156b-2c30-468a-a61a-84804ee165d9.png)


The client could then use the model to evaluate new listings based on their characteristics to identify undervalued properties. If the home meets the client's requirements, the client could approach the home owner about making a deal for the home to convert the space into a public greenspace. The example below uses the model to evaluate projected home prices for poor condition homes in priority ZIP codes with no greenspace and cost less than $300k

![download (3)](https://user-images.githubusercontent.com/66101132/219792735-1c11dd72-d924-4e23-ae94-ebbdbefac861.png)

## Next Steps

The model created can be used to predict home prices in King County with a fair degree of accuracy, but it does not inform the client about whether public greenspaces improve other aspects of community health or how many greenspaces to build in a given zipcode. Given more time and data, the project could be expanded in the following ways:

* Add other environmental data to our model with the hope of getting clearer insights on how greenspace affects price 

* Rework model using environmental data to predict other indicators of a healthy community (student test scores, medical costs, crime rates)

* Expand EDA to categorize homes as empty lots or abandoned. This may require additional housing data











